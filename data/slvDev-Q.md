|    | Issue | Instances |
|----|-------|:---------:|
| [L-01] | Centralization issue caused by admin privileges | 9 |
| [L-02] | Unused/empty `receive()/fallback()` function | 1 |
| [L-03] | Unauthorized `receive()/fallback()` Functions | 1 |
| [L-04] | Large transfers may not work with some `ERC20` tokens | 3 |
| [L-05] | Code does not follow the best practice of check-effects-interaction | 1 |
| [L-06] | Upgradable contracts not taken into account | 3 |
| [L-07] | Lack of index element validation in external/public function | 4 |
| [L-08] | Possible Vulnerability to Fee-On-Transfer Accounting Issues | 2 |
| [L-09] | Use `increaseAllowance`/`decreaseAllowance` instead of `approve`/`safeApprove` | 1 |
| [L-10] | Missing checks for `address(0)` when updating state variables | 3 |
| [L-11] | Input Validation Missing in Setter Functions | 1 |
| [L-12] | Consider the case where `totalSupply` is zero | 1 |
| [L-13] | Missing checks for state variable assignments | 1 |
| [L-14] | Events may be emitted out of order due to reentrancy | 5 |
| [L-15] | Critical functions should be a two step procedure | 2 |
| [L-16] | Loss of precision on division | 1 |
| [L-17] | Unsafe downcast from larger to smaller integer types | 2 |
| [L-18] | Some `ERC20` can revert on a zero value `transfer` | 1 |
| [L-19] | Function Parameters in Public Accessible Functions Need `address(0)` Check | 10 |
| [L-20] | Missing checks for `address(0)` in constructor | 1 |
| [L-21] | Functions calling contracts with transfer hooks are missing reentrancy guards | 3 |
| [L-22] | Casting `block.timestamp` to Smaller Integer Types Limit Contract Lifespan | 3 |
| [L-23] | Critical system parameter changes should be behind a timelock | 3 |
| [L-24] | Ensure Non-Empty Check for Bytes in Function Parameters | 6 |
| [L-25] | Consider adding a block/deny-list | 1 |
| [L-26] | `approve` can revert if the current approval is not zero | 1 |


### [L-01] Centralization issue caused by admin privileges

There are priviliged roles that are a single point of failure, who can use critical functions, posing a centralization issue.

<details>
<summary><i>9 issue instances in 1 files:</i></summary>

```solidity
File: src/PrelaunchPoints.sol

172: function _processLock(address _token, uint256 _amount, address _receiver, bytes32 _referral)
        internal
        onlyBeforeDate(loopActivation)
211: function claim(address _token, uint8 _percentage, Exchange _exchange, bytes calldata _data)
        external
        onlyAfterDate(startClaimDate)
226: function claimAndStake(address _token, uint8 _percentage, Exchange _exchange, bytes calldata _data)
        external
        onlyAfterDate(startClaimDate)
315: function convertAllETH() external onlyAuthorized onlyBeforeDate(startClaimDate)
336: function setOwner(address _owner) external onlyAuthorized
348: function setLoopAddresses(address _loopAddress, address _vaultAddress)
        external
        onlyAuthorized
        onlyBeforeDate(loopActivation)
364: function allowToken(address _token) external onlyAuthorized
372: function setEmergencyMode(bool _mode) external onlyAuthorized
379: function recoverERC20(address tokenAddress, uint256 tokenAmount) external onlyAuthorized
```
[172](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L172) | [211](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L211) | [226](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L226) | [315](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L315) | [336](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L336) | [348](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L348) | [364](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L364) | [372](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L372) | [379](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L379)
</details>

### [L-02] Unused/empty `receive()/fallback()` function

If the intention is for the Ether to be used, the function should call another function, otherwise it should revert (e.g. require(msg.sender == address(weth))).

<details>
<summary><i>1 issue instances in 1 files:</i></summary>

```solidity
File: src/PrelaunchPoints.sol

392: receive() external payable
```
[392](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L392)
</details>

### [L-03] Unauthorized `receive()/fallback()` Functions

Empty receive() or payable fallback() functions without proper authorization can result in a loss of funds. If the intention is for the Ether to be used, the function should call another function, otherwise, it should revert (e.g. require(msg.sender == address(weth))).

Having no access control on the function means that someone may send Ether to the contract and have no way to get anything back out. Even if the concern is spending a small amount of gas to check the sender, the code should at least have a function to rescue unused Ether, ensuring funds are not trapped within the contract.

<details>
<summary><i>1 issue instances in 1 files:</i></summary>

