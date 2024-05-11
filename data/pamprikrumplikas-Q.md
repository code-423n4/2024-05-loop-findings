# [01] `onlyAuthorized` modifier introduces unnecessary centralization via  in `PrelaunchPoints::convertAllETH`

`PrelaunchPoints::convertAllETH` is protected by the `onlyAuthorized` modifier, restricting access to this function to the protocol owner:

```javascript
    function convertAllETH() external onlyAuthorized onlyBeforeDate(startClaimDate) {...}
```

## Impact
The restriction deepens centralization unnecessarily, exposing users to associated risks, and making the flow of events less predictable to the users.

Users should be able to start claiming LpETH right after the `TIMELOCK` period passes following the succesfull execution of a call to `PrelaunchPoints::setLoopAddresses`. 

Claims are enabled upon a successful call to `PrelaunchPoints::convertAllETH`, but as long as it is protected by the `onlyAuthorized` modifier, the authorized account can chose to delay to call this function or can decide to never call it at all, introducing uncertainty and risk for the users.

## Recommended Mitigation Steps
Remove the `onlyAuthorized` modifier in `PrelaunchPoints::convertAllETH`.


# [02] Not allowed tokens cannot be recovered if they get allowed after having been sent to the contract

`PrelaunchPoints::recoverERC20` is designed to enable the protocol owner to recover any not allowed tokens that users mistakeanly send to the protocol. However, `PrelaunchPoints::allowToken` does not check whether the protocol, by user mistake, has previously received any funds in the token to be allowed.

## Impact

ERC20 funds are lost in the following scenario:

1. ERC20 token `tokenA` is not allowed by the protocol.
2. `UserA` mistakeanly transfers some `tokenA` to the protocol.
3. Protocol owner allows `tokenA` in the protocol, and does not check whether the contract already has any balance of the token.
4. When trying to recover the now allowed `tokenA` for `UserA`, `PrelaunchPoints::recoverERC20` will revert with `NotValidToken`.

Proof of code:

```javascript
    function test_cannotRecoverTokenIfWasAllowedMeanwhile() public {
        address user = makeAddr("user");
        uint256 balance = 2e18;

        // token that is not allowed yet
        LRToken lrtB = new LRToken();
        lrtB.mint(address(this), INITIAL_SUPPLY);
        lrtB.transfer(user, balance);

        vm.startPrank(user);
        lrtB.approve(address(prelaunchPoints), balance);
        lrtB.transfer(address(prelaunchPoints), balance);
        vm.stopPrank();

        // when it is allowed, it is possible to recover
        prelaunchPoints.recoverERC20(address(lrtB), balance / 2);

        // when it gets allowed, it becomes impossible to recover
        prelaunchPoints.allowToken(address(lrtB));
        vm.expectRevert(PrelaunchPoints.NotValidToken.selector);
        prelaunchPoints.recoverERC20(address(lrtB), balance / 2);
    }
```

## Recommended Mitigation Steps

Implement a check so that a token cannot be allowed if the contract has any balance in that token - i.e. before allowing `token X`, the protocol owner will need to recover any funds of `token X`  from the protocol.

```diff
    function allowToken(address _token) external onlyAuthorized {
+        if (_token.balanceOf(address(this)) != 0) {
+            recoverERC20(_token, _token.balanceOf(address(this)));
+        }
        isTokenAllowed[_token] = true;
    }

-    function recoverERC20(address tokenAddress, uint256 tokenAmount) external onlyAuthorized{...}
+    function recoverERC20(address tokenAddress, uint256 tokenAmount) public onlyAuthorized{...}
```

# [03] Critical privilages are transferred in one step instead of two

`owner` has critical privilages in the protocol. `PrelaunchPoints::setOwner` allows the `owner` to transfer ownership (and associated critical privilages) in a one-step process.

## Impact

Critical `owner` priviliges could be transferred to an incorrect address e.g. if
- `owner` mistakeanly inputs an incorrect address when calling `PrelaunchPoints::setOwner`,
- the protocol becomes the victim of a Clipboard Replacement Attack: protocol owner copies the address that ownership is supposed to be transferred to, but a malware replaces the address on the clipboard with a different, attacker-controlled address that the protocol owner will eventually end of pasting when preparing to call `PrelaunchPoints::setOwner`.

With the ownership privilages transferred to an incorrect account, the whole protocol will be compromised/unusable.

## Recommended Mitigation Steps

Transfer critical priviliges in a 2-step process. Modify `PrelaunchPoints` as follows:

