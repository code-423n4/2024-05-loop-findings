##

## [L-1] ``userStake * _percentage / 100`` will not give exact percentage value

### POC 

User has a stake (userStake) of 50 tokens.
User wants to claim 25% of their stake, which would be 12.5 tokens.

Impact on Claim Amount:

In our example, 50 * 25 equals 1250.
However, due to integer division, 1250 / 100 results in 12, not 12.5.

The calculated claim amount (userClaim) becomes 12. This is less than the intended claim of 12.5 tokens due to the discarded fractional part (0.5).

```solidity
FILE: 2024-05-loop/src/PrelaunchPoints.sol

  uint256 userClaim = userStake * _percentage / 100;
            _validateData(_token, userClaim, _exchange, _data);
            balances[msg.sender][_token] = userStake - userClaim;

```
https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/src/PrelaunchPoints.sol#L253-L255

##

## [L-2] Risks of disabling locking ETHs after contract deployment 

### Impact

The setLoopAddresses function, which is guarded by the onlyAuthorized modifier, allows an authorized account (typically the contract owner) to set the addresses for the lpETH and lpETHVault contracts. Significantly, this function also resets the loopActivation timestamp to the current block timestamp (block.timestamp). 

The contract relies on the loopActivation timestamp to control the locking of ETH. The original intent appears to be setting a future date (120 days post-deployment) for when certain actions, like token locking, can begin. However, calling setLoopAddresses resets this timestamp to the current time

This functionality allows the authorized user to arbitrarily disabling the ``locking of ETH immediately`` after contract deployment. This will Block the deposits after contract ``PrelaunchPoints`` deployment. This will disable deposits in the same block.

```solidity
FILE: 2024-05-loop/src/PrelaunchPoints.sol

172: function _processLock(address _token, uint256 _amount, address _receiver, bytes32 _referral)
        internal
        onlyBeforeDate(loopActivation)


348: function setLoopAddresses(address _loopAddress, address _vaultAddress)
        external
        onlyAuthorized
        onlyBeforeDate(loopActivation)
    {
        lpETH = ILpETH(_loopAddress);
        lpETHVault = ILpETHVault(_vaultAddress);
        loopActivation = uint32(block.timestamp);

        emit LoopAddressesUpdated(_loopAddress, _vaultAddress);
    }

525: modifier onlyBeforeDate(uint256 limitDate) {
        if (block.timestamp >= limitDate) {
            revert NoLongerPossible();
        }
        _;
    }

```
https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/src/PrelaunchPoints.sol#L342-L358

### POC 

- Contract deployed the with ``loopActivation = uint32(block.timestamp + 120 days)`` which means its possible to lock ETH and ERC20 Tokens till ``block.timestamp + 120 days`` 

- In normal operations ``setLoopAddresses`` called before 120 days ends once contract deployment.

- But if ``onlyAuthorized`` decided to set immediately after contract deployment technically the value of loopActivation set to ``loopActivation = uint32(block.timestamp) ``

- After loopActivation value set the onlyBeforeDate() modifiers reverts because this check become true if (block.timestamp >= limitDate) .

So technically not possible to lock tokens after  ``loopActivation `` set. Reverts all locking process

### Recommended Mitigation
Exactly define when ``setLoopAddresses()`` called .

##

## [L-3] Inability to Withdraw ETH in Emergency Mode Post-Conversion to lpETH

After the conversion of all ETH to lpETH via convertAllETH, the withdraw function, even when called in emergency mode, faces an issue: there's no ETH left to withdraw. This is because the ETH balance is transferred out of the contract during the conversion, leaving the contract's direct balance at zero.

### Comment Suggests

```
   * @dev On emergency mode all withdrawals are accepted at

```
Users attempting to withdraw in emergency mode after the conversion process will find that despite the emergency mode being active, which is supposed to allow withdrawals freely, they are unable to withdraw any ETH because it has been fully converted.

```solidity
FILE: 2024-05-loop/src/PrelaunchPoints.sol

/**
     * @dev Called by a staker to withdraw all their ETH or LRT
     * Note Can only be called after the loop address is set and before claiming lpETH,
     * i.e. for at least TIMELOCK. In emergency mode can be called at any time.
     * @param _token      Address of the token to withdraw
     */
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
        if (_token == ETH) {
            if (block.timestamp >= startClaimDate){
                revert UseClaimInstead();
            }
            totalSupply = totalSupply - lockedAmount;

            (bool sent,) = msg.sender.call{value: lockedAmount}("");

            if (!sent) {
                revert FailedToSendEther();
            }
        } else {
            IERC20(_token).safeTransfer(msg.sender, lockedAmount);
        }

        emit Withdrawn(msg.sender, _token, lockedAmount);
    }


  /**
     * @param _mode boolean to activate/deactivate the emergency mode
     * @dev On emergency mode all withdrawals are accepted at
     */
    function setEmergencyMode(bool _mode) external onlyAuthorized {
        emergencyMode = _mode;
    }

```

