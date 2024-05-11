# QA Report For LoopFi Audit

## 1. CEI Violated

### Description

State change takes place after the external call happening in the `_fillQuote` function here ->

External call -> https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L497

State change -> https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L503

### Recommendation:

CEI should be followed and the external call should take place after the state change.


## 2. No Slippage Control In The lpETH Deposit

### Description:

The claimed amount is deposited in to the lpETH contract (for lpETH tokens)  here
https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L263

This deposit function only takes one amount parameter  , meaning there is no slippage control in the deposit function . Since it is a vault system the users might be subject to slippage while depositing.

### Recommendation:

Introduce slippage protection mechanism for example a min lpETHOut parameter.


## 3. Introduce A Function To Remove An Allowed Token In Case Of Emergency

### Description:

The protocol has a whitelist of tokens that can be locked in the contract (set in the constructor) this includes weth , rsETH etc . It is recommended to have a 
removeToken function in case where a token loses it's peg to ETH (for example) , this will work as a safeguard in emergency situations.

### Recommendation:

 It is recommended to have a removeToken function.


## 4. Account For LRT More Carefully

### Description:

Most of the LRTs allowed by the protocol (weETH, ezETH, rsETH, rswETH, uniETH, pufETH) are upgradeable , therefore it is possible that the implementation of these tokens is upgraded , for example some tokens might introduce a fee on transfer mechanism , or the decimals might be upgraded to something !=18 in that case the whole accounting of lpETH shares will be broken.  Cases like these should be handled carefully and risks should be acknowledged.

### Recommendation:

Handle the risks where token is upgraded carefullly.


## 5. Have A onlyWETH on the receive() function

### Description:

The only use case for the receive function is when the WETH contract sends ETH to the contract , to avoid directly sent ether it is recommended to have a simple require check that only allows eth transfer from WETH.

https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L392

### Recommendation:

Have a check in the receive function something like this ->

`if( msg.sender != WETH ) revert;`


## 6 . Use += For Uniformity

### Description:

Here while updating users balance mapping we use += https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L181 but here https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L180 we have used x = x+y , it is recommended to have uniformity.

### Recommendation:

Use += too at L180


## 7. Centralization Risks

### Description:

The owner address has excessive privileges  , this includes putting the contract in an emergency state , assigning new tokens as whitelisted tokens (might assign a token which
is not pegged to ETH) and call to update the loop activation so that activation period ends abruptly and no users can lock their tokens.

### Recommendation:

It is recommended that the owner key is a multisig with a timelock where each key resides on a different offline server.



## 8. Prohibit Locking In Emergency State

### Description:

In an emergency state the users can withdraw their locked amounts , this might be due to many reasons and the protocol wants to give users a withdraw function to in such a scenario.
It is recommended to pause locking too when an emergency is imposed on the contract to prohibit users from interacting with the contract any further till the emergency situation gets resolved.

### Recommendation:

It is recommended to pause locking too when an emergency is imposed on the contract to prohibit users from interacting with the contract any further till the emergency situation gets resolved.