```diff

contract PrelaunchPoints {

     ...

+    address public newOwner;

     ...

-    event OwnerUpdated(address newOwner);
+    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);
+    event NewOwnerProposed(address indexed proposedOwner);

...

-    function setOwner(address _owner) external onlyAuthorized {
-        owner = _owner;
-        emit OwnerUpdated(_owner);
-    }


+    // Step 1: Propose a new owner
+    function proposeNewOwner(address _newOwner) external onlyAuthorized {
+        newOwner = _newOwner;
+        emit NewOwnerProposed(_newOwner);
+    }

+    // Step 2: New owner accepts the ownership
+    function acceptOwnership() external {
+        require(msg.sender == newOwner, "Not the proposed owner");
+        emit OwnershipTransferred(owner, newOwner);
+        owner = newOwner;
+        newOwner = address(0);
+    }

     ...
}
```


# [04] No event emissions for critical state changes in `PrelaunchPoints::setEmergencyMode` and `PrelaunchPoints::allowToken`

`PrelaunchPoints::setEmergencyMode` and `PrelaunchPoints::allowToken` modify critical state variables, but no events are defined and emitted to broadcast the changes.

## Impact

1. Reduced transparency
2. Difficulty to track changes
3. Inefficient or impossible integration with other contracts and services


## Recommended Mitigation Steps

Define and emit events for critical changes performed in `PrelaunchPoints::setEmergencyMode` and `PrelaunchPoints::allowToken`:


```diff
// Define events to be emitted on state changes
+ event EmergencyModeSet(bool mode);
+ event TokenAllowed(address token);

contract PrelaunchPoints {

    ...

    function setEmergencyMode(bool _mode) external onlyAuthorized {
+       emit EmergencyModeSet(_mode);
        emergencyMode = _mode;
    }

    function allowToken(address _token) external onlyAuthorized {
+       emit TokenAllowed(_token);
        isTokenAllowed[_token] = true;
    }

    ...
}
```

# [05] Missing input validation for `_percentage` in `PrelaunchPoints::_claim`

`PrelaunchPoints::_claim` does not perform input validation on `_percentage`. Transaction flow will continue and only fails much later in the execution. 

## Impact 
Users calling `PrelaunchPoints::claim` or `PrelaunchPoints::claimAndStake` and accidentally inputting `0` for `_percentage` will waste more gas, as the transaction will fail much later in the execution.

## Recommended Mitigation Steps

Perform input validation on `_percentage` as follows:

```diff

    function _claim(address _token, address _receiver, uint8 _percentage, Exchange _exchange, bytes calldata _data)
        internal
        returns (uint256 claimedAmount)
    {
        uint256 userStake = balances[msg.sender][_token];
        if (userStake == 0) {
            revert NothingToClaim();
        }

+       require(_percentage > 0 && _percentage <= 100, "Invalid percentage value");

        ...


    }
```

[06] `PrelaunchPoints::_validateData` accepts `address(0)` as a valid value for `recipient`

`PrelaunchPoints::_validateData` accepts `address(0)` as a valid value for the `recipient` field of `bytes calldata _data`.
This essentially means that later in the transaction flow, when `Prelaunchdata::_fillQuote` is called, the output tokens of the swap will be sent to `address(0)`.

## Impact
Loss of funds; this would essentially be equivalent to burning tokens.

## Recommended Mitigation Steps

Ensure the recipient is strictly the contract itself to prevent unintended token loss:

```diff
   function _validateData(address _token, uint256 _amount, Exchange _exchange, bytes calldata _data) internal view {
        address inputToken;
        address outputToken;
        uint256 inputTokenAmount;
        address recipient;
        bytes4 selector;

        if (_exchange == Exchange.UniswapV3) {
            (inputToken, outputToken, inputTokenAmount, recipient, selector) = _decodeUniswapV3Data(_data);
            if (selector != UNI_SELECTOR) {
                revert WrongSelector(selector);
            }
            // UniswapV3Feature.sellTokenForEthToUniswapV3(encodedPath, sellAmount, minBuyAmount, recipient) requires `encodedPath` to be a Uniswap-encoded path, where the last token is WETH, and sends the NATIVE token to `recipient`
            if (outputToken != address(WETH)) {
                revert WrongDataTokens(inputToken, outputToken);
            }
        } else if (_exchange == Exchange.TransformERC20) {
            (inputToken, outputToken, inputTokenAmount, selector) = _decodeTransformERC20Data(_data);
            if (selector != TRANSFORM_SELECTOR) {
                revert WrongSelector(selector);
            }
            if (outputToken != ETH) {
                revert WrongDataTokens(inputToken, outputToken);
            }
        } else {
            revert WrongExchange();
        }

        if (inputToken != _token) {
            revert WrongDataTokens(inputToken, outputToken);
        }
        if (inputTokenAmount != _amount) {
            revert WrongDataAmount(inputTokenAmount);
        }
-        if (recipient != address(this) && recipient != address(0)) {
+        if (recipient != address(this)) {
            revert WrongRecipient(recipient);
        }
    }
```







