# LoopFi QA Report

## [L-01] Inconsistent Token Balance Handling for WETH Claims

### Context

https://github.com/code-423n4/2024-05-loop/blob/40167e469edde09969643b6808c57e25d1b9c203/src/PrelaunchPoints.sol#L245

### Description

In `function _processLock` there is a separate condition for `WETH` to be
locked - `if (_token == address(WETH))` and in that condition the `WETH` is been
converted into `ETH` and added to the `totalSupply`, and user's balance of `ETH`
(not ERC20 WETH _token)

`PrelaunchPoints.sol#L188`
```
if (_token == address(WETH)) {
WETH.withdraw(_amount);
totalSupply = totalSupply + _amount;
@> balances[_receiver][ETH] += _amount;
}
```

Issue comes when user calls `function _claim` with input param to be WETH token
(because that's what user locked, so now he will claim that token only) But here
user will be getting a revert because line #245 in `function _claim` because
user's WETH balance is not updated instead user's ETH balance is updated in
`function _processLock` -

`PrelaunchPoints.sol#L245`
```
// fetch user stake
uint256 userStake = balances[msg.sender][_token];
// if user has nothing to claim revert the tx
@> if (userStake == 0) {
revert NothingToClaim();
}
```

### Recommendation

To avoid getting revert with WETH token while claiming for locked WETH token,
considering implementing this in `function _claim`

```
uint256 userStake;

if ( _token == address(WETH)) {
 userStake = balances[msg.sender][ETH];
}

userStake = balances[msg.sender][_token];

  if (userStake == 0) {
            revert NothingToClaim();
        }

if (_token == ETH || _token == address(WETH)) {
            claimedAmount = userStake.mulDiv(totalLpETH, totalSupply);
            balances[msg.sender][_token] = 0;
            lpETH.safeTransfer(_receiver, claimedAmount);
        }

```


## [L-02] Missing Percentage Check for ERC20 Token Claims

### Context

https://github.com/code-423n4/2024-05-loop/blob/40167e469edde09969643b6808c57e25d1b9c203/src/PrelaunchPoints.sol#L252

### Description

In `function -_claim` else block, which handles ERC20 token claims, it lacks a check to ensure that the claim percentage (`_percentage` input param) is greater than 0. failing to validate inputs can lead to unexpected behavior and could potentially be exploited in more complex contracts or in combination with other oversights.

### Recommendation

``` 
 // ... existing code ...

else {
      @>    require(_percentage > 0, "Percentage must be greater than 0");    
            uint256 userClaim = userStake * _percentage / 100;
            _validateData(_token, userClaim, _exchange, _data);
            balances[msg.sender][_token] = userStake - userClaim;

 // ... existing code ...
```


## [L-03] User can't withdraw even in the Emergency Mode.

### Context 

https://github.com/code-423n4/2024-05-loop/blob/40167e469edde09969643b6808c57e25d1b9c203/src/PrelaunchPoints.sol#L291

### Description

The docs Invariant says -
`Withdrawals are only active on emergency mode or during 7 days after loopActivation is set`

Real scenario - All functioning is properly set by the owner, and therefore all
dates are also properly set and `ClaimDate` has started from owners side (after
calling `function conertAllETH` , ie. 7 days has passed where user was having
chance to withdraw). Now in future the owner sets the `emergencyMode: true` due to
certain reason, then the user who has locked his `ETH` in protocol, but have not
claimed it yet, and now due to emergency user don't want to claim it anymore,
but want to simply withdraw his locked `ETH` but now he won't be able to
withdraw it because there are no `ETH` present in contract anymore, because of
`function conertAllETH` converted it into `lpETH`.

So here line - `PrelaunchPoints.sol#L291` is executed so that users should
should now claim for his `ETH`, as user can not get back his ETH withdrawn back.

```if (_token == ETH) {
            if (block.timestamp >= startClaimDate){
                revert UseClaimInstead();
            }
```

This issue can be marked as a LOW severity because - Although user can claim
token, but still as invariant says that user must be able to
`Withdraw when emergency mode active (ie. emergencyMode: true)`. So here it still
breaks the invariant.

### Recommendation

Codebase logic should updated in such a way that user can withdraw his locked `ETH` while `emergencyMode: true`.


## [L-04] As ETH:lpETH value must be 1:1, but while emitting event in `_fillQuote` and `_claim` it mismatches.

### Context

https://github.com/code-423n4/2024-05-loop/blob/40167e469edde09969643b6808c57e25d1b9c203/src/PrelaunchPoints.sol#L265

https://github.com/code-423n4/2024-05-loop/blob/40167e469edde09969643b6808c57e25d1b9c203/src/PrelaunchPoints.sol#L504

### Description

So sceanrio where somone mistakly sends ETH into the contract, so now he wont be
able to get those ETH back ever. But that's not the issue, issue of Event values
mismatches -

Now if a normal user try to claim his LpETH on his locked ERC20 token then in
`function _fillQuote` this line -
`boughtETHAmount = address(this).balance - boughtETHAmount;` which tells the
[New Balance -old Balance] is what user `boughtETHAmount` which user is supposed
to get for his locked token, which then is used in Event -
`emit SwappedTokens(address(_sellToken), _amount, boughtETHAmount);`.

But the main issue comes when in `function _claim` in else condition -

```
@> claimedAmount = address(this).balance;
lpETH.deposit{value: claimedAmount}(_receiver);
}
 emit Claimed(msg.sender, _token, claimedAmount);
```

because of this line `claimedAmount = address(this).balance;` it deposit that
amount in lpETH contract and get back those LPETH as claim for that specific
user, which will be more than what that user deserves for his locked token
amount, this is because someone sent ETH from outside of contract.

And in `function _claim` as the Event -
` emit Claimed(msg.sender, _token, claimedAmount);` it emits different value of
`claimedAmount` as compared to `boughtETHAmount` , where as this both must be
same as the ETH:lpETH ratio is 1:1

### Recommendation

Before emitting `event Claimed` in `function _claim`, update the value of
`claimAmount` as same as `boughtETHAmount` by -

```
 // ... existing code ...

    _fillQuote(IERC20(_token), userClaim, _data);

            // Convert swapped ETH to lpETH (1 to 1 conversion)
            claimedAmount = address(this).balance;
            lpETH.deposit{value: claimedAmount}(_receiver);
        }

    @>    claimedAmount = address(this).balance - claimedAmount
        emit Claimed(msg.sender, _token, claimedAmount);
    }
```