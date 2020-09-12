---
description: 'For a given value, express it as a sum of given number'
---

# Number Factor

## Problem Statement

Given N, count the number of ways to express N as sum of numbers 1, 3, 4. Each number can be used as many time as required

### Example

* For N = 4, nums = 1, 3, 4
  * output 4
  * {4}, {1,3}, {3,1}, {1,1,1,1}
* For N = 5, nums = 1,3,4
  * output 6
  * {4,1} {1,4} {3,1,1} {1,3,1} {1,1,3} {1,1,1,1,1}

**returns** A 2-D array, which is basically array of \(numbers as array whose sum is equal to given number\)

### Implementation

#### Using Divide & Conquer

```typescript
function numFactor(n: number, nums: number[]): number[][] {
  if(n === 0) return [[]]
  const numSets: number[][] = [];
  for(let sn of nums) {
    if(sn > n) continue;
    const snSets = numFactor(n - sn, nums)
    snSets.forEach((snSet) => {
      numSets.push([sn, ...snSet])
    })
  }
  return numSets
}

function numFactorCount(n: number, nums: number[], min = Math.min(...nums)): number {
  if(n < min) return 0
  if(n === min) return 1
  let numSetsCount = 0
  for(let sn of nums) {
    if(sn > n) continue;
    // even if the numFactor for n-sn is zero, you got one entry [sn]
    numSetsCount += numFactorCount(n - sn, nums, min) || 1
  }
  return numSetsCount
}


console.log(numFactorCount(2, [3, 4])); // 0
console.log(numFactorCount(4, [1, 3, 4])); // 4
console.log(numFactorCount(5, [1,3,4])) // 6

console.log(numFactor(2, [3, 4])); // []
console.log(numFactor(4, [1, 3, 4])); // [ [ 1, 1, 1, 1 ], [ 1, 3 ], [ 3, 1 ], [ 4 ] ]
console.log(numFactor(5, [1,3,4])) // [ [ 1, 1, 1, 1, 1 ], [ 1, 1, 3 ], [ 1, 3, 1 ], [ 1, 4 ], [ 3, 1, 1 ], [ 4, 1 ] ]
```

#### Using Dynamic Programming \(Memoization\)

```typescript

function numFactorCount(n: number, nums: number[], min = Math.min(...nums), memoized = new Map<number, number>()): number {
  if(n < min) return 0
  if(n === min) return 1
  if(memoized.has(n)) {
    return memoized.get(n) || 0
  }
  let numSetsCount = 0
  for(let sn of nums) {
    if(sn > n) continue;
    // even if the numFactor for n-sn is zero, you got one entry [sn]
    numSetsCount += numFactorCount(n - sn, nums, min, memoized) || 1
  }
  memoized.set(n, numSetsCount)
  return numSetsCount
}


console.log(numFactorCount(2, [3, 4])); // 0
console.log(numFactorCount(4, [1, 3, 4])); // 4
console.log(numFactorCount(5, [1,3,4])) // 6
```

