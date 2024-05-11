### [L-01] User deposits WETH but withdraws ETH

Wen user locks their WETH token, they call `lock()` with the WETH token address and it automatically converts to ETH and gets logged in the ETH mapping.

```
            if (_token == address(WETH)) {
                WETH.withdraw(_amount);
                totalSupply = totalSupply + _amount;
                balances[_receiver][ETH] += _amount;
```

When they withdraw, they need to call `withdraw()` with the ETH token address and it gives them back ETH instead of WETH.

```
 if (_token == ETH) {
            if (block.timestamp >= startClaimDate){
                revert UseClaimInstead();
            }
            totalSupply = totalSupply - lockedAmount;

            (bool sent,) = msg.sender.call{value: lockedAmount}("");

            if (!sent) {
                revert FailedToSendEther();
            }
        } else {
            IERC20(_token).safeTransfer(msg.sender, lockedAmount);
        }
```

This will confuse some users if it is not indicated that they cannot withdraw WETH but only ETH.

https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/src/PrelaunchPoints.sol#L274

### [L-02] Set loop address does not have any failsafe, making it pretty risky

The owner of the contract only has one chance to call setLoopAddress and there is no other way to change it. There is also no failsafe check when setting the loop address (eg zero address check). This is pretty dangerous as a lot of ETH will be at stake if the loop addresses are set wrongly.

```
    function setLoopAddresses(address _loopAddress, address _vaultAddress)
        external
        onlyAuthorized
        onlyBeforeDate(loopActivation)
    {
        lpETH = ILpETH(_loopAddress);
        lpETHVault = ILpETHVault(_vaultAddress);
        loopActivation = uint32(block.timestamp);

        emit LoopAddressesUpdated(_loopAddress, _vaultAddress);
    }
```

The function can only be called once because `loopActivation` will be reset to block.timestamp, which prevent any further function call. 

Also, in the event that the function is not called after 120 days, the loop addresses cannot be set anymore.

The only way to resolve any issue is to set `emergencyMode` to true to allow everyone to withdraw their funds.

Since this function is already a centralized function, it would be good to have other authorized function to change the loop addresses again.

https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/src/PrelaunchPoints.sol#L348-L358

### [L-03] Users that call claim without staking cannot stake anymore

When claiming their lp token, if the user calls `claim()` instead of `claimAndStake()`, the user will not have any option to stake their lp tokens anymore.

In `claimAndStake()`, the function will call `approve()` and `lpETHVault.stake()`. 

```
    function claimAndStake(address _token, uint8 _percentage, Exchange _exchange, bytes calldata _data)
        external
        onlyAfterDate(startClaimDate)
    {
        uint256 claimedAmount = _claim(_token, address(this), _percentage, _exchange, _data);
        lpETH.approve(address(lpETHVault), claimedAmount);
        lpETHVault.stake(claimedAmount, msg.sender);
```

```
    function claim(address _token, uint8 _percentage, Exchange _exchange, bytes calldata _data)
        external
        onlyAfterDate(startClaimDate)
    {
        _claim(_token, msg.sender, _percentage, _exchange, _data);
    }
```

Note that `_claim()` sets `address(this)` as the second parameter, while in `claim()`, `msg.sender` is set as the second parameter in `_claim()`. This means that to stake the lp token, the lp token must be in the contract.

Users that call `claim()` and wants to stake their tokens in the future will not be able to do so.

https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/src/PrelaunchPoints.sol#L211-L234

### [L-04] ETH sent to the contract will not be locked forever

Contrary to the NATSPEC above `receive()`, ETH sent to this contract will be converted to lpETH before `convertAllETH()` is called or given to the first user that calls `claim()` with their LRT staked after `convertAllETH()` is called.

```
    /**
     * Enable receive ETH
     * @dev ETH sent to this contract directly will be locked forever.
     */
    receive() external payable {}
```

`claimedAmount` is set as `address(this).balance`.

```
            // At this point there should not be any ETH in the contract
            // Swap token to ETH
            _fillQuote(IERC20(_token), userClaim, _data);

            // Convert swapped ETH to lpETH (1 to 1 conversion)
            claimedAmount = address(this).balance;
```


### [L-05] ETH donation can mess up the conversion rate


When claiming ETH deposits, the lpETH to ETH conversion should ideally be 1:1. If a user directly deposit ETH into the contract, `totalLpETH` will be greater than `totalSupply` which will affect the conversion rate for all user stakes.

```
        if (_token == ETH) {
            claimedAmount = userStake.mulDiv(totalLpETH, totalSupply);
            balances[msg.sender][_token] = 0;
            lpETH.safeTransfer(_receiver, claimedAmount);
```

Although the depositor loses out, it is still a griefing attack that manipulates the conversion rate.

At `convertAllETH()`, it will be better if the `totalSupply` is equal to the `totalLpETH`, then sweep the remaining ETH in the contract to the owner's address. This makes it such that the conversion rate will always be 1:1.

https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/src/PrelaunchPoints.sol#L315

### [L-06] No zero address check for setOwner and other functions

`setOwner()` has no zero address check. If the owner is set incorrectly, all `onlyAuthorized` functions will fail. Set a failsafe for `setOwner()`

```
    function setOwner(address _owner) external onlyAuthorized {
        owner = _owner;

        emit OwnerUpdated(_owner);
    }
```

https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/src/PrelaunchPoints.sol#L336C1-L340C6

### [L-07] isTokenAllowed should be set to false in case of a mistake

If the owner sets the token to true by mistake, he should be able to set it back to false, but the current iteration of the function does not allow the owner to do so.

```
    function allowToken(address _token) external onlyAuthorized {
        isTokenAllowed[_token] = true;
    }
```

Add a bool value in the parameter so that the owner can change the boolean value.

https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/src/PrelaunchPoints.sol#L364C1-L366C6