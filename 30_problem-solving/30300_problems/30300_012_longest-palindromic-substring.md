---
description: 'For a given string, get the longest palindromic substring'
---

# Longest Palindromic Substring

### Problem Statement

#### Example

#### Approach

Same that of Longest Common Subsequence, with one additional condition, that if the character matches, to consider it in the result, you must check if the remaining characters in-between the two character must also be palindrome.

### Implementation

```typescript
function lps_dnc(s: string, sIdx = 0, eIdx = s.length - 1) {
    if (sIdx > eIdx) return 0
    if (sIdx === eIdx) return 1
    // remaining size = eIdx - sIdx - 1;
    // if match then size = 2 + remaining size
    const match = s[sIdx] === s[eIdx] &&
        lps_dnc(s, sIdx + 1, eIdx - 1) === eIdx - sIdx - 1 ?
        eIdx - sIdx + 1 : 0
    const ignoreFirst = lps_dnc(s, sIdx + 1, eIdx)
    const ignoreLast = lps_dnc(s, sIdx, eIdx - 1)
    return Math.max (match, ignoreFirst, ignoreLast)
}

lps_dnc("ABACAB") // 5
```

