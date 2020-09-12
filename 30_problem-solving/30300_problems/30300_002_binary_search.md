---
description: >-
  Given a sorted array, find the index of a given value if the value exists else
  return -1.
---

# Binary Search

{% code title="binary\_search.ts" %}
```typescript
function binarySearch<T>(n: T, arr: T[], start = 0, end = arr.length - 1): number {
  if(start === end) {
   return arr[start] === n ? start : -1
  }
  const mid = start + Math.floor((end - start) / 2)
  if(arr[mid] === n) {
    return mid
  } else if (n > arr[mid]) {
    // check the right side
    return binarySearch(n, arr, mid+1, end)
  } else {
    // check the left side, make sure, end is not less than start
    return binarySearch(n, arr, start, start === mid ? start : mid - 1)
  }
}

function test(v: number) {
  const arr = [1,4,7,8,9,14,16,19,22,56,67]
  console.group('Binary search  for ', v)
  console.log(binarySearch<number>(v, arr))
  console.groupEnd()
}

test(6)
test(23)
test(1)
test(67)
test(14)
```
{% endcode %}

