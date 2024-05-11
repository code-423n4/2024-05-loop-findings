| Issue Category | Description                                       |
|----------------|---------------------------------------------------|
| **L-1**        | Disorderly emission of events due to reentry      |
| **L-2**        | Users may have a small amount stuck in the contract |
| **L-3**        | Potential overflow due to exceeding percentage    |
| **L-4**        | Users can choose different referrals during each lock |
| **N-1**        | Inability to cancel allowToken                     |
| **N-2**        | Supplementary comments issue                       |

# [L-1] Due to reentry, events may be emit out in a disorderly manner
## Impact
In the withdraw function, the contract first sends an ether to the caller, and then emit an event. Due to reentry, the event may be triggered out of order.  
github:[https://github.com/code-423n4/2024-05-loop/blob/40167e469edde09969643b6808c57e25d1b9c203/src/PrelaunchPoints.sol#L296](https://github.com/code-423n4/2024-05-loop/blob/40167e469edde09969643b6808c57e25d1b9c203/src/PrelaunchPoints.sol#L296)
```solidity
            (bool sent, ) = msg.sender.call{value: lockedAmount}("");

            if (!sent) {
                revert FailedToSendEther();
            }
        } else {
            IERC20(_token).safeTransfer(msg.sender, lockedAmount);
        }

        emit Withdrawn(msg.sender, _token, lockedAmount);
```
## Recommended Mitigation Steps
```solidity
+        emit Withdrawn(msg.sender, _token, lockedAmount);
        if (_token == ETH) {
            ...
            (bool sent,) = msg.sender.call{value: lockedAmount}("");
            ...
        } else {
            IERC20(_token).safeTransfer(msg.sender, lockedAmount);
        }

-        emit Withdrawn(msg.sender, _token, lockedAmount);
```

# [L-2] Users who claim multiple times may have a small amount stuck in the contract
For claims of tokens other than ETH, the amount of each claim is the percentage of the remaining balance.  
In the end, the user may only have a small amount of amount stored in the contract.  
1. "The minimum amount to receive is determined offchain and controlled by a slippage parameter in the frontend dApp" which in readme
2. During swap, no ETH was exchanged due to the small amount
github:[https://github.com/code-423n4/2024-05-loop/blob/40167e469edde09969643b6808c57e25d1b9c203/src/PrelaunchPoints.sol#L253](https://github.com/code-423n4/2024-05-loop/blob/40167e469edde09969643b6808c57e25d1b9c203/src/PrelaunchPoints.sol#L253)
```solidity
            uint256 userClaim = userStake * _percentage / 100;
            _validateData(_token, userClaim, _exchange, _data);
            balances[msg.sender][_token] = userStake - userClaim;
```
## Recommended Mitigation Steps
Suggest checking the balance after userStack - userClaim. If it is less than a certain value in the frontend dApp, set userClaim to userStack.

# [L-3] The _percentage may exceed 100

## Impact
The _percentage represents the percentage of claim quantity.There is no restriction on it in the function, and user input can exceed 100, resulting in a balance overflow that may bring a bad experience and waste gas.  
github:[https://github.com/code-423n4/2024-05-loop/blob/40167e469edde09969643b6808c57e25d1b9c203/src/PrelaunchPoints.sol#L253](https://github.com/code-423n4/2024-05-loop/blob/40167e469edde09969643b6808c57e25d1b9c203/src/PrelaunchPoints.sol#L253)
```solidity
            uint256 userClaim = userStake * _percentage / 100;
            _validateData(_token, userClaim, _exchange, _data);
            balances[msg.sender][_token] = userStake - userClaim;
```
## Recommended Mitigation Steps
Check the input of _percentage.  
```solidity
    function _claim(address _token, address _receiver, uint8 _percentage, Exchange _exchange, bytes calldata _data)
            internal
            returns (uint256 claimedAmount)
        {
+            require(_percentage <= 100, "Invalid percentage")
```

# [L-4] Users can choose different _referrals during each lock
## Impact
_Referral can provide additional points for corresponding users.  
But users can use different referrals in each lock  
github:[https://github.com/code-423n4/2024-05-loop/blob/40167e469edde09969643b6808c57e25d1b9c203/src/PrelaunchPoints.sol#L124](https://github.com/code-423n4/2024-05-loop/blob/40167e469edde09969643b6808c57e25d1b9c203/src/PrelaunchPoints.sol#L124)
```solidity
    function lockETH(bytes32 _referral) external payable {
        _processLock(ETH, msg.value, msg.sender, _referral);
    }
```
## Recommended Mitigation Steps
```solidity
    mapping (address => bytes32 _referral) referral;
    function _processLock(
        address _token,
        uint256 _amount,
        address _receiver,
        bytes32 _referral
    ) internal onlyBeforeDate(loopActivation) {
+        if(referral[receiver] != bytes32(0) && _referral != bytes32(0)){
+            referral[receiver] = _referral;
+        }

```

# [N-1] allowtoken cannot be cancelled
## Impact
Can only allowtoken and cannot cancel, lacking flexibility in emergency situations and incorrect input.  
For example, when allowToken is mistakenly transferred to the contract, it will never be able to be retrieved.  
If a token address is added incorrectly, its impact will not be terminated.  
github:[https://github.com/code-423n4/2024-05-loop/blob/40167e469edde09969643b6808c57e25d1b9c203/src/PrelaunchPoints.sol#L364](https://github.com/code-423n4/2024-05-loop/blob/40167e469edde09969643b6808c57e25d1b9c203/src/PrelaunchPoints.sol#L364)
```solidity
    function allowToken(address _token) external onlyAuthorized {
        isTokenAllowed[_token] = true;
    }
```
## Recommended Mitigation Steps
```solidity
    function updateToken(address _token, bool state) external onlyAuthorized {
        isTokenAllowed[_token] = state;
    }
```

# [N-2] supplementary comments
## Impact
The annotation of the claim function does not fully identify the parameters of NOT useful for ETH
github:[https://github.com/code-423n4/2024-05-loop/blob/40167e469edde09969643b6808c57e25d1b9c203/src/PrelaunchPoints.sol#L361](https://github.com/code-423n4/2024-05-loop/blob/40167e469edde09969643b6808c57e25d1b9c203/src/PrelaunchPoints.sol#L361)
```solidity
     * @param _percentage Proportion in % of tokens to withdraw. NOT useful for ETH
     * @param _exchange   Exchange identifier where the swap takes place
     * @param _data       Swap data obtained from 0x API
```
## Recommended Mitigation Steps
```solidity
     * @param _percentage Proportion in % of tokens to withdraw. NOT useful for ETH
-     * @param _exchange   Exchange identifier where the swap takes place
-     * @param _data       Swap data obtained from 0x API
+     * @param _exchange   Exchange identifier where the swap takes place. NOT useful for ETH
+     * @param _data       Swap data obtained from 0x API. NOT useful for ETH
```