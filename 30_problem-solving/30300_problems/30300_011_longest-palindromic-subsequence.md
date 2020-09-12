---
description: 'for a given string s, find the longest subsequence which is a palindrome'
---

# Longest Palindromic Subsequence

### Problem Statement

#### Example

for string ELRMENMET, the output should be EMEME

For string AMEEWMEA, the output should be AMEEMA

### Implementation

{% embed url="https://codepen.io/sujeetkrjaiswal/pen/ZEQEqRG" caption="Open console to see the results " %}

#### Using Divide and Conquer

```typescript
function LPS_DNC(s: string, sIdx = 0, eIdx = s.length - 1): number {
    if(sIdx > eIdx) return 0
    if(sIdx === eIdx) return 1
    const lpsMatch = s[sIdx]===s[eIdx] ? 2 + LPS_DNC(s, sIdx + 1, eIdx - 1) : 0
    const lpsIgnoreStart = LPS_DNC(s, sIdx + 1, eIdx)
    const lpsIgnoreEnd = LPS_DNC(s, sIdx, eIdx - 1)
    return Math.max(lpsMatch, lpsIgnoreStart, lpsIgnoreEnd)
}
```

