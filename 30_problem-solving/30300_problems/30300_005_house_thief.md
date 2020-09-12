---
description: 'Given houses and values, find the max stolen. Can''t steal in adjacent houses.'
---

# House Thief

## Problem Statement

There are n houses build in a line, each of which contains some value in it. A thief is going to steal the maximum value of these houses. But he can't steal in two adjacent houses.

What is the maximum stolen value

#### Example

Input: 6, 7, 1, 30, 8, 2, 4  
Output: 41  
Thief will steal: House 7, 30, 4

#### Example 2

Input: 20, 5, 1, 13, 6, 11, 40  
Output: 73  
Thief will steal House: 20, 13, 40

### Approach

for a given house array of size n. and a function houseTheif, it will be the max of below two

* first\_house\_Value + maxSteal\(house from third house\)
* maxStea\(house from second house\)

```typescript
function houseThief(valueArr: number[]): number {
    // base condition ignored
    const [a,b,others] = valueArr
    return Math.max(
        a + houseThief(others),
        houseThief([b, ...others])
    )
}
```

### Implementation

{% code title="house\_thief.ts" %}
```typescript
function houseThief(houseNetWorth: number[], currentIdx = 0) {
    if(currentIdx >= houseNetWorth.length) return 0 // base case
    const worthWithCurrentHouse = houseNetWorth[currentIdx] + houseThief(houseNetWorth, currentIdx + 2)
    const worthWithoutCurrentHouse = houseThief(houseNetWorth, currentIdx + 1)
    return Math.max(worthWithCurrentHouse, worthWithoutCurrentHouse)
}

function test(houses) {
    console.log(houses, houseThief(houses))
}

test([6, 7, 1, 30, 8, 2, 4])
test([20, 5, 1, 13, 6, 11, 40])
```
{% endcode %}

