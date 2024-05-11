### Centralization risk for privileged functions
Contracts with privileged functions need owner/admin to be trusted not to perform malicious updates or drain funds. This may also cause a single point failure.


```solidity
Path: ./src/PrelaunchPoints.sol

172:    function _processLock(address _token, uint256 _amount, address _receiver, bytes32 _referral)
173:        internal
174:        onlyBeforeDate(loopActivation)	// @audit-issue

211:    function claim(address _token, uint8 _percentage, Exchange _exchange, bytes calldata _data)
212:        external
213:        onlyAfterDate(startClaimDate)	// @audit-issue

226:    function claimAndStake(address _token, uint8 _percentage, Exchange _exchange, bytes calldata _data)
227:        external
228:        onlyAfterDate(startClaimDate)	// @audit-issue

315:    function convertAllETH() external onlyAuthorized onlyBeforeDate(startClaimDate) {	// @audit-issue

336:    function setOwner(address _owner) external onlyAuthorized {	// @audit-issue

348:    function setLoopAddresses(address _loopAddress, address _vaultAddress)
349:        external
350:        onlyAuthorized	// @audit-issue

348:    function setLoopAddresses(address _loopAddress, address _vaultAddress)
349:        external
350:        onlyAuthorized
351:        onlyBeforeDate(loopActivation)	// @audit-issue

364:    function allowToken(address _token) external onlyAuthorized {	// @audit-issue

372:    function setEmergencyMode(bool _mode) external onlyAuthorized {	// @audit-issue

379:    function recoverERC20(address tokenAddress, uint256 tokenAmount) external onlyAuthorized {	// @audit-issue
```
[174](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L172-L174), [213](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L211-L213), [228](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L226-L228), [315](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L315-L315), [336](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L336-L336), [350](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L348-L350), [351](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L348-L351), [364](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L364-L364), [372](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L372-L372), [379](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L379-L379), 


#### Recommendation

To mitigate centralization risks associated with privileged functions, consider implementing a multi-signature or decentralized governance mechanism. Instead of relying solely on a single owner/administrator, distribute control and decision-making authority among multiple parties or stakeholders. This approach enhances security, reduces the risk of malicious actions by a single entity, and prevents single points of failure. Explore governance solutions and smart contract frameworks that support decentralized control and decision-making to enhance the trustworthiness and resilience of your contract.

