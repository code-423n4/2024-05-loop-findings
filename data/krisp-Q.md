## Low Findings

|        | Issue                                                                                    |
| ------ | ---------------------------------------------------------------------------------------- |
| [L-01] | Centralization risk for privileged functions                                             |
| [L-02] | Unchecked Return Values of the approve() Function                                        |
| [L-03] | Use `forceApprove`/`safeIncreaseAllowance` from SafeERC20 instead of approve/safeApprove |
| [L-04] | Missing checks for address(0x0) when updating address state variables                    |
| [L-05] | Lack of Parameter Validation in Constructor                                              |
| [L-06] | Missing Contract-Existence Checks Before Low-Level Calls                                 |
| [L-07] | Missing address(0) Check in Constructor                                                  |
| [L-08] | Embedding Modifier in Private Function Reduces Contract Size                             |

Total: 8 issues

## NonCritical Findings

|        | Issue                                                                                      |
| ------ | ------------------------------------------------------------------------------------------ |
| [N-01] | Setters should prevent re-setting of the same value                                        |
| [N-02] | Custom error has no error details                                                          |
| [N-03] | Consider using OpenZeppelin's SafeCast for any casting                                     |
| [N-04] | Consider emitting an event at the end of the constructor                                   |
| [N-05] | Events are missing sender information                                                      |
| [N-06] | Leverage Recent Solidity Features with 0.8.23                                              |
| [N-07] | Require statement without an error message                                                 |
| [N-08] | Contract uses both require()/revert() as well as custom errors                             |
| [N-09] | Solidity Version Too Recent to be Trusted                                                  |
| [N-10] | Consider Adding Emergency-Stop Functionality                                               |
| [N-11] | Insufficient Comments in Complex Functions                                                 |
| [N-12] | Constants should be defined rather than using magic numbers                                |
| [N-13] | Consider making contracts Upgradeable                                                      |
| [N-14] | Style guide: Contract does not follow the Solidity style guide's suggested layout ordering |
| [N-15] | Constants in comparisons should appear on the left side                                    |
| [N-16] | Style guide: Function ordering does not follow the Solidity style guide                    |
| [N-17] | Consider using named mappings                                                              |
| [N-18] | Consider using a struct rather than having many function input parameters                  |
| [N-19] | Contracts should have full test coverage                                                   |

Total: 19 issues

## Low Findings Details

### [L-01] Centralization risk for privileged functions

Functions that grant centralized privileges introduce a notable security risk. Such functions can act as a single point of vulnerability, especially if they control critical operations or access to sensitive data.

If the account or entity with these privileges gets compromised, it could lead to the unauthorized alteration of the contract's state, asset misappropriation, or other malicious activities. It's essential to implement robust access controls, frequent audits, and potentially decentralize critical functions to mitigate this risk.

<details>
<summary><i>Instances</i></summary>

