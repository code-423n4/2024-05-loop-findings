# QA Report: LoopFi

## L1: Front-running risk and market manipulation opportunity in the `convertAllETH` function:

### Impact

The `convertAllETH` function in the PrelaunchPoints contract has a front-running risk that could allow the owner or an attacker monitoring the mempool to manipulate the market and profit at the expense of users.

### Issue Description

The `convertAllETH` function allows the contract owner to convert all the locked ETH in the contract to lpETH at any time after a timelock period from when the lpETH address is set.

https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L315

```solidity
function convertAllETH() external onlyAuthorized onlyBeforeDate(startClaimDate) {
    if (block.timestamp - loopActivation <= TIMELOCK) {
        revert LoopNotActivated();
    }

    // deposits all the ETH to lpETH contract. Receives lpETH back 
    uint256 totalBalance = address(this).balance;
    lpETH.deposit{value: totalBalance}(address(this));

    totalLpETH = lpETH.balanceOf(address(this));

    // Claims of lpETH can start immediately after conversion.
    startClaimDate = uint32(block.timestamp); 

    emit Converted(totalBalance, totalLpETH);
}
```

This design has two main issues:

1. Front-running: If the owner's transaction to call `convertAllETH` is visible in the mempool, attackers could front-run it with their own ETH deposits right before the conversion happens. They would then benefit from a large amount of locked ETH being converted to lpETH, potentially increasing their share of lpETH rewards.

2. Market manipulation: The owner has the ability to time the market and choose when to convert the locked ETH based on market conditions that are favorable to them. For example, they could wait until lpETH is trading at a premium and then trigger the conversion, giving themselves an advantageous conversion rate compared to the users who originally locked their ETH.

Both of these issues allow the owner or attackers to extract value from users of the protocol in an unfair way. Users locking ETH have no guarantee about the timing or exchange rate of the eventual conversion to lpETH.

### Proof of Concept

An attacker could monitor the mempool for `convertAllETH` transactions and front-run them with their own `lock`/`lockETH` transactions to deposit ETH right before conversion. 

The contract owner could monitor the lpETH market price off-chain and choose to call `convertAllETH` at a time when lpETH price is high to get a favorable conversion rate for themselves.

### Tools Used

Manual review

### Recommended Mitigation Steps


1. Remove owner control of `convertAllETH` and instead have the conversion triggered automatically based on predetermined time or conditions that are not easily gameable. For example, convert a fixed portion of ETH to lpETH daily over a longer period.

2. Provide users more transparency and guarantees around when their ETH will be converted and the rate they will receive, so they are not disadvantaged compared to the owner or attackers who can time the market.

3. Put a cap on the amount of ETH that can be locked in the final block(s) before conversion to prevent front-running.

4. Use a TWAP (time-weighted average price) oracle to determine the ETH/lpETH conversion rate at the time of conversion, to smooth out any price manipulation.

## L2: block.timestamp is Manipulatable by Miners

### Impact

The PrelaunchPoints contract uses `block.timestamp` for critical time-based conditions and calculations. However, `block.timestamp` can be manipulated by miners to a certain degree, which could allow gaming of the contract's time-dependent functionality.

### Issue Description

The contract uses `block.timestamp` in several places for important time-based logic:

1. In the constructor to set the `loopActivation` and `startClaimDate` variables which determine when ETH can be converted to lpETH and when lpETH rewards can start being claimed:

```solidity
constructor(address _exchangeProxy, address _wethAddress, address[] memory _allowedTokens) {
    owner = msg.sender;
    exchangeProxy = _exchangeProxy;
    WETH = IWETH(_wethAddress);

    loopActivation = uint32(block.timestamp + 120 days); 
    startClaimDate = 4294967295; // Max uint32 ~ year 2107
    ...
}
```

2. In the `convertAllETH` function to check if enough time has passed since `loopActivation` and to set the `startClaimDate` for lpETH claims:

https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L315

