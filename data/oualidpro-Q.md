## Summary

### Low Risk Issues

| |Issue|Instances|
|-|:-|:-:|
| [[L&#x2011;1](#l1-missing-checks-for-address-0-when-assigning-values-to-address-state-variables)] | Missing checks for `address(0)` when assigning values to address state variables | 2 | 
| [[L&#x2011;2](#l2-increase-decrease-or-forceapprove-allowance-should-be-used-instead-of-approve)] | increase/decrease or forceApprove allowance should be used instead of approve | 2 | 
| [[L&#x2011;3](#l3-governance-operations-should-be-behind-a-timelock)] | Governance operations should be behind a timelock | 4 | 
| [[L&#x2011;4](#l4-setters-should-have-initial-value-check)] | Setters should have initial value check | 3 | 
| [[L&#x2011;5](#l5-solidity-version-0-8-20-may-not-work-on-other-chains-due-to-push0)] | Solidity version 0.8.20 may not work on other chains due to `PUSH0` | 1 | 
| [[L&#x2011;6](#l6-int-casting-block-timestamp-can-reduce-the-lifespan-of-a-contract)] | Int casting `block.timestamp` can reduce the lifespan of a contract | 2 | 
| [[L&#x2011;7](#l7-unused-empty-receive-fallback-function)] | Unused/empty `receive()`/`fallback()` function | 1 | 
| [[L&#x2011;8](#l8-consider-implementing-two-step-procedure-for-updating-protocol-addresses)] | Consider implementing two-step procedure for updating protocol addresses | 1 | 
| [[L&#x2011;9](#l9-privileged-functions-can-create-points-of-failure)] | Privileged functions can create points of failure | 5 | 
| [[L&#x2011;10](#l10-functions-calling-contracts-addresses-with-transfer-hooks-are-missing-reentrancy-guards)] | Functions calling contracts/addresses with transfer hooks are missing reentrancy guards | 1 | 
| [[L&#x2011;11](#l11-code-does-not-follow-the-best-practice-of-check-effects-interaction)] | Code does not follow the best practice of check-effects-interaction | 3 | 
| [[L&#x2011;12](#l12-prevent-re-setting-a-state-variable-with-the-same-value)] | prevent re-setting a state variable with the same value | 3 | 
| [[L&#x2011;13](#l13-empty-receive-functions-can-cause-gas-issues)] | Empty receive functions can cause gas issues | 1 | 

Total: 29 instances over 13 issues

### Non-critical Issues

| |Issue|Instances|
|-|:-|:-:|
| [[NC&#x2011;1](#nc1-assembly-blocks-should-have-extensive-comments)] | Assembly blocks should have extensive comments | 2 | 
| [[NC&#x2011;2](#nc2-constants-in-comparisons-should-appear-on-the-left-side)] | Constants in comparisons should appear on the left side | 12 | 
| [[NC&#x2011;3](#nc3-natspec-natspec-description-is-missing-from-contract)] | [NatSpec] Natspec description is missing from `contract` | 46 | 
| [[NC&#x2011;4](#nc4-contract-does-not-follow-the-solidity-style-guide-s-suggested-layout-ordering)] | Contract does not follow the Solidity style guide's suggested layout ordering | 1 | 
| [[NC&#x2011;5](#nc5-custom-error-has-no-error-details)] | Custom error has no error details | 14 | 
| [[NC&#x2011;6](#nc6-consider-using-delete-rather-than-assigning-zero-to-clear-values)] | Consider using `delete` rather than assigning `zero` to clear values | 2 | 
| [[NC&#x2011;7](#nc7-empty-bytes-check-is-missing)] | Empty bytes check is missing | 12 | 
| [[NC&#x2011;8](#nc8-events-are-missing-sender-information)] | Events are missing sender information | 6 | 
| [[NC&#x2011;9](#nc9-events-may-be-emitted-out-of-order-due-to-reentrancy)] | Events may be emitted out of order due to reentrancy | 4 | 
| [[NC&#x2011;10](#nc10-defining-all-external-public-functions-in-contract-interfaces)] | Defining All External/Public Functions in Contract Interfaces | 12 | 
| [[NC&#x2011;11](#nc11-function-ordering-does-not-follow-the-solidity-style-guide)] | Function ordering does not follow the Solidity style guide | 1 | 
| [[NC&#x2011;12](#nc12-address-s-shouldn-t-be-hard-coded)] | `address`s shouldn't be hard-coded | 1 | 
| [[NC&#x2011;13](#nc13-imports-could-be-organized-more-systematically)] | Imports could be organized more systematically | 1 | 
| [[NC&#x2011;14](#nc14-import-declarations-should-import-specific-identifiers-rather-than-the-whole-file)] | Import declarations should import specific identifiers, rather than the whole file | 2 | 
| [[NC&#x2011;15](#nc15-inconsistent-usage-of-require-error)] | Inconsistent usage of `require`/`error` | 1 | 
| [[NC&#x2011;16](#nc16-large-numeric-literals-should-use-underscores-for-readability)] | Large numeric literals should use underscores for readability | 1 | 
| [[NC&#x2011;17](#nc17-long-lines-of-code)] | Long lines of code | 3 | 
| [[NC&#x2011;18](#nc18-missing-event-and-or-timelock-for-critical-parameter-change)] | Missing event and or timelock for critical parameter change | 1 | 
| [[NC&#x2011;19](#nc19-consider-using-named-mappings)] | Consider using named mappings | 3 | 
| [[NC&#x2011;20](#nc20-events-that-mark-critical-parameter-changes-should-contain-both-the-old-and-the-new-value)] | Events that mark critical parameter changes should contain both the old and the new value | 2 | 
| [[NC&#x2011;21](#nc21-require-revert-statements-should-have-descriptive-reason-strings)] | `require()` / `revert()` statements should have descriptive reason strings | 1 | 
| [[NC&#x2011;22](#nc22-setters-should-prevent-re-setting-of-the-same-value)] | Setters should prevent re-setting of the same value | 2 | 
| [[NC&#x2011;23](#nc23-natspec-return-argument-is-missing)] | NatSpec `@return` argument is missing | 3 | 
| [[NC&#x2011;24](#nc24-consider-using-safetransferlib-safetransfereth-or-address-sendvalue-for-clearer-semantic-meaning)] | Consider using `SafeTransferLib.safeTransferETH()` or `Address.sendValue()` for clearer semantic meaning | 2 | 
| [[NC&#x2011;25](#nc25-empty-receive-payable-fallback-function-does-not-authorize-requests)] | Empty `receive()`/`payable fallback()` function does not authorize requests | 1 | 
| [[NC&#x2011;26](#nc26-state-variables-should-have-natspec-comments)] | State variables should have `Natspec` comments | 16 | 
| [[NC&#x2011;27](#nc27-contracts-should-have-full-test-coverage)] | Contracts should have full test coverage | 1 | 
| [[NC&#x2011;28](#nc28-top-level-pragma-declarations-should-be-separated-by-two-blank-lines)] | Top level pragma declarations should be separated by two blank lines | 1 | 
| [[NC&#x2011;29](#nc29-critical-functions-should-be-a-two-step-procedure)] | Critical functions should be a two step procedure | 4 | 
| [[NC&#x2011;30](#nc30-event-is-missing-indexed-fields)] | Event is missing `indexed` fields | 9 | 
| [[NC&#x2011;31](#nc31-missing-upgradability-functionality)] | Missing upgradability functionality | 1 | 
| [[NC&#x2011;32](#nc32-constants-should-be-defined-rather-than-using-magic-numbers)] | Constants should be defined rather than using magic numbers | 3 | 
| [[NC&#x2011;33](#nc33-use-a-single-file-for-system-wide-constants)] | Use a single file for system wide constants | 1 | 
| [[NC&#x2011;34](#nc34-consider-using-smtchecker)] | Consider using SMTChecker | 1 | 
| [[NC&#x2011;35](#nc35-complex-function-controle-flow)] | Complex function controle flow | 2 | 
| [[NC&#x2011;36](#nc36-consider-bounding-input-array-length)] | Consider bounding input array length | 1 | 
| [[NC&#x2011;37](#nc37-natspec-natspec-dev-is-missing-from-contract)] | [NatSpec] Natspec `@dev` is missing from `contract` | 41 | 
| [[NC&#x2011;38](#nc38-contract-should-expose-an-interface)] | Contract should expose an `interface` | 12 | 
| [[NC&#x2011;39](#nc39-natspec-natspec-notice-is-missing-from-contract)] | [NatSpec] Natspec `@notice` is missing from `contract` | 42 | 
| [[NC&#x2011;40](#nc40-contract-uses-both-require-revert-as-well-as-custom-errors)] | Contract uses both `require()`/`revert()` as well as custom errors | 1 | 
| [[NC&#x2011;41](#nc41-immutable-variable-names-don-t-follow-the-solidity-style-guide)] | `immutable` variable names don\'t follow the Solidity style guide | 1 | 
| [[NC&#x2011;42](#nc42-use-the-latest-solidity-version-for-better-security)] | Use the latest Solidity version for better security | 1 | 
| [[NC&#x2011;43](#nc43-consider-adding-formal-verification-proofs)] | Consider adding formal verification proofs | 1 | 
| [[NC&#x2011;44](#nc44-missing-zero-address-check-in-functions-with-address-parameters)] | Missing zero address check in functions with address parameters | 2 | 
| [[NC&#x2011;45](#nc45-use-a-struct-to-encapsulate-multiple-function-parameters)] | Use a struct to encapsulate multiple function parameters | 1 | 
| [[NC&#x2011;46](#nc46-multiple-mappings-with-same-keys-can-be-combined-into-a-single-struct-mapping-for-readability)] | Multiple mappings with same keys can be combined into a single struct mapping for readability | 2 | 
| [[NC&#x2011;47](#nc47-constructor-should-emit-an-event)] | constructor should emit an event | 1 | 
| [[NC&#x2011;48](#nc48-complex-functions-should-include-comments)] | Complex functions should include comments | 2 | 
| [[NC&#x2011;49](#nc49-make-use-of-solidiy-s-using-keyword)] | Make use of Solidiy's `using` keyword | 1 | 
| [[NC&#x2011;50](#nc50-solidity-all-verbatim-blocks-are-considered-identical-by-deduplicator-and-can-incorrectly-be-unified)] | [Solidity]: All `verbatim` blocks are considered identical by deduplicator and can incorrectly be unified | 1 | 
| [[NC&#x2011;51](#nc51-natspec-natspec-param-is-missing-from-error)] | [NatSpec] Natspec `@param` is missing from `error` | 17 | 
| [[NC&#x2011;52](#nc52-not-using-the-latest-version-of-prb-math-from-dependencies)] | Not using the latest version of `prb-math` from dependencies | 1 | 
| [[NC&#x2011;53](#nc53-enforcing-lowercase-and-underscores-only-in-the-name-field-of-package-json)] | Enforcing Lowercase and Underscores Only in the `name` Field of package.json | 1 | 
| [[NC&#x2011;54](#nc54-using-low-level-call-for-transfers)] | Using Low-Level Call for Transfers | 2 | 
| [[NC&#x2011;55](#nc55-off-by-one-timestamp-error)] | Off-by-one timestamp error | 5 | 
| [[NC&#x2011;56](#nc56-incorrect-withdraw-declaration)] | Incorrect withdraw declaration | 1 | 
| [[NC&#x2011;57](#nc57-a-event-should-be-emitted-if-a-non-immutable-state-variable-is-set-in-a-constructor)] | A event should be emitted if a non immutable state variable is set in a constructor | 5 | 
| [[NC&#x2011;58](#nc58-returning-a-struct-instead-of-returning-many-variables-is-better)] | Returning a struct instead of returning many variables is better | 1 | 
| [[NC&#x2011;59](#nc59-solidity-order-of-argument-evaluation-disrupted-in-non-expression-split-code-by-optimizer-sequences-with-fullinliner)] | [Solidity]: Order of Argument Evaluation Disrupted in Non-Expression-Split Code by Optimizer Sequences with FullInliner | 1 | 
| [[NC&#x2011;60](#nc60-consider-adding-validation-of-user-inputs)] | Consider adding validation of user inputs | 10 | 
| [[NC&#x2011;61](#nc61-consider-using-named-function-calls)] | Consider using named function calls | 8 | 

Total: 337 instances over 61 issues

## Low Risk Issues

### [L&#x2011;1] Missing checks for `address(0)` when assigning values to address state variables 



*There are 2 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

99:         exchangeProxy = _exchangeProxy;

337:         owner = _owner;


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L99-L99) , [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L337-L337)


</details>


### [L&#x2011;2] increase/decrease or forceApprove allowance should be used instead of approve 
In order to prevent front running, increase/decrease or forceApprove allowance should be used in place of approve where possible 


*There are 2 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

231:         lpETH.approve(address(lpETHVault), claimedAmount);

495:         require(_sellToken.approve(exchangeProxy, _amount));


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L231-L231) , [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L495-L495)


</details>


### [L&#x2011;3] Governance operations should be behind a timelock 
All critical and governance operations should be protected by a timelock. For example from the point of view of a user, the changing of the owner of a contract is a high risk operation that may have outcomes ranging from an attacker gaining control over the protocol, to the function no longer functioning due to a typo in the destination address. To give users plenty of warning so that they can validate any ownership changes, changes of ownership should be behind a timelock.


*There are 4 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

336:     function setOwner(address _owner) external onlyAuthorized {
337:         owner = _owner;
338: 
339:         emit OwnerUpdated(_owner);
340:     }

364:     function allowToken(address _token) external onlyAuthorized {
365:         isTokenAllowed[_token] = true;
366:     }

372:     function setEmergencyMode(bool _mode) external onlyAuthorized {
373:         emergencyMode = _mode;
374:     }

379:     function recoverERC20(address tokenAddress, uint256 tokenAmount) external onlyAuthorized {
380:         if (tokenAddress == address(lpETH) || isTokenAllowed[tokenAddress]) {
381:             revert NotValidToken();
382:         }
383:         IERC20(tokenAddress).safeTransfer(owner, tokenAmount);
384: 
385:         emit Recovered(tokenAddress, tokenAmount);
386:     }


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L336-L340) , [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L364-L366), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L372-L374), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L379-L386)


</details>


### [L&#x2011;4] Setters should have initial value check 
Setters should have initial value check to prevent assigning wrong value to the variable. Assginment of wrong value can lead to unexpected behavior of the contract.


*There are 3 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

336:     function setOwner(address _owner) external onlyAuthorized {

348:     function setLoopAddresses(address _loopAddress, address _vaultAddress)

372:     function setEmergencyMode(bool _mode) external onlyAuthorized {


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L336-L336) , [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L348-L348), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L372-L372)


</details>


### [L&#x2011;5] Solidity version 0.8.20 may not work on other chains due to `PUSH0` 
The compiler for Solidity 0.8.20 switches the default target EVM version to [Shanghai](https://blog.soliditylang.org/2023/05/10/solidity-0.8.20-release-announcement/#important-note), which includes the new `PUSH0` op code. This op code may not yet be implemented on all L2s, so deployment on these chains will fail. To work around this issue, use an earlier [EVM](https://docs.soliditylang.org/en/v0.8.20/using-the-compiler.html?ref=zaryabs.com#setting-the-evm-version-to-target) [version](https://book.getfoundry.sh/reference/config/solidity-compiler#evm_version)


*There are 1 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

2: pragma solidity 0.8.20;


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L2-L2) 


</details>


### [L&#x2011;6] Int casting `block.timestamp` can reduce the lifespan of a contract 
Consider removing casting to ensure future functionality.


*There are 2 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

327:         startClaimDate = uint32(block.timestamp);

355:         loopActivation = uint32(block.timestamp);


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L327-L327) , [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L355-L355)


</details>


### [L&#x2011;7] Unused/empty `receive()`/`fallback()` function 
If the intention is for the Ether to be used, the function should call another function or emit an event, otherwise it should revert.


*There are 1 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

392:     receive() external payable {}
393: 


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L392-L393) 


</details>


### [L&#x2011;8] Consider implementing two-step procedure for updating protocol addresses 
A copy-paste error or a typo may end up bricking protocol functionality, or sending tokens to an address with no known private key. Consider implementing a two-step procedure for updating protocol addresses, where the recipient is set as pending, and must 'accept' the assignment by making an affirmative call. A straight forward way of doing this would be to have the target contracts implement [EIP-165](https://eips.ethereum.org/EIPS/eip-165), and to have the 'set' functions ensure that the recipient is of the right interface type.


*There are 1 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

336:     function setOwner(address _owner) external onlyAuthorized {
337:         owner = _owner;
338: 
339:         emit OwnerUpdated(_owner);
340:     }


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L336-L340) 


</details>


### [L&#x2011;9] Privileged functions can create points of failure 
Ensure such accounts are protected and consider implementing multi sig to prevent a single point of failure 


*There are 5 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

336:     function setOwner(address _owner) external onlyAuthorized {

348:     function setLoopAddresses(address _loopAddress, address _vaultAddress)
349:         external
350:         onlyAuthorized

364:     function allowToken(address _token) external onlyAuthorized {

372:     function setEmergencyMode(bool _mode) external onlyAuthorized {

379:     function recoverERC20(address tokenAddress, uint256 tokenAmount) external onlyAuthorized {


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L336-L336) , [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L348-L350), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L364-L364), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L372-L372), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L379-L379)


</details>


### [L&#x2011;10] Functions calling contracts/addresses with transfer hooks are missing reentrancy guards 
Even if the function follows the best practice of check-effects-interaction, not using a reentrancy guard when there may be transfer hooks will open the users of this protocol up to [read-only reentrancies](https://chainsecurity.com/curve-lp-oracle-manipulation-post-mortem/) with no way to protect against it, except by block-listing the whole protocol.


*There are 1 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

//@audit function `withdraw()` is not protected against reentrancy
302:             IERC20(_token).safeTransfer(msg.sender, lockedAmount);


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L302-L302) 


</details>


### [L&#x2011;11] Code does not follow the best practice of check-effects-interaction 
Code should follow the best-practice of [check-effects-interaction](https://blockchain-academy.hs-mittweida.de/courses/solidity-coding-beginners-to-intermediate/lessons/solidity-11-coding-patterns/topic/checks-effects-interactions/), where state variables are updated before any external calls are made. Doing so prevents a large class of reentrancy bugs.


*There are 3 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

//@audit the function `withdraw()` is called before the following assignment
190:                 totalSupply = totalSupply + _amount;

//@audit the function `withdraw()` is called before the following assignment
191:                 balances[_receiver][ETH] += _amount;

//@audit the function `safeTransferFrom()` is called before the following assignment
193:                 balances[_receiver][_token] += _amount;


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L190-L190) , [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L191-L191), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L193-L193)


</details>


### [L&#x2011;12] prevent re-setting a state variable with the same value 
Not only is wasteful in terms of gas, but this is especially problematic when an event is emitted and the old and new values set are the same, as listeners might not expect this kind of scenario.


*There are 3 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

336:     function setOwner(address _owner) external onlyAuthorized {

348:     function setLoopAddresses(address _loopAddress, address _vaultAddress)

372:     function setEmergencyMode(bool _mode) external onlyAuthorized {


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L336-L336) , [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L348-L348), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L372-L372)


</details>


### [L&#x2011;13] Empty receive functions can cause gas issues 
In Solidity, functions that receive Ether without corresponding functionality to utilize or withdraw these funds can inadvertently lead to a permanent loss of value. This is because if someone sends Ether to the contract, they may be unable to retrieve it. To avoid this, functions receiving Ether should be accompanied by additional methods that process or allow the withdrawal of these funds. If the intent is to use the received Ether, it should trigger a separate function; if not, it should revert the transaction (for instance, via `require(msg.sender == address(weth))`). Access control checks can also prevent unintended Ether transfers, despite the slight gas cost they entail. If concerns over gas costs persist, at minimum, include a rescue function to recover unused Ether. Missteps in handling Ether in smart contracts can lead to irreversible financial losses, hence these precautions are crucial.


*There are 1 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

392:     receive() external payable {}


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L392-L392) 


</details>


## Non-critical Issues

### [NC&#x2011;1] Assembly blocks should have extensive comments 
Assembly blocks are taking a lot more time to audit than normal Solidity code, and often have gotchas and side-effects that the Solidity versions of the same code do not. Consider adding more comments explaining what is being done in every step of the assembly code


*There are 2 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

454:         assembly {

475:         assembly {


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L454-L454) , [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L475-L475)


</details>


### [NC&#x2011;2] Constants in comparisons should appear on the left side 



*There are 12 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

//@audit `ETH`
144:         if (_token == ETH) {

//@audit `ETH`
158:         if (_token == ETH) {

//@audit `0`
176:         if (_amount == 0) {

//@audit `ETH`
179:         if (_token == ETH) {

//@audit `0`
245:         if (userStake == 0) {

//@audit `ETH`
248:         if (_token == ETH) {

//@audit `0`
287:         if (lockedAmount == 0) {

//@audit `ETH`
290:         if (_token == ETH) {

//@audit `TIMELOCK`
316:         if (block.timestamp - loopActivation <= TIMELOCK) {

//@audit `UNI_SELECTOR`
414:             if (selector != UNI_SELECTOR) {

//@audit `TRANSFORM_SELECTOR`
423:             if (selector != TRANSFORM_SELECTOR) {

//@audit `ETH`
426:             if (outputToken != ETH) {


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L144-L144) , [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L158-L158), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L176-L176), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L179-L179), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L245-L245), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L248-L248), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L287-L287), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L290-L290), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L316-L316), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L414-L414), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L423-L423), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L426-L426)


</details>


### [NC&#x2011;3] [NatSpec] Natspec description is missing from `contract` 
It is recommended that Solidity contracts are fully annotated using NatSpec for all public interfaces (everything in the ABI). It is clearly stated in the Solidity official documentation. In complex projects such as Defi, the interpretation of all functions and their arguments and returns is important for code readability and auditability.[source](https://docs.soliditylang.org/en/v0.8.15/natspec-format.html)


*There are 46 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

25:     ILpETH public lpETH;

26:     ILpETHVault public lpETHVault;

27:     IWETH public immutable WETH;

28:     address public constant ETH = 0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE;

29:     address public immutable exchangeProxy;

31:     address public owner;

33:     uint256 public totalSupply;

34:     uint256 public totalLpETH;

35:     mapping(address => bool) public isTokenAllowed;

42:     bytes4 public constant UNI_SELECTOR = 0x803ba26d;

43:     bytes4 public constant TRANSFORM_SELECTOR = 0x415565b0;

45:     uint32 public loopActivation;

46:     uint32 public startClaimDate;

47:     uint32 public constant TIMELOCK = 7 days;

48:     bool public emergencyMode;

50:     mapping(address => mapping(address => uint256)) public balances; // User -> Token -> Balance

56:     event Locked(address indexed user, uint256 amount, address token, bytes32 indexed referral);

57:     event StakedVault(address indexed user, uint256 amount);

58:     event Converted(uint256 amountETH, uint256 amountlpETH);

59:     event Withdrawn(address indexed user, address token, uint256 amount);

60:     event Claimed(address indexed user, address token, uint256 reward);

61:     event Recovered(address token, uint256 amount);

62:     event OwnerUpdated(address newOwner);

63:     event LoopAddressesUpdated(address loopAddress, address vaultAddress);

64:     event SwappedTokens(address sellToken, uint256 sellAmount, uint256 buyETHAmount);

70:     error InvalidToken();

71:     error NothingToClaim();

72:     error TokenNotAllowed();

73:     error CannotLockZero();

74:     error CannotWithdrawZero();

75:     error UseClaimInstead();

76:     error FailedToSendEther();

77:     error SwapCallFailed();

78:     error WrongSelector(bytes4 selector);

79:     error WrongDataTokens(address inputToken, address outputToken);

80:     error WrongDataAmount(uint256 inputTokenAmount);

81:     error WrongRecipient(address recipient);

82:     error WrongExchange();

83:     error LoopNotActivated();

84:     error NotValidToken();

85:     error NotAuthorized();

86:     error CurrentlyNotPossible();

87:     error NoLongerPossible();

511:     modifier onlyAuthorized() {

518:     modifier onlyAfterDate(uint256 limitDate) {

525:     modifier onlyBeforeDate(uint256 limitDate) {


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L25-L25) , [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L26-L26), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L27-L27), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L28-L28), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L29-L29), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L31-L31), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L33-L33), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L34-L34), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L35-L35), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L42-L42), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L43-L43), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L45-L45), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L46-L46), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L47-L47), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L48-L48), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L50-L50), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L56-L56), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L57-L57), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L58-L58), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L59-L59), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L60-L60), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L61-L61), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L62-L62), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L63-L63), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L64-L64), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L70-L70), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L71-L71), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L72-L72), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L73-L73), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L74-L74), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L75-L75), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L76-L76), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L77-L77), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L78-L78), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L79-L79), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L80-L80), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L81-L81), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L82-L82), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L83-L83), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L84-L84), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L85-L85), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L86-L86), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L87-L87), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L511-L511), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L518-L518), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L525-L525)


</details>


### [NC&#x2011;4] Contract does not follow the Solidity style guide's suggested layout ordering 
The [style guide](https://docs.soliditylang.org/en/v0.8.16/style-guide.html#order-of-layout) says that, within a contract, the ordering should be 1) Type declarations, 2) State variables, 3) Events, 4) Modifiers, and 5) Functions, but the contract(s) below do not follow this ordering


*There are 1 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

//@audit the modifier definition is misplaced
511:     modifier onlyAuthorized() {


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L511-L511) 


</details>


### [NC&#x2011;5] Custom error has no error details 
Consider adding parameters to the error to indicate which user or values caused the failure


*There are 14 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

70:     error InvalidToken();

71:     error NothingToClaim();

72:     error TokenNotAllowed();

73:     error CannotLockZero();

74:     error CannotWithdrawZero();

75:     error UseClaimInstead();

76:     error FailedToSendEther();

77:     error SwapCallFailed();

82:     error WrongExchange();

83:     error LoopNotActivated();

84:     error NotValidToken();

85:     error NotAuthorized();

86:     error CurrentlyNotPossible();

87:     error NoLongerPossible();


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L70-L70) , [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L71-L71), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L72-L72), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L73-L73), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L74-L74), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L75-L75), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L76-L76), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L77-L77), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L82-L82), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L83-L83), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L84-L84), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L85-L85), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L86-L86), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L87-L87)


</details>


### [NC&#x2011;6] Consider using `delete` rather than assigning `zero` to clear values 
The `delete` keyword more closely matches the semantics of what is being done, and draws more attention to the changing of state, which may lead to a more thorough audit of its associated logic


*There are 2 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

285:         balances[msg.sender][_token] = 0;

250:             balances[msg.sender][_token] = 0;


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L285-L285) , [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L250-L250)


</details>


### [NC&#x2011;7] Empty bytes check is missing 
When developing smart contracts in Solidity, it's crucial to validate the inputs of your functions. This includes ensuring that the bytes parameters are not empty, especially when they represent crucial data such as addresses, identifiers, or raw data that the contract needs to process.
Missing empty bytes checks can lead to unexpected behaviour in your contract.For instance, certain operations might fail, produce incorrect results, or consume unnecessary gas when performed with empty bytes.Moreover, missing input validation can potentially expose your contract to malicious activity, including exploitation of unhandled edge cases.
To mitigate these issues, always validate that bytes parameters are not empty when the logic of your contract requires it.


*There are 12 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

//@audit  ,_referral are not checked
124:     function lockETH(bytes32 _referral) external payable {
125:         _processLock(ETH, msg.value, msg.sender, _referral);

//@audit  ,_referral are not checked
133:     function lockETHFor(address _for, bytes32 _referral) external payable {
134:         _processLock(ETH, msg.value, _for, _referral);

//@audit  ,_referral are not checked
143:     function lock(address _token, uint256 _amount, bytes32 _referral) external {
144:         if (_token == ETH) {

//@audit  ,_referral are not checked
157:     function lockFor(address _token, uint256 _amount, address _for, bytes32 _referral) external {
158:         if (_token == ETH) {

//@audit  ,_referral are not checked
172:     function _processLock(address _token, uint256 _amount, address _receiver, bytes32 _referral)
173:         internal
174:         onlyBeforeDate(loopActivation)
175:     {

//@audit  ,_data are not checked
211:     function claim(address _token, uint8 _percentage, Exchange _exchange, bytes calldata _data)
212:         external
213:         onlyAfterDate(startClaimDate)
214:     {

//@audit  ,_data are not checked
226:     function claimAndStake(address _token, uint8 _percentage, Exchange _exchange, bytes calldata _data)
227:         external
228:         onlyAfterDate(startClaimDate)
229:     {

//@audit  ,_data are not checked
240:     function _claim(address _token, address _receiver, uint8 _percentage, Exchange _exchange, bytes calldata _data)
241:         internal
242:         returns (uint256 claimedAmount)
243:     {

//@audit  ,_data are not checked
405:     function _validateData(address _token, uint256 _amount, Exchange _exchange, bytes calldata _data) internal view {
406:         address inputToken;

//@audit  ,_data are not checked
448:     function _decodeUniswapV3Data(bytes calldata _data)
449:         internal
450:         pure
451:         returns (address inputToken, address outputToken, uint256 inputTokenAmount, address recipient, bytes4 selector)
452:     {

//@audit  ,_data are not checked
470:     function _decodeTransformERC20Data(bytes calldata _data)
471:         internal
472:         pure
473:         returns (address inputToken, address outputToken, uint256 inputTokenAmount, bytes4 selector)
474:     {

//@audit  ,_swapCallData are not checked
491:     function _fillQuote(IERC20 _sellToken, uint256 _amount, bytes calldata _swapCallData) internal {
492:         // Track our balance of the buyToken to determine how much we've bought.


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L124-L125) , [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L133-L134), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L143-L144), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L157-L158), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L172-L175), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L211-L214), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L226-L229), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L240-L243), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L405-L406), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L448-L452), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L470-L474), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L491-L492)


</details>


### [NC&#x2011;8] Events are missing sender information 
When an action is triggered based on a user's action, not being able to filter based on who triggered the action makes event processing a lot more cumbersome. Including the `msg.sender` the events of these types of action will make events much more useful to end users, especially when `msg.sender` is not `tx.origin`.


*There are 6 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

197:         emit Locked(_receiver, _amount, _token, _referral);

329:         emit Converted(totalBalance, totalLpETH);

339:         emit OwnerUpdated(_owner);

357:         emit LoopAddressesUpdated(_loopAddress, _vaultAddress);

385:         emit Recovered(tokenAddress, tokenAmount);

504:         emit SwappedTokens(address(_sellToken), _amount, boughtETHAmount);


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L197-L197) , [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L329-L329), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L339-L339), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L357-L357), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L385-L385), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L504-L504)


</details>


### [NC&#x2011;9] Events may be emitted out of order due to reentrancy 
Ensure that events follow the best practice of check-effects-interaction, and are emitted before external calls


*There are 4 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

234:         emit StakedVault(msg.sender, claimedAmount);

329:         emit Converted(totalBalance, totalLpETH);

385:         emit Recovered(tokenAddress, tokenAmount);

504:         emit SwappedTokens(address(_sellToken), _amount, boughtETHAmount);


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L234-L234) , [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L329-L329), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L385-L385), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L504-L504)


</details>


### [NC&#x2011;10] Defining All External/Public Functions in Contract Interfaces 
It is preferable to have all the external and public function in an interface to make using them easier by developers. This helps ensure the whole API is extracted in a interface.


*There are 12 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

124:     function lockETH(bytes32 _referral) external payable {
125:         _processLock(ETH, msg.value, msg.sender, _referral);

133:     function lockETHFor(address _for, bytes32 _referral) external payable {
134:         _processLock(ETH, msg.value, _for, _referral);

143:     function lock(address _token, uint256 _amount, bytes32 _referral) external {
144:         if (_token == ETH) {

157:     function lockFor(address _token, uint256 _amount, address _for, bytes32 _referral) external {
158:         if (_token == ETH) {

211:     function claim(address _token, uint8 _percentage, Exchange _exchange, bytes calldata _data)
212:         external
213:         onlyAfterDate(startClaimDate)
214:     {

226:     function claimAndStake(address _token, uint8 _percentage, Exchange _exchange, bytes calldata _data)
227:         external
228:         onlyAfterDate(startClaimDate)
229:     {

315:     function convertAllETH() external onlyAuthorized onlyBeforeDate(startClaimDate) {
316:         if (block.timestamp - loopActivation <= TIMELOCK) {

336:     function setOwner(address _owner) external onlyAuthorized {
337:         owner = _owner;

348:     function setLoopAddresses(address _loopAddress, address _vaultAddress)
349:         external
350:         onlyAuthorized
351:         onlyBeforeDate(loopActivation)
352:     {

364:     function allowToken(address _token) external onlyAuthorized {
365:         isTokenAllowed[_token] = true;

372:     function setEmergencyMode(bool _mode) external onlyAuthorized {
373:         emergencyMode = _mode;

379:     function recoverERC20(address tokenAddress, uint256 tokenAmount) external onlyAuthorized {
380:         if (tokenAddress == address(lpETH) || isTokenAllowed[tokenAddress]) {


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L124-L125) , [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L133-L134), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L143-L144), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L157-L158), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L211-L214), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L226-L229), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L315-L316), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L336-L337), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L348-L352), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L364-L365), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L372-L373), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L379-L380)


</details>


### [NC&#x2011;11] Function ordering does not follow the Solidity style guide 
According to the [Solidity style guide](https://docs.soliditylang.org/en/v0.8.17/style-guide.html#order-of-functions), functions should be laid out in the following order :`constructor()`, `receive()`, `fallback()`, `external`, `public`, `internal`, `private`, but the cases below do not follow this pattern


*There are 1 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

211:     function claim(address _token, uint8 _percentage, Exchange _exchange, bytes calldata _data)


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L211-L211) 


</details>


### [NC&#x2011;12] `address`s shouldn't be hard-coded 
It is often better to declare `address`es as `immutable`, and assign them via constructor arguments. This allows the code to remain the same across deployments on different networks, and avoids recompilation when addresses need to change.


*There are 1 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

//@audit hardcoded address : 0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE
28:     address public constant ETH = 0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE;


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L28-L28) 


</details>


### [NC&#x2011;13] Imports could be organized more systematically 
The contract used interfaces should be imported first, followed by all other files. The examples below do not follow this layout.


*There are 1 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

7: import {ILpETH, IERC20} from "./interfaces/ILpETH.sol";


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L7-L7) 


</details>


### [NC&#x2011;14] Import declarations should import specific identifiers, rather than the whole file 
Using import declarations of the form `import {<identifier_name>} from "some/ file.sol"` avoids polluting the symbol namespace making flattened files smaller, and speeds up compilation


*There are 2 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

4: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

5: import "@openzeppelin/contracts/utils/math/Math.sol";


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L4-L4) , [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L5-L5)


</details>


### [NC&#x2011;15] Inconsistent usage of `require`/`error` 
Some parts of the codebase use `require` statements, while others use custom `error`s. Consider refactoring the code to use the same approach: the following findings represent the minority of `require` vs `error`, and they show the first occurance in each file, for brevity.


*There are 1 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

495:         require(_sellToken.approve(exchangeProxy, _amount));


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L495-L495) 


</details>


### [NC&#x2011;16] Large numeric literals should use underscores for readability 



*There are 1 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

103:         startClaimDate = 4294967295; // Max uint32 ~ year 2107


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L103-L103) 


</details>


### [NC&#x2011;17] Long lines of code 
Usually lines in source code are limited to [80](https://softwareengineering.stackexchange.com/questions/148677/why-is-80-characters-the-standard-limit-for-code-width) characters. Today's screens are much larger so it's reasonable to stretch this in some cases. The solidity style guide recommends a maximumum line length of [120 characters](https://docs.soliditylang.org/en/v0.8.17/style-guide.html#maximum-line-length), so the lines below should be split when they reach that length.


*There are 3 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

417:             // UniswapV3Feature.sellTokenForEthToUniswapV3(encodedPath, sellAmount, minBuyAmount, recipient) requires `encodedPath` to be a Uniswap-encoded path, where the last token is WETH, and sends the NATIVE token to `recipient`

460:             encodedPathLength := calldataload(add(p, 96)) // Get length of encodedPath (obtained through abi.encodePacked)

461:             inputToken := shr(96, calldataload(add(p, 128))) // Shift to the Right with 24 zeroes (12 bytes = 96 bits) to get address


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L417-L417) , [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L460-L460), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L461-L461)


</details>


### [NC&#x2011;18] Missing event and or timelock for critical parameter change 
Events help non-contract tools to track changes, and events prevent users from being surprised by changes


*There are 1 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

372:     function setEmergencyMode(bool _mode) external onlyAuthorized {
373:         emergencyMode = _mode;
374:     }


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L372-L374) 


</details>


### [NC&#x2011;19] Consider using named mappings 
Consider using [named mappings](https://ethereum.stackexchange.com/a/145555) to make it easier to understand the purpose of each mapping


*There are 3 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

35:     mapping(address => bool) public isTokenAllowed;

50:     mapping(address => mapping(address => uint256)) public balances; // User -> Token -> Balance

50:     mapping(address => mapping(address => uint256)) public balances; // User -> Token -> Balance


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L35-L35) , [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L50-L50), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L50-L50)


</details>


### [NC&#x2011;20] Events that mark critical parameter changes should contain both the old and the new value 
This should especially be done if the new value is not required to be different from the old value


*There are 2 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

62:     event OwnerUpdated(address newOwner);

63:     event LoopAddressesUpdated(address loopAddress, address vaultAddress);


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L62-L62) , [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L63-L63)


</details>


### [NC&#x2011;21] `require()` / `revert()` statements should have descriptive reason strings 



*There are 1 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

495:         require(_sellToken.approve(exchangeProxy, _amount));


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L495-L495) 


</details>


### [NC&#x2011;22] Setters should prevent re-setting of the same value 
This especially problematic when the setter also emits the same value, which may be confusing to offline parsers


*There are 2 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

//@audit `owner` and `_owner` are never checked for the same value setting
336:     function setOwner(address _owner) external onlyAuthorized {
337:         owner = _owner;

//@audit `emergencyMode` and `_mode` are never checked for the same value setting
372:     function setEmergencyMode(bool _mode) external onlyAuthorized {
373:         emergencyMode = _mode;


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L336-L337) , [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L372-L373)


</details>


### [NC&#x2011;23] NatSpec `@return` argument is missing 



*There are 3 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

// @audit the @return is missing
 @dev Claim logic. If necessary converts token to ETH before depositing into lpETH contract.

// @audit the @return is missing
 @notice Decodes the data sent from 0x API when UniswapV3 is used
 @param _data      swap data from 0x API

// @audit the @return is missing
 @notice Decodes the data sent from 0x API when other exchanges are used via 0x TransformERC20 function
 @param _data      swap data from 0x API


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L240-L1) , [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L448-L1), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L470-L1)


</details>


### [NC&#x2011;24] Consider using `SafeTransferLib.safeTransferETH()` or `Address.sendValue()` for clearer semantic meaning 
These Functions indicate their purpose with their name more clearly than using low-level calls.


*There are 2 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

497:         (bool success,) = payable(exchangeProxy).call{value: 0}(_swapCallData);

296:             (bool sent,) = msg.sender.call{value: lockedAmount}("");


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L497-L497) , [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L296-L296)


</details>


### [NC&#x2011;25] Empty `receive()`/`payable fallback()` function does not authorize requests 
If the intention is for the Ether to be used, the function should call another function, otherwise it should revert (e.g. `require(msg.sender == address(weth))`). Having no access control on the function means that someone may send Ether to the contract, and have no way to get anything back out, which is a loss of funds. If the concern is having to spend a small amount of gas to check the sender against an immutable address, the code should at least have a function to rescue unused Ether.


*There are 1 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

392:     receive() external payable {}
393: 


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L392-L393) 


</details>


### [NC&#x2011;26] State variables should have `Natspec` comments 
Consider adding some `Natspec` comments on critical state variables to explain what they are supposed to do: this will help for future code reviews.


*There are 16 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

//@audit lpETH need comments
25:     ILpETH public lpETH;

//@audit lpETHVault need comments
26:     ILpETHVault public lpETHVault;

//@audit WETH need comments
27:     IWETH public immutable WETH;

//@audit ETH need comments
28:     address public constant ETH = 0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE;

//@audit exchangeProxy need comments
29:     address public immutable exchangeProxy;

//@audit owner need comments
31:     address public owner;

//@audit totalSupply need comments
33:     uint256 public totalSupply;

//@audit totalLpETH need comments
34:     uint256 public totalLpETH;

//@audit isTokenAllowed need comments
35:     mapping(address => bool) public isTokenAllowed;

//@audit UNI_SELECTOR need comments
42:     bytes4 public constant UNI_SELECTOR = 0x803ba26d;

//@audit TRANSFORM_SELECTOR need comments
43:     bytes4 public constant TRANSFORM_SELECTOR = 0x415565b0;

//@audit loopActivation need comments
45:     uint32 public loopActivation;

//@audit startClaimDate need comments
46:     uint32 public startClaimDate;

//@audit TIMELOCK need comments
47:     uint32 public constant TIMELOCK = 7 days;

//@audit emergencyMode need comments
48:     bool public emergencyMode;

//@audit balances need comments
50:     mapping(address => mapping(address => uint256)) public balances; // User -> Token -> Balance


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L25-L25) , [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L26-L26), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L27-L27), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L28-L28), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L29-L29), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L31-L31), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L33-L33), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L34-L34), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L35-L35), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L42-L42), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L43-L43), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L45-L45), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L46-L46), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L47-L47), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L48-L48), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L50-L50)


</details>


### [NC&#x2011;27] Contracts should have full test coverage 
While 100% code coverage does not guarantee that there are no bugs, it often will catch easy-to-find bugs, and will ensure that there are fewer regressions when the code invariably has to be modified. Furthermore, in order to get full coverage, code authors will often have to re-organize their code so that it is more modular, so that each component can be tested separately, which reduces interdependencies between modules and layers, and makes for code that is easier to reason about and audit.


*There are 1 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

@audit Multiple files
1: 


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L1-L1) 


</details>


### [NC&#x2011;28] Top level pragma declarations should be separated by two blank lines 



*There are 1 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

2: pragma solidity 0.8.20;
3: 
4: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L2-L4) 


</details>


### [NC&#x2011;29] Critical functions should be a two step procedure 
Critical functions in Solidity contracts should follow a two-step procedure to enhance security, minimize human error, and ensure proper access control. By dividing sensitive operations into distinct phases, such as initiation and confirmation, developers can introduce a safeguard against unintended actions or unauthorized access.


*There are 4 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

336:     function setOwner(address _owner) external onlyAuthorized {

348:     function setLoopAddresses(address _loopAddress, address _vaultAddress)

348:     function setLoopAddresses(address _loopAddress, address _vaultAddress)

372:     function setEmergencyMode(bool _mode) external onlyAuthorized {


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L336-L336) , [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L348-L348), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L348-L348), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L372-L372)


</details>


### [NC&#x2011;30] Event is missing `indexed` fields 
Index event fields make the field more quickly accessible to off-chain tools that parse events. However, note that each index field costs extra gas during emission, so it's not necessarily best to index the maximum allowed per event (three fields). Each event should use three indexed fields if there are three or more fields, and gas usage is not particularly of concern for the events in question. If there are fewer than three fields, all of the fields should be indexed.


*There are 9 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

56:     event Locked(address indexed user, uint256 amount, address token, bytes32 indexed referral);

57:     event StakedVault(address indexed user, uint256 amount);

58:     event Converted(uint256 amountETH, uint256 amountlpETH);

59:     event Withdrawn(address indexed user, address token, uint256 amount);

60:     event Claimed(address indexed user, address token, uint256 reward);

61:     event Recovered(address token, uint256 amount);

62:     event OwnerUpdated(address newOwner);

63:     event LoopAddressesUpdated(address loopAddress, address vaultAddress);

64:     event SwappedTokens(address sellToken, uint256 sellAmount, uint256 buyETHAmount);


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L56-L56) , [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L57-L57), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L58-L58), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L59-L59), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L60-L60), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L61-L61), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L62-L62), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L63-L63), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L64-L64)


</details>


### [NC&#x2011;31] Missing upgradability functionality 
At the begining of a project, there is always the need to modify of add something to the source code especialy if any vulnerability is discovered. Therefore, having such system is crusial at least at the first stages of the project


*There are 1 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

16: contract PrelaunchPoints {


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L16-L16) 


</details>


### [NC&#x2011;32] Constants should be defined rather than using magic numbers 
Even [assembly](https://github.com/code-423n4/2022-05-opensea-seaport/blob/9d7ce4d08bf3c3010304a0476a785c70c0e90ae7/contracts/lib/TokenTransferrer.sol#L35-L39) can benefit from using readable constants instead of hex/numeric literals


*There are 3 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

//@audit Try to make a `constant` with `4294967295` value
103:         startClaimDate = 4294967295; // Max uint32 ~ year 2107

//@audit Try to make a `constant` with `120` value
102:         loopActivation = uint32(block.timestamp + 120 days);

//@audit Try to make a `constant` with `100` value
253:             uint256 userClaim = userStake * _percentage / 100;


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L103-L103) , [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L102-L102), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L253-L253)


</details>


### [NC&#x2011;33] Use a single file for system wide constants 
Consider grouping all the system constants under a single file. This finding shows only the first constant for each file.


*There are 1 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

28:     address public constant ETH = 0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE;


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L28-L28) 


</details>


### [NC&#x2011;34] Consider using SMTChecker 
The SMTChecker is a valuable tool for Solidity developers as it helps detect potential vulnerabilities and logical errors in the contract's code. By utilizing Satisfiability Modulo Theories (SMT) solvers, it can reason about the potential states a contract can be in, and therefore, identify conditions that could lead to undesirable behavior. This automatic formal verification can catch issues that might otherwise be missed in manual code reviews or standard testing, enhancing the overall contract's security and reliability.


*There are 1 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

2: pragma solidity 0.8.20;


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L2-L2) 


</details>


### [NC&#x2011;35] Complex function controle flow 
Due to multiple if, loop and conditions the following functions has a very complex controle flow that could make auditing very difficult to cover all possible path\nTherefore, consider breaking down these blocks into more manageable units, by splitting things into utility functions, by reducing nesting, and by using early returns


*There are 2 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

274:     function withdraw(address _token) external {
275:         if (!emergencyMode) {
276:             if (block.timestamp <= loopActivation) {
277:                 revert CurrentlyNotPossible();
278:             }
279:             if (block.timestamp >= startClaimDate) {
280:                 revert NoLongerPossible();
281:             }
282:         }
283: 
284:         uint256 lockedAmount = balances[msg.sender][_token];
285:         balances[msg.sender][_token] = 0;
286: 
287:         if (lockedAmount == 0) {
288:             revert CannotWithdrawZero();
289:         }
290:         if (_token == ETH) {
291:             if (block.timestamp >= startClaimDate){
292:                 revert UseClaimInstead();
293:             }
294:             totalSupply = totalSupply - lockedAmount;
295: 
296:             (bool sent,) = msg.sender.call{value: lockedAmount}("");
297: 
298:             if (!sent) {
299:                 revert FailedToSendEther();
300:             }
301:         } else {
302:             IERC20(_token).safeTransfer(msg.sender, lockedAmount);
303:         }
304: 
305:         emit Withdrawn(msg.sender, _token, lockedAmount);
306:     }

405:     function _validateData(address _token, uint256 _amount, Exchange _exchange, bytes calldata _data) internal view {
406:         address inputToken;
407:         address outputToken;
408:         uint256 inputTokenAmount;
409:         address recipient;
410:         bytes4 selector;
411: 
412:         if (_exchange == Exchange.UniswapV3) {
413:             (inputToken, outputToken, inputTokenAmount, recipient, selector) = _decodeUniswapV3Data(_data);
414:             if (selector != UNI_SELECTOR) {
415:                 revert WrongSelector(selector);
416:             }
417:             // UniswapV3Feature.sellTokenForEthToUniswapV3(encodedPath, sellAmount, minBuyAmount, recipient) requires `encodedPath` to be a Uniswap-encoded path, where the last token is WETH, and sends the NATIVE token to `recipient`
418:             if (outputToken != address(WETH)) {
419:                 revert WrongDataTokens(inputToken, outputToken);
420:             }
421:         } else if (_exchange == Exchange.TransformERC20) {
422:             (inputToken, outputToken, inputTokenAmount, selector) = _decodeTransformERC20Data(_data);
423:             if (selector != TRANSFORM_SELECTOR) {
424:                 revert WrongSelector(selector);
425:             }
426:             if (outputToken != ETH) {
427:                 revert WrongDataTokens(inputToken, outputToken);
428:             }
429:         } else {
430:             revert WrongExchange();
431:         }
432: 
433:         if (inputToken != _token) {
434:             revert WrongDataTokens(inputToken, outputToken);
435:         }
436:         if (inputTokenAmount != _amount) {
437:             revert WrongDataAmount(inputTokenAmount);
438:         }
439:         if (recipient != address(this) && recipient != address(0)) {
440:             revert WrongRecipient(recipient);
441:         }
442:     }


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L274-L306) , [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L405-L442)


</details>


### [NC&#x2011;36] Consider bounding input array length 
The functions below take in an unbounded array, and make function calls for entries in the array. While the function will revert if it eventually runs out of gas, it may be a nicer user experience to `require()` that the length of the array is below some reasonable maximum, so that the user doesn't have to use up a full transaction's gas only to see that the transaction reverts.


*There are 1 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

//@audit array name _allowedTokens
097:     constructor(address _exchangeProxy, address _wethAddress, address[] memory _allowedTokens) {
098:         owner = msg.sender;
099:         exchangeProxy = _exchangeProxy;
100:         WETH = IWETH(_wethAddress);
101: 
102:         loopActivation = uint32(block.timestamp + 120 days);
103:         startClaimDate = 4294967295; // Max uint32 ~ year 2107
104: 
105:         // Allow intital list of tokens
106:         uint256 length = _allowedTokens.length;
107:         for (uint256 i = 0; i < length;) {
108:             isTokenAllowed[_allowedTokens[i]] = true;
109:             unchecked {
110:                 i++;
111:             }
112:         }
113:         isTokenAllowed[_wethAddress] = true;
114:     }


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L97-L114) 


</details>


### [NC&#x2011;37] [NatSpec] Natspec `@dev` is missing from `contract` 



*There are 41 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

11: /**
12:  * @title   PrelaunchPoints
13:  * @author  Loop
14:  * @notice  Staking points contract for the prelaunch of Loop Protocol.
15:  */

56:     event Locked(address indexed user, uint256 amount, address token, bytes32 indexed referral);

57:     event StakedVault(address indexed user, uint256 amount);

58:     event Converted(uint256 amountETH, uint256 amountlpETH);

59:     event Withdrawn(address indexed user, address token, uint256 amount);

60:     event Claimed(address indexed user, address token, uint256 reward);

61:     event Recovered(address token, uint256 amount);

62:     event OwnerUpdated(address newOwner);

63:     event LoopAddressesUpdated(address loopAddress, address vaultAddress);

64:     event SwappedTokens(address sellToken, uint256 sellAmount, uint256 buyETHAmount);

70:     error InvalidToken();

71:     error NothingToClaim();

72:     error TokenNotAllowed();

73:     error CannotLockZero();

74:     error CannotWithdrawZero();

75:     error UseClaimInstead();

76:     error FailedToSendEther();

77:     error SwapCallFailed();

78:     error WrongSelector(bytes4 selector);

79:     error WrongDataTokens(address inputToken, address outputToken);

80:     error WrongDataAmount(uint256 inputTokenAmount);

81:     error WrongRecipient(address recipient);

82:     error WrongExchange();

83:     error LoopNotActivated();

84:     error NotValidToken();

85:     error NotAuthorized();

86:     error CurrentlyNotPossible();

87:     error NoLongerPossible();

92:     /**
93:      * @param _exchangeProxy address of the 0x protocol exchange proxy
94:      * @param _wethAddress   address of WETH
95:      * @param _allowedTokens list of token addresses to allow for locking
96:      */

120:     /**
121:      * @notice Locks ETH
122:      * @param _referral  info of the referral. This value will be processed in the backend.
123:      */

128:     /**
129:      * @notice Locks ETH for a given address
130:      * @param _for       address for which ETH is locked
131:      * @param _referral  info of the referral. This value will be processed in the backend.
132:      */

137:     /**
138:      * @notice Locks a valid token
139:      * @param _token     address of token to lock
140:      * @param _amount    amount of token to lock
141:      * @param _referral  info of the referral. This value will be processed in the backend.
142:      */

150:     /**
151:      * @notice Locks a valid token for a given address
152:      * @param _token     address of token to lock
153:      * @param _amount    amount of token to lock
154:      * @param _for       address for which ETH is locked
155:      * @param _referral  info of the referral. This value will be processed in the backend.
156:      */

332:     /**
333:      * @notice Sets a new owner
334:      * @param _owner address of the new owner
335:      */

398:     /**
399:      * @notice Validates the data sent from 0x API to match desired behaviour
400:      * @param _token     address of the token to sell
401:      * @param _amount    amount of token to sell
402:      * @param _exchange  exchange identifier where the swap takes place
403:      * @param _data      swap data from 0x API
404:      */

444:     /**
445:      * @notice Decodes the data sent from 0x API when UniswapV3 is used
446:      * @param _data      swap data from 0x API
447:      */

466:     /**
467:      * @notice Decodes the data sent from 0x API when other exchanges are used via 0x TransformERC20 function
468:      * @param _data      swap data from 0x API
469:      */

484:     /**
485:      *
486:      * @param _sellToken     The `sellTokenAddress` field from the API response.
487:      * @param _amount       The `sellAmount` field from the API response.
488:      * @param _swapCallData  The `data` field from the API response.
489:      */

511:     modifier onlyAuthorized() {

518:     modifier onlyAfterDate(uint256 limitDate) {

525:     modifier onlyBeforeDate(uint256 limitDate) {


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L11-L15) , [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L56-L56), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L57-L57), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L58-L58), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L59-L59), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L60-L60), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L61-L61), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L62-L62), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L63-L63), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L64-L64), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L70-L70), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L71-L71), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L72-L72), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L73-L73), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L74-L74), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L75-L75), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L76-L76), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L77-L77), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L78-L78), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L79-L79), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L80-L80), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L81-L81), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L82-L82), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L83-L83), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L84-L84), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L85-L85), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L86-L86), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L87-L87), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L92-L96), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L120-L123), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L128-L132), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L137-L142), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L150-L156), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L332-L335), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L398-L404), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L444-L447), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L466-L469), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L484-L489), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L511-L511), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L518-L518), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L525-L525)


</details>


### [NC&#x2011;38] Contract should expose an `interface` 
The `contract`s should expose an `interface` so that other projects can more easily integrate with it, without having to develop their own non-standard variants.


*There are 12 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

124:     function lockETH(bytes32 _referral) external payable {

133:     function lockETHFor(address _for, bytes32 _referral) external payable {

143:     function lock(address _token, uint256 _amount, bytes32 _referral) external {

157:     function lockFor(address _token, uint256 _amount, address _for, bytes32 _referral) external {

211:     function claim(address _token, uint8 _percentage, Exchange _exchange, bytes calldata _data)

226:     function claimAndStake(address _token, uint8 _percentage, Exchange _exchange, bytes calldata _data)

315:     function convertAllETH() external onlyAuthorized onlyBeforeDate(startClaimDate) {

336:     function setOwner(address _owner) external onlyAuthorized {

348:     function setLoopAddresses(address _loopAddress, address _vaultAddress)

364:     function allowToken(address _token) external onlyAuthorized {

372:     function setEmergencyMode(bool _mode) external onlyAuthorized {

379:     function recoverERC20(address tokenAddress, uint256 tokenAmount) external onlyAuthorized {


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L124-L124) , [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L133-L133), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L143-L143), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L157-L157), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L211-L211), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L226-L226), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L315-L315), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L336-L336), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L348-L348), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L364-L364), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L372-L372), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L379-L379)


</details>


### [NC&#x2011;39] [NatSpec] Natspec `@notice` is missing from `contract` 



*There are 42 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

56:     event Locked(address indexed user, uint256 amount, address token, bytes32 indexed referral);

57:     event StakedVault(address indexed user, uint256 amount);

58:     event Converted(uint256 amountETH, uint256 amountlpETH);

59:     event Withdrawn(address indexed user, address token, uint256 amount);

60:     event Claimed(address indexed user, address token, uint256 reward);

61:     event Recovered(address token, uint256 amount);

62:     event OwnerUpdated(address newOwner);

63:     event LoopAddressesUpdated(address loopAddress, address vaultAddress);

64:     event SwappedTokens(address sellToken, uint256 sellAmount, uint256 buyETHAmount);

70:     error InvalidToken();

71:     error NothingToClaim();

72:     error TokenNotAllowed();

73:     error CannotLockZero();

74:     error CannotWithdrawZero();

75:     error UseClaimInstead();

76:     error FailedToSendEther();

77:     error SwapCallFailed();

78:     error WrongSelector(bytes4 selector);

79:     error WrongDataTokens(address inputToken, address outputToken);

80:     error WrongDataAmount(uint256 inputTokenAmount);

81:     error WrongRecipient(address recipient);

82:     error WrongExchange();

83:     error LoopNotActivated();

84:     error NotValidToken();

85:     error NotAuthorized();

86:     error CurrentlyNotPossible();

87:     error NoLongerPossible();

92:     /**
93:      * @param _exchangeProxy address of the 0x protocol exchange proxy
94:      * @param _wethAddress   address of WETH
95:      * @param _allowedTokens list of token addresses to allow for locking
96:      */

164:     /**
165:      * @dev Generic internal locking function that updates rewards based on
166:      *      previous balances, then update balances.
167:      * @param _token       Address of the token to lock
168:      * @param _amount      Units of ETH or token to add to the users balance
169:      * @param _receiver    Address of user who will receive the stake
170:      * @param _referral    Address of the referral user
171:      */

204:     /**
205:      * @dev Called by a user to get their vested lpETH
206:      * @param _token      Address of the token to convert to lpETH
207:      * @param _percentage Proportion in % of tokens to withdraw. NOT useful for ETH
208:      * @param _exchange   Exchange identifier where the swap takes place
209:      * @param _data       Swap data obtained from 0x API
210:      */

218:     /**
219:      * @dev Called by a user to get their vested lpETH and stake them in a
220:      *      Loop vault for extra rewards
221:      * @param _token      Address of the token to convert to lpETH
222:      * @param _percentage Proportion in % of tokens to withdraw. NOT useful for ETH
223:      * @param _exchange   Exchange identifier where the swap takes place
224:      * @param _data       Swap data obtained from 0x API
225:      */

237:     /**
238:      * @dev Claim logic. If necessary converts token to ETH before depositing into lpETH contract.
239:      */

268:     /**
269:      * @dev Called by a staker to withdraw all their ETH or LRT
270:      * Note Can only be called after the loop address is set and before claiming lpETH,
271:      * i.e. for at least TIMELOCK. In emergency mode can be called at any time.
272:      * @param _token      Address of the token to withdraw
273:      */

312:     /**
313:      * @dev Called by a owner to convert all the locked ETH to get lpETH
314:      */

360:     /**
361:      * @param _token address of a wrapped LRT token
362:      * @dev ONLY add wrapped LRT tokens. Contract not compatible with rebase tokens.
363:      */

368:     /**
369:      * @param _mode boolean to activate/deactivate the emergency mode
370:      * @dev On emergency mode all withdrawals are accepted at
371:      */

376:     /**
377:      * @dev Allows the owner to recover other ERC20s mistakingly sent to this contract
378:      */

388:     /**
389:      * Enable receive ETH
390:      * @dev ETH sent to this contract directly will be locked forever.
391:      */

484:     /**
485:      *
486:      * @param _sellToken     The `sellTokenAddress` field from the API response.
487:      * @param _amount       The `sellAmount` field from the API response.
488:      * @param _swapCallData  The `data` field from the API response.
489:      */

511:     modifier onlyAuthorized() {

518:     modifier onlyAfterDate(uint256 limitDate) {

525:     modifier onlyBeforeDate(uint256 limitDate) {


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L56-L56) , [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L57-L57), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L58-L58), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L59-L59), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L60-L60), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L61-L61), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L62-L62), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L63-L63), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L64-L64), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L70-L70), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L71-L71), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L72-L72), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L73-L73), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L74-L74), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L75-L75), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L76-L76), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L77-L77), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L78-L78), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L79-L79), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L80-L80), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L81-L81), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L82-L82), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L83-L83), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L84-L84), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L85-L85), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L86-L86), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L87-L87), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L92-L96), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L164-L171), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L204-L210), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L218-L225), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L237-L239), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L268-L273), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L312-L314), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L360-L363), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L368-L371), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L376-L378), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L388-L391), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L484-L489), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L511-L511), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L518-L518), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L525-L525)


