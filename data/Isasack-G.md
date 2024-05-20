## Optimization of Initial Token Allowance Loop

The contract includes a loop to allow initial tokens for locking, where each token address in the `_allowedTokens` array is set to `true` in the `isTokenAllowed` mapping. The optimisation involves caching the length of the array and removing the `unchecked` block for improved gas efficiency.

### Original Code:

```
uint256 length = _allowedTokens.length;
for (uint256 i = 0; i < length;) {
    isTokenAllowed[_allowedTokens[i]] = true;
    unchecked {
        i++;
    }
}
isTokenAllowed[_wethAddress] = true;
```

### Suggested Change:
```
for (uint i = 0; i < _allowedTokens.length; i++) {
    isTokenAllowed[_allowedTokens[i]] = true;
}
isTokenAllowed[_wethAddress] = true;
```
**Changes Made:**

- Cached the length of the _allowedTokens array to avoid recalculating it in each iteration.

- Removed the unchecked block, as it's unnecessary and doesn't impact the loop's functionality.
