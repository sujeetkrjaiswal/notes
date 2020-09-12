---
description: >-
  Given a sum, and available denomination, get list of denomination and its
  count
---

# Coin Change

## Problem Statement

Given a value V, we want to make changes for V. We have infinite supply of each of the denominations in a given currency \(1, 2, 5, 10, 20, 50, 100, 500, 1000\). What is the minimum number of coins needed to make changes for V.

It should return array of denominations and its count for each denomination required.

### Example

for V = 225 and d = \[1, 2, 5, 10, 20, 50, 100, 500, 1000\] you need \[100X2, 20X1, 5X1\] = 4 coins

### Approach

1. Sort the denomination \(if not sorted\) \[Descending order\]
2. for each denomination get Math.flore\(remainingValue / denomination\) =&gt; it is the coins required for that denomination
3. set the required value as = remainingValue % denomination , and move to next denomination

**Run Time:** If sorting is required, **O\(n.log\_n\)** If denomination is already sorted, **O\(n\)**

{% code title="coin\_change.ts" %}
```typescript
 function coinChange(value: number, denominations: number[]): [number, number][] {
   const coins:[number, number][] = []
    denominations.sort((a,b) => b-a) // descending order
    let reqValue: number = value
    for(let d of denominations) {
      const coinCount = Math.floor(reqValue/d)
      reqValue = reqValue % d
      if(coinCount > 0) {
        coins.push([d, coinCount])
      }
    }
   return coins;
 }


 function test(v: number) {
  const denominations = [1, 2, 5, 10, 20, 50, 100, 500, 1000]
   console.group("Solution for value", v)
   console.table(coinChange(v, denominations))
   console.groupEnd()
 }


test(225)
test(2456)
test(543625)
```
{% endcode %}