</details>


### [NC&#x2011;40] Contract uses both `require()`/`revert()` as well as custom errors 
Consider using just one method in a single file. The below instances represents the less used technique


*There are 1 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

495:         require(_sellToken.approve(exchangeProxy, _amount));


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L495-L495) 


</details>


### [NC&#x2011;41] `immutable` variable names don\'t follow the Solidity style guide 
For `immutable` variable names, each word should use all capital letters, with underscores separating each word (CONSTANT_CASE)


*There are 1 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

29:     address public immutable exchangeProxy;


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L29-L29) 


</details>


### [NC&#x2011;42] Use the latest Solidity version for better security 
Using the latest solidity version will help avoid old compiler related vulnerabilities


*There are 1 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

2: pragma solidity 0.8.20;


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L2-L2) 


</details>


### [NC&#x2011;43] Consider adding formal verification proofs 
Consider using formal verification to mathematically prove that your code does what is intended, and does not have any edge cases with unexpected behavior. The solidity compiler itself has this functionality [built in](https://docs.soliditylang.org/en/latest/smtchecker.html#smtchecker-and-formal-verification)


*There are 1 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

@audit Should implement invariant tests
1: 


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L1-L1) 


</details>


### [NC&#x2011;44] Missing zero address check in functions with address parameters 
Adding a zero address check for each address type parameter can prevent errors.


