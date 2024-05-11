# QA Report

## Summary

### Low Issues

Total **19 instances** over **7 issues**:

|ID|Issue|Instances|
|:--:|:---|:--:|
| [[L-01]](#l-01-missing-zero-address-check-in-constructor) | Missing zero address check in constructor | 3 |
| [[L-02]](#l-02-constructor--initialization-function-lacks-parameter-validation) | Constructor / initialization function lacks parameter validation | 1 |
| [[L-03]](#l-03-unsafe-solidity-low-level-call-can-cause-gas-grief-attack) | Unsafe solidity low-level call can cause gas grief attack | 1 |
| [[L-04]](#l-04-functions-calling-contractsaddresses-with-transfer-hooks-should-be-protected-by-reentrancy-guard) | Functions calling contracts/addresses with transfer hooks should be protected by reentrancy guard | 4 |
| [[L-05]](#l-05-critical-functions-should-be-controlled-by-time-locks) | Critical functions should be controlled by time locks | 2 |
| [[L-06]](#l-06-missing-contract-existence-checks-before-low-level-calls) | Missing contract existence checks before low-level calls | 1 |
| [[L-07]](#l-07-code-does-not-follow-the-best-practice-of-check-effects-interaction) | Code does not follow the best practice of check-effects-interaction | 7 |

### Non Critical Issues

Total **62 instances** over **34 issues**:

|ID|Issue|Instances|
|:--:|:---|:--:|
| [[N-01]](#n-01-import-declarations-should-import-specific-identifiers-rather-than-the-whole-file) | Import declarations should import specific identifiers, rather than the whole file | 2 |
| [[N-02]](#n-02-custom-errors-should-be-used-rather-than-revertrequire) | Custom errors should be used rather than `revert()`/`require()` | 1 |
| [[N-03]](#n-03-consider-splitting-complex-checks-into-multiple-steps) | Consider splitting complex checks into multiple steps | 1 |
| [[N-04]](#n-04-consider-adding-a-blockdeny-list) | Consider adding a block/deny-list | 1 |
| [[N-05]](#n-05-contract-timekeeping-will-break-earlier-than-the-ethereum-network-itself-will-stop-working) | Contract timekeeping will break earlier than the Ethereum network itself will stop working | 3 |
| [[N-06]](#n-06-consider-emitting-an-event-at-the-end-of-the-constructor) | Consider emitting an event at the end of the constructor | 1 |
| [[N-07]](#n-07-empty-function-body-without-comments) | Empty function body without comments | 1 |
| [[N-08]](#n-08-events-are-emitted-without-the-sender-information) | Events are emitted without the sender information | 1 |
| [[N-09]](#n-09-imports-could-be-organized-more-systematically) | Imports could be organized more systematically | 1 |
| [[N-10]](#n-10-contract-uses-both-requirerevert-as-well-as-custom-errors) | Contract uses both `require()`/`revert()` as well as custom errors | 1 |
| [[N-11]](#n-11-variable-names-for-immutables-should-use-upper_case_with_underscores) | Variable names for `immutable`s should use UPPER_CASE_WITH_UNDERSCORES | 1 |
| [[N-12]](#n-12-constants-should-be-put-on-the-left-side-of-comparisons) | Constants should be put on the left side of comparisons | 12 |
| [[N-13]](#n-13-high-cyclomatic-complexity) | High cyclomatic complexity | 1 |
| [[N-14]](#n-14-typos) | Typos | 1 |
| [[N-15]](#n-15-consider-bounding-input-array-length) | Consider bounding input array length | 1 |
| [[N-16]](#n-16-consider-using-delete-rather-than-assigning-zero-to-clear-values) | Consider using `delete` rather than assigning zero to clear values | 2 |
| [[N-17]](#n-17-use-the-latest-solidity-version) | Use the latest Solidity version | 1 |
| [[N-18]](#n-18-use-transfer-libraries-instead-of-low-level-calls-to-transfer-native-tokens) | Use transfer libraries instead of low level calls to transfer native tokens | 1 |
| [[N-19]](#n-19-use-a-struct-to-encapsulate-multiple-function-parameters) | Use a struct to encapsulate multiple function parameters | 1 |
| [[N-20]](#n-20-returning-a-struct-instead-of-a-bunch-of-variables-is-better) | Returning a struct instead of a bunch of variables is better | 2 |
| [[N-21]](#n-21-events-that-mark-critical-parameter-changes-should-contain-both-the-old-and-the-new-value) | Events that mark critical parameter changes should contain both the old and the new value | 1 |
| [[N-22]](#n-22-missing-event-when-a-state-variables-is-set-in-constructor) | Missing event when a state variables is set in constructor | 5 |
| [[N-23]](#n-23-empty-bytes-check-is-missing) | Empty bytes check is missing | 2 |
| [[N-24]](#n-24-multiple-mappings-with-same-keys-can-be-combined-into-a-single-struct-mapping-for-readability) | Multiple mappings with same keys can be combined into a single struct mapping for readability | 2 |
| [[N-25]](#n-25-missing-event-for-critical-changes) | Missing event for critical changes | 1 |
| [[N-26]](#n-26-duplicated-requirerevert-checks-should-be-refactored) | Duplicated `require()`/`revert()` checks should be refactored | 3 |
| [[N-27]](#n-27-consider-adding-emergency-stop-functionality) | Consider adding emergency-stop functionality | 1 |
| [[N-28]](#n-28-missing-checks-for-uint-state-variable-assignments) | Missing checks for uint state variable assignments | 3 |
| [[N-29]](#n-29-use-the-modern-upgradeable-contract-paradigm) | Use the Modern Upgradeable Contract Paradigm | 1 |
| [[N-30]](#n-30-large-or-complicated-code-bases-should-implement-invariant-tests) | Large or complicated code bases should implement invariant tests | 1 |
| [[N-31]](#n-31-the-default-value-is-manually-set-when-it-is-declared) | The default value is manually set when it is declared | 1 |
| [[N-32]](#n-32-contracts-should-have-all-publicexternal-functions-exposed-by-interfaces) | Contracts should have all `public`/`external` functions exposed by `interface`s | 1 |
| [[N-33]](#n-33-top-level-declarations-should-be-separated-by-at-least-two-lines) | Top-level declarations should be separated by at least two lines | 3 |
| [[N-34]](#n-34-consider-adding-formal-verification-proofs) | Consider adding formal verification proofs | 1 |

## Low Issues

### [L-01] Missing zero address check in constructor

Constructors often take address parameters to initialize important components of a contract, such as owner or linked contracts. However, without a checking, there's a risk that an address parameter could be mistakenly set to the zero address (0x0). This could be due to an error or oversight during contract deployment. A zero address in a crucial role can cause serious issues, as it cannot perform actions like a normal address, and any funds sent to it will be irretrievable. It's therefore crucial to include a zero address check in constructors to prevent such potential problems. If a zero address is detected, the constructor should revert the transaction.

There are 3 instances:

- *PrelaunchPoints.sol* ( [97-97](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L97-L97) ):

```solidity
/// @audit `_exchangeProxy not checked`
/// @audit `_wethAddress not checked`
/// @audit `_allowedTokens not checked`
97:     constructor(address _exchangeProxy, address _wethAddress, address[] memory _allowedTokens) {
```

### [L-02] Constructor / initialization function lacks parameter validation

Constructors and initialization functions play a critical role in contracts by setting important initial states when the contract is first deployed before the system starts. The parameters passed to the constructor and initialization functions directly affect the behavior of the contract / protocol. If incorrect parameters are provided, the system may fail to run, behave abnormally, be unstable, or lack security. Therefore, it's crucial to carefully check each parameter in the constructor and initialization functions. If an exception is found, the transaction should be rolled back.

There is 1 instance:

- *PrelaunchPoints.sol* ( [97-97](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L97-L97) ):

```solidity
/// @audit `_allowedTokens`
97:     constructor(address _exchangeProxy, address _wethAddress, address[] memory _allowedTokens) {
```

### [L-03] Unsafe solidity low-level call can cause gas grief attack

Using the low-level calls of a solidity address can leave the contract open to gas grief attacks. These attacks occur when the called contract returns a large amount of data.
So when calling an external contract, it is necessary to check the length of the return data before reading/copying it (using `returndatasize()`).

There is 1 instance:

- *PrelaunchPoints.sol* ( [497-497](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L497-L497) ):

```solidity
497:         (bool success,) = payable(exchangeProxy).call{value: 0}(_swapCallData);
```

### [L-04] Functions calling contracts/addresses with transfer hooks should be protected by reentrancy guard

Even if the function follows the best practice of check-effects-interaction, not using a reentrancy guard when there may be transfer hooks opens the users of this protocol up to [read-only reentrancy vulnerability](https://chainsecurity.com/curve-lp-oracle-manipulation-post-mortem/) with no way to protect them except by block-listing the entire protocol.

There are 4 instances:

- *PrelaunchPoints.sol* ( [186-186](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L186-L186), [251-251](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L251-L251), [302-302](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L302-L302), [383-383](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L383-L383) ):

```solidity
186:             IERC20(_token).safeTransferFrom(msg.sender, address(this), _amount);

251:             lpETH.safeTransfer(_receiver, claimedAmount);

302:             IERC20(_token).safeTransfer(msg.sender, lockedAmount);

383:         IERC20(tokenAddress).safeTransfer(owner, tokenAmount);
```

### [L-05] Critical functions should be controlled by time locks

It is a good practice to give time for users to react and adjust to critical changes. A timelock provides more guarantees and reduces the level of trust required, thus decreasing risk for users. It also indicates that the project is legitimate (less risk of a malicious owner making a sandwich attack on a user).

There are 2 instances:

- *PrelaunchPoints.sol* ( [336-336](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L336-L336), [348-352](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L348-L352) ):

```solidity
336:     function setOwner(address _owner) external onlyAuthorized {

348:     function setLoopAddresses(address _loopAddress, address _vaultAddress)
349:         external
350:         onlyAuthorized
351:         onlyBeforeDate(loopActivation)
352:     {
```

### [L-06] Missing contract existence checks before low-level calls

Low-level calls return success if there is no code present at the specified address. In addition to the zero-address checks, add a check to verify that `<address>.code.length > 0`.

There is 1 instance:

- *PrelaunchPoints.sol* ( [497-497](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L497-L497) ):

```solidity
497:         (bool success,) = payable(exchangeProxy).call{value: 0}(_swapCallData);
```

### [L-07] Code does not follow the best practice of check-effects-interaction

Code should follow the best-practice of [check-effects-interaction](https://blockchain-academy.hs-mittweida.de/courses/solidity-coding-beginners-to-intermediate/lessons/solidity-11-coding-patterns/topic/checks-effects-interactions/), where state variables are updated before any external calls are made. Doing so prevents a large class of reentrancy bugs.

There are 7 instances:

- *PrelaunchPoints.sol* ( [190-190](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L190-L190), [191-191](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L191-L191), [193-193](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L193-L193), [262-262](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L262-L262), [324-324](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L324-L324), [327-327](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L327-L327), [503-503](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L503-L503) ):

```solidity
/// @audit `safeTransferFrom()` is called on line 186
190:                 totalSupply = totalSupply + _amount;

/// @audit `safeTransferFrom()` is called on line 186
191:                 balances[_receiver][ETH] += _amount;

/// @audit `safeTransferFrom()` is called on line 186
193:                 balances[_receiver][_token] += _amount;

/// @audit `_fillQuote()` is called on line 259, triggering an external call - `call()`
262:             claimedAmount = address(this).balance;

/// @audit `deposit()` is called on line 322
324:         totalLpETH = lpETH.balanceOf(address(this));

/// @audit `deposit()` is called on line 322
327:         startClaimDate = uint32(block.timestamp);

/// @audit `call()` is called on line 497
503:         boughtETHAmount = address(this).balance - boughtETHAmount;
```

## Non Critical Issues

### [N-01] Import declarations should import specific identifiers, rather than the whole file

Using import declarations of the form `import {<identifier_name>} from "some/file.sol"` avoids polluting the symbol namespace making flattened files smaller, and speeds up compilation (but does not save any gas).

There are 2 instances:

- *PrelaunchPoints.sol* ( [4](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L4), [5](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L5) ):

```solidity
4: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

5: import "@openzeppelin/contracts/utils/math/Math.sol";
```

### [N-02] Custom errors should be used rather than `revert()`/`require()`

Custom errors are available from solidity version 0.8.4. Custom errors are more easily processed in try-catch blocks, and are easier to re-use and maintain.

There is 1 instance:

- *PrelaunchPoints.sol* ( [495-495](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L495-L495) ):

```solidity
495:         require(_sellToken.approve(exchangeProxy, _amount));
```

### [N-03] Consider splitting complex checks into multiple steps

Assign the expression's parts to intermediate local variables, and check against those instead.

There is 1 instance:

- *PrelaunchPoints.sol* ( [439-439](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L439-L439) ):

```solidity
439:         if (recipient != address(this) && recipient != address(0)) {
```

### [N-04] Consider adding a block/deny-list

Doing so will significantly increase centralization, but will help to prevent hackers from using stolen tokens

There is 1 instance:

- *PrelaunchPoints.sol* ( [16](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L16) ):

```solidity
16: contract PrelaunchPoints {
```

### [N-05] Contract timekeeping will break earlier than the Ethereum network itself will stop working

When a timestamp is downcast from `uint256` to `uint32`, the value will wrap in the year 2106, and the contracts will break. Other downcasts will have different endpoints. Consider whether your contract is intended to live past the size of the type being used.

There are 3 instances:

- *PrelaunchPoints.sol* ( [102-102](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L102-L102), [327-327](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L327-L327), [355-355](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L355-L355) ):

```solidity
102:         loopActivation = uint32(block.timestamp + 120 days);

327:         startClaimDate = uint32(block.timestamp);

355:         loopActivation = uint32(block.timestamp);
```

### [N-06] Consider emitting an event at the end of the constructor

This will allow users to easily exactly pinpoint when and by whom a contract was constructed.

There is 1 instance:

- *PrelaunchPoints.sol* ( [97-97](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L97-L97) ):

```solidity
97:     constructor(address _exchangeProxy, address _wethAddress, address[] memory _allowedTokens) {
```

### [N-07] Empty function body without comments

Empty function body in solidity is not recommended, consider adding some comments to the body.

There is 1 instance:

- *PrelaunchPoints.sol* ( [392-392](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L392-L392) ):

```solidity
392:     receive() external payable {}
```

### [N-08] Events are emitted without the sender information

When an action is triggered based on a user's action, not being able to filter based on who triggered the action makes event processing a lot more cumbersome. Including the `msg.sender` the events of these types of action will make events much more useful to end users, especially when `msg.sender` is not `tx.origin`.

There is 1 instance:

- *PrelaunchPoints.sol* ( [197-197](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L197-L197) ):

```solidity
197:         emit Locked(_receiver, _amount, _token, _referral);
```

### [N-09] Imports could be organized more systematically

The contract's interface should be imported first, followed by each of the interfaces it uses, followed by all other files. The examples below do not follow this layout.

There is 1 instance:

- *PrelaunchPoints.sol* ( [7-7](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L7-L7) ):

```solidity
/// @audit Out of order with the prev import️ ⬆
7: import {ILpETH, IERC20} from "./interfaces/ILpETH.sol";
```

### [N-10] Contract uses both `require()`/`revert()` as well as custom errors

Consider using just one method in a single file.

There is 1 instance:

- *PrelaunchPoints.sol* ( [16](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L16) ):

```solidity
16: contract PrelaunchPoints {
```

### [N-11] Variable names for `immutable`s should use UPPER_CASE_WITH_UNDERSCORES

For `immutable` variable names, each word should use all capital letters, with underscores separating each word (UPPER_CASE_WITH_UNDERSCORES)

There is 1 instance:

- *PrelaunchPoints.sol* ( [29-29](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L29-L29) ):

```solidity
29:     address public immutable exchangeProxy;
```

### [N-12] Constants should be put on the left side of comparisons

Putting constants on the left side of comparison statements is a best practice known as [Yoda conditions](https://en.wikipedia.org/wiki/Yoda_conditions).
Although solidity's static typing system prevents accidental assignments within conditionals, adopting this practice can improve code readability and consistency, especially when working across multiple languages.

<details>
<summary>There are 12 instances (click to show):</summary>

- *PrelaunchPoints.sol* ( [144](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L144), [158](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L158), [176](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L176), [179](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L179), [245](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L245), [248](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L248), [287](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L287), [290](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L290), [414](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L414), [423](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L423), [426](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L426), [439](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L439) ):

```solidity
/// @audit put `ETH` on the left
144:         if (_token == ETH) {

/// @audit put `ETH` on the left
158:         if (_token == ETH) {

/// @audit put `0` on the left
176:         if (_amount == 0) {

/// @audit put `ETH` on the left
179:         if (_token == ETH) {

/// @audit put `0` on the left
245:         if (userStake == 0) {

/// @audit put `ETH` on the left
248:         if (_token == ETH) {

/// @audit put `0` on the left
287:         if (lockedAmount == 0) {

/// @audit put `ETH` on the left
290:         if (_token == ETH) {

/// @audit put `UNI_SELECTOR` on the left
414:             if (selector != UNI_SELECTOR) {

/// @audit put `TRANSFORM_SELECTOR` on the left
423:             if (selector != TRANSFORM_SELECTOR) {

/// @audit put `ETH` on the left
426:             if (outputToken != ETH) {

/// @audit put `address(0)` on the left
439:         if (recipient != address(this) && recipient != address(0)) {
```

</details>

### [N-13] High cyclomatic complexity

Consider breaking down these blocks into more manageable units, by splitting things into utility functions, by reducing nesting, and by using early returns.

<details>
<summary>There is 1 instance (click to show):</summary>

- *PrelaunchPoints.sol* ( [405-442](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L405-L442) ):

```solidity
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

</details>

### [N-14] Typos

All typos should be corrected.

There is 1 instance:

- *PrelaunchPoints.sol* ( [457](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L457) ):

```solidity
/// @audit lenght
457:             p := add(p, 36) // Data: selector 4 + lenght data 32
```

### [N-15] Consider bounding input array length

The functions below take in an unbounded array, and make function calls for entries in the array. While the function will revert if it eventually runs out of gas, it may be a nicer user experience to require() that the length of the array is below some reasonable maximum, so that the user doesn't have to use up a full transaction's gas only to see that the transaction reverts.

There is 1 instance:

- *PrelaunchPoints.sol* ( [107](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L107) ):

```solidity
107:         for (uint256 i = 0; i < length;) {
```

### [N-16] Consider using `delete` rather than assigning zero to clear values

The `delete` keyword more closely matches the semantics of what is being done, and draws more attention to the changing of state, which may lead to a more thorough audit of its associated logic.

There are 2 instances:

- *PrelaunchPoints.sol* ( [250-250](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L250-L250), [285-285](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L285-L285) ):

```solidity
250:             balances[msg.sender][_token] = 0;

285:         balances[msg.sender][_token] = 0;
```

### [N-17] Use the latest Solidity version

Upgrading to the [latest solidity version](https://github.com/ethereum/solc-js/tags) (0.8.19 for L2s) can optimize gas usage, take advantage of new features and improve overall contract efficiency. Where possible, based on compatibility requirements, it is recommended to use newer/latest solidity version to take advantage of the latest optimizations and features.

There is 1 instance:

- *PrelaunchPoints.sol* ( [2](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L2) ):

```solidity
2: pragma solidity 0.8.20;
```

### [N-18] Use transfer libraries instead of low level calls to transfer native tokens

Consider using [`SafeTransferLib.safeTransferETH`](https://github.com/transmissions11/solmate/blob/fadb2e2778adbf01c80275bfb99e5c14969d964b/src/utils/SafeTransferLib.sol#L15) or [`Address.sendValue`](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/9e3f4d60c581010c4a3979480e07cc7752f124cc/contracts/utils/Address.sol#L41) for clearer semantic meaning instead of using a low level `call`.

There is 1 instance:

- *PrelaunchPoints.sol* ( [296-296](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L296-L296) ):

```solidity
296:             (bool sent,) = msg.sender.call{value: lockedAmount}("");
```

### [N-19] Use a struct to encapsulate multiple function parameters

If a function has too many parameters, replacing them with a struct can improve code readability and maintainability, increase reusability, and reduce the likelihood of errors when passing the parameters.

There is 1 instance:

- *PrelaunchPoints.sol* ( [240-242](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L240-L242) ):

```solidity
240:     function _claim(address _token, address _receiver, uint8 _percentage, Exchange _exchange, bytes calldata _data)
241:         internal
242:         returns (uint256 claimedAmount)
```

### [N-20] Returning a struct instead of a bunch of variables is better

If a function returns [too many variables](https://docs.soliditylang.org/en/v0.8.21/contracts.html#returning-multiple-values), replacing them with a struct can improve code readability, maintainability and reusability.

There are 2 instances:

- *PrelaunchPoints.sol* ( [448-451](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L448-L451), [470-473](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L470-L473) ):

```solidity
448:     function _decodeUniswapV3Data(bytes calldata _data)
449:         internal
450:         pure
451:         returns (address inputToken, address outputToken, uint256 inputTokenAmount, address recipient, bytes4 selector)

470:     function _decodeTransformERC20Data(bytes calldata _data)
471:         internal
472:         pure
473:         returns (address inputToken, address outputToken, uint256 inputTokenAmount, bytes4 selector)
```

### [N-21] Events that mark critical parameter changes should contain both the old and the new value

This should especially be done if the new value is not required to be different from the old value.

There is 1 instance:

- *PrelaunchPoints.sol* ( [339-339](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L339-L339) ):

```solidity
339:         emit OwnerUpdated(_owner);
```

### [N-22] Missing event when a state variables is set in constructor

The initial states set in a constructor are important and should be recorded in the event.

There are 5 instances:

- *PrelaunchPoints.sol* ( [98](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L98), [102](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L102), [103](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L103), [108](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L108), [113](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L113) ):

```solidity
98:         owner = msg.sender;

102:         loopActivation = uint32(block.timestamp + 120 days);

103:         startClaimDate = 4294967295; // Max uint32 ~ year 2107

108:             isTokenAllowed[_allowedTokens[i]] = true;

113:         isTokenAllowed[_wethAddress] = true;
```

### [N-23] Empty bytes check is missing

Passing empty bytes to a function can cause unexpected behavior, such as certain operations failing, producing incorrect results, or wasting gas. It is recommended to check that all byte parameters are not empty.

There are 2 instances:

- *PrelaunchPoints.sol* ( [211-214](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L211-L214), [226-229](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L226-L229) ):

```solidity
/// @audit _data
211:     function claim(address _token, uint8 _percentage, Exchange _exchange, bytes calldata _data)
212:         external
213:         onlyAfterDate(startClaimDate)
214:     {

/// @audit _data
226:     function claimAndStake(address _token, uint8 _percentage, Exchange _exchange, bytes calldata _data)
227:         external
228:         onlyAfterDate(startClaimDate)
229:     {
```

### [N-24] Multiple mappings with same keys can be combined into a single struct mapping for readability

Well-organized data structures make code reviews easier, which may lead to fewer bugs. Consider combining related mappings into mappings to structs, so it's clear what data is related.

There are 2 instances:

- *PrelaunchPoints.sol* ( [35-35](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L35-L35), [50-50](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L50-L50) ):

```solidity
35:     mapping(address => bool) public isTokenAllowed;

50:     mapping(address => mapping(address => uint256)) public balances; // User -> Token -> Balance
```

### [N-25] Missing event for critical changes

Events should be emitted when critical changes are made to the contracts.

There is 1 instance:

- *PrelaunchPoints.sol* ( [372-374](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L372-L374) ):

```solidity
372:     function setEmergencyMode(bool _mode) external onlyAuthorized {
373:         emergencyMode = _mode;
374:     }
```

### [N-26] Duplicated `require()`/`revert()` checks should be refactored

Refactoring duplicate `require()`/`revert()` checks into a modifier or function can make the code more concise, readable and maintainable, and less likely to make errors or omissions when modifying the `require()` or `revert()`.

There are 3 instances:

- *PrelaunchPoints.sol* ( [415-415](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L415-L415), [434-434](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L434-L434) ):

```solidity
/// @audit Duplicated on line 424
415:                 revert WrongSelector(selector);

/// @audit Duplicated on line 419, 427
434:             revert WrongDataTokens(inputToken, outputToken);
```

### [N-27] Consider adding emergency-stop functionality

Adding a way to quickly halt protocol functionality in an emergency, rather than having to pause individual contracts one-by-one, will make in-progress hack mitigation faster and much less stressful.

There is 1 instance:

- *PrelaunchPoints.sol* ( [16](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L16) ):

```solidity
16: contract PrelaunchPoints {
```

### [N-28] Missing checks for uint state variable assignments

Consider whether reasonable bounds checks for variables would be useful.

There are 3 instances:

- *PrelaunchPoints.sol* ( [324-324](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L324-L324), [327-327](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L327-L327), [355-355](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L355-L355) ):

```solidity
324:         totalLpETH = lpETH.balanceOf(address(this));

327:         startClaimDate = uint32(block.timestamp);

355:         loopActivation = uint32(block.timestamp);
```

### [N-29] Use the Modern Upgradeable Contract Paradigm

Modern smart contract development often employs upgradeable contract structures, utilizing proxy patterns like OpenZeppelin’s Upgradeable Contracts. This paradigm separates logic and state, allowing developers to amend and enhance the contract's functionality without altering its state or the deployed contract address. Transitioning to this approach enhances long-term maintainability.
Resolution: Adopt a well-established proxy pattern for upgradeability, ensuring proper initialization and employing transparent proxies to mitigate potential risks. Embrace comprehensive testing and audit practices, particularly when updating contract logic, to ensure state consistency and security are preserved across upgrades. This ensures your contract remains robust and adaptable to future requirements.

There is 1 instance:

- *PrelaunchPoints.sol* ( [16](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L16) ):

```solidity
16: contract PrelaunchPoints {
```

### [N-30] Large or complicated code bases should implement invariant tests

This includes: large code bases, or code with lots of inline-assembly, complicated math, or complicated interactions between multiple contracts.
Invariant fuzzers such as Echidna require the test writer to come up with invariants which should not be violated under any circumstances, and the fuzzer tests various inputs and function calls to ensure that the invariants always hold.
Even code with 100% code coverage can still have bugs due to the order of the operations a user performs, and invariant fuzzers may help significantly.

There is 1 instance:

- Global finding

### [N-31] The default value is manually set when it is declared

In instances where a new variable is defined, there is no need to set it to it's default value.

There is 1 instance:

- *PrelaunchPoints.sol* ( [107-107](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L107-L107) ):

```solidity
107:         for (uint256 i = 0; i < length;) {
```

### [N-32] Contracts should have all `public`/`external` functions exposed by `interface`s

All `external`/`public` functions should extend an `interface`. This is useful to ensure that the whole API is extracted and can be more easily integrated by other projects.

There is 1 instance:

- *PrelaunchPoints.sol* ( [16](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L16) ):

```solidity
16: contract PrelaunchPoints {
```

### [N-33] Top-level declarations should be separated by at least two lines

-

There are 3 instances:

- *PrelaunchPoints.sol* ( [2-4](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L2-L4), [516-518](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L516-L518), [523-525](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L523-L525) ):

```solidity
2: pragma solidity 0.8.20;
3: 
4: import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

516:     }
517: 
518:     modifier onlyAfterDate(uint256 limitDate) {

523:     }
524: 
525:     modifier onlyBeforeDate(uint256 limitDate) {
```

### [N-34] Consider adding formal verification proofs

Formal verification is the act of proving or disproving the correctness of intended algorithms underlying a system with respect to a certain formal specification/property/invariant, using formal methods of mathematics.

Some tools that are currently available to perform these tests on smart contracts are [SMTChecker](https://docs.soliditylang.org/en/latest/smtchecker.html) and [Certora Prover](https://www.certora.com/).

There is 1 instance:

- [*PrelaunchPoints.sol*](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol)
