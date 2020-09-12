# Paths to Reach End of Matrix

### Problem Statement

Given a 2D matrix, where each cell represents a cost, We need to start from first cell \(0,0\) and reach last cell. We have a given total cost to reach end of the matrix. Valid movements are only in right or down direction from the current cell. Find the number of ways to reach end of matrix with given total cost.

### Implementation

#### Using Divide and conquer

```typescript
function noOfPaths(
  cost: number[][],
  costToSpend: number,
  row = cost.length - 1,
  col = cost[row].length - 1
  ): number {
  if(costToSpend < 0) return 0
  if(row === 0 && col === 0) return cost[0][0] - costToSpend <= 0 ? 1 : 0
  if (row === 0) return noOfPaths(cost, costToSpend - cost[row][col], row, col - 1)
  if (col === 0) return noOfPaths(cost, costToSpend - cost[row][col], row - 1, col)
  return noOfPaths(cost, costToSpend - cost[row][col], row - 1, col) + noOfPaths(cost, costToSpend - cost[row][col], row, col - 1)
}

const costMatrix = [
  [4, 7, 8, 6, 4],
  [6, 7, 3, 9, 2],
  [3, 8, 1, 2, 4],
  [7, 1, 7, 3, 7],
  [2, 9, 8, 9, 3]
  ]

console.log(35, noOfPaths(costMatrix, 35))
console.log(36, noOfPaths(costMatrix, 36))
console.log(37, noOfPaths(costMatrix, 37))
console.log(38, noOfPaths(costMatrix, 38))
console.log(39, noOfPaths(costMatrix, 39))
console.log(40, noOfPaths(costMatrix, 40))
```