*There are 2 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

//@audit _exchangeProxy, _allowedTokens,  are not checked
97:     constructor(address _exchangeProxy, address _wethAddress, address[] memory _allowedTokens) {

//@audit _token,  are not checked
364:     function allowToken(address _token) external onlyAuthorized {


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L97-L97) , [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L364-L364)


</details>


### [NC&#x2011;45] Use a struct to encapsulate multiple function parameters 
If a function has too many parameters, replacing them with a struct can improve code readability and maintainability, increase reusability, and reduce the likelihood of errors when passing the parameters.


*There are 1 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

240:     function _claim(address _token, address _receiver, uint8 _percentage, Exchange _exchange, bytes calldata _data)
241:         internal
242:         returns (uint256 claimedAmount)
243:     {
244:         uint256 userStake = balances[msg.sender][_token];
245:         if (userStake == 0) {
246:             revert NothingToClaim();
247:         }
248:         if (_token == ETH) {
249:             claimedAmount = userStake.mulDiv(totalLpETH, totalSupply);
250:             balances[msg.sender][_token] = 0;
251:             lpETH.safeTransfer(_receiver, claimedAmount);
252:         } else {
253:             uint256 userClaim = userStake * _percentage / 100;
254:             _validateData(_token, userClaim, _exchange, _data);
255:             balances[msg.sender][_token] = userStake - userClaim;
256: 
257:             // At this point there should not be any ETH in the contract
258:             // Swap token to ETH
259:             _fillQuote(IERC20(_token), userClaim, _data);
260: 
261:             // Convert swapped ETH to lpETH (1 to 1 conversion)
262:             claimedAmount = address(this).balance;
263:             lpETH.deposit{value: claimedAmount}(_receiver);
264:         }
265:         emit Claimed(msg.sender, _token, claimedAmount);
266:     }


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L240-L266) 


