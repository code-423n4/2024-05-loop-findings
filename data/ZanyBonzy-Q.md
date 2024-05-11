## 1. Tokens cannot be disallowed

Links to affected code *

https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/src/PrelaunchPoints.sol#L364

### Impact
Tokens can be allowed but cannot be disallowed. In case of an admin error, and the wrong token being allowed, there's no way for this to be rectified. In cases of depeg or blackswan, slashing events, worthless tokens will be exchangable for `lpETH`.

```solidity
    function allowToken(address _token) external onlyAuthorized {
        isTokenAllowed[_token] = true;
    }
```
### Recommended Mitigation Steps
Consider introducing a function to disallow tokens.

***

## 2. Users being able to donate to `ETH` or `lpETH` before conversion one of protocol's main invariant 

Links to affected code *

https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/src/PrelaunchPoints.sol#L392

### Impact
From the readme, there's a main invariant - 

> Users that deposit ETH/WETH get the correct amount of lpETH on claim (1 to 1 conversion)

This 1 to 1 invariant can be easily broken through donations, before the `convertAllETH` function is called. The function queries the balance of ETH before conerting all to lpETH, and queries the contract's lpETH balance to set the `totalLpETH` parameter. The conversion rate is 1 to 1.

```solidity
    function convertAllETH() external onlyAuthorized onlyBeforeDate(startClaimDate) {
        if (block.timestamp - loopActivation <= TIMELOCK) {
            revert LoopNotActivated();
        }

        // deposits all the ETH to lpETH contract. Receives lpETH back
        uint256 totalBalance = address(this).balance;
        lpETH.deposit{value: totalBalance}(address(this));

        totalLpETH = lpETH.balanceOf(address(this));

        // Claims of lpETH can start immediately after conversion.
        startClaimDate = uint32(block.timestamp);

        emit Converted(totalBalance, totalLpETH);
    }
```
Note the `totalSupply` tracks `ETH` deposits only through the [`_processLock`](https://github.com/code-423n4/2024-05-loop/blob/40167e469edde09969643b6808c57e25d1b9c203/src/PrelaunchPoints.sol#L180-L181) function, meaning it isn't affected by donations. So if tokens had been donated, they'll be converted to `lpETH` without actually changing the totalSupply.

When claiming, the claimed amount a user is entitled to is calculated as `userStake` * `totalLpETH`/`totalSupply`. And as it has been established that `totalLpETH` was a 1 to 1 exchange, this means that it will be more than `totalSupply` (since donations had increased `address(this).balance` but didn't affect `totalSupply`). As `totalLpETH` is more than `totalSupply`, a user's claimed amount will not be in a 1 to 1 ratio with his staked amount, but rather more.

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
            lpETH.safeTransfer(_receiver, claimedAmount);
        }
        ...
```
The same also applies if `lpETH` is donated to the contract before ETH is converted, due to the use of `lpETH.balanceOf(address(this));` to set `totalLpETH`.

Important to note that this doesn't affect ERC20 stakers, so these donations introduce a disbalance in expected exchange rate of `lpETH` to `ETH` and actual exchange rate.

### Recommended Mitigation Steps
Best way to fix it is to convert only `totalSupply` of ETH, and setting `totalLpETH` to `totalSupply`. That way, the 1 to 1 ratio will be protected. Donations will not be taken into consideration.

```solidity
    function convertAllETH() external onlyAuthorized onlyBeforeDate(startClaimDate) {
        if (block.timestamp - loopActivation <= TIMELOCK) {
            revert LoopNotActivated();
        }

        // deposits all the ETH to lpETH contract. Receives lpETH back
        lpETH.deposit{value: totalSupply}(address(this)); //++++

        totalLpETH = totalSupply; //++++ 

        // Claims of lpETH can start immediately after conversion.
        startClaimDate = uint32(block.timestamp);

        emit Converted(totalBalance, totalLpETH);
    }
```

***

## 3. `_claim` function can be improved by checking for 0 percentage too.

Links to affected code *

https://github.com/code-423n4/2024-05-loop/blob/40167e469edde09969643b6808c57e25d1b9c203/src/PrelaunchPoints.sol#L245

### Impact

The [_claim](https://github.com/code-423n4/2024-05-loop/blob/40167e469edde09969643b6808c57e25d1b9c203/src/PrelaunchPoints.sol#L245) function reverts on 0 user stake to claim. This can be imporved by checking if `_percentage` is also 0, since it equates to also 0 user claim.

### Recommended Mitigation Steps


```solidity
    function _claim(address _token, address _receiver, uint8 _percentage, Exchange _exchange, bytes calldata _data)
        internal
        returns (uint256 claimedAmount)
    {
        uint256 userStake = balances[msg.sender][_token];
        if (userStake == 0 || percentage == 0) { // +++++
            revert NothingToClaim();
        }
...
        } else {
            uint256 userClaim = userStake * _percentage / 100;
...
        }
        emit Claimed(msg.sender, _token, claimedAmount);
    }