```solidity
function convertAllETH() external onlyAuthorized onlyBeforeDate(startClaimDate) {
    if (block.timestamp - loopActivation <= TIMELOCK) {
        revert LoopNotActivated();
    }
    ...
    startClaimDate = uint32(block.timestamp);
    ...
}
```

3. In the `setLoopAddresses` function to update the `loopActivation` timestamp

https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L348

```solidity
function setLoopAddresses(address _loopAddress, address _vaultAddress) 
    external
    onlyAuthorized
    onlyBeforeDate(loopActivation)
{
    ...
    loopActivation = uint32(block.timestamp);
    ...
}
```

The issue is that miners have some influence over the `block.timestamp` value within a certain tolerance (e.g., several seconds). They could choose to publish a block with a timestamp that is advantageous to them if they are also interacting with this contract.

For example, a miner who is also the contract owner could manipulate the timestamp to make it appear that the 120 day `loopActivation` delay has passed when calling `convertAllETH`, even if slightly less time has actually elapsed. Or they could game the exact `startClaimDate` timestamp to give themselves an advantage on lpETH claims.

### Proof of Concept

Let's say the real UTC time is 2023-01-01 11:59:50 when the current block is being mined, and the `loopActivation` timestamp is set to 2023-01-01 12:00:00. 

A miner who is also the contract owner could choose to publish the block with a timestamp of 2023-01-01 12:00:10, making it appear that the 120 day timelock has passed, even though in reality it's still 10 seconds too early.

They could then immediately call `convertAllETH` in the same block, which would pass the `block.timestamp - loopActivation <= TIMELOCK` check due to the manipulated timestamp, even though the full 120 days haven't quite elapsed in real time.

### Tools Used

Manual

### Recommended Mitigation Steps

1. Use `block.number` (the block height) instead of `block.timestamp` where possible for time-based logic. Block numbers can't be manipulated by miners.

2. If `block.timestamp` must be used, be aware of the possible manipulation tolerance (e.g., 15 seconds) and build in a safety margin. For example, check `block.timestamp > loopActivation + TIMELOCK + 15 seconds` to account for possible manipulation.

3. For longer time periods like the 120 day `loopActivation` delay, consider using an external trusted timestamp oracle that miners can't manipulate.

4. Avoid using timestamp-based logic for critical contract state changes or high-value decisions. Prefer block numbers or external oracles.

## L3: Insufficient Frontend Slippage Controls 

### Impact

The PrelaunchPoints contract allows users to claim their lpETH rewards by swapping their locked tokens for ETH through the 0x API in the `claim` and `claimAndStake` functions. However, there are insufficient controls on the slippage of these swaps, which could lead to users receiving less ETH and lpETH than expected.

### Issue Description

In the `claim` and `claimAndStake` functions, users specify an `Exchange` (either UniswapV3 or TransformERC20) and a `bytes` parameter `_data` that encodes the calldata for the 0x API swap

https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L211

https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L226

```solidity
function claim(address _token, uint8 _percentage, Exchange _exchange, bytes calldata _data)
    external 
    onlyAfterDate(startClaimDate)
{
    _claim(_token, msg.sender, _percentage, _exchange, _data);
}

function claimAndStake(address _token, uint8 _percentage, Exchange _exchange, bytes calldata _data) 
    external
    onlyAfterDate(startClaimDate) 
{
    uint256 claimedAmount = _claim(_token, address(this), _percentage, _exchange, _data);
    ...
}
```

This `_data` is expected to be obtained from the 0x API by the frontend. The contract then validates and executes this swap in the `_fillQuote` internal function:

```solidity
function _fillQuote(IERC20 _sellToken, uint256 _amount, bytes calldata _swapCallData) internal {
    // Track our balance of the buyToken to determine how much we've bought.
    uint256 boughtETHAmount = address(this).balance;

    require(_sellToken.approve(exchangeProxy, _amount));
    
    (bool success,) = payable(exchangeProxy).call{value: 0}(_swapCallData);
    if (!success) {
        revert SwapCallFailed();
    }
    
    // Use our current buyToken balance to determine how much we've bought.
    boughtETHAmount = address(this).balance - boughtETHAmount;
    emit SwappedTokens(address(_sellToken), _amount, boughtETHAmount);
}
```

