---
sponsor: "LoopFi"
slug: "2024-05-loop"
date: "2024-06-12"
title: "LoopFi"
findings: "https://github.com/code-423n4/2024-05-loop-findings/issues"
contest: 371
---

# Overview

## About C4

Code4rena (C4) is an open organization consisting of security researchers, auditors, developers, and individuals with domain expertise in smart contracts.

A C4 audit is an event in which community participants, referred to as Wardens, review, audit, or analyze smart contract logic in exchange for a bounty provided by sponsoring projects.

During the audit outlined in this document, C4 conducted an analysis of the LoopFi smart contract system written in Solidity. The audit took place between May 1—May 8 2024.

## Wardens

49 Wardens contributed reports to LoopFi:

  1. [0xrex](https://code4rena.com/@0xrex)
  2. [0xJoyBoy03](https://code4rena.com/@0xJoyBoy03)
  3. [0xnev](https://code4rena.com/@0xnev)
  4. [Rhaydden](https://code4rena.com/@Rhaydden)
  5. [Krace](https://code4rena.com/@Krace)
  6. [gumgumzum](https://code4rena.com/@gumgumzum)
  7. [novamanbg](https://code4rena.com/@novamanbg)
  8. [Greed](https://code4rena.com/@Greed)
  9. [y4y](https://code4rena.com/@y4y)
  10. [DMoore](https://code4rena.com/@DMoore)
  11. [0x04bytes](https://code4rena.com/@0x04bytes)
  12. [0xBugSlayer](https://code4rena.com/@0xBugSlayer)
  13. [sldtyenj12](https://code4rena.com/@sldtyenj12)
  14. [yovchev\_yoan](https://code4rena.com/@yovchev_yoan)
  15. [sandy](https://code4rena.com/@sandy)
  16. [Evo](https://code4rena.com/@Evo)
  17. [Kirkeelee](https://code4rena.com/@Kirkeelee)
  18. [Sajjad](https://code4rena.com/@Sajjad)
  19. [caglankaan](https://code4rena.com/@caglankaan)
  20. [d3e4](https://code4rena.com/@d3e4)
  21. [samuraii77](https://code4rena.com/@samuraii77)
  22. [Pechenite](https://code4rena.com/@Pechenite) ([Bozho](https://code4rena.com/@Bozho) and [radev\_sw](https://code4rena.com/@radev_sw))
  23. [TheFabled](https://code4rena.com/@TheFabled)
  24. [pamprikrumplikas](https://code4rena.com/@pamprikrumplikas)
  25. [web3er](https://code4rena.com/@web3er)
  26. [ZanyBonzy](https://code4rena.com/@ZanyBonzy)
  27. [SBSecurity](https://code4rena.com/@SBSecurity) ([Slavcheww](https://code4rena.com/@Slavcheww) and [Blckhv](https://code4rena.com/@Blckhv))
  28. [nfmelendez](https://code4rena.com/@nfmelendez)
  29. [Topmark](https://code4rena.com/@Topmark)
  30. [XDZIBECX](https://code4rena.com/@XDZIBECX)
  31. [Bigsam](https://code4rena.com/@Bigsam)
  32. [shaflow2](https://code4rena.com/@shaflow2)
  33. [0xSecuri](https://code4rena.com/@0xSecuri)
  34. [petarP1998](https://code4rena.com/@petarP1998)
  35. [bbl4de](https://code4rena.com/@bbl4de)
  36. [\_karanel](https://code4rena.com/@_karanel)
  37. [btk](https://code4rena.com/@btk)
  38. [peanuts](https://code4rena.com/@peanuts)
  39. [SpicyMeatball](https://code4rena.com/@SpicyMeatball)
  40. [chainchief](https://code4rena.com/@chainchief)
  41. [popeye](https://code4rena.com/@popeye)
  42. [karsar](https://code4rena.com/@karsar)
  43. [cheatc0d3](https://code4rena.com/@cheatc0d3)
  44. [krisp](https://code4rena.com/@krisp)
  45. [oualidpro](https://code4rena.com/@oualidpro)
  46. [slvDev](https://code4rena.com/@slvDev)
  47. [ParthMandale](https://code4rena.com/@ParthMandale)

This audit was judged by [Koolex](https://code4rena.com/@koolex).

Final report assembled by [liveactionllama](https://twitter.com/liveactionllama).

# Summary

The C4 analysis yielded a total of 1 vulnerability with a risk rating in the category of HIGH severity and 0 with a risk rating in the category of MEDIUM severity.

Additionally, C4 analysis included 16 reports detailing issues with a risk rating of LOW severity or non-critical. There was also 1 report recommending gas optimizations.

All of the issues presented here are linked back to their original finding.

# Scope

The code under review can be found within the [C4 LoopFi repository](https://github.com/code-423n4/2024-05-loop), and is composed of 1 smart contract written in the Solidity programming language and includes 296 lines of Solidity code.

# Severity Criteria

C4 assesses the severity of disclosed vulnerabilities based on three primary risk categories: high, medium, and low/non-critical.

High-level considerations for vulnerabilities span the following key areas when conducting assessments:

- Malicious Input Handling
- Escalation of privileges
- Arithmetic
- Gas use

For more information regarding the severity criteria referenced throughout the submission review process, please refer to the documentation provided on [the C4 website](https://code4rena.com), specifically our section on [Severity Categorization](https://docs.code4rena.com/awarding/judging-criteria/severity-categorization).

# High Risk Findings (1)
## [[H-01] Availability of deposit invariant can be bypassed](https://github.com/code-423n4/2024-05-loop-findings/issues/33)
*Submitted by [0xnev](https://github.com/code-423n4/2024-05-loop-findings/issues/33), also found by [Krace](https://github.com/code-423n4/2024-05-loop-findings/issues/58), 0xrex ([1](https://github.com/code-423n4/2024-05-loop-findings/issues/56), [2](https://github.com/code-423n4/2024-05-loop-findings/issues/48)), [gumgumzum](https://github.com/code-423n4/2024-05-loop-findings/issues/53), [novamanbg](https://github.com/code-423n4/2024-05-loop-findings/issues/46), [Greed](https://github.com/code-423n4/2024-05-loop-findings/issues/40), [y4y](https://github.com/code-423n4/2024-05-loop-findings/issues/38), [DMoore](https://github.com/code-423n4/2024-05-loop-findings/issues/37), [0x04bytes](https://github.com/code-423n4/2024-05-loop-findings/issues/34), [0xBugSlayer](https://github.com/code-423n4/2024-05-loop-findings/issues/30), [sldtyenj12](https://github.com/code-423n4/2024-05-loop-findings/issues/27), [yovchev\_yoan](https://github.com/code-423n4/2024-05-loop-findings/issues/26), 0xJoyBoy03 ([1](https://github.com/code-423n4/2024-05-loop-findings/issues/23), [2](https://github.com/code-423n4/2024-05-loop-findings/issues/21)), [sandy](https://github.com/code-423n4/2024-05-loop-findings/issues/20), [Evo](https://github.com/code-423n4/2024-05-loop-findings/issues/19), [Kirkeelee](https://github.com/code-423n4/2024-05-loop-findings/issues/18), [Sajjad](https://github.com/code-423n4/2024-05-loop-findings/issues/6), [d3e4](https://github.com/code-423n4/2024-05-loop-findings/issues/57), [samuraii77](https://github.com/code-423n4/2024-05-loop-findings/issues/32), [Pechenite](https://github.com/code-423n4/2024-05-loop-findings/issues/25), [TheFabled](https://github.com/code-423n4/2024-05-loop-findings/issues/22), Rhaydden ([1](https://github.com/code-423n4/2024-05-loop-findings/issues/70), [2](https://github.com/code-423n4/2024-05-loop-findings/issues/55)), [web3er](https://github.com/code-423n4/2024-05-loop-findings/issues/28), [ZanyBonzy](https://github.com/code-423n4/2024-05-loop-findings/issues/137), [SBSecurity](https://github.com/code-423n4/2024-05-loop-findings/issues/136), [nfmelendez](https://github.com/code-423n4/2024-05-loop-findings/issues/82), [Topmark](https://github.com/code-423n4/2024-05-loop-findings/issues/69), [XDZIBECX](https://github.com/code-423n4/2024-05-loop-findings/issues/64), [Bigsam](https://github.com/code-423n4/2024-05-loop-findings/issues/51), [shaflow2](https://github.com/code-423n4/2024-05-loop-findings/issues/47), [0xSecuri](https://github.com/code-423n4/2024-05-loop-findings/issues/43), [petarP1998](https://github.com/code-423n4/2024-05-loop-findings/issues/41), [bbl4de](https://github.com/code-423n4/2024-05-loop-findings/issues/39), [\_karanel](https://github.com/code-423n4/2024-05-loop-findings/issues/35), and [btk](https://github.com/code-423n4/2024-05-loop-findings/issues/24)*

### Impact

One of the [main invariants](https://github.com/code-423n4/2024-05-loop?tab=readme-ov-file#main-invariants) stated in the audit is the following:

> Deposits are active up to the lpETH contract and lpETHVault contract are set

However, there are currently two ways this invariant can be broken, allowing users to gain lpETH without explicitly locking tokens before contracts are set.

1.  Sandwich a call to `convertAllETH` by  front-running to directly donate ETH and then back-running to claim lpETH via multiple lock positions

2.  Bundle a transaction of donation and claiming with a previously locked position of wrapped LRT

This bypasses the `onlyBeforeDate(loopActivation)` modifier included in all lock functions. It also potentially allows no cap in lpETH minted and also discourages users from locking ETH in the `PrelaunchPoints.sol` contract before contract addresses are set (i.e. `loopActivation` is assigned).

### Proof of Concept

**Scenario 1**

Bundle a transaction of donation and claiming with a previously locked position of wrapped LRT. User can execute the following in one transaction, assuming the user previously has a locked position with any of the allowed LRTs.

1.  Donate ETH directly to contract.
2.  Call `claim()/claimAndStake()`, claiming all tokens, this swaps users wrapped LRT to ETH via `_fillQuote`.
3.  Since `claimedAmount` is set as `address(this).balance` [here](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L262), `claimedAmount`is computed as the addition of ETH receive from LRT swapped + user donated ETH.
4.  `claimedAmount` worth of lpETH is subsequently claimed/staked via `lpETH.deposit()/lpETHVault.stake()`.

**Scenario 2**

Sandwich a call to `convertAllETH` by  front-running to directly donate ETH and then back-running to claim lpETH via multiple lock positions.

1.  Assume user has 10 positions of ETH/WETH locked each with 1 ether with different addresses, with 100 ETH total locked.
2.  Front-run call to `convertAllETH` by donating 1 ETH. This sets `totalLpETH` to be 101 ETH as seen [here](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L324).
3.  Ratio of `totalLpETH : totalSupply` computed [here](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L249) is now 1.01 (1.01).
4.  Back-run `convertAllETH` by executing claims for all 10 position, each claim will mint 0.01 ETH extra (1.01 lpETH).

This scenario is less likely than scenario 1 as it requires user to perfectly back-run `convertAllETH()` with claims, although it also breaks another invariant of a 1:1 swap.

> Users that deposit ETH/WETH get the correct amount of lpETH on claim (1 to 1 conversion)

### Recommended Mitigation Steps

1.  For scenario 1, set `claimedAmount` to amount of ETH bought from LRT swap (such as `buyAmount` for uniV3).
2.  For scenario 2, no fix is likely required since it would require users to risk their funds to be transferred by other users due to inflated `totalLpETH : totalSupply` ratio. If not, you can consider setting `totalLpETH` to `totalSupply` and allow admin to retrieve any additional ETH donated.

**[0xd4n1el (Loop) confirmed](https://github.com/code-423n4/2024-05-loop-findings/issues/33#event-12908513603)**

**[Koolex (judge) increased severity to High](https://github.com/code-423n4/2024-05-loop-findings/issues/33#issuecomment-2141622967)**



***

# Low Risk and Non-Critical Issues

For this audit, 16 reports were submitted by wardens detailing low risk and non-critical issues. The [report highlighted below](https://github.com/code-423n4/2024-05-loop-findings/issues/118) by **pamprikrumplikas** received the top score from the judge.

*The following wardens also submitted reports: [peanuts](https://github.com/code-423n4/2024-05-loop-findings/issues/121), [0xrex](https://github.com/code-423n4/2024-05-loop-findings/issues/115), [SpicyMeatball](https://github.com/code-423n4/2024-05-loop-findings/issues/2), [ZanyBonzy](https://github.com/code-423n4/2024-05-loop-findings/issues/117), [Pechenite](https://github.com/code-423n4/2024-05-loop-findings/issues/103), [caglankaan](https://github.com/code-423n4/2024-05-loop-findings/issues/127), [chainchief](https://github.com/code-423n4/2024-05-loop-findings/issues/120), [popeye](https://github.com/code-423n4/2024-05-loop-findings/issues/119), [karsar](https://github.com/code-423n4/2024-05-loop-findings/issues/116), [cheatc0d3](https://github.com/code-423n4/2024-05-loop-findings/issues/114), [0xnev](https://github.com/code-423n4/2024-05-loop-findings/issues/113), [krisp](https://github.com/code-423n4/2024-05-loop-findings/issues/112), [oualidpro](https://github.com/code-423n4/2024-05-loop-findings/issues/111), [slvDev](https://github.com/code-423n4/2024-05-loop-findings/issues/105), and [ParthMandale](https://github.com/code-423n4/2024-05-loop-findings/issues/104).*

## [01] `onlyAuthorized` modifier introduces unnecessary centralization via  in `PrelaunchPoints::convertAllETH`

`PrelaunchPoints::convertAllETH` is protected by the `onlyAuthorized` modifier, restricting access to this function to the protocol owner:

```javascript
    function convertAllETH() external onlyAuthorized onlyBeforeDate(startClaimDate) {...}
```

### Impact
The restriction deepens centralization unnecessarily, exposing users to associated risks, and making the flow of events less predictable to the users.

Users should be able to start claiming LpETH right after the `TIMELOCK` period passes following the succesfull execution of a call to `PrelaunchPoints::setLoopAddresses`. 

Claims are enabled upon a successful call to `PrelaunchPoints::convertAllETH`, but as long as it is protected by the `onlyAuthorized` modifier, the authorized account can chose to delay to call this function or can decide to never call it at all, introducing uncertainty and risk for the users.

### Recommended Mitigation Steps
Remove the `onlyAuthorized` modifier in `PrelaunchPoints::convertAllETH`.

## [02] Not allowed tokens cannot be recovered if they get allowed after having been sent to the contract

`PrelaunchPoints::recoverERC20` is designed to enable the protocol owner to recover any not allowed tokens that users mistakeanly send to the protocol. However, `PrelaunchPoints::allowToken` does not check whether the protocol, by user mistake, has previously received any funds in the token to be allowed.

### Impact

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

### Recommended Mitigation Steps

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

## [03] Critical privilages are transferred in one step instead of two

`owner` has critical privilages in the protocol. `PrelaunchPoints::setOwner` allows the `owner` to transfer ownership (and associated critical privilages) in a one-step process.

### Impact

Critical `owner` priviliges could be transferred to an incorrect address e.g. if
- `owner` mistakenly inputs an incorrect address when calling `PrelaunchPoints::setOwner`,
- the protocol becomes the victim of a Clipboard Replacement Attack: protocol owner copies the address that ownership is supposed to be transferred to, but a malware replaces the address on the clipboard with a different, attacker-controlled address that the protocol owner will eventually end of pasting when preparing to call `PrelaunchPoints::setOwner`.

With the ownership privilages transferred to an incorrect account, the whole protocol will be compromised/unusable.

### Recommended Mitigation Steps

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

## [04] No event emissions for critical state changes in `PrelaunchPoints::setEmergencyMode` and `PrelaunchPoints::allowToken`

`PrelaunchPoints::setEmergencyMode` and `PrelaunchPoints::allowToken` modify critical state variables, but no events are defined and emitted to broadcast the changes.

### Impact

1. Reduced transparency
2. Difficulty to track changes
3. Inefficient or impossible integration with other contracts and services

### Recommended Mitigation Steps

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

## [05] Missing input validation for `_percentage` in `PrelaunchPoints::_claim`

`PrelaunchPoints::_claim` does not perform input validation on `_percentage`. Transaction flow will continue and only fails much later in the execution. 

### Impact

Users calling `PrelaunchPoints::claim` or `PrelaunchPoints::claimAndStake` and accidentally inputting `0` for `_percentage` will waste more gas, as the transaction will fail much later in the execution.

### Recommended Mitigation Steps

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

## [06] `PrelaunchPoints::_validateData` accepts `address(0)` as a valid value for `recipient`

`PrelaunchPoints::_validateData` accepts `address(0)` as a valid value for the `recipient` field of `bytes calldata _data`.

This essentially means that later in the transaction flow, when `Prelaunchdata::_fillQuote` is called, the output tokens of the swap will be sent to `address(0)`.

### Impact

Loss of funds; this would essentially be equivalent to burning tokens.

### Recommended Mitigation Steps

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

**[0xd4n1el (Loop) confirmed and commented](https://github.com/code-423n4/2024-05-loop-findings/issues/118#issuecomment-2108207764):**
 > Excellent report, all findings could be considered.



***

# Gas Optimizations

For this audit, the [report highlighted below](https://github.com/code-423n4/2024-05-loop-findings/issues/126) by **caglankaan** details gas optimizations and received the top score from the judge.

## [G-01] Prevent re-setting a state variable with the same value
Not only is wasteful in terms of gas, but this is especially problematic when an event is emitted and the old and new values set are the same, as listeners might not expect this kind of scenario.

```solidity
Path: ./src/PrelaunchPoints.sol

337:        owner = _owner;	// @audit-issue

353:        lpETH = ILpETH(_loopAddress);	// @audit-issue

354:        lpETHVault = ILpETHVault(_vaultAddress);	// @audit-issue

373:        emergencyMode = _mode;	// @audit-issue
```
[337](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L337-L337), [353](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L353-L353), [354](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L354-L354), [373](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L373-L373)

### Recommendation

Implement checks in your Solidity contracts to avoid re-setting state variables to their existing values. Prior to updating a state variable, compare the new value with the current value and proceed with the assignment only if they differ. Additionally, ensure that events related to state variable updates are emitted only when actual changes occur. This approach not only saves gas but also prevents confusion and unnecessary triggers in event listeners. Regularly reviewing and optimizing your contract for such redundancies can significantly enhance efficiency and clarity in contract operations.

## [G-02] State Variable Access Within a Loop
State variable reads and writes are more expensive than local variable reads and writes. Therefore, it is recommended to replace state variable reads and writes within loops with a local variable. Gas savings should be multiplied by the average loop length.

```solidity
Path: ./src/PrelaunchPoints.sol

108:            isTokenAllowed[_allowedTokens[i]] = true;	// @audit-issue: `isTokenAllowed` used in loop.
```
[108](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L108-L108)

### Recommendation

Optimize gas usage in Solidity by replacing state variable reads and writes within loops with local variable operations. This reduces gas costs significantly, especially when multiplied by the average loop length.

## [G-03] `address(this)` should be cached
Caching saves gas when compared to repeating the calculation at each point it is used in the contract.

```solidity
Path: ./src/PrelaunchPoints.sol

321:        uint256 totalBalance = address(this).balance;	// @audit-issue: `adress(this)` also used on line(s): [324, 322]

503:        boughtETHAmount = address(this).balance - boughtETHAmount;	// @audit-issue: `adress(this)` also used on line(s): [493]
```
[321](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L321-L321), [503](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L503-L503)

### Recommendation

To enhance gas efficiency, cache the contract's address by storing `address(this)` in a state variable at the point of contract deployment or initialization. Use this cached address throughout the contract instead of repeatedly calling `address(this)`. This practice reduces the gas cost associated with multiple computations of the contract's address, leading to more efficient contract execution, especially in scenarios with frequent usage of the contract's address.

## [G-04] Custom Errors in Solidity for Gas Efficiency
Starting from Solidity version 0.8.4, the language introduced a feature known as "custom errors". These custom errors provide a way for developers to define more descriptive and semantically meaningful error conditions without relying on string messages. Prior to this version, developers often used the `require` statement with string error messages to handle specific conditions or validations. However, every unique string used as a revert reason consumes gas, making transactions more expensive.

Custom errors, on the other hand, are identified by their name and the types of their parameters only, and they do not have the overhead of string storage. This means that, when using custom errors instead of `require` statements with string messages, the gas consumption can be significantly reduced, leading to more gas-efficient contracts.

```solidity
Path: ./src/PrelaunchPoints.sol

495:        require(_sellToken.approve(exchangeProxy, _amount));	// @audit-issue
```
[495](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L495-L495)

### Recommendation

It is recommended to use custom errors instead of revert strings to reduce gas costs, especially during contract deployment. Custom errors can be defined using the error keyword and can include dynamic information.

## [G-05] Avoid repeating computations
In Solidity development, repeating the same computations within a contract can lead to unnecessary gas consumption and reduce the contract's efficiency. This is particularly relevant in functions that are called frequently or involve complex calculations. Repeating computations not only wastes computational resources but also increases the cost of executing transactions. By identifying and eliminating redundant calculations, developers can optimize contract performance, reduce gas costs, and improve overall execution speed.

```solidity
Path: ./src/PrelaunchPoints.sol

279:            if (block.timestamp >= startClaimDate) {	// @audit-issue: Same binary operation statement in line(s) between: ['291:291']
```
[279](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L279-L279)

### Recommendation

Review your Solidity contracts to identify any computations that are performed multiple times with the same inputs. Cache the results of these computations in local variables and reuse them within the function or across function calls if the state remains unchanged.

## [G-06] Divisions can be unchecked to save gas
The expression `type(int).min/(-1)` is the only case where division causes an overflow. Therefore, uncheck can be used to save gas in scenarios where it is certain that such an overflow will not occur.

```solidity
Path: ./src/PrelaunchPoints.sol

253:            uint256 userClaim = userStake * _percentage / 100;	// @audit-issue
```
[253](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L253-L253)

### Recommendation

Utilize 'unchecked' blocks in Solidity for divisions where overflow is impossible, such as when `type(int).min/(-1)` is not a concern. This can save gas by bypassing overflow checks in these specific cases.

## [G-07] Stack variable is only used once
If the variable is only accessed once, it's cheaper to use the assigned value directly that one time, and save the 3 gas the extra stack assignment would spend.

```solidity
Path: ./src/PrelaunchPoints.sol

296:            (bool sent,) = msg.sender.call{value: lockedAmount}("");	// @audit-issue: sent used only on line: 298

497:        (bool success,) = payable(exchangeProxy).call{value: 0}(_swapCallData);	// @audit-issue: success used only on line: 498
```
[296](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L296-L296), [497](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L497-L497)

### Recommendation

Eliminate single-use stack variables in Solidity to optimize gas consumption. Directly use the assigned value in the place of the variable. This approach saves the 3 gas typically used for the extra stack assignment, streamlining the function's execution and enhancing overall gas efficiency.

## [G-08] Storage Layout Optimization
Storage Layout Optimization in Solidity involves arranging state variables to minimize gas costs. Since storage is expensive, combining variables into as few slots as possible and deleting unneeded variables can significantly reduce the gas needed for contract operations.

```solidity
Path: ./src/PrelaunchPoints.sol

25:    ILpETH public lpETH;	// @audit-issue: ['Current storage layout is like this: ', "However it can be optimized by 1 storage by storing variables like in order: \n\t`['totalSupply', 'totalLpETH', 'isTokenAllowed', 'balances', 'lpETH', 'lpETHVault', 'owner', 'loopActivation', 'startClaimDate', 'emergencyMode']`"]
26:    ILpETHVault public lpETHVault;
27:    IWETH public immutable WETH;
28:    address public constant ETH = 0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE;
29:    address public immutable exchangeProxy;
30:
31:    address public owner;
32:
33:    uint256 public totalSupply;
34:    uint256 public totalLpETH;
35:    mapping(address => bool) public isTokenAllowed;
36:
37:    enum Exchange {
38:        UniswapV3,
39:        TransformERC20
40:    }
41:
42:    bytes4 public constant UNI_SELECTOR = 0x803ba26d;
43:    bytes4 public constant TRANSFORM_SELECTOR = 0x415565b0;
44:
45:    uint32 public loopActivation;
46:    uint32 public startClaimDate;
47:    uint32 public constant TIMELOCK = 7 days;
48:    bool public emergencyMode;
49:
50:    mapping(address => mapping(address => uint256)) public balances; // User -> Token -> Balance
```
[25](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L25-L50)

### Recommendation

To optimize storage and reduce gas costs, rearrange the storage variables in a way that makes the most of each 32-byte storage slot.

## [G-09] `Internal` functions only called once can be inlined to save gas
If an internal function is only used once, there is no need to modularize it, unless the function calling it would otherwise be too long and complex. Not inlining costs 20 to 40 gas because of two extra JUMP instructions and additional stack operations needed for function calls.

```solidity
Path: ./src/PrelaunchPoints.sol

405:    function _validateData(address _token, uint256 _amount, Exchange _exchange, bytes calldata _data) internal view {	// @audit-issue

448:    function _decodeUniswapV3Data(bytes calldata _data)	// @audit-issue

470:    function _decodeTransformERC20Data(bytes calldata _data)	// @audit-issue

491:    function _fillQuote(IERC20 _sellToken, uint256 _amount, bytes calldata _swapCallData) internal {	// @audit-issue
```
[405](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L405-L405), [448](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L448-L448), [470](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L470-L470), [491](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L491-L491)

### Recommendation

Inline 'internal' functions in Solidity that are called only once to save gas. This avoids the additional gas cost of 20 to 40 units associated with extra JUMP instructions and stack operations required for separate function calls, unless the calling function becomes too complex.

## [G-10] Consider pre-calculating the address of `address(this)` to save gas
Use `foundry`'s [`script.sol`](https://book.getfoundry.sh/reference/forge-std/compute-create-address) or `solady`'s [`LibRlp.sol`](https://github.com/Vectorized/solady/blob/main/src/utils/LibRLP.sol) to save the value in a constant, which will avoid having to spend gas to push the value on the stack every time it's used.

```solidity
Path: ./src/PrelaunchPoints.sol

186:            IERC20(_token).safeTransferFrom(msg.sender, address(this), _amount);	// @audit-issue

230:        uint256 claimedAmount = _claim(_token, address(this), _percentage, _exchange, _data);	// @audit-issue

262:            claimedAmount = address(this).balance;	// @audit-issue

321:        uint256 totalBalance = address(this).balance;	// @audit-issue

322:        lpETH.deposit{value: totalBalance}(address(this));	// @audit-issue

324:        totalLpETH = lpETH.balanceOf(address(this));	// @audit-issue

439:        if (recipient != address(this) && recipient != address(0)) {	// @audit-issue

493:        uint256 boughtETHAmount = address(this).balance;	// @audit-issue

503:        boughtETHAmount = address(this).balance - boughtETHAmount;	// @audit-issue
```
[186](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L186-L186), [230](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L230-L230), [262](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L262-L262), [321](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L321-L321), [322](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L322-L322), [324](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L324-L324), [439](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L439-L439), [493](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L493-L493), [503](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L503-L503)

### Recommendation

To enhance gas efficiency, cache the contract's address by storing `address(this)` in a state variable at the point of contract deployment or initialization. Use this cached address throughout the contract instead of repeatedly calling `address(this)`. This practice reduces the gas cost associated with multiple computations of the contract's address, leading to more efficient contract execution, especially in scenarios with frequent usage of the contract's address.

## [G-11] Counting down in for statements is more gas efficient
Looping downwards in Solidity is more gas efficient due to how the EVM compares variables. In a 'for' loop that counts down, the end condition is usually to compare with zero, which is cheaper than comparing with another number. As such, restructure your loops to count downwards where possible.

```solidity
Path: ./src/PrelaunchPoints.sol

110:                i++;	// @audit-issue
```
[110](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L110-L110)

### Recommendation

Where feasible, refactor `for` loops in your Solidity contracts to count downwards. Adjust the loop initialization, condition, and iteration statements to decrement the loop variable and terminate the loop when it reaches zero. This approach can lead to gas savings, making your contract more efficient in terms of execution costs. Ensure that this refactoring aligns with the logic and requirements of your contract, and thoroughly test to confirm that the revised loop behavior matches the intended functionality.

## [G-12] Use solady library where possible to save gas
The following OpenZeppelin imports have a Solady equivalent, as such they can be used to save GAS as Solady modules have been specifically designed to be as GAS efficient as possible

```solidity
Path: ./src/PrelaunchPoints.sol

4:import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";	// @audit-issue
```
[4](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L4-L4)

### Recommendation

Evaluate and, where appropriate, integrate Solady modules in your Solidity contracts as alternatives to similar OpenZeppelin imports. Focus on areas where gas efficiency can be significantly improved. Ensure that any replacement with Solady's modules does not compromise the security or functionality of your contracts. Conduct thorough testing and code reviews when making such substitutions to confirm compatibility and maintain the integrity of your application. Stay informed about updates and community feedback on both libraries to make informed decisions about their use in your projects.

## [G-13] Consider using solady's `FixedPointMathLib`
Using Solady's `FixedPointMathLib` for multiplication or division operations in Solidity can lead to significant gas savings. This library is designed to optimize fixed-point arithmetic operations, which are common in financial calculations involving tokens or currencies. By implementing more efficient algorithms and assembly optimizations, `FixedPointMathLib` minimizes the computational resources required for these operations. For contracts that frequently perform such calculations, integrating this library can reduce transaction costs, thereby enhancing overall performance and cost-effectiveness. However, developers must ensure compatibility with their existing codebase and thoroughly test for accuracy and expected behavior to avoid any unintended consequences.

```solidity
Path: ./src/PrelaunchPoints.sol

253:            uint256 userClaim = userStake * _percentage / 100;	// @audit-issue
```
[253](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L253-L253)

### Recommendation

Consider integrating Solady's `FixedPointMathLib` into your Solidity contracts for optimized fixed-point arithmetic operations. This library can provide substantial gas savings and enhance the performance of your contract. Before integration, evaluate how `FixedPointMathLib` aligns with your contract’s requirements. Ensure thorough testing for accuracy and compatibility with your existing contract logic. Carefully document any changes and keep track of how these optimizations affect your contract's operations to maintain transparency and reliability in your application. Adopting `FixedPointMathLib` should be a considered decision, balancing the benefits of gas efficiency with the need for maintaining code clarity and functionality.

## [G-14] Consider using OZ EnumerateSet in place of nested mappings
Nested mappings and multi-dimensional arrays in Solidity operate through a process of double hashing, wherein the original storage slot and the first key are concatenated and hashed, and then this hash is again concatenated with the second key and hashed. This process can be quite gas expensive due to the double-hashing operation and subsequent storage operation (sstore).

A possible optimization involves manually concatenating the keys followed by a single hash operation and an sstore. However, this technique introduces the risk of storage collision, especially when there are other nested hash maps in the contract that use the same key types. Because Solidity is unaware of the number and structure of nested hash maps in a contract, it follows a conservative approach in computing the storage slot to avoid possible collisions.

OpenZeppelin's EnumerableSet provides a potential solution to this problem. It creates a data structure that combines the benefits of set operations with the ability to enumerate stored elements, which is not natively available in Solidity. EnumerableSet handles the element uniqueness internally and can therefore provide a more gas-efficient and collision-resistant alternative to nested mappings or multi-dimensional arrays in certain scenarios.

```solidity
Path: ./src/PrelaunchPoints.sol

50:    mapping(address => mapping(address => uint256)) public balances; // User -> Token -> Balance	// @audit-issue
```
[50](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L50-L50)

### Recommendation

Consider using OpenZeppelin's EnumerableSet library as a more gas-efficient and collision-resistant alternative to nested mappings or multi-dimensional arrays, especially when managing unique elements. This library simplifies the handling of unique data sets and allows for the enumeration of elements, a feature not natively supported in Solidity. When refactoring your contract, replace nested mappings or arrays with EnumerableSet where appropriate, ensuring both gas efficiency and enhanced functionality. Be sure to understand the use cases and limitations of EnumerableSet to apply it effectively in your contract's design and implementation.

## [G-15] Use bitmap to save gas
Bitmaps in Solidity are essentially a way of representing a set of boolean values within an integer type variable such as `uint256`. Each bit in the integer represents a true or false value (1 or 0), thus allowing efficient storage of multiple boolean values.

Bitmaps can save gas in the Ethereum network because they condense a lot of information into a small amount of storage. In Ethereum, storage is one of the most significant costs in terms of gas usage. By reducing the amount of storage space needed, you can potentially save on gas fees.

Here's a quick comparison:

If you were to represent 256 different boolean values in the traditional way, you would have to declare 256 different `bool` variables. Given that each `bool` occupies a storage slot and each storage slot costs 20,000 gas to initialize, you would end up paying a considerable amount of gas.

On the other hand, if you were to use a bitmap, you could store these 256 boolean values within a single `uint256` variable. In other words, you'd only pay for a single storage slot, resulting in significant gas savings.

However, it's important to note that while bitmaps can provide gas efficiencies, they do add complexity to the code, making it harder to read and maintain. Also, using bitmaps is efficient only when dealing with a large number of boolean variables that are frequently changed or accessed together. 

In contrast, the straightforward counterpart to bitmaps would be using arrays or mappings to store boolean values, with each `bool` value occupying its own storage slot. This approach is simpler and more readable but could potentially be more expensive in terms of gas usage.

```solidity
Path: ./src/PrelaunchPoints.sol

108:            isTokenAllowed[_allowedTokens[i]] = true;	// @audit-issue

113:        isTokenAllowed[_wethAddress] = true;	// @audit-issue

365:        isTokenAllowed[_token] = true;	// @audit-issue
```
[108](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L108-L108), [113](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L113-L113), [365](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L365-L365)

### Recommendation

Consider using bitmaps in your Solidity contracts when you need to store and manipulate a large set of boolean values. This approach is particularly advantageous in terms of gas efficiency for scenarios involving frequent changes or accesses to these values. However, balance this efficiency with code readability and maintainability. Ensure that the use of bitmaps is well-documented, and consider the complexity it introduces into the code. Employ bitmaps judiciously, especially when their use results in significant gas savings, and the logic they represent is a core aspect of the contract's functionality.

## [G-16] Usage of `uints`/`ints` smaller than 32 bytes (256 bits) incurs overhead
When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size.<br>
https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html<br>
Each operation involving a `uint8` costs an extra [22-28](https://gist.github.com/IllIllI000/9388d20c70f9a4632eb3ca7836f54977) gas (depending on whether the other operand is also a variable of type `uint8`) as compared to ones involving `uint256`, due to the compiler having to clear the higher bits of the memory word before operating on the `uint8`, as well as the associated stack operations of doing so. Use a larger size then downcast where needed.

```solidity
Path: ./src/PrelaunchPoints.sol

45:    uint32 public loopActivation;	// @audit-issue

46:    uint32 public startClaimDate;	// @audit-issue

47:    uint32 public constant TIMELOCK = 7 days;	// @audit-issue

211:    function claim(address _token, uint8 _percentage, Exchange _exchange, bytes calldata _data)	// @audit-issue

226:    function claimAndStake(address _token, uint8 _percentage, Exchange _exchange, bytes calldata _data)	// @audit-issue

240:    function _claim(address _token, address _receiver, uint8 _percentage, Exchange _exchange, bytes calldata _data)	// @audit-issue
```
[45](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L45-L45), [46](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L46-L46), [47](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L47-L47), [211](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L211-L211), [226](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L226-L226), [240](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L240-L240)

### Recommendation

Minimize gas overhead by using `uint256` or `int256` instead of smaller integer types in Solidity contracts. The EVM operates more efficiently with 32-byte sizes. Downcast to smaller types only when necessary, as operations with smaller types like `uint8` incur extra gas due to additional EVM operations for size adjustment.

## [G-17] Refactor modifiers to call a local function
Modifiers code is copied in all instances where it's used, increasing bytecode size. If deployment gas costs are a concern for this contract, refactoring modifiers into functions can reduce bytecode size significantly at the cost of one JUMP.

```solidity
Path: ./src/PrelaunchPoints.sol

511:    modifier onlyAuthorized() {	// @audit-issue

518:    modifier onlyAfterDate(uint256 limitDate) {	// @audit-issue

525:    modifier onlyBeforeDate(uint256 limitDate) {	// @audit-issue
```
[511](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L511-L511), [518](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L518-L518), [525](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L525-L525)

### Recommendation

Evaluate your contract's use of modifiers, particularly those applied across multiple functions, to identify candidates for refactoring into functions. Convert these modifiers into external or public functions that perform the same checks or actions. Then, replace the modifier usage in function declarations with calls to these functions at the start of your functions

## [G-18] Avoid Unnecessary Public Variables
Public state variables in Solidity automatically generate getter functions, increasing contract size and potentially leading to higher deployment and interaction costs. To optimize gas usage and contract efficiency, minimize the use of public variables unless external access is necessary. Instead, use internal or private visibility combined with explicit getter functions when required. This practice not only reduces contract size but also provides better control over data access and manipulation, enhancing security and readability. Prioritize lean, efficient contracts to ensure cost-effectiveness and better performance on the blockchain.

```solidity
Path: ./src/PrelaunchPoints.sol

25:    ILpETH public lpETH;	// @audit-issue

26:    ILpETHVault public lpETHVault;	// @audit-issue

27:    IWETH public immutable WETH;	// @audit-issue

28:    address public constant ETH = 0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE;	// @audit-issue

29:    address public immutable exchangeProxy;	// @audit-issue

31:    address public owner;	// @audit-issue

33:    uint256 public totalSupply;	// @audit-issue

34:    uint256 public totalLpETH;	// @audit-issue

35:    mapping(address => bool) public isTokenAllowed;	// @audit-issue

42:    bytes4 public constant UNI_SELECTOR = 0x803ba26d;	// @audit-issue

43:    bytes4 public constant TRANSFORM_SELECTOR = 0x415565b0;	// @audit-issue

45:    uint32 public loopActivation;	// @audit-issue

46:    uint32 public startClaimDate;	// @audit-issue

47:    uint32 public constant TIMELOCK = 7 days;	// @audit-issue

48:    bool public emergencyMode;	// @audit-issue

50:    mapping(address => mapping(address => uint256)) public balances; // User -> Token -> Balance	// @audit-issue
```
[25](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L25-L25), [26](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L26-L26), [27](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L27-L27), [28](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L28-L28), [29](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L29-L29), [31](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L31-L31), [33](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L33-L33), [34](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L34-L34), [35](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L35-L35), [42](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L42-L42), [43](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L43-L43), [45](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L45-L45), [46](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L46-L46), [47](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L47-L47), [48](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L48-L48), [50](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L50-L50)

### Recommendation

Avoid creating explicit getter functions for 'public' state variables in Solidity. The compiler automatically generates getters for such variables, making additional functions redundant. This practice helps reduce contract size, lowers deployment costs, and simplifies maintenance and understanding of the contract.

## [G-19] Use `do while` loops intead of for loops
A `do while` loop will cost less gas since the condition is not being checked for the first iteration.
```solidity
uint256 i = 1;
do {                   
    param2 += i;
    i++;
}
while (i < 50);
``` 
is better than
```solidity
for(uint256 i = 1; i< 50; i++){
    param1 += i;
}
```


```solidity
Path: ./src/PrelaunchPoints.sol

107:        for (uint256 i = 0; i < length;) {	// @audit-issue
```
[107](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L107-L107)

### Recommendation

Where appropriate, consider using a `do while` loop instead of a `for` loop in your Solidity contracts. This is especially beneficial when the first iteration of the loop does not require a condition check. Refactor your loop logic to fit the `do while` structure for more gas-efficient execution. However, ensure that the loop's logic and termination conditions are correctly implemented to avoid infinite loops or other logical errors. Always balance gas efficiency with code readability and the specific requirements of your contract's logic.

## [G-20] Using XOR (`^`) and AND (`&`) bitwise equivalents for gas optimizations
Given 4 variables a, b, c and d represented as such:
```
0 0 0 0 0 1 1 0 <- a
0 1 1 0 0 1 1 0 <- b
0 0 0 0 0 0 0 0 <- c
1 1 1 1 1 1 1 1 <- d
```
To have a == b means that every 0 and 1 match on both variables. Meaning that a XOR (operator ^) would evaluate to 0 ((a ^ b) == 0), as it excludes by definition any equalities. Now, if a != b, this means that there’s at least somewhere a 1 and a 0 not matching between a and b, making (a ^ b) != 0. Both formulas are logically equivalent and using the XOR bitwise operator costs actually the same amount of gas. However, it is much cheaper to use the bitwise OR operator (|) than comparing the truthy or falsy values. These are logically equivalent too, as the OR bitwise operator (|) would result in a 1 somewhere if any value is not 0 between the XOR (^) statements, meaning if any XOR (^) statement verifies that its arguments are different.

```solidity
Path: ./src/PrelaunchPoints.sol

144:        if (_token == ETH) {	// @audit-issue

158:        if (_token == ETH) {	// @audit-issue

176:        if (_amount == 0) {	// @audit-issue

179:        if (_token == ETH) {	// @audit-issue

188:            if (_token == address(WETH)) {	// @audit-issue

245:        if (userStake == 0) {	// @audit-issue

248:        if (_token == ETH) {	// @audit-issue

287:        if (lockedAmount == 0) {	// @audit-issue

290:        if (_token == ETH) {	// @audit-issue

380:        if (tokenAddress == address(lpETH) || isTokenAllowed[tokenAddress]) {	// @audit-issue

412:        if (_exchange == Exchange.UniswapV3) {	// @audit-issue

421:        } else if (_exchange == Exchange.TransformERC20) {	// @audit-issue
```
[144](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L144-L144), [158](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L158-L158), [176](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L176-L176), [179](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L179-L179), [188](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L188-L188), [245](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L245-L245), [248](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L248-L248), [287](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L287-L287), [290](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L290-L290), [380](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L380-L380), [412](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L412-L412), [421](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L421-L421)

### Recommendation

Review your Solidity contracts to identify any computations that are performed multiple times with the same inputs. Cache the results of these computations in local variables and reuse them within the function or across function calls if the state remains unchanged.

## [G-21] The result of a function call should be cached rather than re-calling the function
The function calls in solidity are expensive. If the same result of the same function calls are to be used several times, the result should be cached to reduce the gas consumption of repeated calls.        

```solidity
Path: ./src/PrelaunchPoints.sol

415:                revert WrongSelector(selector);	// @audit-issue: Function call `WrongSelector` is called multiple times at lines [424].

419:                revert WrongDataTokens(inputToken, outputToken);	// @audit-issue: Function call `WrongDataTokens` is called multiple times at lines [434, 427].
```
[415](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L415-L415), [419](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L419-L419)

### Recommendation

Cache the result of function calls in Solidity instead of making repeated calls to the same function. This practice significantly reduces gas consumption by minimizing costly function call operations.

## [G-22] Avoid updating storage when the value hasn't changed
Manipulating storage in solidity is gas-intensive. It can be optimized by avoiding unnecessary storage updates when the new value equals the existing value. If the old value is equal to the new value, not re-storing the value will avoid a Gsreset (2900 gas), potentially at the expense of a Gcoldsload (2100 gas) or a Gwarmaccess (100 gas).

```solidity
Path: ./src/PrelaunchPoints.sol

337:        owner = _owner;	// @audit-issue

353:        lpETH = ILpETH(_loopAddress);	// @audit-issue

354:        lpETHVault = ILpETHVault(_vaultAddress);	// @audit-issue

373:        emergencyMode = _mode;	// @audit-issue
```
[337](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L337-L337), [353](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L353-L353), [354](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L354-L354), [373](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L373-L373)

### Recommendation

Optimize gas usage by avoiding storage updates in Solidity when the new value is the same as the existing one. This prevents unnecessary gas expenditure from storage resets, balancing the cost against cold or warm storage access as needed.

## [G-23] Avoid zero transfers calls
In Solidity, unnecessary operations can waste gas. For example, a transfer function without a zero amount check uses gas even if called with a zero amount, since the contract state remains unchanged. Implementing a zero amount check avoids these unnecessary function calls, saving gas and improving efficiency.

```solidity
Path: ./src/PrelaunchPoints.sol

383:        IERC20(tokenAddress).safeTransfer(owner, tokenAmount);	// @audit-issue
```
[383](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L383-L383)

### Recommendation

Include a condition in your transfer functions to check for and prevent zero-value transfers. Implement a `require(amount > 0, "Transfer amount must be greater than zero");` statement at the beginning of the function. This preemptive check ensures that the function only proceeds with non-zero transfer amounts, avoiding wasteful operations and saving gas. Apply this optimization across all functions involving token transfers or similar operations to improve your contract's gas efficiency and operational effectiveness.

## [G-24] Use calldata instead of memory for function arguments that do not get mutated
Mark data types as `calldata` instead of `memory` where possible. This makes it so that the data is not automatically loaded into memory. If the data passed into the function does not need to be changed (like updating values in an array), it can be passed in as `calldata`. The one exception to this is if the argument must later be passed into another function that takes an argument that specifies `memory` storage.

```solidity
Path: ./src/PrelaunchPoints.sol

97:    constructor(address _exchangeProxy, address _wethAddress, address[] memory _allowedTokens) {	// @audit-issue
```
[97](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L97-L97)

### Recommendation

To optimize gas usage in your Solidity functions, mark data types as `calldata` instead of `memory` wherever applicable. This prevents unnecessary data loading into memory. Use `calldata` for function arguments that do not require changes within the function, except when passing them into another function that explicitly requires `memory` storage.

## [G-25] Nesting `if` statements that uses `&&` saves gas
In Solidity, the way conditional checks are structured can impact the gas consumption of a transaction. When conditions are combined using `&&` within an `if` statement, Solidity short-circuits the evaluation, meaning that if the first condition is `false`, the subsequent conditions won't be evaluated. This behavior can lead to gas savings compared to using separate nested `if` statements because not all conditions might need to be checked every time. By efficiently structuring these conditional checks, contracts can optimize the gas required for execution, leading to reduced costs for users.

```solidity
Path: ./src/PrelaunchPoints.sol

439:        if (recipient != address(this) && recipient != address(0)) {	// @audit-issue
```
[439](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L439-L439)

### Recommendation

When multiple conditions need to be checked successively, try to combine them in a single `if` statement using `&&` instead of nesting separate `if` statements. This will leverage short-circuit evaluation for potential gas savings.

## [G-26] Constructor Can Be Marked As Payable
`payable` functions cost less gas to execute, since the compiler does not have to add extra checks to ensure that a payment wasn't provided.

A `constructor` can safely be marked as `payable`, since only the deployer would be able to pass funds, and the project itself would not pass any funds.

```solidity
Path: ./src/PrelaunchPoints.sol

97:    constructor(address _exchangeProxy, address _wethAddress, address[] memory _allowedTokens) {	// @audit-issue
```
[97](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L97-L97)

### Recommendation

Mark constructors as 'payable' in Solidity contracts to reduce gas costs, as this eliminates the need for the compiler to add checks against incoming payments. This is safe because only the deployer can send funds during contract creation, and typically no funds are sent at this stage.

## [G-27] Use `selfbalance()` instead of `address(this).balance`
In Solidity, contracts often need to query their own Ether balance. While `address(this).balance` has been a widely used approach to retrieve the current contract's balance, it is not the most gas-efficient method, especially with the introduction of the `SELFBALANCE` opcode in more recent EVM versions.

The `SELFBALANCE` opcode provides a more gas-efficient way to obtain the balance of the current contract. In Solidity, this can be invoked using the `selfbalance()` function. Compared to the `BALANCE` opcode used by `address(this).balance`, `SELFBALANCE` offers significant gas savings:

- `BALANCE`: The static gas cost is 0. If the accessed address is warm, the dynamic gas cost is 100. If the address is cold, the dynamic cost is 2,600.
  
- `SELFBALANCE`: Semantically equivalent to the `BALANCE` opcode when called on the contract's own address, but with a dramatically reduced minimum gas cost of 5.

Switching to `selfbalance()` can lead to significant gas savings, especially in operations or functions that frequently check the contract's balance.

```solidity
Path: ./src/PrelaunchPoints.sol

262:            claimedAmount = address(this).balance;	// @audit-issue

321:        uint256 totalBalance = address(this).balance;	// @audit-issue

493:        uint256 boughtETHAmount = address(this).balance;	// @audit-issue

503:        boughtETHAmount = address(this).balance - boughtETHAmount;	// @audit-issue
```
[262](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L262-L262), [321](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L321-L321), [493](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L493-L493), [503](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L503-L503)

### Recommendation

To optimize gas usage when querying a contract's own Ether balance in Solidity, it's recommended to use the `selfbalance()` function, which utilizes the more gas-efficient `SELFBALANCE` opcode. This can result in significant gas savings compared to `address(this).balance`.

## [G-28] Optimize names to save gas
`public`/`external` function names and `public` member variable names can be optimized to save gas. Below are the interfaces/abstract contracts that can be optimized so that the most frequently-called functions use the least amount of gas possible during method lookup. Method IDs that have two leading zero bytes can save 128 gas each during deployment, and renaming functions to have lower method IDs will save 22 gas per call, [per sorted position shifted](https://medium.com/joyso/solidity-how-does-function-name-affect-gas-consumption-in-smart-contract-47d270d8ac92).

```solidity
Path: ./src/PrelaunchPoints.sol

16:contract PrelaunchPoints {	// @audit-issue
```
[16](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L16-L16)

### Recommendation

Optimize gas usage by renaming 'public'/'external' functions and 'public' member variables in Solidity. Aim for shorter and more efficient names, especially for frequently called functions. This can save gas during deployment and reduce gas costs per call due to lower method ID sorting positions.

## [G-29] Use `uint256(1)`/`uint256(2)` instead of `true`/`false` to save gas for changes
Avoids a Gsset (**20000 gas**) when changing from `false` to `true`, after having been `true` in the past. Since most of the bools aren't changed twice in one transaction, I've counted the amount of gas as half of the full amount, for each variable. Note that public state variables can be re-written to be `private` and use `uint256`, but have public getters returning `bool`s.

```solidity
Path: ./src/PrelaunchPoints.sol

35:    mapping(address => bool) public isTokenAllowed;	// @audit-issue

48:    bool public emergencyMode;	// @audit-issue
```
[35](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L35-L35), [48](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L48-L48)

### Recommendation

To minimize gas overhead in your Solidity contracts, consider using `uint256(1)` and `uint256(2)` to represent `true` and `false`, respectively, instead of `bool` types for storage. This approach avoids additional `SLOAD` and `SSTORE` operations, resulting in more gas-efficient code.

## [G-30] Consider activating `via-ir` for deploying
The IR-based code generator was developed to make code generation more performant by enabling optimization passes that can be applied across functions.

It is possible to activate the IR-based code generator through the command line by using the flag `--via-ir`or by including the option `{"viaIR": true}`.

Keep in mind that compiling with this option may take longer. However, you can simply test it before deploying your code. If you find that it provides better performance, you can add the `--via-ir` flag to your deploy command.
```solidity
/// @audit Global finding.
```
[1](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L1)

### Recommendation

Consider activating `via-ir`.

## [G-31] Optimize Deployment Size by Fine-tuning IPFS Hash
The Solidity compiler appends 53 bytes of metadata to the smart contract code, incurring an extra cost of 10,600 gas. This additional expense arises from 200 gas per bytecode, plus calldata cost, which amounts to 16 gas for non-zero bytes and 4 gas for zero bytes. This results in a maximum of 848 extra gas in calldata cost.

Reducing this cost is crucial for the following reasons:

The metadata's 53-byte addition leads to a deployment cost increase of 10,600 gas. It can also result in an additional calldata cost of up to 848 gas. 

Ways to Minimize Gas Consumption:

Employ the `--no-cbor-metadata` compiler option to exclude metadata. Be cautious as this might impact contract verification. Search for code comments that yield an IPFS hash with more zeros, thereby reducing calldata costs.

```solidity
/// @audit Global finding.
```
[1](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L1)

### Recommendation

To optimize deployment size and reduce associated costs, consider the following strategies:
1. **Exclude Metadata with Compiler Option**: Use the `solc` compiler’s `--metadata-hash none` or `--no-cbor-metadata` option to prevent the inclusion of metadata in the compiled bytecode. This action reduces the bytecode size, thus lowering deployment gas costs. However, exercise caution with this approach, as it might affect the ability to verify the contract on platforms like Etherscan.

2. **Optimize IPFS Hash for More Zeros**: If excluding metadata is not desirable, another approach involves optimizing code comments or elements that influence the metadata hash generation to achieve an IPFS hash with a higher proportion of zeros. Since calldata costs are lower for zero bytes, a metadata hash with more zeros can reduce the calldata costs associated with contract interactions.

Example for excluding metadata:
```bash
solc --metadata-hash none YourContract.sol
```

## [G-32] Low level `call` can be optimized with assembly
`returnData` is copied to memory even if the variable is not utilized: the proper way to handle this is through a low level assembly call.
```solidity
// before
(bool success,) = payable(receiver).call{gas: gas, value: value}("");

//after
bool success;
assembly {
    success := call(gas, receiver, value, 0, 0, 0, 0)
}
```

```solidity
Path: ./src/PrelaunchPoints.sol

296:            (bool sent,) = msg.sender.call{value: lockedAmount}("");	// @audit-issue

497:        (bool success,) = payable(exchangeProxy).call{value: 0}(_swapCallData);	// @audit-issue
```
[296](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L296-L296), [497](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L497-L497)

### Recommendation

Optimize low-level 'call' operations in Solidity using assembly, especially when 'returnData' is not needed. This avoids unnecessary copying to memory and can lead to gas savings. For example, replace `(bool success,) = payable(receiver).call{gas: gas, value: value}("");` with an assembly block: `assembly { success := call(gas, receiver, value, 0, 0, 0, 0) }`. This ensures a more efficient execution.

## [G-33] Assembly: Use scratch space for building calldata
If an external call's calldata can fit into two or fewer words, use the scratch space to build the calldata, rather than allowing Solidity to do a memory expansion.

```solidity
Path: ./src/PrelaunchPoints.sol

189:                WETH.withdraw(_amount);	// @audit-issue

231:        lpETH.approve(address(lpETHVault), claimedAmount);	// @audit-issue

232:        lpETHVault.stake(claimedAmount, msg.sender);	// @audit-issue

249:            claimedAmount = userStake.mulDiv(totalLpETH, totalSupply);	// @audit-issue

251:            lpETH.safeTransfer(_receiver, claimedAmount);	// @audit-issue

302:            IERC20(_token).safeTransfer(msg.sender, lockedAmount);	// @audit-issue

324:        totalLpETH = lpETH.balanceOf(address(this));	// @audit-issue

383:        IERC20(tokenAddress).safeTransfer(owner, tokenAmount);	// @audit-issue

495:        require(_sellToken.approve(exchangeProxy, _amount));	// @audit-issue
```
[189](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L189-L189), [231](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L231-L231), [232](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L232-L232), [249](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L249-L249), [251](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L251-L251), [302](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L302-L302), [324](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L324-L324), [383](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L383-L383), [495](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L495-L495)

### Recommendation

Review your smart contracts to identify and remove `block.number` and `block.timestamp` from the parameters of emitted events. Instead of manually adding these fields, rely on the Ethereum blockchain's inherent inclusion of this information within the block context. Simplify your event definitions to include only the essential data specific to the event's purpose, excluding universally available block metadata.

## [G-34] Use assembly to check for `0`
Using assembly to check for zero can save gas by allowing more direct access to the evm and reducing some of the overhead associated with high-level operations in solidity.

```solidity
Path: ./src/PrelaunchPoints.sol

176:        if (_amount == 0) {	// @audit-issue

245:        if (userStake == 0) {	// @audit-issue

287:        if (lockedAmount == 0) {	// @audit-issue
```
[176](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L176-L176), [245](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L245-L245), [287](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L287-L287)

### Recommendation

To optimize gas usage in your Solidity code, consider using inline assembly for checking `0`. This approach can significantly reduce gas costs, especially in high-frequency or gas-sensitive operations, leading to more efficient contract execution.

## [G-35] Use assembly to write `address` storage values
Using assembly `{ sstore(state.slot, addr)}` instead of `state = addr` can save gas.

```solidity
Path: ./src/PrelaunchPoints.sol

99:        exchangeProxy = _exchangeProxy;	// @audit-issue

100:        WETH = IWETH(_wethAddress);	// @audit-issue

337:        owner = _owner;	// @audit-issue

353:        lpETH = ILpETH(_loopAddress);	// @audit-issue

354:        lpETHVault = ILpETHVault(_vaultAddress);	// @audit-issue
```
[99](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L99-L99), [100](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L100-L100), [337](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L337-L337), [353](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L353-L353), [354](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L354-L354)

### Recommendation

To reduce gas costs in your Solidity code, consider using assembly with `{ sstore(state.slot, addr) }` for writing `address` storage values instead of `state = addr`. This approach can result in significant gas savings.

## [G-36] Use assembly to emit an `event`
To efficiently emit events, it's possible to utilize assembly by making use of scratch space and the free memory pointer. This approach has the advantage of potentially avoiding the costs associated with memory expansion.

However, it's important to note that in order to safely optimize this process, it is preferable to cache and restore the free memory pointer.

A good example of such practice can be seen in [Solady's](https://github.com/Vectorized/solady/blob/main/src/tokens/ERC1155.sol#L167) codebase.

```solidity
Path: ./src/PrelaunchPoints.sol

197:        emit Locked(_receiver, _amount, _token, _referral);	// @audit-issue

234:        emit StakedVault(msg.sender, claimedAmount);	// @audit-issue

265:        emit Claimed(msg.sender, _token, claimedAmount);	// @audit-issue

305:        emit Withdrawn(msg.sender, _token, lockedAmount);	// @audit-issue

329:        emit Converted(totalBalance, totalLpETH);	// @audit-issue

339:        emit OwnerUpdated(_owner);	// @audit-issue

357:        emit LoopAddressesUpdated(_loopAddress, _vaultAddress);	// @audit-issue

385:        emit Recovered(tokenAddress, tokenAmount);	// @audit-issue

504:        emit SwappedTokens(address(_sellToken), _amount, boughtETHAmount);	// @audit-issue
```
[197](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L197-L197), [234](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L234-L234), [265](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L265-L265), [305](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L305-L305), [329](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L329-L329), [339](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L339-L339), [357](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L357-L357), [385](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L385-L385), [504](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L504-L504)

### Recommendation

To optimize event emission in your Solidity code, consider using assembly with scratch space and the free memory pointer. This approach can help reduce gas costs by avoiding memory expansion expenses. However, it's crucial to ensure safe optimization by caching and restoring the free memory pointer, as demonstrated in examples like Solady's codebase.

## [G-37] Use assembly to validate `msg.sender`
We can use assembly to efficiently validate `msg.sender` with the least amount of opcodes necessary. For more details check the following report [Here](https://code4rena.com/reports/2023-05-juicebox#g-06-use-assembly-to-validate-msgsender)

```solidity
Path: ./src/PrelaunchPoints.sol

512:        if (msg.sender != owner) {	// @audit-issue
```
[512](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L512-L512)

### Recommendation
To optimize the validation of `msg.sender` in your Solidity code, consider using assembly to achieve this with the minimum number of opcodes required. You can refer to the detailed report [Here](https://code4rena.com/reports/2023-05-juicebox#g-06-use-assembly-to-validate-msgsender) for more insights and examples on efficient implementation.



***

# Disclosures

C4 is an open organization governed by participants in the community.

C4 audits incentivize the discovery of exploits, vulnerabilities, and bugs in smart contracts. Security researchers are rewarded at an increasing rate for finding higher-risk issues. Audit submissions are judged by a knowledgeable security researcher and solidity developer and disclosed to sponsoring developers. C4 does not conduct formal verification regarding the provided code but instead provides final verification.

C4 does not provide any guarantee or warranty regarding the security of this project. All smart contract software should be used at the sole risk and responsibility of users.
