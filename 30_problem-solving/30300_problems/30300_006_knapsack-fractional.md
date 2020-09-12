# Knapsack Fractional

## Problem Statement

Fill the knapsack such that the value is maximum and the total weight is almost W. Items can be broken down to maximize the knapsack value. Each item has only one quantity.

### Approach

1. Calculate the value density for each item \(Value/weight\)
2. Sort the items based on the density calculate
3. Start picking up the item of the highest value, then move to next highest item

     If you can't accomodate the full amount of a item, break it as per the available space

Run Time: **O\(n.log\_n\)**

### Example

For a given weight W = 60 KG and items \(details below\), what are the items knapsack will keep and in what quantity

#### Items

| A | B | C | D | E |  |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Weight \(in Kg\) | 30 | 20 | 10 | 5 | 15 |
| Value | 120 | 130 | 200 | 50 | 100 |

#### Output

| Items | C | D | E | B | A |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Weight \(in Kg\) | 10 | 05 | 15 | 20 | 10 |
| Value | 200 | 150 | 100 | 130 | 40 |

{% code title="fractional\_knapsack.ts" %}
```typescript
type Item = {
  name: string;
  weight: number;
  value: number;
};

function knapsackFractional(weight: number, items: Item[]): Item[] {
  const toCarry: Item[] = [];
  let canCarry = weight;
  items.sort((a, b) => b.value/b.weight - a.value/a.weight);
  console.table(items)
  for (let item of items) {
    if (canCarry === 0) {
      break;
    }
    if (item.weight <= canCarry) {
      toCarry.push(item);
      canCarry -= item.weight;
    } else {
      toCarry.push({
        name: item.name,
        value: (item.value * canCarry) / item.weight,
        weight: canCarry,
      });
      break;
    }
  }
  return toCarry;
}

const items = [
  { name: "A", weight: 30, value: 120 },
  { name: "B", weight: 20, value: 130 },
  { name: "C", weight: 10, value: 200 },
  { name: "D", weight: 5, value: 100 },
  { name: "E", weight: 15, value: 100 },
];

console.table(knapsackFractional(60, items))
```
{% endcode %}