##

## [L-4] Unreachable if Block in line 291

If ``if (block.timestamp >= startClaimDate) {`` true then line 279 itself reverts. Line no 291 is not reached anytime.

```solidity
FILE: 2024-05-loop/src/PrelaunchPoints.sol

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
        if (_token == ETH) {
            if (block.timestamp >= startClaimDate){
                revert UseClaimInstead();
            }
            totalSupply = totalSupply - lockedAmount;

            (bool sent,) = msg.sender.call{value: lockedAmount}("");

            if (!sent) {
                revert FailedToSendEther();
            }
        } else {
            IERC20(_token).safeTransfer(msg.sender, lockedAmount);
        }

        emit Withdrawn(msg.sender, _token, lockedAmount);
    }


```
https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/src/PrelaunchPoints.sol#L274-L306

##

## [L-5] ``if (block.timestamp <= loopActivation) { `` this check is not implemented as per docs 

The docs states that withdrawal only allowed after 7 Days once addresses are set

```
Once these addresses are set, all deposits are paused and users have 7 days to withdraw their tokens in case they changed their mind, or they detected a malicious contract being set. On withdrawal, users loose all their points.

```

```solidity
FILE: 2024-05-loop/src/PrelaunchPoints.sol

276: if (block.timestamp <= loopActivation) {

```
https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/src/PrelaunchPoints.sol#L276

If the Eth is not converted in time after 7 days technically this will allow 
withdrawal even after 7 days completed.

### Recommended Mitigation

The check should be 

```solidity

if (block.timestamp - loopActivation >= TIMELOCK) {

```

##

## [L-6] State Update After External Calls vulnerable to Reentrancy attacks

 In the _processLock function, the contract interacts with external tokens (via safeTransferFrom) after updating the internal state. This generally contradicts the recommended practice in Solidity to prevent reentrancy attacks, which advises updating the contract's state before making external calls.

Relying on this without strict adherence to the checks-effects-interactions pattern could expose the contract to risks if the external called contracts behave unexpectedly.

```solidity
FILE: 2024-05-loop/src/PrelaunchPoints.sol

 } else {
            if (!isTokenAllowed[_token]) {
                revert TokenNotAllowed();
            }
            IERC20(_token).safeTransferFrom(msg.sender, address(this), _amount);

            if (_token == address(WETH)) {
                WETH.withdraw(_amount);
                totalSupply = totalSupply + _amount;
                balances[_receiver][ETH] += _amount;
            } else {
                balances[_receiver][_token] += _amount;
            }

```
https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/src/PrelaunchPoints.sol#L182-L194

### Recommended Mitigation
Implement the Check-Effects-Intraction patteren (CEI)

##

## 

## [L-7] Uncertainty in calling ``setLoopAddresses() `` function

The uncertainty surrounding when the setLoopAddresses() function can be executed—whether immediately after deployment or just before the 120-day threshold—is indeed a significant point of concern. 

The function can technically be called at any moment before the 120-day mark, which adds an element of unpredictability. Users and stakeholders might not be aware of when these addresses could change, affecting their strategies and interactions with the contract.

```solidity
FILE: 2024-05-loop/src/PrelaunchPoints.sol

/**
     * @notice Sets the lpETH contract address
     * @param _loopAddress address of the lpETH contract
     * @dev Can only be set once before 120 days have passed from deployment.
     *      After that users can only withdraw ETH.
     */
    function setLoopAddresses(address _loopAddress, address _vaultAddress)
        external
        onlyAuthorized
        onlyBeforeDate(loopActivation)
    {
        lpETH = ILpETH(_loopAddress);
        lpETHVault = ILpETHVault(_vaultAddress);
        loopActivation = uint32(block.timestamp);

        emit LoopAddressesUpdated(_loopAddress, _vaultAddress);
    }


```
https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/src/PrelaunchPoints.sol#L342-L358



##

## [L-8] Missing the token uniqueness check in ``isTokenAllowed`` mapping

The code doesn't explicitly check for duplicate entries in the ``_allowedTokens`` array. This could allow to add the same token address multiple times, and updates the same value multiple times

```solidity
FILE: 2024-05-loop/src/PrelaunchPoints.sol

 // Allow intital list of tokens
        uint256 length = _allowedTokens.length;
        for (uint256 i = 0; i < length;) {
            isTokenAllowed[_allowedTokens[i]] = true;
            unchecked {
                i++;
            }
        }

```
https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/src/PrelaunchPoints.sol#L105-L112

##

## [L-9] Inconsistent Data Types in Date Modifiers (``onlyBeforeDate``, ``onlyAfterDate``)

The loopActivation and startClaimDate variable is declared as uint32, which can only represent values up to around 4.29 billion seconds (roughly 42.9 years). However, the modifiers onlyBeforeDate and onlyAfterDate are defined to take uint256 arguments, which can hold much larger values. This mismatch could lead to unexpected behavior and potentially creates the confusion

```solidity
FILE: 2024-05-loop/src/PrelaunchPoints.sol


    uint32 public loopActivation;
    uint32 public startClaimDate;

172: function _processLock(address _token, uint256 _amount, address _receiver, bytes32 _referral)
        internal
        onlyBeforeDate(loopActivation)

226: function claimAndStake(address _token, uint8 _percentage, Exchange _exchange, bytes calldata _data)
        external
        onlyAfterDate(startClaimDate)

  modifier onlyAfterDate(uint256 limitDate) {
        if (block.timestamp <= limitDate) {
            revert CurrentlyNotPossible();
        }
        _;
    }

    modifier onlyBeforeDate(uint256 limitDate) {
        if (block.timestamp >= limitDate) {
            revert NoLongerPossible();
        }
        _;
    }

``` 
https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/src/PrelaunchPoints.sol#L518-L530

##

## [L-10] ``onlyAuthorized`` admin can intentionally enable emergency mode even during the deposit phase 

There is a significant control given to the admin through the onlyAuthorized modifier, which allows the admin to toggle the emergency mode at will. This functionality can potentially be exploited during sensitive phases of the contract operation, particularly before critical addresses are set using the setLoopAddresses function.

- An admin, leveraging the onlyAuthorized capability, can intentionally switch the contract into emergency mode.

- If this activation occurs during the initial deposit phase, before setLoopAddresses has been called to finalize operational parameters, it can lead to unexpected behaviors.

- In emergency mode, the withdrawal restrictions are lifted, allowing users to withdraw their ETH or other tokens irrespective of other logical checks that would normally prevent such actions until the contract is fully operational or certain conditions are met.

```solidity
FILE: 2024-05-loop/src/PrelaunchPoints.sol

 /**
     * @param _mode boolean to activate/deactivate the emergency mode
     * @dev On emergency mode all withdrawals are accepted at
     */
    function setEmergencyMode(bool _mode) external onlyAuthorized {
        emergencyMode = _mode;
    }

```
https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/src/PrelaunchPoints.sol#L368-L374

##

## [L-11] Lack of Disabling Mechanism for Authorized Tokens

The allowToken function, accessible only by authorized personnel (typically contract administrators), allows a specific ERC20 token to be used within the contract by setting its corresponding entry in the isTokenAllowed mapping to true. As currently implemented, there is no method provided to set this entry back to false, effectively making the allowance irreversible

The tokens mentioned (WETH, weETH, ezETH, rsETH, rswETH, uniETH, pufETH) represent various forms or functionalities related to Ethereum, such as wrapped or specialized versions of ETH. Once any of these tokens is allowed, it remains perpetually allowable under the current system.

### Impact
- If any of the allowed tokens encounter security vulnerabilities, such as susceptibility to hacks or other exploitative behaviors, the contract lacks a straightforward method to halt their use. This can expose users and the contract itself to increased risk.

- If a token becomes deprecated or its usage within the ecosystem becomes harmful or undesirable, the inability to disallow it could lead to complications in managing the overall health and integrity of the contract.

```solidity
FILE: 2024-05-loop/src/PrelaunchPoints.sol

 // Allow intital list of tokens
        uint256 length = _allowedTokens.length;
        for (uint256 i = 0; i < length;) {
            isTokenAllowed[_allowedTokens[i]] = true;
            unchecked {
                i++;
            }

 /**
     * @param _token address of a wrapped LRT token
     * @dev ONLY add wrapped LRT tokens. Contract not compatible with rebase tokens.
     */
    function allowToken(address _token) external onlyAuthorized {
        isTokenAllowed[_token] = true;
    }

```
https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/src/PrelaunchPoints.sol#L360-L366

##

## [L-12] Missing contract-existence checks before low-level calls

Low-level calls return success if there is no code present at the specified address. In addition to the zero-address checks, add a check to verify that <address>.code.length > 0

```solidity
FILE: 2024-05-loop/src/PrelaunchPoints.sol   

296: (bool sent,) = msg.sender.call{value: lockedAmount}("");

```
https://github.com/code-423n4/2024-05-loop/blob/0dc8467ccff27230e7c0530b619524cc8401e22a/src/PrelaunchPoints.sol#L296C12-L296C69


