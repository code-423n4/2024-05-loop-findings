## [L-01] Users can withdraw even after 7 days when the `loopActivation` is set

[PrelaunchPoints.sol#L274-L306](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L274-L306)
The LoopFi protocol promises users a 7 day grace period to withdraw their deposited tokens after the `loopActivation` date is set but there is no logic in the contract to stop withdraws after 7 days when the `loopActivation` is set but `startClaimDate` is not yet set. Assuming the call to `convertAllETH()` doesn't happen exactly 7 days after `loopActivation` date was set, users will still be able to withdraw past the 7 days time threshold.

Short POC
```solidity
function testCanWithdrawPost7Days(uint256 lockAmount) public {
        lockAmount = bound(lockAmount, 1, INITIAL_SUPPLY);
        lrt.approve(address(prelaunchPoints), lockAmount);
        prelaunchPoints.lock(address(lrt), lockAmount, referral);

        uint256 balanceBefore = lrt.balanceOf(address(this));

        prelaunchPoints.setLoopAddresses(address(lpETH), address(lpETHVault));
        vm.warp(prelaunchPoints.loopActivation() + 8 days);
        prelaunchPoints.withdraw(address(lrt));

        assertEq(prelaunchPoints.balances(address(this), address(lrt)), 0);
        assertEq(lrt.balanceOf(address(this)) - balanceBefore, lockAmount);
    }
```

Using this updated `withdraw()` implementation below renders a fix to this potential issue:

```diff
function withdraw(address _token) external {
        if (!emergencyMode) {
            if (block.timestamp <= loopActivation) {
                revert CurrentlyNotPossible();
            }
            if (block.timestamp >= startClaimDate) {
                revert NoLongerPossible();
            }
+            if (block.timestamp >= loopActivation + 7 days) {
+                revert("past time to do so");
+            }
        }
        ...
}
```

## [L-02] `minBuyAmount` of 0 opens the user claiming `lpETH` with their `LRT` balance to MEV loss

[PrelaunchPoints.sol#L491-L505](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L491-L505)
Swap data for `lpETH` claims when encoded/decoded include a `minBuyAmount` that works similar to `minAmountOut` which in this case is not checked to not be 0 which would open users claiming `lpETH` to MEV attacks when it is 0.

Decoding the `_swapCallData` and checking that the `minBuyAmount` is not zero will close this open loophole that MEV can utilize.


## [L-03] Missing zero address checks in constructor addresses

[PrelaunchPoints.sol#L97-L114](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L97-L114)
The constructor arguments of the `PrelaunchPoints` contract are assumed to be inputted correctly but any copied zero address in the clipboard could get pasted by mistake with deployment being executed and forced to be redone. Zero address checks can be used in this case to prevent this.

```diff
constructor(address _exchangeProxy, address _wethAddress, address[] memory _allowedTokens) {
        owner = msg.sender; // @note good
        exchangeProxy = _exchangeProxy;
+       if (_exchangeProxy == address(0)) {
+         revert("zero address");
+       }        
        WETH = IWETH(_wethAddress);
+       if (_wethAddress == address(0)) {
+         revert("zero address");
+       }    

        loopActivation = uint32(block.timestamp + 120 days); // @note good
        startClaimDate = 4294967295; // Max uint32 ~ year 2107 @note good

        // Allow intital list of tokens
        uint256 length = _allowedTokens.length;
        for (uint256 i = 0; i < length;) {
+       if (_allowedTokens[i] == address(0)) {
+          revert("zero address");
+       }   
            isTokenAllowed[_allowedTokens[i]] = true;
            unchecked {
                i++;
            }
        }
        isTokenAllowed[_wethAddress] = true;
    }
```

## [L-04] User sending any approved `LRT` to the contract by mistake loses the value sent which cannot be recovered

[PrelaunchPoints.sol#L379-L386](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L379-L386)
Assuming a transfer of one of the approved `LRTs` is put into the `PrelaunchPoints` contract, such token cannot be recovered as the `recoverERC20` call would revert at the `if` check because such token would be whitelisted

```solidity
function recoverERC20(address tokenAddress, uint256 tokenAmount) external onlyAuthorized {
@>        if (tokenAddress == address(lpETH) || isTokenAllowed[tokenAddress]) { // @audit LOW LRTs sent by mistake & whitelisted cannot be pulled
            revert NotValidToken();
        }
        IERC20(tokenAddress).safeTransfer(owner, tokenAmount);

        emit Recovered(tokenAddress, tokenAmount);
    }
```

## [L-05] User can earn extra points on the backend by setting themselves as the referral in the encoded `_referral` data

[PrelaunchPoints.sol#L124-L126](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L124-L126)
[PrelaunchPoints.sol#L133-L135](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L133-L135)
[PrelaunchPoints.sol#L143](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L143)
[PrelaunchPoints.sol#L157](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L157)


When making a deposit/lock call on the `PrelaunchPoints` contract, users can specify a referral who is encoded as bytes32 and then decoded on the backend to award points to them in the pointing system. Users who encode themselves as the referral can use this mechanism to inflate their points

```solidity
function lockETH(bytes32 _referral) external payable {
@>        _processLock(ETH, msg.value, msg.sender, _referral); // @audit setting yourself as the referral gives you extra points
    }
```

## [GOV-01] Setting the owner to a wrong address forces the protocol to lose access to crucial functions

[PrelaunchPoints.sol#L336-L340](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L336-L340)
When a transfer of the contract's ownership is done to another address by mistake, the current owner ends up losing the contract ownership. Utilizing a two-step ownership transfer where ownership is not transferred unless the new owner accepts it would be great.

```solidity
function setOwner(address _owner) external onlyAuthorized {
        owner = _owner;

        emit OwnerUpdated(_owner);
    }
```

## [GOV-02] Protocol could drain `LRT` balances by disallowing the `LRT` first

[PrelaunchPoints.sol#L364-L366](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L364-L366)
[PrelaunchPoints.sol#L379-L386](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L379-L386)

The protocol can decide to drain all user deposits in `LRTs` by removing the token addresses from the `isTokenAllowed` first in order to bypass the check in the `recoverERC20` function which ensures that only non-whitelisted tokens can be withdrawn.

```solidity
function allowToken(address _token) external onlyAuthorized {
        isTokenAllowed[_token] = true;
    }
```

```solidity
function recoverERC20(address tokenAddress, uint256 tokenAmount) external onlyAuthorized {
@>        if (tokenAddress == address(lpETH) || isTokenAllowed[tokenAddress]) { // @audit check bypass
            revert NotValidToken();
        }
        IERC20(tokenAddress).safeTransfer(owner, tokenAmount);

        emit Recovered(tokenAddress, tokenAmount);
    }
```