The issue is that there are no checks on the slippage of the swap in the contract. It blindly executes whatever swap calldata is provided in `_data`, regardless of the exchange rate.

This means that if the frontend passes in a `_data` that encodes a swap with a very poor exchange rate (high slippage), the contract will still execute it, and the user will receive much less ETH and lpETH than they were expecting based on current market prices. 

The contract is fully trusting the frontend to provide a fair swap, but a malicious or buggy frontend could easily pass in a swap that is very disadvantageous to the user. The contract should enforce some slippage controls as a backstop.

### Proof of Concept

Let's say the current fair market rate is 1 ABC token = 1 ETH. 

A user with 100 ABC tokens locked in the contract goes to claim their rewards. The frontend, due to a bug, provides a `_data` payload that encodes a swap of 100 ABC for only 50 ETH (a 50% slippage).

When the user calls `claim` with this `_data`, the contract will execute this swap without any checks, even though the rate is much worse than market rate. 

The user will only receive 50 ETH and proportionally less lpETH, when they were expecting closer to 100 based on the fair exchange rate.

### Tools Used

Manual code review

### Recommended Mitigation Steps

Consider implementing some slippage controls in the PrelaunchPoints contract itself:

1. Add a `_maxSlippage` parameter to `claim` and `claimAndStake` that specifies the maximum allowed slippage percentage for the swap. 

2. In `_fillQuote`, after executing the swap, calculate the actual exchange rate achieved and revert if the slippage is greater than `_maxSlippage`.

For example:
```solidity
function _fillQuote(IERC20 _sellToken, uint256 _amount, bytes calldata _swapCallData, uint256 _maxSlippage) internal {
    uint256 boughtETHAmount = address(this).balance;
    require(_sellToken.approve(exchangeProxy, _amount));
    
    (bool success,) = payable(exchangeProxy).call{value: 0}(_swapCallData);
    if (!success) {
        revert SwapCallFailed();
    }
    
    boughtETHAmount = address(this).balance - boughtETHAmount;
    
    // Check slippage
    uint256 expectedETHAmount = oracle.getExpectedRate(_sellToken, ETH, _amount); 
    uint256 slippage = (expectedETHAmount - boughtETHAmount) * 1e18 / expectedETHAmount;
    require(slippage <= _maxSlippage, "Slippage too high");

    emit SwappedTokens(address(_sellToken), _amount, boughtETHAmount);
}
```

This way, even if the frontend provides a swap with a poor rate, the contract will revert the transaction if the slippage is higher than the user-specified max, protecting the user.

## L4: Lack of post-swap validation checks

### Impact

The PrelaunchPoints contract does not perform sufficient validation checks on the results of token swaps executed via the 0x API in the `claim` and `claimAndStake` functions. This could allow malicious or buggy 0x API responses to cause unintended behavior, such as users receiving less ETH and lpETH than expected, or the contract accepting ERC20 tokens it shouldn't.

### Issue Description

When a user calls the `claim` or `claimAndStake` functions to claim their lpETH rewards, the contract swaps their locked tokens for ETH using a 0x API swap specified by the `_data` parameter.

https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L211

https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L226

https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L240

https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L491

```solidity
function claim(address _token, uint8 _percentage, Exchange _exchange, bytes calldata _data)
    external 
    onlyAfterDate(startClaimDate)
{
    _claim(_token, msg.sender, _percentage, _exchange, _data);
}

function claimAndStake(address _token, uint8 _percentage, Exchange _exchange, bytes calldata _data) 
    external
    onlyAfterDate(startClaimDate) 
{
    uint256 claimedAmount = _claim(_token, address(this), _percentage, _exchange, _data);
    ...
}
```

