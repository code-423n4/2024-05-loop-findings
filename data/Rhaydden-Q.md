
## Quality Assurance

| Number | Issues |
|:------:|:--------------|
| [L-01](#l-01--incorrect-emission-in-the-withdraw-function)  | Incorrect Emission in the `withdraw` function | 
| [L-02](#l-02-potential-misleading-behavior-in-convertAllETH()-function)  | Potential Misleading Behavior in `convertAllETH()` Function |
| [L-03](#l-03-setowner-allows-the-current-owner-to-brick-the-ownership-by-setting-owner-=-address(0)-which-would-prevent-future-access-to-admin-functionality) | `setOwner` allows the current owner to brick the ownership by setting owner = address(0), which would prevent future access to admin functionality |
| [L-04](#l-04-unchecked-return-value)  | Unchecked Return Value|
| [L-05](#l-05-erc20-approve-call-is-not-safe)  | ERC20 Approve Call is Not Safe |
| [L-06](#l-06-event-is-missing-indexed-fields)  | Event is missing `indexed` fields |
| [L-07](#l-07-empty-require()-/-revert()-statements)  | Empty `require()` / `revert()` statements |
| [L-08](#l-08-push0-is-not-supported-by-all-chains)  | PUSH0 is not supported by all chains |
| [L-09](#l-09-loss-of-precision)  | Loss of precision |



## [L-01] Incorrect Emission in the `withdraw` function

https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/src/PrelaunchPoints.sol#L274-L306

The `withdraw` function checks if the `lockedAmount` is zero and reverts with a `CannotWithdrawZero()` error if that's the case. However, the function still emits the `Withdrawn` event with the lockedAmount parameter, which would be `zero` in this case.
Emitting an event with a zero amount for a withdrawal might not accurately reflect the actual behavior of the function and could potentially lead to confusion or misinterpretation by off-chain systems or users monitoring the contract events.

Recommendation:
To address this issue and maintain consistency with the intended behavior, consider moving the `emit Withdrawn(msg.sender, _token, lockedAmount);` statement inside the `if` block that checks for a non-zero lockedAmount. This way, the event will only be emitted when there is an actual withdrawal of a non-zero amount.





## [L-02] Potential Misleading Behavior in `convertAllETH()` Function

https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/src/PrelaunchPoints.sol#L315-L330

In the `convertAllETH()` function, there is a scenario where the function proceeds with operations that imply a successful conversion of ETH to `lpETH`, even when there is no ETH to convert. Specifically, if the contract's balance is zero at the time `convertAllETH()` is called, the function will still attempt to deposit zero ETH into the `lpETH` contract, update `startClaimDate`, and emit the `Converted` event with a `totalBalance` and `totalLpETH` of zero. This behavior could be misleading or incorrect, as it suggests a conversion has occurred when it has not.

Recommendation:
To address this issue, the `convertAllETH()` function should include a check to ensure that there is ETH to convert before proceeding with the conversion and other operations. If the contract's balance is zero, the function could revert with an appropriate error message or simply return without making any state changes or emitting events. This would prevent the misleading implication that a conversion has occurred when it has not. Additionally, it would be beneficial to document this behavior clearly in the function's comments to ensure that users of the contract are aware of the conditions under which the function operates.



## [L-03] `setOwner` allows the current owner to brick the ownership by setting owner = address(0), which would prevent future access to admin functionality

https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/src/PrelaunchPoints.sol#L336-L340

If set to address(0), future access to functions restricted by the onlyAuthorized modifier would be lost. Use an explicit function for renouncing ownership to prevent this occurring by mistake and prefer a 2-step ownership transfer mechanism. Both of these features are available in OZ [Ownable2step](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/access/Ownable2Step.sol)

```solidity
function setOwner(address _owner) external onlyAuthorized {
        owner = _owner;

        emit OwnerUpdated(_owner);
    }
```


## [L-04] Unchecked Return Value

https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/src/PrelaunchPoints.sol#L296-L296

The `sent` variable in the `withdraw` function is not checked after the low-level call to transfer ETH. If the call fails, the function will continue executing, and the user's balance will be set to zero without actually receiving the funds. Always check the return value of low-level calls and revert on failure.

```solidity
  (bool sent,) = msg.sender.call{value: lockedAmount}("");
```




## [L-05] ERC20 Approve Call is Not Safe

The `approve` function can be vulnerable to a specific attack, where a malicious actor can double spend tokens. This scenario occurs when the owner changes the approved allowance from N to M (N>0, M>0). The malicious spender can observe this change and quickly transfer the original N tokens before the change is mined, and then spend the additional M tokens afterwards. This could result in a total transfer of N+M tokens, which is not what the owner intended. More info you can see in this [link](https://docs.google.com/document/d/1YLPtQxZu1UAvO9cZ1O2RPXBbT0mooh4DYKjA_jp-RLM/edit#heading=h.m9fhqynw2xvt)

For this reason, it's recommended to use safeIncreaseAllowance() or safeDecreaseAllowance() for better control. If these methods are not available, using safeApprove() is also an option as it reverts if the current approval is not zero, providing an additional layer of security.


[Line: 231](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/src/PrelaunchPoints.sol#L231-L231)

	```solidity
	        lpETH.approve(address(lpETHVault), claimedAmount);
	```

[Line: 495](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/src/PrelaunchPoints.sol#L495-L495)

	```solidity
	        require(_sellToken.approve(exchangeProxy, _amount));
	```




## [L-06] Event is missing `indexed` fields

Index event fields make the field more quickly accessible to off-chain tools that parse events. However, note that each index field costs extra gas during emission, so it's not necessarily best to index the maximum allowed per event (three fields). Each event should use three indexed fields if there are three or more fields, and gas usage is not particularly of concern for the events in question. If there are fewer than three fields, all of the fields should be indexed.


[Line: 56](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/src/PrelaunchPoints.sol#L56-L56)

	```solidity
	    event Locked(address indexed user, uint256 amount, address token, bytes32 indexed referral);
	```

[Line: 57](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/src/PrelaunchPoints.sol#L57-L57)

	```solidity
	    event StakedVault(address indexed user, uint256 amount);
	```

 [Line: 58](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/src/PrelaunchPoints.sol#L58-L64)

	```solidity
	    event Converted(uint256 amountETH, uint256 amountlpETH);
	```

[Line: 59](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/src/PrelaunchPoints.sol#L58-L64)

	```solidity
	    event Withdrawn(address indexed user, address token, uint256 amount);
	```

[Line: 60](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/src/PrelaunchPoints.sol#L58-L64)

	```solidity
	    event Claimed(address indexed user, address token, uint256 reward);
	```

[Line: 61](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/src/PrelaunchPoints.sol#L58-L64)

	```solidity
	    event Recovered(address token, uint256 amount);
	```

[Line: 62](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/src/PrelaunchPoints.sol#L58-L64)

	```solidity
	    event OwnerUpdated(address newOwner);
	```

[Line: 63](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/src/PrelaunchPoints.sol#L58-L64)

	```solidity
	    event LoopAddressesUpdated(address loopAddress, address vaultAddress);
	```

[Line: 64](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/src/PrelaunchPoints.sol#L58-L64)

	```solidity
	    event SwappedTokens(address sellToken, uint256 sellAmount, uint256 buyETHAmount);
	```







## [L-07] Empty `require()` / `revert()` statements

Use descriptive reason strings or custom errors for revert paths.

[Line: 495](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/src/PrelaunchPoints.sol#L495-L495)

	```solidity
	        require(_sellToken.approve(exchangeProxy, _amount));
	```



## [L-08] PUSH0 is not supported by all chains

Solc compiler version 0.8.20 switches the default target EVM version to Shanghai, which means that the generated bytecode will include PUSH0 opcodes. Be sure to select the appropriate EVM version in case you intend to deploy on a chain other than mainnet like L2 chains that may not support PUSH0, otherwise deployment of your contracts will fail.


[Line: 2](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/src/PrelaunchPoints.sol#L2-L2)

	```solidity
	pragma solidity 0.8.20;
	```





## [L-09] Loss of precision

Division by large numbers may result in the result being zero, due to solidity not supporting fractions. Consider requiring a minimum amount for the numerator to ensure that it is always larger than the denominator

[Line 253:](https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/src/PrelaunchPoints.sol#L253-L253)

```Solidity
 uint256 userClaim = userStake * _percentage / 100;
```