- [PrelaunchPoints#convertAllETH()](https://github.com/code-423n4/2024-05-loop/blob/40167e469edde09969643b6808c57e25d1b9c203/src/PrelaunchPoints.sol#L315-L330)
- [PrelaunchPoints#setOwner()](<https://github.com/code-423n4/2024-05-loop/blob/40167e469edde09969643b6808c57e25d1b9c203/src/PrelaunchPoints.sol#L336-L340()>)
- [PrelaunchPoints#setLoopAddresses()](https://github.com/code-423n4/2024-05-loop/blob/40167e469edde09969643b6808c57e25d1b9c203/src/PrelaunchPoints.sol#L348-L358)
- [PrelaunchPoints#allowToken()](https://github.com/code-423n4/2024-05-loop/blob/40167e469edde09969643b6808c57e25d1b9c203/src/PrelaunchPoints.sol#L364-L366)
- [PrelaunchPoints#setEmergencyMode()](https://github.com/code-423n4/2024-05-loop/blob/40167e469edde09969643b6808c57e25d1b9c203/src/PrelaunchPoints.sol#L372-L374)
- [PrelaunchPoints#recoverERC20()](https://github.com/code-423n4/2024-05-loop/blob/40167e469edde09969643b6808c57e25d1b9c203/src/PrelaunchPoints.sol#L379-L386)
</details>

### [L-02] Unchecked Return Values of the approve() Function

The unchecked return value of the approve() method can potentially cause transaction failures to go unnoticed in your contract.

<details>
<summary><i>Instances</i></summary>

[PrelaunchPoints#claimAndStake()](https://github.com/code-423n4/2024-05-loop/blob/40167e469edde09969643b6808c57e25d1b9c203/src/PrelaunchPoints.sol#L231)

</details>

### [L-03] Use `forceApprove`/`safeIncreaseAllowance` from SafeERC20 instead of approve/safeApprove

Certain tokens, exemplified by USDT, have quirks where adjusting an allowance directly from a non-zero value can be problematic. For such tokens, it's a common requirement to first reset the allowance to zero before setting a new value. Failing to do so can result in transaction reverts, disrupting protocol functionality.

<details>
<summary><i>Instances</i></summary>

- [PrelaunchPoints#claimAndStake()](https://github.com/code-423n4/2024-05-loop/blob/40167e469edde09969643b6808c57e25d1b9c203/src/PrelaunchPoints.sol#L231)
- [PrelaunchPoints#\_fillQuote](https://github.com/code-423n4/2024-05-loop/blob/40167e469edde09969643b6808c57e25d1b9c203/src/PrelaunchPoints.sol#L495)
</details>

### [L-04] Missing checks for address(0x0) when updating address state variables

State variables that are of type address should always be checked to ensure that they are not being assigned the null address (address(0x0)). A null address often implies an error or omission in the code. Without proper checks, such a scenario can lead to unexpected behavior and potential vulnerabilities in the smart contract.

<details>
<summary><i>Instances</i></summary>

- [PrelaunchPoints#setOwner()](<https://github.com/code-423n4/2024-05-loop/blob/40167e469edde09969643b6808c57e25d1b9c203/src/PrelaunchPoints.sol#L336-L340()>)
- [PrelaunchPoints#setLoopAddresses()](https://github.com/code-423n4/2024-05-loop/blob/40167e469edde09969643b6808c57e25d1b9c203/src/PrelaunchPoints.sol#L348-L358)
- [PrelaunchPoints#allowToken()](https://github.com/code-423n4/2024-05-loop/blob/40167e469edde09969643b6808c57e25d1b9c203/src/PrelaunchPoints.sol#L364-L366)

</details>

### [L-05] Lack of Parameter Validation in Constructor

The constructor doesn't validate input parameters before assigning them to state variables. It is crucial to ensure that input parameters meet certain conditions to avoid unexpected states or behaviors in the contract. Consider adding appropriate checks or constraints.

### [L-06] Missing Contract-Existence Checks Before Low-Level Calls

When making low-level calls, it's crucial to ensure the existence of the contract at the specified address. If the contract doesn't exist at the given address, low-level calls will still return success, potentially causing errors in the code execution. Therefore, alongside zero-address checks, adding an additional check to verify that

.code.length > 0 before making low-level calls would be recommended.

<details>
<summary><i>Instances</i></summary>

- [PrelaunchPoints#\_fillQuote()](https://github.com/code-423n4/2024-05-loop/blob/40167e469edde09969643b6808c57e25d1b9c203/src/PrelaunchPoints.sol#L497)
- [PrelaunchPoints#withdraw()](https://github.com/code-423n4/2024-05-loop/blob/40167e469edde09969643b6808c57e25d1b9c203/src/PrelaunchPoints.sol#L296)
</details>

### [L-07] Missing address(0) Check in Constructor

The constructor does not include a check for address(0) when set state variables that hold addresses. Initializing a state variable with address(0) can lead to unintended behavior and vulnerabilities in the contract, such as sending funds to an inaccessible address. It is recommended to include a validation step to ensure that address parameters are not set to address(0).

### [L-08] Embedding Modifier in Private Function Reduces Contract Size

Consider consolidating the logic of a modifier within a private function to optimize contract size. Employing a private visibility, which is more efficient for function calls compared to internal visibility, is advisable since the modifier will exclusively invoke this function internally within the contract.

Example:

```solidity
    function _onlyAuthorized() private view {
        if (msg.sender != owner) {
            revert NotAuthorized();
        }
    }
    modifier onlyAuthorized() {
        _onlyAuthorized();
        _;
    }
```

## Non-Critical Findings Details

### [N-01] Setters should prevent re-setting of the same value

Setter functions should include a condition to check if the new value being assigned is different from the current value. This practice prevents redundant state changes and the emission of unnecessary events, ensuring that changes only occur when actual updates to the values are made.

This not only helps to maintain the integrity of the contract's state but also keeps the event logs cleaner and more meaningful by avoiding the recording of identical consecutive values.

<details>
<summary><i>Instances</i></summary>

- [PrelaunchPoints#setOwner()](<https://github.com/code-423n4/2024-05-loop/blob/40167e469edde09969643b6808c57e25d1b9c203/src/PrelaunchPoints.sol#L336-L340()>)
- [PrelaunchPoints#setLoopAddresses()](https://github.com/code-423n4/2024-05-loop/blob/40167e469edde09969643b6808c57e25d1b9c203/src/PrelaunchPoints.sol#L348-L358)
- [PrelaunchPoints#setEmergencyMode()](https://github.com/code-423n4/2024-05-loop/blob/40167e469edde09969643b6808c57e25d1b9c203/src/PrelaunchPoints.sol#L372-L374)
</details>

### [N-02] Custom error has no error details

Take advantage of custom error's return value property.

An important feature of Custom Error is that values such as address, tokenID, msg.value can be written inside the () sign, providing a serious advantage in debugging and examining the revert details.

<details>
<summary><i>Instances</i></summary>

```solidity
error InvalidToken();
    error NothingToClaim();
    error TokenNotAllowed();
    error CannotLockZero();
    error CannotWithdrawZero();
    error UseClaimInstead();
    error FailedToSendEther();
    error SwapCallFailed();
    error WrongExchange();
    error LoopNotActivated();
    error NotValidToken();
    error NotAuthorized();
    error CurrentlyNotPossible();
    error NoLongerPossible();
```

</details>

### [N-03] Consider using OpenZeppelin's SafeCast for any casting

OpenZeppelin's has SafeCast library that provides functions to safely cast. Recommended to use it instead of casting directly in any case.

<details>
<summary><i>Instances</i></summary>

- [PrelaunchPoints.sol#L102](https://github.com/code-423n4/2024-05-loop/blob/40167e469edde09969643b6808c57e25d1b9c203/src/PrelaunchPoints.sol#L102)
- [PrelaunchPoints.sol#L327](https://github.com/code-423n4/2024-05-loop/blob/40167e469edde09969643b6808c57e25d1b9c203/src/PrelaunchPoints.sol#L327)
- [PrelaunchPoints.sol#L355](https://github.com/code-423n4/2024-05-loop/blob/40167e469edde09969643b6808c57e25d1b9c203/src/PrelaunchPoints.sol#L355)
- </details>

### [N-04] Consider emitting an event at the end of the constructor

This will allow users to easily exactly pinpoint when and by whom a contract was constructed.

### [N-05] Events are missing sender information

Events should include the sender information when emitted in public or external functions for easier traceability.

<details>
<summary><i>Instances</i></summary>

```solidity
        emit Locked(_receiver, _amount, _token, _referral);
```

```solidity
        emit Converted(totalBalance, totalLpETH);
```

</details>

### [N-06] Leverage Recent Solidity Features with 0.8.23

The recent updates in Solidity provide several features and optimizations that, when leveraged appropriately, can significantly add value to your contract's code clarity and maintainability. Key things include the use of push0 for placing 0 on the stack for EVM versions starting from "Shanghai", making your code simpler and more straightforward. Moreover, Solidity has extended NatSpec documentation support to enum and struct definitions, facilitating more comprehensive and insightful code documentation.

Additionally, the re-implementation of the UnusedAssignEliminator and UnusedStoreEliminator in the Solidity optimizer provides the ability to remove unused assignments in deeply nested loops. This results in a cleaner, more efficient contract code, reducing clutter and potential points of confusion during code review or debugging. It's recommended to make full use of these features and optimizations for more robustness and readability of your smart contracts.

### [N-07] Require statement without an error message

Require statements should include clear error message in event when the required statement reverts.

<details>
<summary><i>Instances</i></summary>

```solidity
        require(_sellToken.approve(exchangeProxy, _amount));
```

</details>

### [N-08] Contract uses both require()/revert() as well as custom errors

The contract uses both `require()/revert()` and custom errors for error handling.

It's recommended to use a consistent approach for error handling in a single file.

### [N-09] Solidity Version Too Recent to be Trusted

The current Solidity version used in the code is too recent to be trusted. Using a newer version might introduce unrecovered bugs. It is recommended to use version 0.8.18.

### [N-10] Consider Adding Emergency-Stop Functionality

Smart contracts that hold significant value, interact with external contracts, or have complex logic should include an emergency-stop mechanism for added security. This allows pausing certain contract functionalities in case of emergencies, mitigating potential damages.

This contract seems to lack such a mechanism. Implementing an emergency stop can increase contract security and reliability.

### [N-11] Insufficient Comments in Complex Functions

Complex functions spanning multiple lines should have more comments to easy explain the purpose of each logic step. Proper commenting can make easy the readability and maintainability of the codebase. Consider adding explicit comments to provide insights into the function's operation.

<details>
<summary><i>Instances</i></summary>

```solidity
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
        if (recipient != address(this) && recipient != address(0)) {
            revert WrongRecipient(recipient);
        }
    }
```

</details>

### [N-12] Constants should be defined rather than using magic numbers

Magic numbers are hardcoded numerical values used directly in the code, making it harder to read and maintain. Consider using constants instead of magic numbers.

<details>
<summary><i>Instances</i></summary>

[PrelaunchPoints.sol#L497](https://github.com/code-423n4/2024-05-loop/blob/40167e469edde09969643b6808c57e25d1b9c203/src/PrelaunchPoints.sol#L497)

```solidity
(bool success,) = payable(exchangeProxy).call{value: 0}(_swapCallData);
```

</details>

### [N-13] Consider making contracts Upgradeable

Contract uses a non-upgradeable design. Transitioning to an upgradeable contract structure is more aligned with contemporary smart contract practices. This approach not only increases flexibility but also ensures the contract stays relevant and robust in an ever-evolving ecosystem.

### [N-14] Style guide: Contract does not follow the Solidity style guide's suggested layout ordering

Adhering to a recommended order in Solidity contracts enhances code readability and maintenance.
[More information in Documentation](https://docs.soliditylang.org/en/latest/style-guide.html#order-of-layout)
It's recommended to use the following order:

1. Type declarations
2. State variables
3. Events
4. Errors
5. Modifiers
6. Functions

### [N-15] Constants in comparisons should appear on the left side

Placing constants on the left side of comparison statements can help prevent accidental assignments. In languages like C, placing constants on the left can protect against unintended assignments that would be treated as true conditions, leading to bugs. Although Solidity does not have this specific issue, using the same practice can still be beneficial for code readability and consistency.

<details>
<summary><i>Instances</i></summary>

[PrelaunchPoints.sol#L316](https://github.com/code-423n4/2024-05-loop/blob/40167e469edde09969643b6808c57e25d1b9c203/src/PrelaunchPoints.sol#L316)

```solidity
        if (block.timestamp - loopActivation <= TIMELOCK)

```

</details>

### [N-16] Style guide: Function ordering does not follow the Solidity style guide

Ordering helps readers identify which functions they can call and to find the constructor and fallback definitions easier. But there are contracts in the project that do not comply with this. Functions should be grouped according to their visibility and ordered:

- constructor
- receive function (if exists)
- fallback function (if exists)
- external
- public
- internal
- private

### [N-17] Consider using named mappings

As of Solidity version 0.8.18, it is possible to use named mappings to clarify the purpose of each mapping in the code.
It is recommended to use this feature for code readability and maintainability.

More information: [Solidity 0.8.18 Release Announcement](https://blog.soliditylang.org/2023/02/01/solidity-0.8.18-release-announcement/)

[PrelaunchPoints.sol#L35](https://github.com/code-423n4/2024-05-loop/blob/40167e469edde09969643b6808c57e25d1b9c203/src/PrelaunchPoints.sol#L35)

```solidity
        mapping(address => bool) public isTokenAllowed;

```

[PrelaunchPoints.sol#L50](https://github.com/code-423n4/2024-05-loop/blob/40167e469edde09969643b6808c57e25d1b9c203/src/PrelaunchPoints.sol#L50)

```solidity
    mapping(address => mapping(address => uint256)) public balances; // User -> Token -> Balance
```

</details>

### [N-18] Consider using a struct rather than having many function input parameters

Functions with many parameters can become difficult to read and maintain. Using a struct to encapsulate these parameters can increase code readability, reusability, and reduce the likelihood of errors. Consider refactoring functions that take more than three parameters to use a struct instead.

### [N-19] Contracts should have full test coverage

It's recommended to have full test coverage for all contracts. While 100% code coverage does not guarantee absence of bugs, it can catch simple bugs and reduce regressions during code modifications. Moreover, to achieve full coverage, authors often have to refactor their code into more modular components, each testable separately. This leads to lower interdependencies, and results in code that is easier to understand and audit.