The `_claim` internal function then calls `_fillQuote` to execute this swap:

```solidity
function _fillQuote(IERC20 _sellToken, uint256 _amount, bytes calldata _swapCallData) internal {
    // Track our balance of the buyToken to determine how much we've bought.
    uint256 boughtETHAmount = address(this).balance;

    require(_sellToken.approve(exchangeProxy, _amount));
    
    (bool success,) = payable(exchangeProxy).call{value: 0}(_swapCallData);
    if (!success) {
        revert SwapCallFailed();
    }
    
    // Use our current buyToken balance to determine how much we've bought.
    boughtETHAmount = address(this).balance - boughtETHAmount;
    emit SwappedTokens(address(_sellToken), _amount, boughtETHAmount);
}
```

The issue is that after executing the swap, there are no checks on the results other than that the call didn't revert. In particular:

1. There is no check that the contract actually received ETH from the swap. If the 0x API returned data for a swap to a different token, the contract would accept it.

2. There is no check on how much ETH was received compared to the amount of tokens sold. If the 0x API returned data for a very unfavorable swap rate, the contract would execute it.

3. There is no check that the contract's balance of the sold token decreased by the expected amount. If the 0x API returned data that didn't actually transfer tokens from the contract, it wouldn't be caught.

Without these post-swap validation checks, the contract is blindly trusting that the 0x API response will always be for a fair swap that transfers the expected amount of tokens. A malicious or buggy API response could lead to the contract misbehaving.

### Proof of Concept

Consider a scenario where a malicious actor gains control of the 0x API endpoint that the frontend uses to fetch swap data.

When a user goes to claim their rewards, the malicious API returns `_data` that encodes a swap selling the user's locked tokens for a very small amount of ETH (say 1 wei).

The `_fillQuote` function would execute this swap without any checks, severely shortchanging the user on their expected ETH and lpETH amount.

Alternatively, the malicious API could return `_data` that doesn't actually transfer the sold tokens out of the PrelaunchPoints contract. The swap would complete, but the contract would still have the tokens, allowing them to potentially be drained later by the attacker.

### Tools Used

Manual code review

### Recommended Mitigation Steps

Add post-swap validation checks to the `_fillQuote` function to verify the results of the swap:

```solidity
function _fillQuote(IERC20 _sellToken, uint256 _amount, bytes calldata _swapCallData) internal {
    uint256 boughtETHAmount = address(this).balance;
    uint256 startingSellTokenBalance = _sellToken.balanceOf(address(this));

    require(_sellToken.approve(exchangeProxy, _amount));
    
    (bool success,) = payable(exchangeProxy).call{value: 0}(_swapCallData);
    require(success, "Swap failed");
    
    boughtETHAmount = address(this).balance - boughtETHAmount;
    require(boughtETHAmount > 0, "No ETH received from swap");

    uint256 endingSellTokenBalance = _sellToken.balanceOf(address(this));
    require(endingSellTokenBalance == startingSellTokenBalance - _amount, "Incorrect amount of sell token transferred");

    // Check slippage
    uint256 expectedETHAmount = oracle.getExpectedRate(_sellToken, ETH, _amount); 
    uint256 maxSlippage = 1e16; // 1%
    uint256 slippage = (expectedETHAmount - boughtETHAmount) * 1e18 / expectedETHAmount;
    require(slippage <= maxSlippage, "Swap slippage too high");

    emit SwappedTokens(address(_sellToken), _amount, boughtETHAmount);
}
```

This checks that:
1. The swap call succeeded and the contract's ETH balance increased
2. The contract's balance of the sold token decreased by the expected amount
3. The amount of ETH received is within an acceptable slippage tolerance of the expected amount based on an oracle price


## L5: Event log spamming due insufficient parameter validation can affect off-chain referral analysis

### Impact

