---
description: 'Given a matrix mXn, start from first cell 0,0 and reach to last cell m-1, n-1'
---

# Min cost to Reach End of Matrix

### Problem Statement

Given a matrix, whose each cell represent cost. You need to start from first cell and reach to last cell. The only movement possible is down or right. Which path would you choose to reach the end with minimum cost.

#### Approach

Start at the end

### Implementation

#### Using Divide and Conquer

```typescript
function reachEnd(cost: number[][], eRow = cost.length - 1, eCol = cost[eRow].length - 1): number {
  if(eRow < 0 || eCol < 0) return Number.POSITIVE_INFINITY
  if(eRow === 0 && eCol === 0) return cost[0][0]
  return cost[eRow][eCol] + Math.min(
      reachEnd(cost, eRow - 1, eCol),
      reachEnd(cost, eRow, eCol - 1),
  )
}

const costMatrix = [
  [4, 7, 8, 6, 4],
  [6, 7, 3, 9, 2],
  [3, 8, 1, 2, 4],
  [7, 1, 7, 3, 7],
  [2, 9, 8, 9, 3]
  ]

console.log(reachEnd(costMatrix))
```

