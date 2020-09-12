---
description: >-
  Convert one string to another by deleting, inserting or replacing characters
  with minimum operations.
---

# Convert Str1 to Str2

### Problem Statement

For a given string `str1` and `str2`, write a function to calculate the count of the minimum number of edit operations. Edit operations are deleting, inserting, or replacing a character.

#### Example 1: Convert str1 to str2

s1 = "carch"  
s2 = "catch"  
output: 1

We just need to replace "r" with "t" to transform.

#### Example 2

"tbres" to "table" takes 3 edit which are

1. Insert "a" at second position
2. Replace r with l
3. Delete s

### Approach \(str1 -&gt; str2\)

Break the problem in smaller problem. You compare character in both string, and recursively call the function for next comparison while taking into account the current edit.

```typescript
type convert = (str1: string, str2: string, idx1: number, idx2: number) => number
```

While comparing, there could be four cases, discussed below:

* two characters are equal, `convert(str1, str2, idx1 + 1, idx2 + 1)`
* unequal: return `1 + min(below_cases)`
  * insert operation: character inserted same as that of `str2[idx2]`, hence use next character for `str2`.
    * `convert(str1, str2, idx1, idx2 + 1)`
  * delete operation: current `str1[idx1]` is deleted, hence use the next index for `str1`.
    * `convert(str1, str2, idx1 + 1, idx2)`
  * replace operation: replace `str1[idx1]` with `str2[idx2]`, and use next idx for both string
    * `convert(str1, str2, idx1 + 1, idx2 + 1)`

#### Execute the code in codepen \(Code is available below this embed\)

{% embed url="https://codepen.io/sujeetkrjaiswal/pen/QWyWNMR" %}



### Implementation

{% code title="Convert\_string.ts" %}
```typescript
function convertString(str1: string, str2: string, idx1 = 0, idx2 = 0): number {
    // if one of the string is exhausted,
    // use the remaining characters of other string (equal to delete/insert Operation)
    if(idx1 >= str1.length) { // insert operation
        return str2.length - idx2
    } else if (idx2 >= str2.length) { // delete operations
        return str1.length - idx1
    }
    // if characters are equal, jump to next idx for each
    if(str1[idx1] === str2[idx2]) {
        return convertString(str1, str2, idx1 + 1, idx2 + 1)
    }
    // if characters are not equal use the min of three operations
    const insertOp = convertString(str1, str2, idx1, idx2 + 1)
    const deleteOp = convertString(str1, str2, idx1 + 1, idx2)
    const replaceOp = convertString(str1, str2, idx1 + 1, idx2 + 1)
    return 1 + Math.min(insertOp, deleteOp, replaceOp)
}

function test(str1: string, str2: string) {
    console.log(str1, str2, convertString(str1, str2))
}

test('tbres', 'table') // 3
test('carch', 'catch') // 1
test('carchxs', 'catch') // 3
test('carchxs', 'catchcccc') // 5
```
{% endcode %}