```solidity
File: src/PrelaunchPoints.sol

392: receive() external payable
```
[392](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L392)
</details>

### [L-04] Large transfers may not work with some `ERC20` tokens

Some `IERC20` implementations (e.g `UNI`, `COMP`)
            
- [COMP (Compound Protocol)](https://github.com/compound-finance/compound-protocol/blob/a3214f67b73310d547e00fc578e8355911c9d376/contracts/Governance/Comp.sol#L115-L142)
- [UNI (Uniswap)](https://github.com/Uniswap/governance/blob/eabd8c71ad01f61fb54ed6945162021ee419998e/contracts/Uni.sol#L209-L236)
            
may fail if the valued `transferred` is larger than `uint96`.

<details>
<summary><i>3 issue instances in 1 files:</i></summary>

```solidity
File: src/PrelaunchPoints.sol

186: IERC20(_token).safeTransferFrom(msg.sender, address(this), _amount)
302: IERC20(_token).safeTransfer(msg.sender, lockedAmount)
383: IERC20(tokenAddress).safeTransfer(owner, tokenAmount)
```
[186](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L186) | [302](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L302) | [383](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L383)
</details>

### [L-05] Code does not follow the best practice of check-effects-interaction

Code should follow the best-practice of [CEI](https://blockchain-academy.hs-mittweida.de/courses/solidity-coding-beginners-to-intermediate/lessons/solidity-11-coding-patterns/topic/checks-effects-interactions/), where state variables are updated before any external calls are made. Doing so prevents a large class of reentrancy bugs.

<details>
<summary><i>1 issue instances in 1 files:</i></summary>

```solidity
File: src/PrelaunchPoints.sol

/// @audit - State change after external call: `IERC20(_token).safeTransferFrom(msg.sender, address(this), _amount)`
190: totalSupply = totalSupply + _amount
```
[190](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L190)
</details>

### [L-06] Upgradable contracts not taken into account

In the realm of blockchain development, it's crucial to consider the impact of upgradable contracts, especially when handling token addresses through interfaces like IERC20.
These contracts can evolve over time, potentially altering their behavior or interface. Such changes may lead to compatibility issues or security vulnerabilities in the protocol that relies on them.

<details>
<summary><i>3 issue instances in 1 files:</i></summary>

```solidity
File: src/PrelaunchPoints.sol

186: IERC20(_token).safeTransferFrom(msg.sender, address(this), _amount);
302: IERC20(_token).safeTransfer(msg.sender, lockedAmount);
383: IERC20(tokenAddress).safeTransfer(owner, tokenAmount);
```
[186](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L186) | [302](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L302) | [383](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L383)
</details>

### [L-07] Lack of index element validation in external/public function

There's no validation to check whether the index element provided as an argument actually exists in the call. This omission could lead to unintended behavior if an element that does not exist in the call is passed to the function.

The function should validate that the provided index element exists in the call before proceeding.

Without this validation, the function could cause unintended behaviour as it will call an non-existing index element. This could lead to inconsistencies in data and potentially affect the integrity of the call structure.

<details>
<summary><i>4 issue instances in 1 files:</i></summary>

```solidity
File: src/PrelaunchPoints.sol

284: balances[msg.sender][_token]
285: balances[msg.sender][_token]
365: isTokenAllowed[_token]
380: isTokenAllowed[tokenAddress]
```
[284](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L284) | [285](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L285) | [365](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L365) | [380](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L380)
</details>

### [L-08] Possible Vulnerability to Fee-On-Transfer Accounting Issues

Contracts transfer funds using the `transferFrom()` function but do not verify that the actual number of tokens received matches the input amount to the transfer.
This could lead to accounting issues if the token involves a fee on transfer.
An attacker might exploit latent funds to get a free credit.
To prevent this, consider checking the balance before and after the transfer and use the difference as the actual amount received.

<details>
<summary><i>2 issue instances in 1 files:</i></summary>

```solidity
File: src/PrelaunchPoints.sol

302: IERC20(_token).safeTransfer(msg.sender, lockedAmount);
383: IERC20(tokenAddress).safeTransfer(owner, tokenAmount);
```
[302](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L302) | [383](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L383)
</details>

### [L-09] Use `increaseAllowance`/`decreaseAllowance` instead of `approve`/`safeApprove`

Changing an allowance with `approve` brings the risk that someone may use both the old and the new allowance by unfortunate transaction ordering. Refer to [ERC20 API: An Attack Vector on the Approve/TransferFrom Methods](https://docs.google.com/document/d/1YLPtQxZu1UAvO9cZ1O2RPXBbT0mooh4DYKjA_jp-RLM/edit#heading=h.m9fhqynw2xvt)

It is recommended to use the `increaseAllowance`/`decreaseAllowance` to avoid ths problem.

<details>
<summary><i>1 issue instances in 1 files:</i></summary>

```solidity
File: src/PrelaunchPoints.sol

495: require(_sellToken.approve(exchangeProxy, _amount));
```
[495](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L495)
</details>

### [L-10] Missing checks for `address(0)` when updating state variables

Check for zero-address to avoid the risk of setting `address(0)` for state variables after an update.

<details>
<summary><i>3 issue instances in 1 files:</i></summary>

```solidity
File: src/PrelaunchPoints.sol

337: owner = _owner;
353: lpETH = ILpETH(_loopAddress);
354: lpETHVault = ILpETHVault(_vaultAddress);
```
[337](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L337) | [353](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L353) | [354](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L354)
</details>

### [L-11] Input Validation Missing in Setter Functions

Setter functions are utilized to update the state variables of a contract.
It is critical to ensure these functions have adequate input sanitization to prevent unwanted alterations or malicious attacks.
Without input validation, there's a potential risk of enabling vulnerabilities like overflow/underflow, unauthorized access, or insertion of invalid data.
Consider incorporating appropriate validation mechanisms, such as checking the range or type of inputs, to enhance the security of your contract.

<details>
<summary><i>1 issue instances in 1 files:</i></summary>

```solidity
File: src/PrelaunchPoints.sol

/// @audit `setEmergencyMode` function does not validate `_mode` input
372: function setEmergencyMode(bool _mode) external onlyAuthorized {
```
[372](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L372)
</details>

### [L-12] Consider the case where `totalSupply` is zero

The following functions should handle the edge case where the `totalSupply` is zero, for example to avoid division by zero errors, as such errors may negatively impact the logic of these functions.

<details>
<summary><i>1 issue instances in 1 files:</i></summary>

```solidity
File: src/PrelaunchPoints.sol

249: claimedAmount = userStake.mulDiv(totalLpETH, totalSupply);
```
[249](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L249)
</details>

### [L-13] Missing checks for state variable assignments

There are some missing checks in these functions, and this could lead to unexpected scenarios. Consider always adding a sanity check for state variables.

<details>
<summary><i>1 issue instances in 1 files:</i></summary>

```solidity
File: src/PrelaunchPoints.sol

337: owner = _owner;
```
[337](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L337)
</details>

### [L-14] Events may be emitted out of order due to reentrancy

It's essential to ensure that events follow the best practice of check-effects-interaction and are emitted before any external calls to prevent out-of-order events due to reentrancy.
Emitting events post external interactions may cause them to be out of order due to reentrancy, which can be misleading or erroneous for event listeners.
[Refer to the Solidity Documentation for best practices.](https://solidity.readthedocs.io/en/latest/security-considerations.html#reentrancy)

<details>
<summary><i>5 issue instances in 1 files:</i></summary>

```solidity
File: src/PrelaunchPoints.sol

/// @audit `safeTransferFrom()` before `Locked` event emit
196: emit Locked(_receiver, _amount, _token, _referral);
/// @audit `safeTransfer()` before `Claimed` event emit
265: emit Claimed(msg.sender, _token, claimedAmount);
/// @audit `call()` before `Withdrawn` event emit
304: emit Withdrawn(msg.sender, _token, lockedAmount);
/// @audit `safeTransfer()` before `Recovered` event emit
384: emit Recovered(tokenAddress, tokenAmount);
/// @audit `call()` before `SwappedTokens` event emit
504: emit SwappedTokens(address(_sellToken), _amount, boughtETHAmount);
```
[196](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L196) | [265](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L265) | [304](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L304) | [384](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L384) | [504](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L504)
</details>

### [L-15] Critical functions should be a two step procedure

Critical functions in Solidity contracts should follow a two-step procedure to enhance security, minimize human error, and ensure proper access control. By dividing sensitive operations into distinct phases, such as initiation and confirmation, developers can introduce a safeguard against unintended actions or unauthorized access.

<details>
<summary><i>2 issue instances in 1 files:</i></summary>

```solidity
File: src/PrelaunchPoints.sol

348: function setLoopAddresses(address _loopAddress, address _vaultAddress)
        external
        onlyAuthorized
        onlyBeforeDate(loopActivation)
    {
372: function setEmergencyMode(bool _mode) external onlyAuthorized {
```
[348](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L348) | [372](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L372)
</details>

### [L-16] Loss of precision on division

Solidity doesn't support fractions, so divisions by large numbers could result in the quotient being zero.

To avoid this, it's recommended to require a minimum numerator amount to ensure that it is always greater than the denominator.

<details>
<summary><i>1 issue instances in 1 files:</i></summary>

```solidity
File: src/PrelaunchPoints.sol

253: uint256 userClaim = userStake * _percentage / 100;
```
[253](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L253)
</details>

### [L-17] Unsafe downcast from larger to smaller integer types

When a type is downcast to a smaller type, the higher order bits are discarded, resulting in the application of a modulo operation to the original value.

If the downcasted value is large enough, this may result in an overflow that will not revert.

<details>
<summary><i>2 issue instances in 1 files:</i></summary>

```solidity
File: src/PrelaunchPoints.sol

/// @audit cast from `uint256` to `uint32`
327: startClaimDate = uint32(block.timestamp);
/// @audit cast from `uint256` to `uint32`
355: loopActivation = uint32(block.timestamp);
```
[327](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L327) | [355](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L355)
</details>

### [L-18] Some `ERC20` can revert on a zero value `transfer`

Not all `ERC20` implementations are totally compliant, and some (e.g `LEND`) may fail while transfering a zero amount.

<details>
<summary><i>1 issue instances in 1 files:</i></summary>

```solidity
File: src/PrelaunchPoints.sol

/// @audit `tokenAmount` has not been checked for zero value before transfer
383: IERC20(tokenAddress).safeTransfer(owner, tokenAmount);
```
[383](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L383)
</details>

### [L-19] Function Parameters in Public Accessible Functions Need `address(0)` Check

Parameters of type `address` in your functions should be checked to ensure that they are not assigned the null address (`address(0x0)`). 
Failure to validate these parameters can lead to transaction reverts, wasted gas, the need for transaction resubmission, and may even require redeployment of contracts within the protocol in certain situations.
Implement checks for `address(0x0)` to avoid these potential issues.

<details>
<summary><i>10 issue instances in 1 files:</i></summary>

```solidity
File: src/PrelaunchPoints.sol

/// @audit `_for` parameter without address(0) check
133: function lockETHFor(address _for, bytes32 _referral) external payable {
/// @audit `_token` parameter without address(0) check
143: function lock(address _token, uint256 _amount, bytes32 _referral) external {
/// @audit `_token` parameter without address(0) check
/// @audit `_for` parameter without address(0) check
157: function lockFor(address _token, uint256 _amount, address _for, bytes32 _referral) external {
/// @audit `_token` parameter without address(0) check
211: function claim(address _token, uint8 _percentage, Exchange _exchange, bytes calldata _data)
        external
        onlyAfterDate(startClaimDate)
    {
/// @audit `_token` parameter without address(0) check
226: function claimAndStake(address _token, uint8 _percentage, Exchange _exchange, bytes calldata _data)
        external
        onlyAfterDate(startClaimDate)
    {
/// @audit `_token` parameter without address(0) check
274: function withdraw(address _token) external {
/// @audit `_owner` parameter without address(0) check
336: function setOwner(address _owner) external onlyAuthorized {
/// @audit `_loopAddress` parameter without address(0) check
/// @audit `_vaultAddress` parameter without address(0) check
348: function setLoopAddresses(address _loopAddress, address _vaultAddress)
        external
        onlyAuthorized
        onlyBeforeDate(loopActivation)
    {
/// @audit `_token` parameter without address(0) check
364: function allowToken(address _token) external onlyAuthorized {
/// @audit `tokenAddress` parameter without address(0) check
379: function recoverERC20(address tokenAddress, uint256 tokenAmount) external onlyAuthorized {
```
[133](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L133) | [143](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L143) | [157](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L157) | [211](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L211) | [226](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L226) | [274](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L274) | [336](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L336) | [348](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L348) | [364](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L364) | [379](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L379)
</details>

### [L-20] Missing checks for `address(0)` in constructor

Check for zero-address to avoid the risk of setting `address(0)` for state variables when deploying.

<details>
<summary><i>1 issue instances in 1 files:</i></summary>

```solidity
File: src/PrelaunchPoints.sol

/// @audit `_exchangeProxy` has lack of `address(0)` check before use
/// @audit `_wethAddress` has lack of `address(0)` check before use
/// @audit `_allowedTokens` has lack of `address(0)` check before use
97: constructor(address _exchangeProxy, address _wethAddress, address[] memory _allowedTokens) {
```
[97](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L97)
</details>

### [L-21] Functions calling contracts with transfer hooks are missing reentrancy guards

Even if the function follows the best practice of check-effects-interaction, not using a reentrancy guard when there may be transfer hooks will open the users of this protocol up to read-only reentrancies with no way to protect against it, except by block-listing the whole protocol.

<details>
<summary><i>3 issue instances in 1 files:</i></summary>

```solidity
File: src/PrelaunchPoints.sol

/// @audit function `_processLock()` 
186: IERC20(_token).safeTransferFrom(msg.sender, address(this), _amount);
/// @audit function `withdraw()` 
302: IERC20(_token).safeTransfer(msg.sender, lockedAmount);
/// @audit function `recoverERC20()` 
383: IERC20(tokenAddress).safeTransfer(owner, tokenAmount);
```
[186](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L186) | [302](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L302) | [383](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L383)
</details>

### [L-22] Casting `block.timestamp` to Smaller Integer Types Limit Contract Lifespan

Casting `block.timestamp` to a smaller integer type like uint32 can potentially reduce the lifespan of a contract.
While the current Unix timestamp fits within the uint32 range as of now, it will eventually exceed this range by the year 2106.

At this point, casting the Unix timestamp to a uint32 will lead to overflow errors, resulting in unpredictable contract behavior.
To future-proof your contract, it's advisable to use larger integer types such as uint40 or uint (up to uint256) when dealing with timestamps.

<details>
<summary><i>3 issue instances in 1 files:</i></summary>

```solidity
File: src/PrelaunchPoints.sol

102: loopActivation = uint32(block.timestamp + 120 days);
327: startClaimDate = uint32(block.timestamp);
355: loopActivation = uint32(block.timestamp);
```
[102](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L102) | [327](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L327) | [355](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L355)
</details>

### [L-23] Critical system parameter changes should be behind a timelock

It is a good practice to give time for users to react and adjust to critical changes. A timelock provides more guarantees and reduces the level of trust required, thus decreasing risk for users. It also indicates that the project is legitimate (less risk of a malicious owner making a sandwich attack on a user).

<details>
<summary><i>3 issue instances in 1 files:</i></summary>

```solidity
File: src/PrelaunchPoints.sol

336: function setOwner(address _owner) external onlyAuthorized {
348: function setLoopAddresses(address _loopAddress, address _vaultAddress)
        external
        onlyAuthorized
        onlyBeforeDate(loopActivation)
    {
372: function setEmergencyMode(bool _mode) external onlyAuthorized {
```
[336](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L336) | [348](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L348) | [372](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L372)
</details>

### [L-24] Ensure Non-Empty Check for Bytes in Function Parameters

To avoid mistakenly accepting empty bytes as valid parameters, it is advisable to implement checks for non-empty bytes within functions.
This ensures that empty bytes are not treated as valid inputs, preventing potential issues.

<details>
<summary><i>6 issue instances in 1 files:</i></summary>

```solidity
File: src/PrelaunchPoints.sol

124: function lockETH(bytes32 _referral) external payable {
133: function lockETHFor(address _for, bytes32 _referral) external payable {
143: function lock(address _token, uint256 _amount, bytes32 _referral) external {
157: function lockFor(address _token, uint256 _amount, address _for, bytes32 _referral) external {
211: function claim(address _token, uint8 _percentage, Exchange _exchange, bytes calldata _data)
        external
        onlyAfterDate(startClaimDate)
    {
226: function claimAndStake(address _token, uint8 _percentage, Exchange _exchange, bytes calldata _data)
        external
        onlyAfterDate(startClaimDate)
    {
```
[124](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L124) | [133](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L133) | [143](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L143) | [157](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L157) | [211](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L211) | [226](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L226)
</details>

### [L-25] Consider adding a block/deny-list

While adding a block or deny-list may increase the level of centralization in the contract, it provides an additional layer of security by preventing hackers from using stolen tokens or carrying out other malicious activities.

Although it's a trade-off, a block or deny-list can help improve the overall security posture of the contract.

<details>
<summary><i>1 issue instances in 1 files:</i></summary>

```solidity
File: src/PrelaunchPoints.sol

16: contract PrelaunchPoints {
```
[16](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L16)
</details>

### [L-26] `approve` can revert if the current approval is not zero

Some tokens like USDT check for the current approval, and they revert if it's not zero.
While Tether is known to do this, it applies to other tokens as well, which are trying to protect against [this](https://docs.google.com/document/d/1YLPtQxZu1UAvO9cZ1O2RPXBbT0mooh4DYKjA_jp-RLM/edit#heading=h.m9fhqynw2xvt) attack vector.

<details>
<summary><i>1 issue instances in 1 files:</i></summary>

```solidity
File: src/PrelaunchPoints.sol

495: require(_sellToken.approve(exchangeProxy, _amount));
```
[495](https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L495)
</details>