The PrelaunchPoints contract does not perform sufficient validation on the `_referral` parameter passed to its `lock` and `lockFor` functions, and this parameter is not validated in the internal `_processLock` function either. This could allow users to spam the contract with arbitrary `_referral` values, leading to a large number of `Locked` events being emitted with meaningless `_referral` data. This referral spam could bloat the event logs, increase gas costs for legitimate users, and make it harder to interpret the true referral activity of the contract.

### Issue Description

The `lockETH`, `lockETHFor`, `lock` and `lockFor` functions are the entry points for users to lock tokens in the PrelaunchPoints contract. They both take a `_referral` parameter, which is a `bytes32` value intended to represent the referrer for the lock action. However, these functions do not validate the `_referral` parameter at all before passing it to the internal `_processLock` function

https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L124C5-L199C1


```solidity
function lock(address _token, uint256 _amount, bytes32 _referral) external {
    if (_token == ETH) {
        revert InvalidToken();
    }
    _processLock(_token, _amount, msg.sender, _referral);
}

function lockFor(address _token, uint256 _amount, address _for, bytes32 _referral) external {
    if (_token == ETH) {
        revert InvalidToken();
    }
    _processLock(_token, _amount, _for, _referral);
}
```

The `_processLock` function also does not perform any validation on the `_referral` parameter before emitting it in a `Locked` event:

```solidity
function _processLock(address _token, uint256 _amount, address _receiver, bytes32 _referral) 
    internal
    onlyBeforeDate(loopActivation) 
{
    if (_amount == 0) {
        revert CannotLockZero();
    }
    ...
    emit Locked(_receiver, _amount, _token, _referral);
}
```

This means that a user can pass any arbitrary `bytes32` value as `_referral`, and it will be emitted in a `Locked` event, even if it does not represent a valid referrer.

A malicious user could exploit this to spam the contract with `lock` and `lockFor` calls with random `_referral` values, causing a large number of `Locked` events to be emitted with meaningless referral data. 

This referral spam would bloat the contract's event logs, making it harder for off-chain services to interpret the true referral activity. It could also increase gas costs for legitimate users, as the cost of emitting events is shared by all users of the contract.

### Proof of Concept

Consider a malicious user who writes a script to repeatedly call `lock` with a small amount and a random `_referral` value:

```solidity
function spam(PrelaunchPoints pp, address token) external {
    for (uint i = 0; i < 1000; i++) {
        pp.lock(token, 1, bytes32(uint256(keccak256(abi.encodePacked(i)))));
    }
}
```

This would result in 1000 `Locked` events being emitted, each with a different random `_referral` value. 

If this spamming were repeated over many transactions, it could significantly bloat the contract's event logs with meaningless referral data, making it difficult for off-chain services to distinguish real referrals from spam.

### Tools Used

Manual code review

### Recommended Mitigation Steps

1. Validate that the `_referral` parameter is not zero, as a zero value is likely to indicate a lack of a referrer rather than a valid referrer ID.

2. Consider validating that the `_referral` parameter corresponds to a valid referrer ID in your system. This could involve checking that the ID exists in a referrer registry contract or database.

3. If a referrer registry is not feasible, consider adding a mapping of valid referrer IDs to the PrelaunchPoints contract itself, and checking that `_referral` is present in this mapping.

4. Enforce a reasonable minimum lock threshold to prevent spamming

For example, the `lock` function could be modified as follows:

```solidity
function lock(address _token, uint256 _amount, bytes32 _referral) external {
    if (_token == ETH) {
        revert InvalidToken();
    }
    if (_referral == bytes32(0)) {
        revert InvalidReferral();
    }
    if (!isValidReferral(_referral)) {
        revert InvalidReferral();
    }
    _processLock(_token, _amount, msg.sender, _referral);
}
```


## L6: No check if _loopAddress and _vaultAddress implement the required interfaces

### Impact

The `setLoopAddresses` function in the PrelaunchPoints contract does not check if the provided `_loopAddress` and `_vaultAddress` parameters implement the required `ILpETH` and `ILpETHVault` interfaces respectively. This could lead to the contract interacting with invalid or malicious contracts, potentially causing loss of funds or unexpected behavior.

