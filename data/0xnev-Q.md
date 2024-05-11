## Summary

| Number | Title |
|:--:|:---:|
| [L-01](#l-01-users-can-self-refer-to-gain-referral-extra-points)| Users can self-refer to gain referral extra pointsne of the ERC20 tokens current approval is not zero | 
| [L-02](#l-02-during-emergency-withdrawals-refferal-events-can-be-spoofed) | During emergency withdrawals, refferal events can be spoofed | 
| [L-03](#l-03-claiming-allow-inputs-of-disallowed-token-typesweth) | claiming allow inputs of disallowed token types/WETH | 
| [I-04](#i-04-include-on-chain-slippage-within-_fillquote) | Include on-chain slippage within `_fillQuote()` | 
| [I-05](#i-05-improvement-for-naming-of-totallpethtotalsupply) | Improvement for naming of totalLpETH/totalSupply | 



## [L-01] Users can self-refer to gain referral extra points

## Impact

In LoopFi `PrelaunchPoints` contract, users can lock ETH, WETH and wrapped LRTs into this contract, which will emit events tracked on a backend to calculate their corresponding amount of points. Users can use a referral code encoded as bytes32 that will give the referral extra points. We can presume the event referred in this case is the [`Locked` event emitted within `_processLock()`](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L197). Users with referral codes can self-refer themselves with their personal `referral` codes to gain extra points aside from the position already locked.


## Recommended Mitigation Steps

Consider not allowing users with referral code to self lock positions, if not acknowledge it as a design choice


## [L-02] During emergency withdrawals, refferal events can be spoofed

## Impact

When emergency withdrawals are enabled before vault and lpETH contract is set (i.e. before `loopActivation` is set within `setLoopAddresses`), users can still choose to lock ETH, WETH or LRTs. 

This way, they can bundle transactions and loop between locking and withdrawing when emergency withdrawal is enabled to spam `Locked` events, which could potentially flood backend systems.

Additionally, if emergency withdrawals are enabled, it could mean a bug/hack has occured, so it would be wise to prevent further locking of funds.

## Recommended Mitigation Steps
Consider adding the following check to all lock functions:

```
if (emergencyMode) revert EmergencyModeEnabled();
```

## [L-03] claiming allow inputs of disallowed token types/WETH

## Impact

Within the[`_claim`](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L240-L266) function, user can input `_token` addresses that are not allowed. This can allow users to utilize any donated ETH to mint additional lpETH without a prior locking of funds by donating ETH directly into contract and subsequently calling `claim()/claimAndStake()`, bypassing the `onlyBeforeDate(loopActivation)` modifier within lock functions. However, user will risk their ETH donated being front-runned and claimed by other users claiming.

## Recommended Mitigation Steps
Consider adding the following check to `_claim()`.

```
if (!isTokenAllowed[token]) revert TokenNotAllowed();
```

## [I-04] Include on-chain slippage within `_fillQuote`

## Impact 

It could be a good improvement to include an on-chain slippage as a input for users for better clarity and usage within the [claim functions](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L211-L236) directly when executing swaps for wrapped LRTs to ETH when claiming lpETH.

## Recommended Mitigation Steps

Consider adding a input variable representing slippage for `claim/claimAndStake()` that can be checked within `_fillQuote()` 


## [I-05] Improvement for naming of totalLpETH/totalSupply

## Impact

Within the contract, [`totalSupply`](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L33) tracks all the ETH/WETH locked while [`totalLpETH`](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L34) is the amount received after `convertAllETH()` is executed. This variable names are abit misleading and could be refactored for better clarity

## Recommended Mitigation Steps
Consider changing names from

- `totalSupply` to `totalETHLocked`
- `totalLpETH` to `totalLpETHConvertedFromETH`