</details>


### [NC&#x2011;46] Multiple mappings with same keys can be combined into a single struct mapping for readability 
Well-organized data structures make code reviews easier, which may lead to fewer bugs. Consider combining related mappings into mappings to structs, so it's clear what data is related.


*There are 2 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

35:     mapping(address => bool) public isTokenAllowed;

50:     mapping(address => mapping(address => uint256)) public balances; // User -> Token -> Balance


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L35-L35) , [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L50-L50)


</details>


### [NC&#x2011;47] constructor should emit an event 
Use events to signal significant changes to off-chain monitoring tools.


*There are 1 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

097:     constructor(address _exchangeProxy, address _wethAddress, address[] memory _allowedTokens) {
098:         owner = msg.sender;
099:         exchangeProxy = _exchangeProxy;
100:         WETH = IWETH(_wethAddress);
101: 
102:         loopActivation = uint32(block.timestamp + 120 days);
103:         startClaimDate = 4294967295; // Max uint32 ~ year 2107
104: 
105:         // Allow intital list of tokens
106:         uint256 length = _allowedTokens.length;
107:         for (uint256 i = 0; i < length;) {
108:             isTokenAllowed[_allowedTokens[i]] = true;
109:             unchecked {
110:                 i++;
111:             }
112:         }
113:         isTokenAllowed[_wethAddress] = true;
114:     }


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L97-L114) 