### Issue Description

The `setLoopAddresses` function is used to set the addresses of the lpETH token contract (`lpETH`) and the lpETH vault contract (`lpETHVault`) that the PrelaunchPoints contract interacts with:

https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L348

```solidity
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

The function takes `_loopAddress` and `_vaultAddress` as parameters and assigns them to the `lpETH` and `lpETHVault` state variables respectively, after casting them to the `ILpETH` and `ILpETHVault` interfaces.

However, there is no check to ensure that the provided addresses actually implement these interfaces. If a non-compliant address is provided, the casting will succeed, but subsequent interactions with `lpETH` or `lpETHVault` will fail or behave unexpectedly.

For example, when a user calls `claim`, the contract attempts to transfer lpETH to the user:

```solidity
function _claim(address _token, address _receiver, uint8 _percentage, Exchange _exchange, bytes calldata _data)
    internal
    returns (uint256 claimedAmount) 
{
    ...
    if (_token == ETH) {
        ...
        lpETH.safeTransfer(_receiver, claimedAmount);
    } else {
        ...
        lpETH.deposit{value: claimedAmount}(_receiver);
    }
    ...
}
```

If `lpETH` does not actually implement the `safeTransfer` or `deposit` functions as expected by the `ILpETH` interface, these calls will revert, and users will be unable to claim their rewards.

Similarly, when a user calls `claimAndStake`, the contract attempts to stake the claimed lpETH in the vault:

```solidity
function claimAndStake(address _token, uint8 _percentage, Exchange _exchange, bytes calldata _data) 
    external
    onlyAfterDate(startClaimDate) 
{
    uint256 claimedAmount = _claim(_token, address(this), _percentage, _exchange, _data);
    lpETH.approve(address(lpETHVault), claimedAmount);
    lpETHVault.stake(claimedAmount, msg.sender);

    emit StakedVault(msg.sender, claimedAmount);
}
```

If `lpETHVault` does not implement the `stake` function as expected by the `ILpETHVault` interface, this call will revert, and users will be unable to stake their claimed lpETH.

In the worst case, if malicious contracts are set as `lpETH` or `lpETHVault`, they could potentially drain funds from the PrelaunchPoints contract or from users interacting with it.

### Proof of Concept

Consider a scenario where the contract owner accidentally sets `lpETH` to the address of a token contract that does not implement the `ILpETH` interface:

```solidity
NonCompliantToken nonCompliantToken = new NonCompliantToken();
prelaunchPoints.setLoopAddresses(address(nonCompliantToken), address(validVault));
```

Now, when a user tries to claim their rewards, the `lpETH.safeTransfer` call in `_claim` will revert because `NonCompliantToken` does not have a `safeTransfer` function, effectively blocking all claims.

Similarly, if `lpETHVault` is set to a non-compliant contract, the `lpETHVault.stake` call in `claimAndStake` will revert, blocking the staking of claimed lpETH.

### Tools Used

Manual code review

### Recommended Mitigation Steps

Add checks in the `setLoopAddresses` function to ensure that the provided addresses implement the required interfaces:

```solidity
function setLoopAddresses(address _loopAddress, address _vaultAddress) 
    external
    onlyAuthorized
    onlyBeforeDate(loopActivation)
{
    require(_loopAddress.supportsInterface(type(ILpETH).interfaceId), "Invalid lpETH address");
    require(_vaultAddress.supportsInterface(type(ILpETHVault).interfaceId), "Invalid vault address");

    lpETH = ILpETH(_loopAddress);
    lpETHVault = ILpETHVault(_vaultAddress);
    loopActivation = uint32(block.timestamp);

    emit LoopAddressesUpdated(_loopAddress, _vaultAddress);
}
```

## L7: Lack of a pausable lock functionality

### Impact

The PrelaunchPoints contract does not have a pause mechanism to halt deposits and locks in case a major issue or exploit is detected. This could lead to a situation where a vulnerability is discovered but the contract owner is unable to prevent further deposits, potentially exacerbating the impact of the exploit and leading to greater loss of funds.

### Issue Description

The PrelaunchPoints contract allows users to lock ETH and other allowed tokens via the `lock`, `lockETH`, `lockFor`, and `lockETHFor` functions

https://github.com/code-423n4/2024-05-loop/blob/main/src/PrelaunchPoints.sol#L124C5-L199C1

```solidity
function lockETH(bytes32 _referral) external payable {
    _processLock(ETH, msg.value, msg.sender, _referral);
}

