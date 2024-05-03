### Possible re-entrancy in `_claim`
https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L259
https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L497

The user has the ability to sneak a callback to the `PrelaunchPoints.sol` contract into the 0x swap data. For example, one of the hops in the Uniswap calldata can be directed to a malicious pool, which can trigger a re-entrancy attack in the `_claim` function. 

https://github.com/0xProject/protocol/blob/development/contracts/zero-ex/contracts/src/features/UniswapV3Feature.sol#L107
```solidity
    function sellTokenForEthToUniswapV3(
        bytes memory encodedPath,
        uint256 sellAmount,
        uint256 minBuyAmount,
        address payable recipient
    ) public override returns (uint256 buyAmount) {
        buyAmount = _swap(
            encodedPath,
            sellAmount,
            minBuyAmount,
            msg.sender,
            address(this) // we are recipient because we need to unwrap WETH
        );
```

Although no harm can be done in the current implementation, it is recommended to use the `nonReentrant` modifier in the `_claim` function for added security.

### Wrong `boughtETHAmount`  in `SwappedTokens` event

It is possible to make the `_fillQuote` function to emit incorrect `boughtETHAmount` in the `SwappedToken` event. Currently `boughtETHAmount` is calculated as the difference in balances before and after the swap.

```solidity
    function _fillQuote(IERC20 _sellToken, uint256 _amount, bytes calldata _swapCallData) internal {
        // Track our balance of the buyToken to determine how much we've bought.
        uint256 boughtETHAmount = address(this).balance;

        require(_sellToken.approve(exchangeProxy, _amount));

        (bool success,) = payable(exchangeProxy).call{value: 0}(_swapCallData);
        if (!success) {
            revert SwapCallFailed();
        }

        // Use our current buyToken balance to determine how much we've bought.
>>      boughtETHAmount = address(this).balance - boughtETHAmount;
        emit SwappedTokens(address(_sellToken), _amount, boughtETHAmount);
    }
```

However as shown in re-entrancy issue above, users can use a callback in a malicious pool and send some ETH tokens directly to the contract, resulting in a larger `boughtETHAmount` than was actually swapped.

Consider using return data from low-level call to the 0x contract to obtain actual bought amount.

### Unnecessary self-transfer of LpEth in `claimAndStake`

The user can call `claimAndStake` function to stake his claimed LpEth tokens.

```solidity
    function claimAndStake(address _token, uint8 _percentage, Exchange _exchange, bytes calldata _data)
        external
        onlyAfterDate(startClaimDate)
    {
>>      uint256 claimedAmount = _claim(_token, address(this), _percentage, _exchange, _data);
        lpETH.approve(address(lpETHVault), claimedAmount);
        lpETHVault.stake(claimedAmount, msg.sender);

        emit StakedVault(msg.sender, claimedAmount);
    }
```
This way the `PrelaunchPoints.sol` contract acts as a receiver in the `_claim` function, however if `_token == ETH` it will attempt to transfer LpETH to itself.

```solidity
    function _claim(address _token, address _receiver, uint8 _percentage, Exchange _exchange, bytes calldata _data)
        internal
        returns (uint256 claimedAmount)
    {
        uint256 userStake = balances[msg.sender][_token];
        if (userStake == 0) {
            revert NothingToClaim();
        }
        if (_token == ETH) {
            claimedAmount = userStake.mulDiv(totalLpETH, totalSupply);
            balances[msg.sender][_token] = 0;
>>          lpETH.safeTransfer(_receiver, claimedAmount);
```
Consider skipping transfer if `receiver` is `address(this)`.

### No way to remove token from allowed list

Adding a token into the allowed list is one-way process, there is no way to remove it from the list if something goes wrong.

```solidity
    function allowToken(address _token) external onlyAuthorized {
        isTokenAllowed[_token] = true;
    }
```  

Consider adding a toggle option for more flexibility.

### Users can lock tokens even if the contract is in emergency mode

Users can currently lock tokens even if the contract is in emergency mode. The owner of the `PrelaunchPoints.sol` contract has the ability to put the contract into emergency mode if it is deemed unsafe to use. Despite this, new users can still lock their tokens in this mode.

Consider implementing a restriction on the lock functionality when the contract is in emergency mode.

### `setLoopAddresses` doesn't check for zero addresses

> NOT INCLUDED IN THE BOT REPORT

```solidity
    function setLoopAddresses(address _loopAddress, address _vaultAddress)
        external
        onlyAuthorized
        onlyBeforeDate(loopActivation)
    {
>>      lpETH = ILpETH(_loopAddress);
>>      lpETHVault = ILpETHVault(_vaultAddress);
        loopActivation = uint32(block.timestamp);

        emit LoopAddressesUpdated(_loopAddress, _vaultAddress);
    }
```

### `_fillQuote` doesn't show reason of the low-level call revert

The function `_fillQuote` does not display the reason for the low-level call reverting. To enhance debugging, it is recommended to decode the return data and display the revert message when a low-level call to the 0x proxy reverts.

```solidity
    function _fillQuote(IERC20 _sellToken, uint256 _amount, bytes calldata _swapCallData) internal {
        // Track our balance of the buyToken to determine how much we've bought.
        uint256 boughtETHAmount = address(this).balance;

        require(_sellToken.approve(exchangeProxy, _amount));

>>      (bool success,) = payable(exchangeProxy).call{value: 0}(_swapCallData);
        if (!success) {
            revert SwapCallFailed();
        }
```

### Wrong natspec comment in the `lockFor` function

```diff
    /**
     * @notice Locks a valid token for a given address
     * @param _token     address of token to lock
     * @param _amount    amount of token to lock
-    * @param _for       address for which ETH is locked
+    * @param _for       address for token is locked
     * @param _referral  info of the referral. This value will be processed in the backend.
     */
    function lockFor(address _token, uint256 _amount, address _for, bytes32 _referral) external {
```