</details>


### [NC&#x2011;48] Complex functions should include comments 
Large and/or complex functions should include comments to make them easier to understand and reduce margin for error.


*There are 2 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

172:     function _processLock(address _token, uint256 _amount, address _receiver, bytes32 _referral)

274:     function withdraw(address _token) external {


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L172-L172) , [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L274-L274)


</details>


### [NC&#x2011;49] Make use of Solidiy's `using` keyword 
The directive `using A for B` can be used to attach functions (`A`) as operators to user-defined value types or as member functions to any type (`B`). The member functions receive the object they are called on as their first parameter (like the `self` variable in Python). The operator functions receive operands as parameters.  Doing so improves readability, makes debugging easier, and promotes modularity and reusability in the code.


*There are 1 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

189:                 WETH.withdraw(_amount);


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L189-L189) 


</details>


### [NC&#x2011;50] [Solidity]: All `verbatim` blocks are considered identical by deduplicator and can incorrectly be unified 
The block deduplicator is a step of the opcode-based optimizer which identifies equivalent assembly blocks and merges them into a single one. However, when blocks contained `verbatim`, their comparison was performed incorrectly, leading to the collapse of assembly blocks which are identical except for the contents of the ``verbatim`` items. Since `verbatim` is only available in Yul, compilation of Solidity sources is not affected. For more details check the following [link](https://blog.soliditylang.org/2023/11/08/verbatim-invalid-deduplication-bug/)


*There are 1 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

2: pragma solidity 0.8.20;


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L2-L2) 