function lockETHFor(address _for, bytes32 _referral) external payable {
    _processLock(ETH, msg.value, _for, _referral);
}

function lock(address _token, uint256 _amount, bytes32 _referral) external {
    if (_token == ETH) {
        revert InvalidToken();
    }
    _processLock(_token, _amount, msg.sender, _referral);
}

function lockFor(address _token, uint256 _amount, address _for, bytes32 _referral) external {
    if (_token == ETH) {
        revert InvalidToken();
    }
    _processLock(_token, _amount, _for, _referral);
}
```

However, there is no way for the contract owner to pause these functions in case of an emergency, such as the discovery of a critical bug or an ongoing exploit.

This means that even if a vulnerability is identified, users could continue to deposit and lock funds into the contract, potentially increasing the amount of funds at risk.

Moreover, the inability to pause deposits could delay the remediation process. The contract owner would have to rush to deploy a fix while the exploit is ongoing, rather than being able to pause deposits first, and then carefully develop and test a solution.

In a worst-case scenario, this could lead to a complete draining of funds from the contract before a fix can be implemented, causing significant financial loss for users and damage to the platform's reputation.

### Proof of Concept

Consider a scenario where a critical vulnerability is discovered in the PrelaunchPoints contract that allows an attacker to drain funds.

The contract owner becomes aware of the exploit and starts working on a fix. However, they have no way to prevent users from continuing to lock funds into the contract while the fix is being developed.

The attacker continues to exploit the vulnerability, draining more and more funds from the contract as new deposits come in.

By the time the contract owner is able to deploy a fix, a significant portion of the contract's funds have been stolen, causing major losses for users who deposited during the time the exploit was known but unpatched.

### Tools Used

Manual code review

## Recommended Mitigation Steps

Implement a pausable lock functionality in the PrelaunchPoints contract:

1. Add a `paused` state variable to track whether locks are currently paused:

```solidity
bool public paused;
```

2. Add a `setPaused` function that allows the contract owner to pause or unpause locks:

```solidity
function setPaused(bool _paused) external onlyAuthorized {
    paused = _paused;
    emit PauseChanged(_paused);
}
```

3. Add a `whenNotPaused` modifier to the `lock`, `lockETH`, `lockFor`, and `lockETHFor` functions:

```solidity
modifier whenNotPaused() {
    require(!paused, "Locks are paused");
    _;
}

function lockETH(bytes32 _referral) external payable whenNotPaused {
    _processLock(ETH, msg.value, msg.sender, _referral);
}

function lockETHFor(address _for, bytes32 _referral) external payable whenNotPaused {
    _processLock(ETH, msg.value, _for, _referral);
}

function lock(address _token, uint256 _amount, bytes32 _referral) external whenNotPaused {
    if (_token == ETH) {
        revert InvalidToken();
    }
    _processLock(_token, _amount, msg.sender, _referral);
}

function lockFor(address _token, uint256 _amount, address _for, bytes32 _referral) external whenNotPaused {
    if (_token == ETH) {
        revert InvalidToken();
    }
    _processLock(_token, _amount, _for, _referral);
}
```

With these changes, the contract owner can call `setPaused(true)` to halt all new locks in case of an emergency. Once the issue is resolved, they can call `setPaused(false)` to resume normal operation.

