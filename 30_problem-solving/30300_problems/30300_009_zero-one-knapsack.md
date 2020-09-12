---
description: >-
  Given a capacity, and items with Weight & Value, pick items under the capacity
  to maximize the value
---

# Zero One Knapsack

### Problem Statement

Given the weights and profits of N items and a capacity C, choose the items to maximize the profit by picking the subset of items. Any item can not be broken into smaller pieces.

#### Example 

| Items | Mango | Apple | Banana | Orange |
| :--- | :--- | :--- | :--- | :--- |
| Value | 31 | 26 | 72 | 17 |
| Weights | 3 | 1 | 5 | 2 |

### Codepen Implementation.

Codepen has implementation using Just Divide-n-conquer and Dynamic Programming.

The codepen example also list down the items those are selected. Below codepen, you have an code example for the implementation.

{% embed url="https://codepen.io/sujeetkrjaiswal/pen/jOWOqRP?height=265&theme-id=light&default-tab=js" %}



### Implementation \(with Divide & Conquer - NO DYNAMIC PROGRAMMING\)

{% code title="zero\_one\_knapsack.ts" %}
```typescript
interface Item {
    name: string
    value: number
    weight: number
}
function zeroOneKnapsack(items: Item[], capacity: number, startIdx = 0): number {
    if(capacity <= 0 || startIdx >= items.length || startIdx < 0) return 0
    const {value, weight} = items[startIdx]
    if(weight > capacity) {
        return zeroOneKnapsack(items, capacity, startIdx + 1)
    }
    return Math.max(
        value + zeroOneKnapsack(items, capacity - weight, startIdx + 1),
        zeroOneKnapsack(items, capacity, startIdx + 1)
    )
}

const items: Item[] = [
    {name: 'Mango', value: 31, weight: 3},
    {name: 'Apple', value: 26, weight: 1},
    {name: 'Banana', value: 72, weight: 5},
    {name: 'Orange', value: 17, weight: 2},
]

function test(capacity) {
    console.log(capacity, zeroOneKnapsack(items, capacity))
}
test(1)
test(2)
test(3)
test(4)
test(5)
test(7)
test(9)
test(11)
```
{% endcode %}

