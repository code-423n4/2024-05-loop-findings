### Governance

|                              Number                               | Issue                                               |
| :---------------------------------------------------------------: | :-------------------------------------------------- |
| [G&#x2011;1](#G1-Owner-can-miss-calling-setLoopAddresses-on-time) | Owner can miss calling `setLoopAddresses()` on time |
|  [G&#x2011;2](#G2-Compromised-owner-can-allow-malicious-tokens)   | Compromised owner can allow malicious tokens        |

### Low

|                                    Number                                     | Issue                                                       |
| :---------------------------------------------------------------------------: | :---------------------------------------------------------- |
| [L&#x2011;1](#L1-ETH-sent-directly-for-a-contract-will-not-be-locked-forever) | ETH sent directly for a contract will not be locked forever |
|  [L&#x2011;2](#L2-Allow-users-to-withdraw-a-portion-of-their-locked-tokens)   | Allow users to withdraw a portion of their locked tokens    |
|      [L&#x2011;3](#L3-Implement-a-function-for-removing-allowed-tokens)       | Implement a function for removing allowed tokens            |
|     [L&#x2011;4](#L4-User-can-steal-all-of-the-locked-ethers-for-himself)     | User can steal all of the locked ethers for himself         |

---

### [G&#x2011;1] Owner can miss calling `setLoopAddresses()` on time

The `setLoopAddresses()` must be called before the loopActivation timestamp. If the owner forgets or misses to call this function on time, the `lpETH` and `lpETHVault` variables would be null and the whole contract's logic will be bricked.

```solidity
    function setLoopAddresses(address _loopAddress, address _vaultAddress)
        external
        onlyAuthorized
        ❌ onlyBeforeDate(loopActivation)
    {
        lpETH = ILpETH(_loopAddress);
        lpETHVault = ILpETHVault(_vaultAddress);
        loopActivation = uint32(block.timestamp);

        emit LoopAddressesUpdated(_loopAddress, _vaultAddress);
    }
```

[Link](https://github.com/code-423n4/2024-05-loop/blob/40167e469edde09969643b6808c57e25d1b9c203/src/PrelaunchPoints.sol#L348)

### [G&#x2011;2] Compromised owner can allow malicious tokens

The `allowToken()` function allows the owner to add any token to the allowed tokens list. If the owner's account is compromised, the attacker can add a malicious token to the allowed tokens list and break contract's logic.

```solidity
    function allowToken(address _token) external onlyAuthorized {
        isTokenAllowed[_token] = true;
    }
```

[Link](https://github.com/code-423n4/2024-05-loop/blob/40167e469edde09969643b6808c57e25d1b9c203/src/PrelaunchPoints.sol#L364)

---

### [L&#x2011;1] ETH sent directly for a contract will not be locked forever.

According to the NatSpec comment of the `receive()` function: [_"ETH sent to this contract directly will be locked forever."_](https://github.com/code-423n4/2024-05-loop/blob/40167e469edde09969643b6808c57e25d1b9c203/src/PrelaunchPoints.sol#L390) - all ethers sent directly to the contract should be locked forever. However, ETH sent directly to the contract won't be locked forever but will be distributed among users who have locked ETH.

If the contract receives ETH directly before `convertAllETH()` is called, the ETH in the contract will be converted to lpETH tokens and distributed among users who have locked ETH. This means that the ETH sent directly to the contract will not be locked forever.

```solidity
function convertAllETH() external onlyAuthorized onlyBeforeDate(startClaimDate) {
        if (block.timestamp - loopActivation <= TIMELOCK) {
            revert LoopNotActivated();
        }

        // deposits all the ETH to lpETH contract. Receives lpETH back
        ❌ uint256 totalBalance = address(this).balance;
        lpETH.deposit{value: totalBalance}(address(this));

        // @audit modifies totalLpETH to be the balance of lpETH
        ❌ totalLpETH = lpETH.balanceOf(address(this));

        // Claims of lpETH can start immediately after conversion.
        startClaimDate = uint32(block.timestamp);

        emit Converted(totalBalance, totalLpETH);
    }
```

[Link](https://github.com/code-423n4/2024-05-loop/blob/40167e469edde09969643b6808c57e25d1b9c203/src/PrelaunchPoints.sol#L315)

and then inside the `_claim()` function the lpETH tokens will be distributed among users

```solidity
        if (_token == ETH) {
            // @audit 'totalLpETH' will be modified and excess ETH will be distributed to users
            ❌ claimedAmount = userStake.mulDiv(totalLpETH, totalSupply);
            balances[msg.sender][_token] = 0;
            lpETH.safeTransfer(_receiver, claimedAmount);
        } else {
```

[Link](https://github.com/code-423n4/2024-05-loop/blob/40167e469edde09969643b6808c57e25d1b9c203/src/PrelaunchPoints.sol#L248-L252)

### [L&#x2011;2] Allow users to withdraw a portion of their locked tokens

Currently there is only one `withdraw()` function. It allows a user to withdraw all of his locked tokens, but there isn't a way to make a withdraw of specific amount of tokens.

This behavior exists in the claim functions, but is missing for withdraw which creates inconsistency and a discrepancy in the user experience.

```solidity
// @audit there is no way to withdraw a specific amount of tokens
function withdraw(address _token) external {
```

[Link](https://github.com/code-423n4/2024-05-loop/blob/40167e469edde09969643b6808c57e25d1b9c203/src/PrelaunchPoints.sol#L274)

### [L&#x2011;3] Implement a function for removing allowed tokens

Currently there isn't a way to remove a token from the allowed tokens list. Consider implementing a function for removing allowed tokens.

There is a function for adding allowed tokens but not for removing them.

```solidity
    function allowToken(address _token) external onlyAuthorized {
        isTokenAllowed[_token] = true;
    }
```

[Link](https://github.com/code-423n4/2024-05-loop/blob/40167e469edde09969643b6808c57e25d1b9c203/src/PrelaunchPoints.sol#L364)

### [L&#x2011;4] No fees are provided to the exchange proxy

According to the starter guide of 0x API, [the exchange proxy contract should be provided with some wei, to cover fees.](https://github.com/0xProject/0x-api-starter-guide-code/blob/1b661d451352d8383ac4e70d221c558294b2befc/contracts/SimpleTokenSwap.sol#L96).

The problem is that `_fillQuote` function doesn't provide any fees to the exchange proxy. Which would result in a failed swap.

```solidity
        function _fillQuote(IERC20 _sellToken, uint256 _amount, bytes calldata _swapCallData) internal {
        // Track our balance of the buyToken to determine how much we've bought.
        uint256 boughtETHAmount = address(this).balance;

        require(_sellToken.approve(exchangeProxy, _amount));

        // @audit hardcoded value of 0
        ❌ (bool success,) = payable(exchangeProxy).call{value: 0}(_swapCallData);
        if (!success) {
            revert SwapCallFailed();
        }

        // Use our current buyToken balance to determine how much we've bought.
        boughtETHAmount = address(this).balance - boughtETHAmount;
        emit SwappedTokens(address(_sellToken), _amount, boughtETHAmount);
    }
```

[Link](https://github.com/code-423n4/2024-05-loop/blob/40167e469edde09969643b6808c57e25d1b9c203/src/PrelaunchPoints.sol#L497)