### Code does not follow the best practice of check-effects-interactions pattern
Code should follow the best-practice of [CEI](https://blockchain-academy.hs-mittweida.de/courses/solidity-coding-beginners-to-intermediate/lessons/solidity-11-coding-patterns/topic/checks-effects-interactions/), where state variables are updated before any external calls are made. Doing so prevents a large class of reentrancy bugs.

```solidity
Path: ./src/PrelaunchPoints.sol

186:            IERC20(_token).safeTransferFrom(msg.sender, address(this), _amount);
187:
188:            if (_token == address(WETH)) {
189:                WETH.withdraw(_amount);
190:                totalSupply = totalSupply + _amount;	// @audit-issue

186:            IERC20(_token).safeTransferFrom(msg.sender, address(this), _amount);
187:
188:            if (_token == address(WETH)) {
189:                WETH.withdraw(_amount);
190:                totalSupply = totalSupply + _amount;
191:                balances[_receiver][ETH] += _amount;	// @audit-issue

186:            IERC20(_token).safeTransferFrom(msg.sender, address(this), _amount);
187:
188:            if (_token == address(WETH)) {
189:                WETH.withdraw(_amount);
190:                totalSupply = totalSupply + _amount;
191:                balances[_receiver][ETH] += _amount;
192:            } else {
193:                balances[_receiver][_token] += _amount;	// @audit-issue

251:            lpETH.safeTransfer(_receiver, claimedAmount);
252:        } else {
253:            uint256 userClaim = userStake * _percentage / 100;
254:            _validateData(_token, userClaim, _exchange, _data);
255:            balances[msg.sender][_token] = userStake - userClaim;	// @audit-issue
```
[190](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L186-L190), [191](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L186-L191), [193](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L186-L193), [255](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L251-L255), 


#### Recommendation

To enhance the security of your smart contract and prevent reentrancy attacks, it's crucial to adhere to the check-effects-interactions pattern. Restructure your contract's functions to follow this pattern by first performing condition checks (checks), then updating the contract's state (effects), and finally, executing any external calls or interactions (interactions). This sequential approach reduces the risk of reentrancy vulnerabilities by ensuring that state changes are isolated from external interactions and condition checks are made before any state modifications. Review and refactor your contract code to align with this best practice for improved security.

### Missing Reentrancy Modifier
A reentrancy attack is a common vulnerability in smart contracts, particularly when a contract makes an external call to another untrusted contract and then continues to execute code afterwards. If the called contract is malicious, it can call back into the original contract in a way that causes the original function to run again, potentially leading to effects like multiple withdrawals and other unintended actions.

The absence of reentrancy guards, such as the `nonReentrant` modifier provided by OpenZeppelin's ReentrancyGuard contract, makes a function susceptible to reentrancy attacks. This modifier prevents a function from being called again until it has finished executing.

```solidity
Path: ./src/PrelaunchPoints.sol

296:            (bool sent,) = msg.sender.call{value: lockedAmount}("");	// @audit-issue
```
[296](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L296-L296), 


#### Recommendation

Consider adding `nonReentrant` modifier to given functions.

### Missing checks for `address(0)`  when updating state variables
In Solidity, the Ethereum address `0x0000000000000000000000000000000000000000` is known as the "zero address". This address has significance because it's the default value for uninitialized address variables and is often used to represent an invalid or non-existent address. The "
Missing zero address control" issue arises when a Solidity smart contract does not properly check or prevent interactions with the zero address, leading to unintended behavior.
For instance, a contract might allow tokens to be sent to the zero address without any checks, which essentially burns those tokens as they become irretrievable. While sometimes this is intentional, without proper control or checks, accidental transfers could occur.    
        

```solidity
Path: ./src/PrelaunchPoints.sol

100:        WETH = IWETH(_wethAddress);	// @audit-issue

353:        lpETH = ILpETH(_loopAddress);	// @audit-issue

354:        lpETHVault = ILpETHVault(_vaultAddress);	// @audit-issue
```
[337](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L337-L337), [353](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L353-L353), [354](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L354-L354), 


#### Recommendation

It is strongly recommended to implement checks to prevent the zero address from being set during the initialization of contracts. This can be achieved by adding require statements that ensure address parameters are not the zero address. 

### The Contract Should `approve(0)` First
Some tokens (like USDT L199) do not work when changing the allowance from an existing non-zero allowance value. They must first be approved by zero and then the actual allowance must be approved.

```solidity
Path: ./src/PrelaunchPoints.sol

231:        lpETH.approve(address(lpETHVault), claimedAmount);	// @audit-issue
```
[231](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L231-L231), 


#### Recommendation

When developing functions that update allowances for ERC20 tokens, especially when interacting with tokens known to require this pattern, implement a two-step approval process. This process first sets the allowance for a spender to zero, and then sets it to the desired new value. For example:
```solidity
function resetAndApprove(address token, address spender, uint256 amount) public {
    IERC20(token).approve(spender, 0);
    IERC20(token).approve(spender, amount);
}
```


### Int casting `block.timestamp` can reduce the lifespan of a contract
In Solidity, `block.timestamp` returns the Unix timestamp of the current block, which is a continuously increasing value. Casting `block.timestamp` to a smaller integer type, like `uint32`, can limit the maximum value it can hold. As time progresses and the Unix timestamp exceeds this maximum value, the casted value will overflow, leading to incorrect and potentially harmful behavior in the contract. This issue can significantly reduce the functional lifespan of a contract, as it becomes unreliable and potentially insecure once the timestamp exceeds the capacity of the casted integer type.

```solidity
Path: ./src/PrelaunchPoints.sol

327:        startClaimDate = uint32(block.timestamp);	// @audit-issue

355:        loopActivation = uint32(block.timestamp);	// @audit-issue
```
[327](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L327-L327), [355](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L355-L355), 


#### Recommendation

Avoid casting `block.timestamp` to smaller integer types in your Solidity contracts. Use `uint256` to store timestamp values to accommodate the increasing nature of Unix timestamps and ensure the contract remains functional in the long term. Regularly review and update your contracts to remove any unnecessary casting of `block.timestamp` that could lead to overflows and reduce the contract's lifespan. By doing so, you maintain the contract's reliability and integrity well into the future.

### Functions calling contracts/addresses with transfer hooks are missing reentrancy guards
Even if the function follows the best practice of check-effects-interaction, not using a reentrancy guard when there may be transfer hooks will open the users of this protocol up to [read-only reentrancies](https://chainsecurity.com/curve-lp-oracle-manipulation-post-mortem/) with no way to protect against it, except by block-listing the whole protocol.

```solidity
Path: ./src/PrelaunchPoints.sol

186:            IERC20(_token).safeTransferFrom(msg.sender, address(this), _amount);	// @audit-issue

251:            lpETH.safeTransfer(_receiver, claimedAmount);	// @audit-issue

302:            IERC20(_token).safeTransfer(msg.sender, lockedAmount);	// @audit-issue

383:        IERC20(tokenAddress).safeTransfer(owner, tokenAmount);	// @audit-issue
```
[186](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L186-L186), [251](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L251-L251), [302](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L302-L302), [383](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L383-L383), 


#### Recommendation

To ensure the security of your protocol and protect users from potential vulnerabilities, consider implementing reentrancy guards in functions that call contracts or addresses with transfer hooks. Even if your function follows the check-effects-interaction pattern, using reentrancy guards is essential to prevent read-only reentrancy attacks, as demonstrated in incidents like the [Curve LP Oracle Manipulation](https://chainsecurity.com/curve-lp-oracle-manipulation-post-mortem/). Implementing reentrancy guards adds an extra layer of protection and safeguards your protocol against such attacks.

### Consider using OpenZeppelin’s `SafeCast` library to prevent unexpected overflows when casting from various type int/uint values
In Solidity, when casting from `int` to `uint` or vice versa, there is a risk of unexpected overflow or underflow, especially when dealing with large values. To mitigate this risk and ensure safe type conversions, you can consider using OpenZeppelin’s `SafeCast` library. This library provides functions that check for overflow/underflow conditions before performing the cast, helping you prevent potential issues related to type conversion in your smart contracts.

```solidity
Path: ./src/PrelaunchPoints.sol

102:        loopActivation = uint32(block.timestamp + 120 days);	// @audit-issue

327:        startClaimDate = uint32(block.timestamp);	// @audit-issue

355:        loopActivation = uint32(block.timestamp);	// @audit-issue
```
[102](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L102-L102), [327](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L327-L327), [355](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L355-L355), 


#### Recommendation

To enhance the safety and reliability of your Solidity smart contracts, it is advisable to utilize OpenZeppelin’s `SafeCast` library when casting between `int` and `uint` types. Incorporating this library into your codebase will help prevent unexpected overflows and underflows during type conversion, reducing the risk of vulnerabilities and ensuring secure contract execution.

### Revert on transfer to the zero address
It's good practice to revert a token transfer transaction if the recipient's address is the zero address. This can prevent unintentional transfers to the zero address due to accidental operations or programming errors. Many token contracts implement such a safeguard, such as [OpenZeppelin - ERC20](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/9e3f4d60c581010c4a3979480e07cc7752f124cc/contracts/token/ERC20/ERC20.sol#L232), [OpenZeppelin - ERC721](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/9e3f4d60c581010c4a3979480e07cc7752f124cc/contracts/token/ERC721/ERC721.sol#L142).

```solidity
Path: ./src/PrelaunchPoints.sol

251:            lpETH.safeTransfer(_receiver, claimedAmount);	// @audit-issue

302:            IERC20(_token).safeTransfer(msg.sender, lockedAmount);	// @audit-issue

383:        IERC20(tokenAddress).safeTransfer(owner, tokenAmount);	// @audit-issue
```
[251](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L251-L251), [302](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L302-L302), [383](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L383-L383), 


#### Recommendation

To enhance the security and reliability of your token contracts, it's advisable to implement a safeguard that reverts token transfer transactions if the recipient's address is the zero address. This practice helps prevent unintentional transfers to the zero address, reducing the risk of fund loss due to accidental operations or programming errors. Many token contracts, including [OpenZeppelin's ERC20](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/9e3f4d60c581010c4a3979480e07cc7752f124cc/contracts/token/ERC20/ERC20.sol#L232) and [ERC721](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/9e3f4d60c581010c4a3979480e07cc7752f124cc/contracts/token/ERC721/ERC721.sol#L142), incorporate this safeguard for added security.

### Missing contract existence checks before low-level calls
Low-level calls return success if there is no code present at the specified address. In addition to the zero-address checks, add a check to verify that `<address>.code.length > 0`.

```solidity
Path: ./src/PrelaunchPoints.sol

296:            (bool sent,) = msg.sender.call{value: lockedAmount}("");	// @audit-issue

497:        (bool success,) = payable(exchangeProxy).call{value: 0}(_swapCallData);	// @audit-issue
```
[296](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L296-L296), [497](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L497-L497), 


#### Recommendation

To enhance the security and reliability of your Solidity smart contracts, always include contract existence checks before making low-level calls. In addition to verifying that the address is not the zero address, also confirm that `<address>.code.length > 0`. These checks help ensure that the target address corresponds to a valid and functioning contract, reducing the risk of unexpected behavior and vulnerabilities in your contract interactions.

### Use `increaseAllowance()`/`decreaseAllowance()` instead of `approve()`/`safeApprove()`
Changing an allowance with `approve()` brings the risk that someone may use both the old and the new allowance by unfortunate transaction ordering. [Refer to ERC20 API: An Attack Vector on the Approve/TransferFrom Methods](https://docs.google.com/document/d/1YLPtQxZu1UAvO9cZ1O2RPXBbT0mooh4DYKjA_jp-RLM/edit#heading=h.m9fhqynw2xvt). It is recommended to use the `increaseAllowance()`/`decreaseAllowance()` to avoid ths problem.

```solidity
Path: ./src/PrelaunchPoints.sol

231:        lpETH.approve(address(lpETHVault), claimedAmount);	// @audit-issue

495:        require(_sellToken.approve(exchangeProxy, _amount));	// @audit-issue
```
[231](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L231-L231), [495](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L495-L495), 


#### Recommendation

To enhance the security of your Solidity smart contracts and avoid potential issues related to transaction ordering, it is advisable to use the `increaseAllowance()` and `decreaseAllowance()` functions instead of `approve()` or `safeApprove()`. These functions provide a safer and more atomic way to modify allowances and mitigate the risk associated with potential attack vectors like those described in the [ERC20 API: An Attack Vector on the Approve/TransferFrom Methods](https://docs.google.com/document/d/1YLPtQxZu1UAvO9cZ1O2RPXBbT0mooh4DYKjA_jp-RLM/edit#heading=h.m9fhqynw2xvt) document.

### Critical functions should have a timelock
Critical functions, especially those affecting protocol parameters or user funds, are potential points of failure or exploitation. To mitigate risks, incorporating a timelock on such functions can be beneficial. A timelock requires a waiting period between the time an action is initiated and when it's executed, giving stakeholders time to react, potentially vetoing malicious or erroneous changes. To implement, integrate a smart contract like OpenZeppelin's `TimelockController` or build a custom mechanism. This ensures governance decisions or administrative changes are transparent and allows for community or multi-signature interventions, enhancing protocol security and trustworthiness.

```solidity
Path: ./src/PrelaunchPoints.sol

336:    function setOwner(address _owner) external onlyAuthorized {
337:        owner = _owner;	// @audit-issue

348:    function setLoopAddresses(address _loopAddress, address _vaultAddress)
349:        external
350:        onlyAuthorized
351:        onlyBeforeDate(loopActivation)
352:    {
353:        lpETH = ILpETH(_loopAddress);	// @audit-issue

348:    function setLoopAddresses(address _loopAddress, address _vaultAddress)
349:        external
350:        onlyAuthorized
351:        onlyBeforeDate(loopActivation)
352:    {
353:        lpETH = ILpETH(_loopAddress);
354:        lpETHVault = ILpETHVault(_vaultAddress);	// @audit-issue

372:    function setEmergencyMode(bool _mode) external onlyAuthorized {
373:        emergencyMode = _mode;	// @audit-issue
```
[337](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L336-L337), [353](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L348-L353), [354](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L348-L354), [373](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L372-L373), 


#### Recommendation

Integrate a timelock mechanism into your Solidity contracts for critical functions, especially those controlling protocol parameters or managing user funds. Consider using established solutions like OpenZeppelin's `TimelockController` for robustness and reliability. Alternatively, develop a custom timelock mechanism tailored to your specific requirements. Ensure that the timelock duration is appropriate for your contract's use case and stakeholder needs, providing sufficient time for review and intervention. Clearly document and communicate the presence and workings of the timelock mechanism to stakeholders to maintain transparency and trust in your contract's operations.

### Return values of `approve` not checked.
Not all `IERC20` implementations `revert` when there's a failure in `approve`. The function signature has a boolean return value and they indicate errors that way instead.

By not checking the return value, operations that should have marked as failed, may potentially go through without actually approving anything. 


```solidity
Path: ./src/PrelaunchPoints.sol

231:        lpETH.approve(address(lpETHVault), claimedAmount);	// @audit-issue

495:        require(_sellToken.approve(exchangeProxy, _amount));	// @audit-issue
```
[231](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L231-L231), [495](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L495-L495), 


#### Recommendation

To ensure the accuracy of token approvals and prevent potential issues, it's important to check the return values when calling the `approve` function of an `IERC20` contract. While some implementations use a boolean return value to indicate errors, not checking this return value may allow operations to proceed without the intended approval. Always verify the return value to confirm the success of the approval operation.

### Large transfers may not work with some `ERC20` tokens
Not all IERC20 implementations are totally compliant, and some (e.g UNI, COMP) may fail if the valued passed is larger than uint96. [Source](https://github.com/d-xo/weird-erc20#revert-on-large-approvals--transfers)


```solidity
Path: ./src/PrelaunchPoints.sol

186:            IERC20(_token).safeTransferFrom(msg.sender, address(this), _amount);	// @audit-issue

251:            lpETH.safeTransfer(_receiver, claimedAmount);	// @audit-issue

302:            IERC20(_token).safeTransfer(msg.sender, lockedAmount);	// @audit-issue

383:        IERC20(tokenAddress).safeTransfer(owner, tokenAmount);	// @audit-issue
```
[186](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L186-L186), [251](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L251-L251), [302](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L302-L302), [383](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L383-L383), 


#### Recommendation

When transferring tokens using ERC-20 contracts, such as UNI or COMP, it's important to be aware that not all IERC20 implementations are fully compliant, and they may not support large transfer values exceeding uint96. To avoid potential issues, it's essential to check the documentation or source code of the specific token you're using and ensure that your transfers adhere to the token's limitations.

### Large approvals may not work with some `ERC20` tokens
Not all IERC20 implementations are totally compliant, and some (e.g UNI, COMP) may fail if the valued passed is larger than uint96. [Source](https://github.com/d-xo/weird-erc20#revert-on-large-approvals--transfers)

```solidity
Path: ./src/PrelaunchPoints.sol

231:        lpETH.approve(address(lpETHVault), claimedAmount);	// @audit-issue

495:        require(_sellToken.approve(exchangeProxy, _amount));	// @audit-issue
```
[231](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L231-L231), [495](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L495-L495), 


#### Recommendation

When working with ERC-20 tokens, particularly tokens like UNI or COMP, be aware that not all IERC20 implementations are fully compliant, and they may not support large approval values exceeding uint96. To avoid potential issues, it's essential to check the documentation or source code of the specific token you're interacting with and ensure that your approvals conform to the token's limitations.

### Unsafe ERC20 operation `approve()`
Approve call do not handle non-standard erc20 behavior. Use `safeApprove` instead of `approve`.
- Some token contracts do not return any value.
- Some token contracts revert the transaction when the allowance is not zero.

```solidity
Path: ./src/PrelaunchPoints.sol

231:        lpETH.approve(address(lpETHVault), claimedAmount);	// @audit-issue

495:        require(_sellToken.approve(exchangeProxy, _amount));	// @audit-issue
```
[231](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L231-L231), [495](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L495-L495), 


#### Recommendation

Use `safeApprove` instead of `approve`

### Possible loss of precision
Division by large numbers may result in precision loss due to rounding down, or even the result being erroneously equal to zero. Consider adding checks on the numerator to ensure precision loss is handled appropriately.


```solidity
Path: ./src/PrelaunchPoints.sol

249:            claimedAmount = userStake.mulDiv(totalLpETH, totalSupply);	// @audit-issue
```
[249](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L249-L249), 


#### Recommendation

Incorporate strategies in your Solidity contracts to mitigate precision loss in division operations. This can include:
1. Performing checks on the numerator and denominator to ensure they are within a reasonable range to avoid significant rounding errors.
2. Considering the use of fixed-point arithmetic libraries or scaling factors to handle divisions with higher precision.
3. Clearly documenting any inherent limitations of your division logic and providing guidelines for inputs to minimize unexpected behavior.
Always thoroughly test division operations under various scenarios to ensure that the outcomes are consistent with your contract's intended logic and accuracy requirements.

### `approve` can revert if the current approval is not zero
Some tokens like USDT check for the current approval, and they revert if it's not zero. While Tether is known to do this, it applies to other tokens as well, which are trying to protect against [this](https://docs.google.com/document/d/1YLPtQxZu1UAvO9cZ1O2RPXBbT0mooh4DYKjA_jp-RLM/edit) attack vector.

```solidity
Path: ./src/PrelaunchPoints.sol

231:        lpETH.approve(address(lpETHVault), claimedAmount);	// @audit-issue

495:        require(_sellToken.approve(exchangeProxy, _amount));	// @audit-issue
```
[231](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L231-L231), [495](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L495-L495), 


#### Recommendation

When working with tokens that may revert when calling the `approve` function, be cautious and ensure that the current approval is set to zero before attempting to set a new approval. Tokens like USDT and others may follow this behavior to protect against potential attack vectors. Always check the token's documentation and handle approvals accordingly to avoid unexpected reverts.

### `approve` will always revert as the `IERC20` interface mismatch
Some tokens, such as [USDT](https://etherscan.io/token/0xdac17f958d2ee523a2206206994597c13d831ec7#code#L199), have a different implementation for the approve function: when the address is cast to a compliant `IERC20` interface and the approve function is used, it will always revert due to the interface mismatch.

```solidity
Path: ./src/PrelaunchPoints.sol

231:        lpETH.approve(address(lpETHVault), claimedAmount);	// @audit-issue

495:        require(_sellToken.approve(exchangeProxy, _amount));	// @audit-issue
```
[231](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L231-L231), [495](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L495-L495), 


#### Recommendation

When interacting with tokens like USDT that have a different implementation for the `approve` function, ensure that you use the correct interface and method to avoid reverts due to interface mismatches. Instead of casting to `IERC20`, use the specific interface that matches the token's implementation. Always review the token's documentation or contract to determine the correct interface and method to use when working with such tokens.

### Non-compliant `IERC20` tokens may revert with `transfer`
Some `IERC20` tokens (e.g. `BNB`, `OMG`, `USDT`) do not implement the standard properly, but they are still accepted by most code that accepts `ERC20` tokens.

For example, `USDT` transfer functions on L1 do not return booleans: when casted to `IERC20`, their function signatures do not match, and therefore the calls made will revert.

```solidity
Path: ./src/PrelaunchPoints.sol

186:            IERC20(_token).safeTransferFrom(msg.sender, address(this), _amount);	// @audit-issue

251:            lpETH.safeTransfer(_receiver, claimedAmount);	// @audit-issue

302:            IERC20(_token).safeTransfer(msg.sender, lockedAmount);	// @audit-issue

383:        IERC20(tokenAddress).safeTransfer(owner, tokenAmount);	// @audit-issue
```
[186](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L186-L186), [251](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L251-L251), [302](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L302-L302), [383](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L383-L383), 


#### Recommendation

When dealing with tokens that may not fully comply with the `IERC20` standard, it's important to exercise caution and carefully verify the behavior of the specific token in question. In cases where tokens do not return booleans for transfer functions, consider implementing additional error-handling mechanisms to account for potential failures. This may include checking the token balance before and after the transfer to detect any discrepancies or using try-catch blocks to handle potential exceptions. Ensuring robust error handling can help your smart contract interact more gracefully with non-compliant tokens while maintaining security and reliability.

### Use transfer libraries instead of low level calls
Consider using `SafeTransferLib.safeTransferETH` or `Address.sendValue` for clearer semantic meaning instead of using a low level call.

```solidity
Path: ./src/PrelaunchPoints.sol

296:            (bool sent,) = msg.sender.call{value: lockedAmount}("");	// @audit-issue

497:        (bool success,) = payable(exchangeProxy).call{value: 0}(_swapCallData);	// @audit-issue
```
[296](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L296-L296), [497](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L497-L497), 


#### Recommendation

To improve code readability and safety, consider using higher-level transfer libraries like `SafeTransferLib.safeTransferETH` or `Address.sendValue` instead of low-level calls for handling Ether transfers. These libraries provide clearer semantic meaning and help prevent common pitfalls associated with low-level calls.

### Contract uses both `require()`/`revert()` as well as custom errors
Consider using just one method in a single file

```solidity
Path: ./src/PrelaunchPoints.sol

16:contract PrelaunchPoints {	// @audit-issue
```
[16](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L16-L16), 


#### Recommendation

Consistently use either `require()`/`revert()` or custom errors for error handling in your contract to maintain code consistency and readability. Using both methods in the same contract can lead to confusion and make the code harder to understand.

### Custom error has no error details
Consider adding parameters to the error to indicate which user or values caused the failure

```solidity
Path: ./src/PrelaunchPoints.sol

70:    error InvalidToken();	// @audit-issue

71:    error NothingToClaim();	// @audit-issue

72:    error TokenNotAllowed();	// @audit-issue

73:    error CannotLockZero();	// @audit-issue

74:    error CannotWithdrawZero();	// @audit-issue

75:    error UseClaimInstead();	// @audit-issue

76:    error FailedToSendEther();	// @audit-issue

77:    error SwapCallFailed();	// @audit-issue

82:    error WrongExchange();	// @audit-issue

83:    error LoopNotActivated();	// @audit-issue

84:    error NotValidToken();	// @audit-issue

85:    error NotAuthorized();	// @audit-issue

86:    error CurrentlyNotPossible();	// @audit-issue

87:    error NoLongerPossible();	// @audit-issue
```
[70](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L70-L70), [71](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L71-L71), [72](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L72-L72), [73](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L73-L73), [74](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L74-L74), [75](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L75-L75), [76](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L76-L76), [77](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L77-L77), [82](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L82-L82), [83](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L83-L83), [84](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L84-L84), [85](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L85-L85), [86](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L86-L86), [87](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L87-L87), 


#### Recommendation

When defining custom errors, consider adding parameters or error details that provide information about the specific conditions or inputs that caused the error. Including error details can make debugging and troubleshooting easier by providing context on the cause of the failure.

### Constants in comparisons should appear on the left side
Doing so will prevent [typo bugs](https://www.moserware.com/2008/01/constants-on-left-are-better-but-this.html)

```solidity
Path: ./src/PrelaunchPoints.sol

176:        if (_amount == 0) {	// @audit-issue

245:        if (userStake == 0) {	// @audit-issue

287:        if (lockedAmount == 0) {	// @audit-issue
```
[176](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L176-L176), [245](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L245-L245), [287](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L287-L287), 


#### Recommendation

To prevent typo bugs and improve code readability, it's advisable to place constants on the left side of comparisons. This coding practice helps catch accidental assignment (=) instead of comparison (==) and enhances code quality.

### Style guide: Function ordering does not follow the Solidity style guide
According to the Solidity [style guide](https://docs.soliditylang.org/en/v0.8.17/style-guide.html#order-of-functions), functions should be laid out in the following order :`constructor()`, `receive()`, `fallback()`, `external`, `public`, `internal`, `private`, but the cases below do not follow this pattern

```solidity
Path: ./src/PrelaunchPoints.sol

211:    function claim(address _token, uint8 _percentage, Exchange _exchange, bytes calldata _data)	// @audit-issue: claim should come before than _processLock
```
[211](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L211-L211), 


#### Recommendation

Follow the official [Solidity guidelines](https://docs.soliditylang.org/en/latest/style-guide.html).

### Style guide: Contract does not follow the Solidity style guide's suggested layout ordering
The [style guide](https://docs.soliditylang.org/en/latest/style-guide.html#order-of-layout) says that, within a contract, the ordering should be 1) Type declarations, 2) State variables, 3) Events, 4) Errors, 5) Modifiers, and 6) Functions, but the contract(s) below do not follow this ordering


```solidity
Path: ./src/PrelaunchPoints.sol

16:contract PrelaunchPoints {	// @audit-issue : Order layout of this contract is not correct. It should be like this: 
	1 -> using Math
	2 -> using SafeERC20
	3 -> using SafeERC20
	4 -> lpETH
	5 -> lpETHVault
	6 -> WETH
	7 -> ETH
	8 -> exchangeProxy
	9 -> owner
	10 -> totalSupply
	11 -> totalLpETH
	12 -> isTokenAllowed
	13 -> UNI_SELECTOR
	14 -> TRANSFORM_SELECTOR
	15 -> loopActivation
	16 -> startClaimDate
	17 -> TIMELOCK
	18 -> emergencyMode
	19 -> balances
	20 -> Locked
	21 -> StakedVault
	22 -> Converted
	23 -> Withdrawn
	24 -> Claimed
	25 -> Recovered
	26 -> OwnerUpdated
	27 -> LoopAddressesUpdated
	28 -> SwappedTokens
	29 -> InvalidToken
	30 -> NothingToClaim
	31 -> TokenNotAllowed
	32 -> CannotLockZero
	33 -> CannotWithdrawZero
	34 -> UseClaimInstead
	35 -> FailedToSendEther
	36 -> SwapCallFailed
	37 -> WrongSelector
	38 -> WrongDataTokens
	39 -> WrongDataAmount
	40 -> WrongRecipient
	41 -> WrongExchange
	42 -> LoopNotActivated
	43 -> NotValidToken
	44 -> NotAuthorized
	45 -> CurrentlyNotPossible
	46 -> NoLongerPossible
	47 -> onlyAuthorized
	48 -> onlyAfterDate
	49 -> onlyBeforeDate
	50 -> constructor
	51 -> lockETH
	52 -> lockETHFor
	53 -> lock
	54 -> lockFor
	55 -> _processLock
	56 -> claim
	57 -> claimAndStake
	58 -> _claim
	59 -> withdraw
	60 -> convertAllETH
	61 -> setOwner
	62 -> setLoopAddresses
	63 -> allowToken
	64 -> setEmergencyMode
	65 -> recoverERC20
	66 -> receive
	67 -> _validateData
	68 -> _decodeUniswapV3Data
	69 -> _decodeTransformERC20Data
	70 -> _fillQuote

```
[16](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L16-L16), 


#### Recommendation

Follow the official [Solidity guidelines](https://docs.soliditylang.org/en/v0.8.17/style-guide.html).

### Duplicated `require()`/`revert()` checks should be refactored to a modifier or function
In Solidity contracts, it's common to encounter duplicated `require()` or `revert()` statements across multiple functions. These statements are crucial for validating conditions and ensuring contract integrity. However, repeated checks can lead to code redundancy, making the contract larger and more difficult to maintain. This redundancy can be streamlined by encapsulating the repeated logic in a modifier or a dedicated function. By doing so, the code becomes more concise, easier to audit, and more gas-efficient. Furthermore, centralizing the validation logic in a single location makes the codebase more adaptable to changes and reduces the risk of inconsistencies or errors in future updates.

```solidity
Path: ./src/PrelaunchPoints.sol

144:        if (_token == ETH) {	// @audit-issue: Same if statement in line(s): ['158']
```
[144](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L144-L144), 


#### Recommendation

Consolidate repeated `require()` or `revert()` checks in Solidity by creating a modifier or a separate function. Apply this modifier to all functions requiring the same validation, or call the function at the beginning of each relevant function. This refactoring enhances contract efficiency, maintainability, and consistency, while potentially reducing gas costs associated with deploying and executing redundant code.

### Consider using `delete` rather than assigning zero to clear values
The `delete` keyword more closely matches the semantics of what is being done, and draws more attention to the changing of state, which may lead to a more thorough audit of its associated logic.

```solidity
Path: ./src/PrelaunchPoints.sol

250:            balances[msg.sender][_token] = 0;	// @audit-issue

285:        balances[msg.sender][_token] = 0;	// @audit-issue
```
[250](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L250-L250), [285](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L285-L285), 


#### Recommendation

When you need to clear or reset values in storage variables, consider using the `delete` keyword instead of manually assigning zero or other values. Using `delete` provides a more explicit and efficient way to clear storage variables. For example, instead of `myVariable = 0;`, you can use `delete myVariable;` to clear the value.

### Dependence on external protocols
External protocols should be monitored as such dependencies may introduce vulnerabilities if a vulnerability is found /introduced in the external protocol

```solidity
Path: ./src/PrelaunchPoints.sol

4:import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";	// @audit-issue

5:import "@openzeppelin/contracts/utils/math/Math.sol";	// @audit-issue
```
[4](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L4-L4), [5](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L5-L5), 


#### Recommendation

Regularly monitor and review the external protocols your Solidity contracts depend on for any updates or identified vulnerabilities. Consider implementing fallback mechanisms or contingency plans in your contracts to handle potential failures or compromises in these external protocols. Additionally, thoroughly audit and test the integration points with external protocols to ensure they adhere to your contract's security standards. Where possible, design your contract architecture to minimize reliance on external protocols or allow for upgradability in response to changes in these dependencies. Stay informed about developments in the protocols you depend on and actively participate in their community for early awareness of potential issues.

### Incorrect withdraw declaration
In Solidity, it's essential for clarity and interoperability to correctly specify return types in function declarations. If the `withdraw` function is expected to return a `bool` to indicate success or failure, its omission could lead to ambiguity or unexpected behavior when interacting with or calling this function from other contracts or off-chain systems. Missing return values can mislead developers and potentially lead to contract integrations built on incorrect assumptions. To resolve this, the function declaration for `withdraw` should be modified to explicitly include the `bool` return type, ensuring clarity and correctness in contract interactions.

```solidity
Path: ./src/PrelaunchPoints.sol

274:    function withdraw(address _token) external {	// @audit-issue
```
[274](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L274-L274), 


#### Recommendation

Review the function declarations in your Solidity contracts, particularly for critical operations like `withdraw`, to ensure that they correctly specify return types. If a function is intended to indicate success or failure, it should explicitly declare a `bool` return type in its signature. For example, modify the `withdraw` function declaration from:
```solidity
function withdraw(uint256 amount) public {
    // Implementation
}

// to

function withdraw(uint256 amount) public returns (bool) {
    // Implementation
    return true; // Indicate success
}
```


### Bug in Deduplication of Verbatim Blocks
The current Solidity version has the [Bug in Deduplication of Verbatim Blocks](https://soliditylang.org/blog/2023/11/08/verbatim-invalid-deduplication-bug/) issue.

```solidity
Path: ./src/PrelaunchPoints.sol

2:pragma solidity 0.8.20;	// @audit-issue
```
[2](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L2-L2), 


#### Recommendation

It is recommended to follow the fix provided in the [link](https://soliditylang.org/blog/2023/11/08/verbatim-invalid-deduplication-bug/).

### Style Guide: Surround top level declarations in Solidity source with two blank lines.
1- Surround top level declarations in Solidity source with two blank lines.
2- Within a contract surround function declarations with a single blank line.


```solidity
Path: ./src/PrelaunchPoints.sol

87:    error NoLongerPossible();
88:
89:    /*//////////////////////////////////////////////////////////////
90:                             INITIALIZATION
91:    //////////////////////////////////////////////////////////////*/
92:    /**
93:     * @param _exchangeProxy address of the 0x protocol exchange proxy
94:     * @param _wethAddress   address of WETH
95:     * @param _allowedTokens list of token addresses to allow for locking
96:     */	// @audit-issue: There should be single blank line between function declarations.
97:    constructor(address _exchangeProxy, address _wethAddress, address[] memory _allowedTokens) {

114:    }
115:
116:    /*//////////////////////////////////////////////////////////////
117:                            STAKE FUNCTIONS
118:    //////////////////////////////////////////////////////////////*/
119:
120:    /**
121:     * @notice Locks ETH
122:     * @param _referral  info of the referral. This value will be processed in the backend.
123:     */	// @audit-issue: There should be single blank line between function declarations.
124:    function lockETH(bytes32 _referral) external payable {

126:    }
127:
128:    /**
129:     * @notice Locks ETH for a given address
130:     * @param _for       address for which ETH is locked
131:     * @param _referral  info of the referral. This value will be processed in the backend.
132:     */	// @audit-issue: There should be single blank line between function declarations.
133:    function lockETHFor(address _for, bytes32 _referral) external payable {

135:    }
136:
137:    /**
138:     * @notice Locks a valid token
139:     * @param _token     address of token to lock
140:     * @param _amount    amount of token to lock
141:     * @param _referral  info of the referral. This value will be processed in the backend.
142:     */	// @audit-issue: There should be single blank line between function declarations.
143:    function lock(address _token, uint256 _amount, bytes32 _referral) external {

148:    }
149:
150:    /**
151:     * @notice Locks a valid token for a given address
152:     * @param _token     address of token to lock
153:     * @param _amount    amount of token to lock
154:     * @param _for       address for which ETH is locked
155:     * @param _referral  info of the referral. This value will be processed in the backend.
156:     */	// @audit-issue: There should be single blank line between function declarations.
157:    function lockFor(address _token, uint256 _amount, address _for, bytes32 _referral) external {

162:    }
163:
164:    /**
165:     * @dev Generic internal locking function that updates rewards based on
166:     *      previous balances, then update balances.
167:     * @param _token       Address of the token to lock
168:     * @param _amount      Units of ETH or token to add to the users balance
169:     * @param _receiver    Address of user who will receive the stake
170:     * @param _referral    Address of the referral user
171:     */	// @audit-issue: There should be single blank line between function declarations.
172:    function _processLock(address _token, uint256 _amount, address _receiver, bytes32 _referral)

198:    }
199:
200:    /*//////////////////////////////////////////////////////////////
201:                        CLAIM AND WITHDRAW FUNCTIONS
202:    //////////////////////////////////////////////////////////////*/
203:
204:    /**
205:     * @dev Called by a user to get their vested lpETH
206:     * @param _token      Address of the token to convert to lpETH
207:     * @param _percentage Proportion in % of tokens to withdraw. NOT useful for ETH
208:     * @param _exchange   Exchange identifier where the swap takes place
209:     * @param _data       Swap data obtained from 0x API
210:     */	// @audit-issue: There should be single blank line between function declarations.
211:    function claim(address _token, uint8 _percentage, Exchange _exchange, bytes calldata _data)

216:    }
217:
218:    /**
219:     * @dev Called by a user to get their vested lpETH and stake them in a
220:     *      Loop vault for extra rewards
221:     * @param _token      Address of the token to convert to lpETH
222:     * @param _percentage Proportion in % of tokens to withdraw. NOT useful for ETH
223:     * @param _exchange   Exchange identifier where the swap takes place
224:     * @param _data       Swap data obtained from 0x API
225:     */	// @audit-issue: There should be single blank line between function declarations.
226:    function claimAndStake(address _token, uint8 _percentage, Exchange _exchange, bytes calldata _data)

235:    }
236:
237:    /**
238:     * @dev Claim logic. If necessary converts token to ETH before depositing into lpETH contract.
239:     */	// @audit-issue: There should be single blank line between function declarations.
240:    function _claim(address _token, address _receiver, uint8 _percentage, Exchange _exchange, bytes calldata _data)

266:    }
267:
268:    /**
269:     * @dev Called by a staker to withdraw all their ETH or LRT
270:     * Note Can only be called after the loop address is set and before claiming lpETH,
271:     * i.e. for at least TIMELOCK. In emergency mode can be called at any time.
272:     * @param _token      Address of the token to withdraw
273:     */	// @audit-issue: There should be single blank line between function declarations.
274:    function withdraw(address _token) external {

306:    }
307:
308:    /*//////////////////////////////////////////////////////////////
309:                            PROTECTED FUNCTIONS
310:    //////////////////////////////////////////////////////////////*/
311:
312:    /**
313:     * @dev Called by a owner to convert all the locked ETH to get lpETH
314:     */	// @audit-issue: There should be single blank line between function declarations.
315:    function convertAllETH() external onlyAuthorized onlyBeforeDate(startClaimDate) {

330:    }
331:
332:    /**
333:     * @notice Sets a new owner
334:     * @param _owner address of the new owner
335:     */	// @audit-issue: There should be single blank line between function declarations.
336:    function setOwner(address _owner) external onlyAuthorized {

340:    }
341:
342:    /**
343:     * @notice Sets the lpETH contract address
344:     * @param _loopAddress address of the lpETH contract
345:     * @dev Can only be set once before 120 days have passed from deployment.
346:     *      After that users can only withdraw ETH.
347:     */	// @audit-issue: There should be single blank line between function declarations.
348:    function setLoopAddresses(address _loopAddress, address _vaultAddress)

358:    }
359:
360:    /**
361:     * @param _token address of a wrapped LRT token
362:     * @dev ONLY add wrapped LRT tokens. Contract not compatible with rebase tokens.
363:     */	// @audit-issue: There should be single blank line between function declarations.
364:    function allowToken(address _token) external onlyAuthorized {

366:    }
367:
368:    /**
369:     * @param _mode boolean to activate/deactivate the emergency mode
370:     * @dev On emergency mode all withdrawals are accepted at
371:     */	// @audit-issue: There should be single blank line between function declarations.
372:    function setEmergencyMode(bool _mode) external onlyAuthorized {

374:    }
375:
376:    /**
377:     * @dev Allows the owner to recover other ERC20s mistakingly sent to this contract
378:     */	// @audit-issue: There should be single blank line between function declarations.
379:    function recoverERC20(address tokenAddress, uint256 tokenAmount) external onlyAuthorized {

386:    }
387:
388:    /**
389:     * Enable receive ETH
390:     * @dev ETH sent to this contract directly will be locked forever.
391:     */	// @audit-issue: There should be single blank line between function declarations.
392:    receive() external payable {}

392:    receive() external payable {}
393:
394:    /*//////////////////////////////////////////////////////////////
395:                            INTERNAL FUNCTIONS
396:    //////////////////////////////////////////////////////////////*/
397:
398:    /**
399:     * @notice Validates the data sent from 0x API to match desired behaviour
400:     * @param _token     address of the token to sell
401:     * @param _amount    amount of token to sell
402:     * @param _exchange  exchange identifier where the swap takes place
403:     * @param _data      swap data from 0x API
404:     */	// @audit-issue: There should be single blank line between function declarations.
405:    function _validateData(address _token, uint256 _amount, Exchange _exchange, bytes calldata _data) internal view {

442:    }
443:
444:    /**
445:     * @notice Decodes the data sent from 0x API when UniswapV3 is used
446:     * @param _data      swap data from 0x API
447:     */	// @audit-issue: There should be single blank line between function declarations.
448:    function _decodeUniswapV3Data(bytes calldata _data)

464:    }
465:
466:    /**
467:     * @notice Decodes the data sent from 0x API when other exchanges are used via 0x TransformERC20 function
468:     * @param _data      swap data from 0x API
469:     */	// @audit-issue: There should be single blank line between function declarations.
470:    function _decodeTransformERC20Data(bytes calldata _data)

482:    }
483:
484:    /**
485:     *
486:     * @param _sellToken     The `sellTokenAddress` field from the API response.
487:     * @param _amount       The `sellAmount` field from the API response.
488:     * @param _swapCallData  The `data` field from the API response.
489:     */
490:	// @audit-issue: There should be single blank line between function declarations.
491:    function _fillQuote(IERC20 _sellToken, uint256 _amount, bytes calldata _swapCallData) internal {

505:    }
506:
507:    /*//////////////////////////////////////////////////////////////
508:                                MODIFIERS
509:    //////////////////////////////////////////////////////////////*/
510:	// @audit-issue: There should be single blank line between function declarations.
511:    modifier onlyAuthorized() {
```
[96](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L87-L97), [123](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L114-L124), [132](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L126-L133), [142](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L135-L143), [156](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L148-L157), [171](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L162-L172), [210](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L198-L211), [225](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L216-L226), [239](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L235-L240), [273](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L266-L274), [314](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L306-L315), [335](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L330-L336), [347](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L340-L348), [363](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L358-L364), [371](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L366-L372), [378](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L374-L379), [391](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L386-L392), [404](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L392-L405), [447](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L442-L448), [469](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L464-L470), [490](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L482-L491), [510](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L505-L511), 


#### Recommendation

Follow the official [Solidity guidelines](https://docs.soliditylang.org/en/latest/style-guide.html#blank-lines).

### Lack of index element validation in function
There's no validation to check whether the index element provided as an argument actually exists in the call. This omission could lead to unintended behavior if an element that does not exist in the call is passed to the function. The function should validate that the provided index element exists in the call before proceeding.

```solidity
Path: ./src/PrelaunchPoints.sol

108:            isTokenAllowed[_allowedTokens[i]] = true;	// @audit-issue
```
[108](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L108-L108), 


#### Recommendation

Integrate explicit index validation checks at the beginning of functions that operate based on index elements. Use conditional statements to verify that the provided index falls within the valid range of existing elements. For array operations, ensure the index is less than the array's length. For mappings, consider additional logic to confirm the presence of a key. For example, in an array-based function:
```solidity
function getElementByIndex(uint256 index) public view returns (ElementType) {
    require(index < array.length, "Index out of bounds");
    return array[index];
}
```


### Contract should expose an `interface`
All `external`/`public` functions should extend an `interface`. This is useful to make sure that the whole API is extracted.


```solidity
Path: ./src/PrelaunchPoints.sol

16:contract PrelaunchPoints {	// @audit-issue
```
[16](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L16-L16), 


#### Recommendation

Consider defining an `interface` that includes all `external`/`public` functions of the contract. Exposing a well-defined interface helps ensure that the entire API is extracted and provides a clear and standardized way for other contracts or users to interact with your contract.

### Contract timekeeping will break earlier than the Ethereum network itself will stop working
When a timestamp is downcast from `uint256` to `uint32`, the value will wrap in the year 2106, and the contracts will break. Other downcasts will have different endpoints. Consider whether your contract is intended to live past the size of the type being used.

```solidity
Path: ./src/PrelaunchPoints.sol

102:        loopActivation = uint32(block.timestamp + 120 days);	// @audit-issue

327:        startClaimDate = uint32(block.timestamp);	// @audit-issue

355:        loopActivation = uint32(block.timestamp);	// @audit-issue
```
[102](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L102-L102), [327](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L327-L327), [355](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L355-L355), 


#### Recommendation

Carefully consider the choice of data types for timestamps in your Solidity contracts. If downcasting from `uint256` to smaller types like `uint32`, be aware of the overflow risks and the potential for the contract to break prematurely. For contracts intended to operate beyond these time limits, use larger integer types capable of representing dates far into the future. Additionally, document any decisions around the use of specific timestamp types and their implications for the contract's longevity and reliability. Regularly review and update your contracts to ensure that they remain robust against future changes in the Ethereum network and the broader blockchain ecosystem.

### Inefficient Array Usage
Use mappings instead of arrays for managing lists of data in order to conserve gas. Mappings are less expensive and more efficient for accessing any value without having to iterate through an array.

```solidity
Path: ./src/PrelaunchPoints.sol

97:    constructor(address _exchangeProxy, address _wethAddress, address[] memory _allowedTokens) {	// @audit-issue
```
[97](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L97-L97), 


#### Recommendation

In scenarios where data access efficiency is critical, prefer using mappings over arrays in Solidity contracts. Mappings offer more efficient and gas-effective data retrieval and updates, especially when dealing with large or frequently accessed datasets. Ensure to structure your data and choose keys thoughtfully to maximize the efficiency gains offered by mappings. While arrays might be suitable for ordered data or when the entire dataset needs to be iterated, for most other use cases, mappings are likely to be the more gas-efficient choice.

### Lack of specific import identifier
It is better to use `import {<identifier>} from "<file.sol>"` instead of `import "<file.sol>"` to improve readability and speed up the compilation time.

```solidity
Path: ./src/PrelaunchPoints.sol

4:import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";	// @audit-issue

5:import "@openzeppelin/contracts/utils/math/Math.sol";	// @audit-issue
```
[4](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L4-L4), [5](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L5-L5), 


#### Recommendation

To improve code clarity and avoid naming conflicts, it's recommended to use specific import identifiers when importing from other contracts or libraries. Instead of using `import "<file.sol>";`, specify the desired identifiers using `import { <identifier1>, <identifier2> } from "<file.sol>";`. This not only enhances readability but also can speed up compilation times by only importing the necessary symbols.

### Returning a struct instead of returning many variables is better
If a function returns [too many variables](https://docs.soliditylang.org/en/latest/contracts.html#returning-multiple-values), replacing them with a struct can improve code readability, maintainability and reusability.

```solidity
Path: ./src/PrelaunchPoints.sol

448:    function _decodeUniswapV3Data(bytes calldata _data)
449:        internal
450:        pure
451:        returns (address inputToken, address outputToken, uint256 inputTokenAmount, address recipient, bytes4 selector)	// @audit-issue

470:    function _decodeTransformERC20Data(bytes calldata _data)
471:        internal
472:        pure
473:        returns (address inputToken, address outputToken, uint256 inputTokenAmount, bytes4 selector)	// @audit-issue
```
[451](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L448-L451), [473](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L470-L473), 


#### Recommendation

Consider returning a struct instead of multiple variables when a function returns many variables. This can enhance code readability, maintainability, and reusability, as well as make the contract interface more user-friendly.

### Consider using a `struct` rather than having many function input parameters
In Solidity, functions with a large number of input parameters can become cumbersome to manage and call. This can lead to readability issues and increased likelihood of errors, especially if the order of parameters is complex or not intuitive. To streamline this, consider consolidating multiple parameters into a single `struct`. This approach not only simplifies function signatures but also enhances code clarity. Structs allow for grouping related data together, making it easier to understand the relationship between parameters and manage them as a single entity.

```solidity
Path: ./src/PrelaunchPoints.sol

240:    function _claim(address _token, address _receiver, uint8 _percentage, Exchange _exchange, bytes calldata _data)
```
[266](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L240-L240), 


#### Recommendation

When faced with functions having numerous input parameters, refactor them to accept a `struct` instead. Define a `struct` that encapsulates all these parameters, thereby simplifying the function signature and improving code readability and maintainability. This method is particularly effective in complex functions or those with parameters that are logically related, making the code more intuitive and less error-prone.

### Ensure Events Emission Prior to External Calls to Prevent Out-of-Order Issues
It's essential to ensure that events follow the best practice of check-effects-interaction and are emitted before any external calls to prevent out-of-order events due to reentrancy. Emitting events post external interactions may cause them to be out of order due to reentrancy, which can be misleading or erroneous for event listeners. [Refer to the Solidity Documentation for best practices](https://solidity.readthedocs.io/en/latest/security-considerations.html#reentrancy).

```solidity
Path: ./src/PrelaunchPoints.sol

296:            (bool sent,) = msg.sender.call{value: lockedAmount}("");
297:
298:            if (!sent) {
299:                revert FailedToSendEther();
300:            }
301:        } else {
302:            IERC20(_token).safeTransfer(msg.sender, lockedAmount);
303:        }
304:
305:        emit Withdrawn(msg.sender, _token, lockedAmount);	// @audit-issue

497:        (bool success,) = payable(exchangeProxy).call{value: 0}(_swapCallData);
498:        if (!success) {
499:            revert SwapCallFailed();
500:        }
501:
502:        // Use our current buyToken balance to determine how much we've bought.
503:        boughtETHAmount = address(this).balance - boughtETHAmount;
504:        emit SwappedTokens(address(_sellToken), _amount, boughtETHAmount);	// @audit-issue
```
[305](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L296-L305), [504](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L497-L504), 


#### Recommendation

To prevent out-of-order events and ensure consistency, always emit events before making any external calls or interactions within your smart contracts. This practice adheres to the check-effects-interaction pattern and helps provide a clear and accurate event log for event listeners. Following this approach enhances the reliability and predictability of your smart contract's behavior.

### Missing events in sensitive functions
Events should be emitted when sensitive changes are made to the contracts, but some functions lack them.

```solidity
Path: ./src/PrelaunchPoints.sol

364:    function allowToken(address _token) external onlyAuthorized {	// @audit-issue

372:    function setEmergencyMode(bool _mode) external onlyAuthorized {	// @audit-issue
```
[364](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L364-L364), [372](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L372-L372), 


#### Recommendation

To enhance transparency and auditability, ensure that events are emitted when sensitive changes are made to the contracts. Review and update functions that lack event emissions, especially in cases where sensitive operations or state changes occur, to provide a clear record of such actions.

### Include sender information in events
When an action is triggered based on a user's action, not being able to filter based on who triggered the action makes event processing a lot more cumbersome. Including the `msg.sender` the events of these types of action will make events much more useful to end users, especially when `msg.sender` is not `tx.origin`.

```solidity
Path: ./src/PrelaunchPoints.sol

197:        emit Locked(_receiver, _amount, _token, _referral);	// @audit-issue

329:        emit Converted(totalBalance, totalLpETH);	// @audit-issue

339:        emit OwnerUpdated(_owner);	// @audit-issue

357:        emit LoopAddressesUpdated(_loopAddress, _vaultAddress);	// @audit-issue

385:        emit Recovered(tokenAddress, tokenAmount);	// @audit-issue

504:        emit SwappedTokens(address(_sellToken), _amount, boughtETHAmount);	// @audit-issue
```
[197](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L197-L197), [329](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L329-L329), [339](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L339-L339), [357](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L357-L357), [385](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L385-L385), [504](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L504-L504), 


#### Recommendation

To improve the usability and analysis of your smart contract events, consider including the `msg.sender` address as part of the event data. This enables easier filtering and identification of the sender's actions within your contract, providing valuable insights for users and external tools.

### Unnecessary Constant Variable in Function Parameters
Passing a constant variable as a function parameter is redundant because the function can access the constant directly.

```solidity
Path: ./src/PrelaunchPoints.sol

125:        _processLock(ETH, msg.value, msg.sender, _referral);	// @audit-issue

134:        _processLock(ETH, msg.value, _for, _referral);	// @audit-issue
```
[125](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L125-L125), [134](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L134-L134), 


#### Recommendation


        Reference constant variables directly within the function body instead of passing them as parameters. This simplifies the function signature and conserves gas.


### Consider making contracts `Upgradeable`
This allows for bugs to be fixed in production, at the expense of significantly increasing centralization.

```solidity
Path: ./src/PrelaunchPoints.sol

16:contract PrelaunchPoints {	// @audit-issue
```
[16](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L16-L16), 


#### Recommendation

Assess the need for upgradeability in your Solidity contracts based on the project's requirements and lifecycle. If chosen, implement a well-known proxy pattern ensuring rigorous security and governance mechanisms are in place. Be aware of the increased centralization and plan accordingly to mitigate potential risks, such as through decentralized governance models or multi-sig control for upgrade decisions.

### Large numeric literals should use underscores for readability
In Solidity, as in many programming languages, large numeric literals can be difficult to read and interpret at a glance, especially when they consist of many digits. To improve readability and reduce the likelihood of errors, it's beneficial to use underscores (`_`) as separators in these literals. This practice breaks down long numbers into smaller, more readable segments, similar to how commas are used in conventional numeric notation. For instance, `1000000` can be rewritten as `1_000_000`, making it immediately clear that it represents one million. This method of formatting large numbers enhances code clarity without affecting the actual value of the literals.

```solidity
Path: ./src/PrelaunchPoints.sol

103:        startClaimDate = 4294967295; // Max uint32 ~ year 2107	// @audit-issue
```
[103](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L103-L103), 


#### Recommendation

For improved readability, consider using underscores in large numeric literals. This can make the numbers easier to parse and understand. For example, instead of writing `1000000000000000000`, you can write `1_000_000_000_000_000_000`.

### Variable names for `immutable`s should use CONSTANT_CASE
For `immutable` variable names, each word should use all capital letters, with underscores separating each word (CONSTANT_CASE)

```solidity
Path: ./src/PrelaunchPoints.sol

29:    address public immutable exchangeProxy;	// @audit-issue name should be: EXCHANGE_PROXY
```
[29](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L29-L29), 


#### Recommendation

When naming `immutable` variables, follow the CONSTANT_CASE convention, which means using all capital letters with underscores to separate words. This naming convention improves code readability and aligns with best practices for naming constants.

### Avoid using `owner` or `_owner` as a parameter name
Using 'owner' or '_owner' as a parameter name in functions, especially in contracts inheriting from or interacting with OpenZeppelin's `Ownable` contract, can lead to confusion and potential bugs. These contracts often have a state variable named `owner`, managed by the `Ownable` pattern for access control. Using the same name for function parameters may obscure the contract's `owner` state variable, complicating code readability and maintenance. Prefer alternative descriptive names for parameters to maintain clarity and prevent conflicts with established ownership patterns.

```solidity
Path: ./src/PrelaunchPoints.sol

336:    function setOwner(address _owner) external onlyAuthorized {	// @audit-issue
```
[336](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L336-L336), 


#### Recommendation

Refactor your Solidity contracts to use descriptive and unambiguous names for function parameters, avoiding the use of `owner` or `_owner` if your contract inherits from or interacts with the `Ownable` contract or follows a similar ownership pattern. Opt for alternative names that clearly indicate the parameter's purpose without conflicting with the `owner` state variable.

### Revert statements within external and public functions can be used to perform DOS attacks
In Solidity, `revert` statements are used to undo changes and throw an exception when certain conditions are not met. However, in public and external functions, improper use of `revert` can be exploited for Denial of Service (DoS) attacks. An attacker can intentionally trigger these `revert' conditions, causing legitimate transactions to consistently fail. For example, if a function relies on specific conditions from user input or contract state, an attacker could manipulate these to continually force `revert`s, blocking the function's execution. Therefore, it's crucial to design contract logic to handle exceptions properly and avoid scenarios where `revert` can be predictably triggered by malicious actors. This includes careful input validation and considering alternative design patterns that are less susceptible to such abuses.

```solidity
Path: ./src/PrelaunchPoints.sol

145:            revert InvalidToken();	// @audit-issue

159:            revert InvalidToken();	// @audit-issue

277:                revert CurrentlyNotPossible();	// @audit-issue

280:                revert NoLongerPossible();	// @audit-issue

288:            revert CannotWithdrawZero();	// @audit-issue

292:                revert UseClaimInstead();	// @audit-issue

299:                revert FailedToSendEther();	// @audit-issue
```
[145](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L145-L145), [159](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L159-L159), [277](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L277-L277), [280](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L280-L280), [288](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L288-L288), [292](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L292-L292), [299](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L299-L299), 


#### Recommendation

Design your Solidity contract's public and external functions with care to mitigate the risk of DoS attacks via `revert` statements. Implement robust input validation to ensure inputs are within expected bounds and conditions. Consider alternative logic or design patterns that reduce the reliance on `revert` for critical operations, particularly those that can be influenced externally. Evaluate the use of modifiers, try-catch blocks, or state checks that allow for safer handling of conditions and exceptions. Ensure that your contract's critical functionality remains accessible and resilient against potential abuse of `revert` behavior by malicious actors.

### Consider adding emergency-stop functionality
In the event of a security breach or any unforeseen emergency, swiftly suspending all protocol operations becomes crucial. Having a mechanism in place to halt all functions collectively, instead of pausing individual contracts separately, substantially enhances the efficiency of mitigating ongoing attacks or vulnerabilities. This not only quickens the response time to potential threats but also reduces operational stress during these critical periods. Therefore, consider integrating a 'circuit breaker' or 'emergency stop' function into the smart contract system architecture. Such a feature would provide the capability to suspend the entire protocol instantly, which could prove invaluable during a time-sensitive crisis management situation.

```solidity
Path: ./src/PrelaunchPoints.sol

16:contract PrelaunchPoints {	// @audit-issue
```
[16](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L16-L16), 


#### Recommendation

Implement an emergency-stop feature in your Solidity contract system to enhance security and crisis response capabilities. This can be achieved through a 'circuit breaker' pattern, where a central switch or set of conditions can instantly suspend critical operations across the contract ecosystem. Ensure that this mechanism is accessible to authorized parties only, such as contract administrators or a decentralized governance system. Design the emergency-stop functionality to be transparent and auditable, with clear conditions and processes for activation and deactivation. Regularly test and audit this feature to ensure its reliability and effectiveness in potential emergency situations.

### Assembly block creates dirty bits
Manipulating data directly at the free memory pointer location without subsequently adjusting the pointer can lead to unwanted data remnants, or "dirty bits", in that memory spot. This can cause challenges for the Solidity optimizer, making it difficult to determine if memory cleaning is required before reuse, potentially resulting in less efficient optimization. To mitigate this issue, it's advised to always update the free memory pointer following any data write operation. Furthermore, using the `assembly ("memory-safe") { ... }` annotation will clearly indicate to the optimizer the sections of your code that are memory-safe, improving code efficiency and reducing the potential for errors.

```solidity
Path: ./src/PrelaunchPoints.sol

454:        assembly {	// @audit-issue
455:            let p := _data.offset
456:            selector := calldataload(p)
457:            p := add(p, 36) // Data: selector 4 + lenght data 32
458:            inputTokenAmount := calldataload(p)
459:            recipient := calldataload(add(p, 64))
460:            encodedPathLength := calldataload(add(p, 96)) // Get length of encodedPath (obtained through abi.encodePacked)
461:            inputToken := shr(96, calldataload(add(p, 128))) // Shift to the Right with 24 zeroes (12 bytes = 96 bits) to get address
462:            outputToken := shr(96, calldataload(add(p, add(encodedPathLength, 108)))) // Get last address of the hop
463:        }

475:        assembly {	// @audit-issue
476:            let p := _data.offset
477:            selector := calldataload(p)
478:            inputToken := calldataload(add(p, 4)) // Read slot, selector 4 bytes
479:            outputToken := calldataload(add(p, 36)) // Read slot
480:            inputTokenAmount := calldataload(add(p, 68)) // Read slot
481:        }
```
[454](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L454-L463), [475](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L475-L481), 


#### Recommendation

Always update the free memory pointer after writing data in Solidity assembly blocks to ensure memory cleanliness. This practice prevents the creation of 'dirty bits' and aids the Solidity optimizer in performing efficient memory management. Additionally, consider using the `assembly ("memory-safe") { ... }` annotation for sections of your code that are memory-safe. This annotation clearly communicates to the optimizer which parts of your assembly code are handling memory correctly, leading to better optimization and reduced risk of memory-related errors. Regularly review and audit your assembly blocks for proper memory handling to maintain the efficiency and reliability of your Solidity contracts.

### For extended 'using-for' usage, use the latest pragma version
Solidity versions of 0.8.13 or above can make use of enhanced using-for notation within contracts.

```solidity
Path: ./src/PrelaunchPoints.sol

16:contract PrelaunchPoints {	// @audit-issue
```
[16](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L16-L16), 


#### Recommendation

Update your Solidity contracts to use the latest pragma version, ideally 0.8.13 or higher, to leverage the enhanced 'using-for' notation. This upgrade will enable you to apply library functions to user-defined types more effectively, enhancing the functionality and readability of your contracts. Make sure to thoroughly test your contracts after upgrading to ensure compatibility and to take full advantage of the latest Solidity features and improvements. Regularly keep your contracts updated with the latest Solidity versions to stay aligned with the evolving capabilities and best practices of the language.

### Cyclomatic complexity in functions
Cyclomatic complexity is a software metric used to measure the complexity of a program. It quantifies the number of linearly independent paths through a program's source code, giving an idea of how complex the control flow is. High cyclomatic complexity may indicate a higher risk of defects and can make the code harder to understand, test, and maintain. It often suggests that a function or method is trying to do too much, and a refactor might be needed. By breaking down complex functions into smaller, more focused pieces, you can improve readability, ease of testing, and overall maintainability.

```solidity
Path: ./src/PrelaunchPoints.sol

274:    function withdraw(address _token) external {	// @audit-issue
275:        if (!emergencyMode) {
276:            if (block.timestamp <= loopActivation) {
277:                revert CurrentlyNotPossible();
278:            }
279:            if (block.timestamp >= startClaimDate) {
280:                revert NoLongerPossible();
281:            }
282:        }
283:
284:        uint256 lockedAmount = balances[msg.sender][_token];
285:        balances[msg.sender][_token] = 0;
286:
287:        if (lockedAmount == 0) {
288:            revert CannotWithdrawZero();
289:        }
290:        if (_token == ETH) {
291:            if (block.timestamp >= startClaimDate){
292:                revert UseClaimInstead();
293:            }
294:            totalSupply = totalSupply - lockedAmount;
295:
296:            (bool sent,) = msg.sender.call{value: lockedAmount}("");
297:
298:            if (!sent) {
299:                revert FailedToSendEther();
300:            }
301:        } else {
302:            IERC20(_token).safeTransfer(msg.sender, lockedAmount);
303:        }
304:
305:        emit Withdrawn(msg.sender, _token, lockedAmount);
306:    }

405:    function _validateData(address _token, uint256 _amount, Exchange _exchange, bytes calldata _data) internal view {	// @audit-issue
406:        address inputToken;
407:        address outputToken;
408:        uint256 inputTokenAmount;
409:        address recipient;
410:        bytes4 selector;
411:
412:        if (_exchange == Exchange.UniswapV3) {
413:            (inputToken, outputToken, inputTokenAmount, recipient, selector) = _decodeUniswapV3Data(_data);
414:            if (selector != UNI_SELECTOR) {
415:                revert WrongSelector(selector);
416:            }
417:            // UniswapV3Feature.sellTokenForEthToUniswapV3(encodedPath, sellAmount, minBuyAmount, recipient) requires `encodedPath` to be a Uniswap-encoded path, where the last token is WETH, and sends the NATIVE token to `recipient`
418:            if (outputToken != address(WETH)) {
419:                revert WrongDataTokens(inputToken, outputToken);
420:            }
421:        } else if (_exchange == Exchange.TransformERC20) {
422:            (inputToken, outputToken, inputTokenAmount, selector) = _decodeTransformERC20Data(_data);
423:            if (selector != TRANSFORM_SELECTOR) {
424:                revert WrongSelector(selector);
425:            }
426:            if (outputToken != ETH) {
427:                revert WrongDataTokens(inputToken, outputToken);
428:            }
429:        } else {
430:            revert WrongExchange();
431:        }
432:
433:        if (inputToken != _token) {
434:            revert WrongDataTokens(inputToken, outputToken);
435:        }
436:        if (inputTokenAmount != _amount) {
437:            revert WrongDataAmount(inputTokenAmount);
438:        }
439:        if (recipient != address(this) && recipient != address(0)) {
440:            revert WrongRecipient(recipient);
441:        }
442:    }
```
[274](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L274-L306), [405](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L405-L442), 


#### Recommendation

Regularly analyze your Solidity contracts for functions with high cyclomatic complexity. Consider refactoring these functions into smaller, more focused units. This could involve breaking down complex functions into multiple simpler functions, reducing the number of conditional branches, or simplifying logic where possible. Such refactoring improves the readability and testability of your code and reduces the likelihood of defects. Additionally, simpler functions are easier to audit and maintain, enhancing the overall security and robustness of your contract. Employ tools or metrics to periodically assess the complexity of your functions as part of your development and maintenance processes.

### Consider only defining one library/interface/contract per sol file
Combining multiple libraries, interfaces, or contracts in a single .sol file can lead to clutter, reduced readability, and versioning issues. Resolution: Adopt the best practice of defining only one library, interface, or contract per Solidity file. This modular approach enhances clarity, simplifies unit testing, and streamlines code review. Furthermore, segregating components makes version management easier, as updates to one component won't necessitate changes to a file housing multiple unrelated components. Structured file management can further assist in avoiding naming collisions and ensure smoother integration into larger systems or DApps.

```solidity
Path: ./src/PrelaunchPoints.sol

4:import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";	// @audit-issue
5:import "@openzeppelin/contracts/utils/math/Math.sol";
6:
7:import {ILpETH, IERC20} from "./interfaces/ILpETH.sol";
8:import {ILpETHVault} from "./interfaces/ILpETHVault.sol";
9:import {IWETH} from "./interfaces/IWETH.sol";
```
[4](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L4-L9), 


#### Recommendation

Adopt a modular file structure in your Solidity projects by defining only one library, interface, or contract per file. This approach significantly enhances the clarity and readability of your code, making it easier to manage, test, and review. It also simplifies version control, as updates to individual components are isolated to their respective files, reducing the risk of unintended side effects. Organize your project directory to reflect this structure, with a clear naming convention that matches file names to their contained contracts, libraries, or interfaces. This organization not only prevents naming collisions but also facilitates smoother integration into larger systems or decentralized applications (DApps).

### Reduce deployment costs by tweaking contracts' metadata
When solidity generates the bytecode for the smart contract to be deployed, it appends metadata about the compilation at the end of the bytecode.
By default, the solidity compiler appends metadata at the end of the “actual” initcode, which gets stored to the blockchain when the constructor finishes executing.
Consider tweaking the metadata to avoid this unnecessary allocation. A full guide can be found [here](https://www.rareskills.io/post/solidity-metadata).
```solidity
/// @audit Global finding.
```
[1](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L1), 


#### Recommendation

See [this](https://www.rareskills.io/post/solidity-metadata) link, at its bottom, for full details

### All verbatim blocks are considered identical by deduplicator and can incorrectly be unified
The Solidity Team reported a bug on October 24, 2023, affecting Yul code using the verbatim builtin, specifically in the Block Deduplicator optimizer step. This bug, present since Solidity version 0.8.5, caused incorrect deduplication of verbatim assembly items surrounded by identical opcodes, considering them identical regardless of their data. The bug was confined to pure Yul compilation with optimization enabled and was unlikely to be exploited as an attack vector. The conditions triggering the bug were very specific, and its occurrence was deemed to have a low likelihood. The bug was rated with an overall low score due to these factors.
```solidity
/// @audit Global finding.
```
[1](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L1), 


#### Recommendation

Review and assess any Solidity contracts, especially those involving Yul code, that may be impacted by this deduplication bug. If your contracts rely on the Block Deduplicator optimizer and use verbatim blocks in a way that could be affected by this issue, consider updating your Solidity version to one where this bug is fixed, or adjust your contract to avoid this specific scenario. Stay informed about updates from the Solidity Team regarding this and similar issues, and regularly update your Solidity compiler to the latest version to benefit from bug fixes and optimizations. Given the specific and limited nature of this bug, its impact may be minimal, but caution is advised for contracts with complex assembly code or those heavily reliant on optimizer behaviors.

### Consider adding formal verification proofs
Formal verification is the act of proving or disproving the correctness of intended algorithms underlying a system with respect to a certain formal specification/property/invariant, using formal methods of mathematics.
```solidity
/// @audit Global finding.
```
[1](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/./src/PrelaunchPoints.sol#L1), 


#### Recommendation

Consider integrating formal verification into your Solidity contract development process. This can be done by defining formal specifications and properties that your contract should adhere to and using mathematical methods to verify these aspects. Tools and platforms like Certora Prover, Scribble, or OpenZeppelin's test environment can assist in this process. Formal verification should complement traditional testing and auditing methods, offering an additional layer of security assurance. Keep in mind that formal verification requires a thorough understanding of mathematical logic and contract specifications, so it may necessitate additional resources or expertise. Nevertheless, the investment in formal verification can significantly enhance the trustworthiness and robustness of your smart contracts.
