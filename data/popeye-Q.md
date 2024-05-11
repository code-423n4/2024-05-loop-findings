# QA Report for LoopFi

- [Low Issues](#low-issues)
  - [L-1: Inconsistent Variable Naming for User's Locked Funds ](#l-1-inconsistent-variable-naming-for-users-locked-funds)
  - [L-2: Missing Valid Token Checks in the `withdraw` Function will create problem](#l-2-missing-valid-token-checks-in-the-withdraw-function-will-create-problem)
  - [L-3: High Gas Costs and DOS Risk in `recoverERC20` Function](#l-3-high-gas-costs-and-dos-risk-in-recovererc20-function)
  - [L-4: ETH sent to this contract directly will be locked forever](#l-4-eth-sent-will-be-locked)
  - [L-5: Excessively High Timestamp seted for `startClaimDate` in constructor](#l-5-excessively-high-timestamp-seted-for-startclaimdate-in-constructor)
  - [L-6: Possible Race Condition issue in `_fillQuote` due to `approve`](#l-6-possible-race-condition-issue-in-_fillquote-due-to-approve)



# Low Issues

## L-1: Inconsistent Variable Naming for User's Locked Funds 

### Proof of Concept:
In the ontract, the variable `userStake` is used to represent the user's locked funds for a specific token. This can be seen in the `_claim` function:

https://github.com/code-423n4/2024-05-loop/blob/40167e469edde09969643b6808c57e25d1b9c203/src/PrelaunchPoints.sol#L244

```solidity
function _claim(address _token, address _receiver, uint8 _percentage, Exchange _exchange, bytes calldata _data)
    internal
    returns (uint256 claimedAmount)
{
    uint256 userStake = balances[msg.sender][_token];
    // ...
}
```

However, the naming of `userStake` is inconsistent with the purpose and functionality of the contract, which is related to locking user funds. The term "stake" implies a different concept, such as staking rewards or staking mechanisms, which are not present in this contract.

### Impact:
The inconsistent naming of the `userStake` variable can lead to confusion and misunderstanding for developers, auditors, and other stakeholders who review or interact with the contract. 

### Recommended Mitigation Steps:
To improve clarity and consistency, it is recommended to rename the `userStake` variable to `userLockedFunds` or a similar name that accurately reflects the purpose of the variable. This change should be applied throughout the contract wherever the variable is used.

For example, in the `_claim` function, the variable declaration should be updated as follows:

```solidity
function _claim(address _token, address _receiver, uint8 _percentage, Exchange _exchange, bytes calldata _data)
    internal
    returns (uint256 claimedAmount)
{
    uint256 userLockedFunds = balances[msg.sender][_token];
    // ...
}
```




## L-2: Missing Valid Token Checks in the `withdraw` Function will create problem

### Proof of Concept:
The `withdraw` function allows users to withdraw their locked funds for ETH and a specific LRT token. However, the function lacks necessary checks for valid token addresses. This can be seen in the following code snippet:

https://github.com/code-423n4/2024-05-loop/blob/40167e469edde09969643b6808c57e25d1b9c203/src/PrelaunchPoints.sol#L274-L306

```solidity
function withdraw(address _token) external {
    if (!emergencyMode) {
        if (block.timestamp <= loopActivation) {
            revert CurrentlyNotPossible();
        }
        if (block.timestamp >= startClaimDate) {
            revert NoLongerPossible();
        }
    }

    uint256 lockedAmount = balances[msg.sender][_token];
    balances[msg.sender][_token] = 0;

    if (lockedAmount == 0) {
        revert CannotWithdrawZero();
    }
    // ...
}
```

The `withdraw` function takes the `_token` address as input, but it does not perform any checks to ensure that the provided address is not the zero address (0x0) or that it is a valid token address allowed by the contract.

### Impact:
The absence of valid token checks and address(0) checks in the `withdraw` function can lead to several issues:

- If a user accidentally provides the zero address (0x0) as the `_token` parameter, the function will proceed with the withdrawal process. However, since the zero address is not a valid token address, this can lead to unexpected behavior and potential loss of funds.

- Without a check to ensure that the provided `_token` address is a valid and allowed token within the contract, users may attempt to withdraw tokens that are not supported or recognized by the contract. This can result in failed transactions or unintended consequences.

- The contract includes token validation checks in other functions, such as the `lock` and `lockFor` functions, which revert if the provided token is ETH. The absence of similar checks in the `withdraw` function creates inconsistency and may confuse users or introduce vulnerabilities.

### Recommended Mitigation Steps:
To address valid token checks and the address(0) checks in the `withdraw` function, the following mitigation steps should be implemented:

1. Before processing the withdrawal, the function should verify that the provided `_token` address is not the zero address. If the `_token` is the zero address, the function should revert with an appropriate error message.

```solidity
function withdraw(address _token) external {
    if (_token == address(0)) {
        revert InvalidTokenAddress();
    }
    // ...
}
```

2. The function should check if the provided `_token` address is a valid and allowed token within the contract. This can be done by utilizing the existing `isTokenAllowed` mapping or by implementing a separate function to validate token addresses.

```solidity
function withdraw(address _token) external {
    if (_token == address(0)) {
        revert InvalidTokenAddress();
    }
    if (!isTokenAllowed[_token]) {
        revert TokenNotAllowed();
    }
    // ...
}
```

By adding these checks, the `withdraw` function will ensure that only valid and allowed token addresses can be used for withdrawals, preventing accidental or unintended use of invalid addresses.


## L-3: High Gas Costs and DOS Risk in `recoverERC20` Function

### Proof of Concept:
The `recoverERC20` function allows the contract owner to recover different LRT tokens that were mistakenly sent to the contract. The function is defined as follows:

https://github.com/code-423n4/2024-05-loop/blob/40167e469edde09969643b6808c57e25d1b9c203/src/PrelaunchPoints.sol#L379-L386

```solidity
function recoverERC20(address tokenAddress, uint256 tokenAmount) external onlyAuthorized {
    if (tokenAddress == address(lpETH) || isTokenAllowed[tokenAddress]) {
        revert NotValidToken();
    }
    IERC20(tokenAddress).safeTransfer(owner, tokenAmount);

    emit Recovered(tokenAddress, tokenAmount);
}
```

While this function serves a useful purpose, it may lead to high gas costs and a potential Denial of Service (DOS) situation if a user intentionally sends a large number of different LRT tokens thats are supported to the contract.

### Impact:
If a malicious user intentionally sends a significant number of different wLRT tokens to the `PrelaunchPoints` contract, the contract owner would need to make multiple calls to the `recoverERC20` function to recover each token individually. This can have the following impacts:

1. Each call to the `recoverERC20` function requires a separate transaction, consuming gas. If there are numerous tokens to recover, the total gas cost for the recovery process can become substantial, placing a financial burden on the contract owner.

2. If the contract owner needs to make a large number of transactions to recover all the tokens, it may temporarily prevent or delay other important operations within the contract. The contract owner may be unable to perform critical functions or respond to user interactions promptly, leading to a potential Denial of Service situation.

3. The need to manually recover each token individually can be time-consuming and inefficient for the contract owner. It requires continuous monitoring and multiple transactions, diverting attention and resources from other essential tasks.

### Recommended Mitigation Steps:
To mitigate the risk of high gas costs and potential DOS situations arising from the `recoverERC20` function, the following steps can be considered:

- Instead of recovering tokens individually, the contract can include a batch recovery function that allows the owner to recover multiple tokens in a single transaction. This function could accept an array of token addresses and amounts, reducing the number of transactions required and minimizing gas costs.

```solidity
function recoverMultipleERC20(address[] calldata tokenAddresses, uint256[] calldata tokenAmounts) external onlyAuthorized {
    require(tokenAddresses.length == tokenAmounts.length, "Invalid input lengths");
    for (uint256 i = 0; i < tokenAddresses.length; i++) {
        if (tokenAddresses[i] == address(lpETH) || isTokenAllowed[tokenAddresses[i]]) {
            revert NotValidToken();
        }
        IERC20(tokenAddresses[i]).safeTransfer(owner, tokenAmounts[i]);
        emit Recovered(tokenAddresses[i], tokenAmounts[i]);
    }
}
```

- Set a Minimum Recovery Threshold: To avoid the need to recover small amounts of tokens, the contract can define a minimum threshold for token recovery. If the token balance falls below this threshold, the recovery process can be skipped or deferred until the balance reaches a significant amount, reducing unnecessary transactions and gas costs.





## L-4: ETH sent to this contract directly will be locked forever and it's a not good thing

### Proof of Concept:
The PrelaunchPoints contract has `receive` function that allows the contract to receive ETH directly. And the NatSpec says that directly sent ETH will be locked for ever. It's not a good thing. If the funds will be stuck, so why the `receive` function is used? Threre are some functions that need ETH to transfer, like LockETH. `payable` modifiers is used there. So, there are no need to have this receive function
The function is defined as follows:

```solidity
receive() external payable {}
```
https://github.com/code-423n4/2024-05-loop/blob/40167e469edde09969643b6808c57e25d1b9c203/src/PrelaunchPoints.sol#L389-L392

The contract also includes a comment above the `receive` function that states:

```solidity
/**
 * Enable receive ETH
 * @dev ETH sent to this contract directly will be locked forever.
 */
```

This comment indicates that any ETH sent directly to the contract, outside of the `lockETH` or `lockETHFor` functions, will be locked forever and cannot be claimed or withdrawn.

### Impact:
If a user accidentally sends ETH directly to the contract address, instead of using the designated `lockETH` or `lockETHFor` functions, the sent ETH will be locked forever. The user will have no way to retrieve or claim the locked ETH, resulting in a permanent loss of funds.


### Recommended Mitigation Steps:
To address the issue of ETH being locked forever when sent directly to the contract via the `receive` function, the following mitigation steps can be considered:

1. Instead of allowing the `receive` function to lock ETH forever, the contract can be modified to redirect any ETH received through the `receive` function to a designated function, such as `lockETH`. This way, ETH sent directly to the contract will be treated as a regular lock operation, and users will be able to claim or withdraw it according to the contract's rules.

```solidity
receive() external payable {
    _processLock(ETH, msg.value, msg.sender, bytes32(0));
}
```

2. The contract should include clear and prominent documentation or user interfaces that inform users about the correct way to lock ETH using the `lockETH` or `lockETHFor` functions. This guidance should emphasize that sending ETH directly to the contract address will result in the funds being locked permanently and cannot be recovered.

3. n cases where ETH is accidentally sent directly to the contract, the contract owner or a designated administrator can be given the ability to manually recover and return the locked ETH to the original sender. This recovery mechanism should be used sparingly and with appropriate checks and balances to prevent abuse.

```solidity
function recoverLockedETH(address payable recipient, uint256 amount) external onlyOwner {
    require(address(this).balance >= amount, "Insufficient locked ETH");
    recipient.transfer(amount);
    emit LockedETHRecovered(recipient, amount);
}
```
4. SimplY remove the `receive` function




## L-5: Excessively High Timestamp seted for `startClaimDate` in constructor


### Proof of Concept:
In the PrelaunchPoints contract, the constructor sets the `startClaimDate` variable to a very high timestamp value. The relevant code snippet is as follows:

https://github.com/code-423n4/2024-05-loop/blob/40167e469edde09969643b6808c57e25d1b9c203/src/PrelaunchPoints.sol#L103

```solidity
constructor(address _exchangeProxy, address _wethAddress, address[] memory _allowedTokens) {
    // ...
    startClaimDate = 4294967295; // Max uint32 ~ year 2107
    // ...
}
```

The `startClaimDate` is set to the value `4294967295`, which is the maximum value for a `uint32` data type. The comment indicates that this timestamp corresponds to the year 2107.

### Impact:
Setting the `startClaimDate` to such a high timestamp value can have several implications:

- Users will be unable to claim their tokens until the year 2107, which is an extremely long time into the future. This effectively means that the claiming functionality of the contract will be unusable for nearly a century, rendering it impractical for users.

- Reduced Contract Usability

- Potential Confusion

- Future Maintenance

## Recommended Mitigation Steps:
Instead of setting the `startClaimDate` to the maximum value of `uint32`, a more reasonable and practical timestamp should be chosen. The timestamp should be based on the project's roadmap, token distribution timeline, and user expectations. It should allow users to claim their tokens within a reasonable timeframe.





## L-6: Possible Race Condition issue in `_fillQuote` due to `approve`

### Proof of Concept:
The `_fillQuote` function interacts with the 0x API for token swaps. However, the function has a possible issue related to the way it sets the allowance for the 0x exchange proxy. The relevant code snippet is as follows:

https://github.com/code-423n4/2024-05-loop/blob/40167e469edde09969643b6808c57e25d1b9c203/src/PrelaunchPoints.sol#L495

```solidity
function _fillQuote(IERC20 _sellToken, uint256 _amount, bytes calldata _swapCallData) internal {
    // ...

    require(_sellToken.approve(exchangeProxy, _amount));

    // ...
}
```

The issue arises due to the direct use of the `approve` method on the ERC20 token (`_sellToken`). The `approve` method sets the allowance for the 0x exchange proxy (`exchangeProxy`) to spend the specified amount (`_amount`) of tokens.

The issue is that the current implementation does not take into account any previously set allowances. It directly sets a new allowance, overwriting any existing allowance. This can lead to a race condition vulnerability if multiple invocations of the `_fillQuote` function overlap.

### Impact:
If multiple invocations of `_fillQuote` overlap and the allowances are not managed correctly, it can lead to unauthorized spending of tokens. An attacker could potentially exploit this vulnerability to perform unintended or malicious token transfers. Or The direct overwriting of allowances can result in an inconsistent state where the actual allowance granted to the 0x exchange proxy does not match the intended allowance. This can cause discrepancies and confusion in the token swap process.

### Recommended Mitigation Steps:
To mitigate the race condition issue in the `_fillQuote` function, the following steps should be taken:

- Instead of directly setting the allowance using the `approve` method, use the `increaseAllowance` or `decreaseAllowance` methods provided by the ERC20 token contract. These methods atomically increase or decrease the allowance, taking into account the existing allowance value.

```solidity
function _fillQuote(IERC20 _sellToken, uint256 _amount, bytes calldata _swapCallData) internal {
    // ...

    require(_sellToken.increaseAllowance(exchangeProxy, _amount));

    // ...
}
```

- Before setting the allowance, check the current allowance granted to the 0x exchange proxy. Ensure that the new allowance is set correctly based on the existing allowance and the desired amount.

```solidity
function _fillQuote(IERC20 _sellToken, uint256 _amount, bytes calldata _swapCallData) internal {
    // ...

    uint256 currentAllowance = _sellToken.allowance(address(this), exchangeProxy);
    require(_sellToken.approve(exchangeProxy, currentAllowance + _amount));

    // ...
}
```

- Consider using the SafeERC20 library from OpenZeppelin, which provides safe versions of the ERC20 token operations. The library includes functions like `safeIncreaseAllowance` and `safeDecreaseAllowance` that handle allowance updates securely.

```solidity
import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

function _fillQuote(IERC20 _sellToken, uint256 _amount, bytes calldata _swapCallData) internal {
    // ...

    SafeERC20.safeIncreaseAllowance(_sellToken, exchangeProxy, _amount);

    // ...
}
```