</details>


### [NC&#x2011;51] [NatSpec] Natspec `@param` is missing from `error` 



*There are 17 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

56:     event Locked(address indexed user, uint256 amount, address token, bytes32 indexed referral);

57:     event StakedVault(address indexed user, uint256 amount);

58:     event Converted(uint256 amountETH, uint256 amountlpETH);

59:     event Withdrawn(address indexed user, address token, uint256 amount);

60:     event Claimed(address indexed user, address token, uint256 reward);

61:     event Recovered(address token, uint256 amount);

62:     event OwnerUpdated(address newOwner);

63:     event LoopAddressesUpdated(address loopAddress, address vaultAddress);

64:     event SwappedTokens(address sellToken, uint256 sellAmount, uint256 buyETHAmount);

78:     error WrongSelector(bytes4 selector);

79:     error WrongDataTokens(address inputToken, address outputToken);

80:     error WrongDataAmount(uint256 inputTokenAmount);

81:     error WrongRecipient(address recipient);

237:     /**
238:      * @dev Claim logic. If necessary converts token to ETH before depositing into lpETH contract.
239:      */

376:     /**
377:      * @dev Allows the owner to recover other ERC20s mistakingly sent to this contract
378:      */

518:     modifier onlyAfterDate(uint256 limitDate) {

525:     modifier onlyBeforeDate(uint256 limitDate) {


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L56-L56) , [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L57-L57), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L58-L58), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L59-L59), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L60-L60), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L61-L61), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L62-L62), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L63-L63), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L64-L64), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L78-L78), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L79-L79), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L80-L80), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L81-L81), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L237-L239), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L376-L378), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L518-L518), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L525-L525)