```
***

## 4. Potential precision loss when claiming

Links to affected code *

https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/src/PrelaunchPoints.sol#L253

https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/src/PrelaunchPoints.sol#L249

### Impact

Depending on how much amount a user had staked, and how much percentage they're willing to claim, users might not be able to claim any token due to precision loss, due to the product of `userStake` and `_percentage` being smaller than 100. This is mostly for users with smaller stakes which will not be uncommon, due to the prices of LSTs. Consider scaling up the percentages to handle this potential precision loss.

```solidity
        if (_token == ETH) {
            claimedAmount = userStake.mulDiv(totalLpETH, totalSupply);
            balances[msg.sender][_token] = 0;
            lpETH.safeTransfer(_receiver, claimedAmount);
        } else {
            uint256 userClaim = userStake * _percentage / 100;
            _validateData(_token, userClaim, _exchange, _data);
            balances[msg.sender][_token] = userStake - userClaim;
```

***
## 5. Potential transfer to self when claiming and staking `ETH`/`WETH`.

Links to affected code *

https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/src/PrelaunchPoints.sol#L230

https://github.com/code-423n4/2024-05-loop/blob/40167e469edde09969643b6808c57e25d1b9c203/src/PrelaunchPoints.sol#L251 

### Impact

When users with `ETH`/`WETH` locks call `claimAndStake`, the function first claims the users' tokens to the contract (as receiver), before staking on their behalf.

```solidity
    function claimAndStake(address _token, uint8 _percentage, Exchange _exchange, bytes calldata _data)
        external
        onlyAfterDate(startClaimDate)
    {
        uint256 claimedAmount = _claim(_token, address(this), _percentage, _exchange, _data);
...
    }
```

The `_claim` function then transfers `lpETH` (that had existed in the contract upon all `ETH` conversion) to the receiver, which in this case is address(this) (i.e the contract), introducing a transfer to self situation.
```solidity
    function _claim(address _token, address _receiver, uint8 _percentage, Exchange _exchange, bytes calldata _data)
        internal
        returns (uint256 claimedAmount)
    {
...
        if (_token == ETH) {
            claimedAmount = userStake.mulDiv(totalLpETH, totalSupply);
            balances[msg.sender][_token] = 0;
            lpETH.safeTransfer(_receiver, claimedAmount);
        } 
```

The effects and severity of this is highly dependent on how `lpETH` is designed to handle transfer to self cases. Some tokens revert on transfer to self, and if `lpETH` is implemented like that, the `claimAndStake` function will be virtually unusable for ETH/WETH lockers.
Another possible effect might be accounting issues, if the balanceTo and balanceFrom parameters are cached in its transfer function, and so on.

### Recommended Mitigation Steps

It's worth taking into mind when designing the `lpETH` token.
***

## 6. Ownership can be renounced.

Links to affected code *

https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/src/PrelaunchPoints.sol#L336
### Impact

The `setOwner` function lacks a zero address check, which causes that the ownership can be renounced. It's especially dangerous if this is done before `startClaimDate` as users funds will be permanently locked in the protocol. 

```solidity
    function setOwner(address _owner) external onlyAuthorized {
        owner = _owner;

        emit OwnerUpdated(_owner);
    }
```

### Recommended Mitigation Steps
Best way to fix this is to include a zero address check in the function.
If its desired that ownership can be renounced, to keep the contract fully decentralized, the `onlyBeforeDate(startClaimDate)` modifier can be added, to prevent renouncing ownership before important protocol functionalities have been completed.

***

## 7. Consider offering a lock with permit function 

Links to affected code *

https://etherscan.io/address/0xe629ee84c1bd9ea9c677d2d5391919fcf5e7d5d9#code#F1#L12

https://etherscan.io/address/0x39ca0a6438b6050ea2ac909ba65920c7451305c1#code#F1#L32

### Impact

The ERC20 permit functionality is a streamlined, gas efficient and pretty secure way of granting approvals (with limits and deadlines) to spenders. It's become a major feature of many ERC20 tokens and many protocols in the sphere. Some of the tokens supported, e.g `pufETH`, `weETH`, offer this functionality. Consider offering a lock with permit function to also take advantage of it.

***

## 8. Some of the tokens in use can be paused which can dos withdrawals, claims and locks

Links to affected code *

https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/src/PrelaunchPoints.sol#L186

https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/src/PrelaunchPoints.sol#L259

https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/src/PrelaunchPoints.sol#L302

https://etherscan.io/address/0x60ff20bacd9a647e4025ed8b17ce30e40095a1d2#code#F1#L13

https://etherscan.io/address/0x8a94866df557bb7fce88eff9917237286098e590#code#F1#L11

### Impact

Rseth, unieth and ezeth can be paused, which temporarily DOSses transfers. This causes that users will not be able to lock their tokens, claim tokens, or withdraw them.

***

## 9. `ETH` and `lpETH` deposited after conversion cannot be retrieved, this can be fixed

Links to affected code *

https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/src/PrelaunchPoints.sol#L392

### Impact

After the `convertAllETH` function has been called and all the contract's `ETH` has been completely converted to lpETH, their respective values are tracked with `totalSupply` and `totalLpETH` parameters. Since these cannot be changed any longer after `startClaimDate`, their values can be used, to determine if excess `ETH` and `lpETH` have been sent to the contract, and can be used to retrieve them without disturbing the contracts internal accounting. That way the tokens will not be lost forever.

### Recommended Mitigation Steps
The function can be a variation of this:
```solidity
    function recoverETHorlpETH(address tokenAddress) external onlyAfterDate(startClaimDate) onlyAuthorized {
        if (tokenAddress == address(lpETH)) {
            excessAmount = lpETH.balanceOf(address(this)) - totalLpETH;
            IERC20(tokenAddress).safeTransfer(owner, excessAmount);
        }
        else if (tokenAddress == ETH) {
            excessAmount = address(this).balance - totalSupply;
            (bool sent,) = owner.call{value: excessAmount}(""); 
            if (!sent) {
                revert FailedToSendEther();
            }
        }
        emit Recovered(tokenAddress, tokenAmount);
    }
```
***