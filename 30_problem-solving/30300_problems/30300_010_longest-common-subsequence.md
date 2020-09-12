---
description: 'Given two strings, find the length of longest common subsequence.'
---

# Longest Common Subsequence

### Problem Statement

We are given two strings s1 and s2. We need to find the length of the longest subsequence which is common in both the strings.

#### What is subsequence

Subsequence is a sequence that can be derieved from another sequence by deleting some elements without changing the order of remaining elements.

#### Example for subsequence

For a given string "ABCDE"

* Size 3
  * ADE, ACE, ACD, ABD
* Size 4
  * ABCE, ABDE, ...

#### Example for Problem

for s1 = "elephant"  
for s2 = "eretpat"  
output = 5  
longest subsequence is "eepat"

s1 = "houdini"  
s2 = "hdupti"  
output = 3  
longest subsequence is "hui"  

### Approach for Divide and Conquer

At any index, the result is  \(1 - if match or 0 - otherwise\) + 

Assume the function is `f(str1, str2, idx1, idx2)`.

* c1 = if str1\[idx1\] !== str2\[idx2\] ? 0 :  `1 + f(str1, str2, idx1 + 1, idx2 + 1)`
* else
  * `c2 = f(str1, str2, idx1 + 1, idx2)` // omitting str1\[idx1\]
  * `c3 = f(str1, str2, idx1, idx2 + 1`\) // omitting str2\[idx2\]
* Get Max \(c1, c2, c3\)

### Implementation

#### Using Divide and conquer

{% code title="longest\_subsequence\_divide-n-conquer.ts" %}
```typescript
function findLCS(s1: string, s2: string, idx1 = 0, idx2 = 0): number {
    if(idx1 >= s1.length || idx2 >= s2.length) return 0
    const c1 = s1[idx1] !== s2[idx2] ? 0 : 1 + findLCS(s1, s2, idx1+ 1, idx2 + 1)
    const c2 = findLCS(s1, s2, idx1 + 1, idx2)
    const c3 = findLCS(s1, s2, idx1, idx2 + 1)
    return Math.max(c1, c2, c3)
}

function test(s1, s2) {
    console.log(s1, s2, findLCS(s1, s2))
}

test('elephant', 'eretpat') // 5
test('houdini', 'hdupti') // 3
```
{% endcode %}