</details>


### [NC&#x2011;52] Not using the latest version of `prb-math` from dependencies 
`prb-math` is an important mathematical library The package.json configuration file says that the project is using an old version of `prb-math`.


*There are 1 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: package.json

//@audit `prb-math` version is 
1: 


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//package.json#L1-L1) 


</details>


### [NC&#x2011;53] Enforcing Lowercase and Underscores Only in the `name` Field of package.json 
package.json name variable should only consist of lowercase letters and underscores


*There are 1 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: package.json

//@audit package.json name is loop-prelaunch-contracts
1: {


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//package.json#L1-L1) 


</details>


### [NC&#x2011;54] Using Low-Level Call for Transfers 
Utilizing low-level calls like `.call{value: value}` for Ether transfers in Ethereum can be risky, as it can inadvertently allow malicious contract executions through fallback functions. To mitigate these risks and ensure safer Ether transfers, it is recommended to adopt more secure and explicit methods provided by reputable libraries such as OpenZeppelin. Functions like `Address.sendValue()` from OpenZeppelin provide a clearer and safer alternative for sending Ether, as they encapsulate necessary checks and error handling, ensuring that Ether is transferred securely and any errors are appropriately dealt with. This not only enhances the security of your smart contract but also improves code readability and maintainability, aligning with modern Solidity development practices.


*There are 2 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

497:         (bool success,) = payable(exchangeProxy).call{value: 0}(_swapCallData);

296:             (bool sent,) = msg.sender.call{value: lockedAmount}("");


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L497-L497) , [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L296-L296)


</details>


### [NC&#x2011;55] Off-by-one timestamp error 
In Solidity, using `>=` or `<=` to compare against `block.timestamp` (alias `now`) may introduce off-by-one errors due to the fact that `block.timestamp` is only updated once per block and its value remains constant throughout the block's execution. If an operation happens at the exact second when `block.timestamp` changes, it could result in unexpected behavior. To avoid this, it's safer to use strict inequality operators (`>` or `<`). For instance, if a condition should only be met after a certain time, use `block.timestamp > time` rather than `block.timestamp >= time`. This way, potential off-by-one errors due to the exact timing of block mining are mitigated, leading to safer, more predictable contract behavior.


*There are 5 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

519:         if (block.timestamp <= limitDate) {

526:         if (block.timestamp >= limitDate) {

276:             if (block.timestamp <= loopActivation) {

279:             if (block.timestamp >= startClaimDate) {

291:             if (block.timestamp >= startClaimDate){


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L519-L519) , [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L526-L526), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L276-L276), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L279-L279), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L291-L291)


</details>


### [NC&#x2011;56] Incorrect withdraw declaration 
In Solidity, it's essential for clarity and interoperability to correctly specify return types in function declarations. If the `withdraw` function is expected to return a `bool` to indicate success or failure, its omission could lead to ambiguity or unexpected behavior when interacting with or calling this function from other contracts or off-chain systems. Missing return values can mislead developers and potentially lead to contract integrations built on incorrect assumptions. To resolve this, the function declaration for `withdraw` should be modified to explicitly include the `bool` return type, ensuring clarity and correctness in contract interactions.


*There are 1 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

274:     function withdraw(address _token) external {
275:         if (!emergencyMode) {
276:             if (block.timestamp <= loopActivation) {
277:                 revert CurrentlyNotPossible();
278:             }
279:             if (block.timestamp >= startClaimDate) {
280:                 revert NoLongerPossible();
281:             }
282:         }
283: 
284:         uint256 lockedAmount = balances[msg.sender][_token];
285:         balances[msg.sender][_token] = 0;
286: 
287:         if (lockedAmount == 0) {
288:             revert CannotWithdrawZero();
289:         }
290:         if (_token == ETH) {
291:             if (block.timestamp >= startClaimDate){
292:                 revert UseClaimInstead();
293:             }
294:             totalSupply = totalSupply - lockedAmount;
295: 
296:             (bool sent,) = msg.sender.call{value: lockedAmount}("");
297: 
298:             if (!sent) {
299:                 revert FailedToSendEther();
300:             }
301:         } else {
302:             IERC20(_token).safeTransfer(msg.sender, lockedAmount);
303:         }
304: 
305:         emit Withdrawn(msg.sender, _token, lockedAmount);
306:     }


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L274-L306) 


</details>


### [NC&#x2011;57] A event should be emitted if a non immutable state variable is set in a constructor 



*There are 5 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

98:         owner = msg.sender;

99:         exchangeProxy = _exchangeProxy;

100:         WETH = IWETH(_wethAddress);

102:         loopActivation = uint32(block.timestamp + 120 days);

103:         startClaimDate = 4294967295; // Max uint32 ~ year 2107


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L98-L98) , [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L99-L99), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L100-L100), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L102-L102), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L103-L103)


</details>


### [NC&#x2011;58] Returning a struct instead of returning many variables is better 
Returning a struct from a Solidity function instead of multiple variables offers several benefits, enhancing code clarity and efficiency. Structs allow for the grouping of related data into a single entity, making the function's return values more organized and easier to manage. This approach significantly improves readability, as it encapsulates the data logically, helping developers quickly understand the returned information's structure. Additionally, it simplifies function interfaces, reducing the potential for errors when handling multiple return values. By using structs, you can also easily extend or modify the returned data without altering the function signature, facilitating smoother updates and maintenance of your smart contract code.


*There are 1 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

448:     function _decodeUniswapV3Data(bytes calldata _data)
449:         internal
450:         pure
451:         returns (address inputToken, address outputToken, uint256 inputTokenAmount, address recipient, bytes4 selector)
452:     {
453:         uint256 encodedPathLength;
454:         assembly {
455:             let p := _data.offset
456:             selector := calldataload(p)
457:             p := add(p, 36) // Data: selector 4 + lenght data 32
458:             inputTokenAmount := calldataload(p)
459:             recipient := calldataload(add(p, 64))
460:             encodedPathLength := calldataload(add(p, 96)) // Get length of encodedPath (obtained through abi.encodePacked)
461:             inputToken := shr(96, calldataload(add(p, 128))) // Shift to the Right with 24 zeroes (12 bytes = 96 bits) to get address
462:             outputToken := shr(96, calldataload(add(p, add(encodedPathLength, 108)))) // Get last address of the hop
463:         }
464:     }


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L448-L464) 


</details>


### [NC&#x2011;59] [Solidity]: Order of Argument Evaluation Disrupted in Non-Expression-Split Code by Optimizer Sequences with FullInliner 
Function call arguments in Yul are evaluated right to left. This order matters when the argument expressions have side-effects, and changing it may change contract behavior. FullInliner is an optimizer step that can replace a function call with the body of that function. The transformation involves assigning argument expressions to temporary variables, which imposes an explicit evaluation order. FullInliner was written with the assumption that this order does not necessarily have to match usual argument evaluation order because the argument expressions have no side-effects. In most circumstances this assumption is true because the default optimization step sequence contains the ExpressionSplitter step. ExpressionSplitter ensures that the code is in *expression-split form*, which means that function calls cannot appear nested inside expressions, and all function call arguments have to be variables. The assumption is, however, not guaranteed to be true in general. Version 0.6.7 introduced a setting allowing users to specify an arbitrary optimization step sequence, making it possible for the FullInliner to actually encounter argument expressions with side-effects, which can result in behavior differences between optimized and unoptimized bytecode. Contracts compiled without optimization or with the default optimization sequence are not affected. To trigger the bug the user has to explicitly choose compiler settings that contain a sequence with FullInliner step not preceded by ExpressionSplitter. [Ref](https://blog.soliditylang.org/2023/07/19/full-inliner-non-expression-split-argument-evaluation-order-bug/)


*There are 1 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

2: pragma solidity 0.8.20;


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L2-L2) 


</details>


### [NC&#x2011;60] Consider adding validation of user inputs 



*There are 10 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

//@audit these parameters `_for`, are not checked
133:     function lockETHFor(address _for, bytes32 _referral) external payable {

//@audit these parameters `_for`, are not checked
157:     function lockFor(address _token, uint256 _amount, address _for, bytes32 _referral) external {

//@audit these parameters `_receiver`, are not checked
172:     function _processLock(address _token, uint256 _amount, address _receiver, bytes32 _referral)

//@audit these parameters `_token`, are not checked
211:     function claim(address _token, uint8 _percentage, Exchange _exchange, bytes calldata _data)

//@audit these parameters `_token`, are not checked
226:     function claimAndStake(address _token, uint8 _percentage, Exchange _exchange, bytes calldata _data)

//@audit these parameters `_receiver`, are not checked
240:     function _claim(address _token, address _receiver, uint8 _percentage, Exchange _exchange, bytes calldata _data)

//@audit these parameters `_owner`, are not checked
336:     function setOwner(address _owner) external onlyAuthorized {

//@audit these parameters `_loopAddress`,`_vaultAddress`, are not checked
348:     function setLoopAddresses(address _loopAddress, address _vaultAddress)

//@audit these parameters `_token`, are not checked
364:     function allowToken(address _token) external onlyAuthorized {

//@audit these parameters `_sellToken`, are not checked
491:     function _fillQuote(IERC20 _sellToken, uint256 _amount, bytes calldata _swapCallData) internal {


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L133-L133) , [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L157-L157), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L172-L172), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L211-L211), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L226-L226), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L240-L240), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L336-L336), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L348-L348), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L364-L364), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L491-L491)


</details>


### [NC&#x2011;61] Consider using named function calls 
Named function calls in Solidity greatly improve code readability by explicitly mapping arguments to their respective parameter names. This clarity becomes critical when dealing with functions that have numerous or complex parameters, reducing potential errors due to misordered arguments. Therefore, adopting named function calls contributes to more maintainable and less error-prone code.


*There are 8 instances of this issue:*



<details>
<summary>see instances</summary>


```solidity
File: ./src/PrelaunchPoints.sol

125:         _processLock(ETH, msg.value, msg.sender, _referral);

134:         _processLock(ETH, msg.value, _for, _referral);

147:         _processLock(_token, _amount, msg.sender, _referral);

161:         _processLock(_token, _amount, _for, _referral);

215:         _claim(_token, msg.sender, _percentage, _exchange, _data);

230:         uint256 claimedAmount = _claim(_token, address(this), _percentage, _exchange, _data);

254:             _validateData(_token, userClaim, _exchange, _data);

259:             _fillQuote(IERC20(_token), userClaim, _data);


```

[link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L125-L125) , [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L134-L134), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L147-L147), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L161-L161), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L215-L215), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L230-L230), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L254-L254), [link](https://github.com/code-423n4/2024-05-loop/blob/main//./src/PrelaunchPoints.sol#L259-L259)


</details>
