# Es Features

| ES Version | Feature Name | Feature Description |  |  |
| :--- | :--- | :--- | :--- | :--- |
| ES2016 | Array.prototype.{includes} | - |  |  |
| ES2016 | Exponential operator | **eg: \`var1** var2`,`a**b**c === a**\(b**c\)`also uniary operator can't be ambiguous eg:`-2  **2`gives error, use`\(-2\)**2`or`-\(2 \*\* 2\)\` |  |  |
| ES2017 | Object.{values, entries} | TODO |  |  |
| ES2017 | String.prototype.{padStart, padEnd} | TODO |  |  |
| ES2017 | Object.getOwnPropertyDescriptors | TODO |  |  |
| ES2017 | Trailing commas in function parameters, list and calls | TODO |  |  |
| ES2017 | Async functions | async - await |  |  |
| ES2017 | Shared Memory and atomics | eg. Shared Array Buffer for sharing data between workers and main thread and hence avoid copy of data for large data transfers. Atomics finish an operation |  |  |
| ES2018 | Lifting template literal restriction | TO\_EXPLORE |  |  |
| ES2018 | s \(dotAll\) flag for regular expression |  |  |  |
| ES2018 | RegExp named capture groups |  |  |  |
| ES2018 | RegExp Lookbehind Assertions |  |  |  |
| ES2018 | RegExp Unicode Property Escapes |  |  |  |
| ES2018 | Rest/Spread Properties |  |  |  |
| ES2018 | Promise.prototype.finally |  |  |  |
| ES2018 | Async Iterations |  |  |  |
| ES2019 | Optional `catch` binding |  |  |  |
| ES2019 | JSON superset |  |  |  |
| ES2019 | Symbol.prototype.description | returns the string passed in the constructor |  |  |
| ES2019 | Function.prototype.toString revesion | displays the comments in the function even if its outside the body and after the function keyword |  |  |
| ES2019 | Object.fromEntries |  |  |  |
| ES2019 | Well-formed JSON.stringify |  |  |  |
| ES2019 | String.prototype.{trimStart, trimEnd} |  |  |  |
| ES2019 | Array.prototype.{flat, flatMap} | `[].flat(depth = 1)` |  |  |
| ES2020 | String.prototype.matchAll |  |  |  |
| ES2020 | `import()` |  |  |  |
| ES2020 | Bigint |  |  |  |
| ES2020 | Promise.allSettled |  |  |  |
| ES2020 | globalThis |  |  |  |
| ES2020 | for-in mechanics |  |  |  |
| ES2020 | Optional Chaining |  |  |  |
| ES2020 | Nullish coalescing Operator `(??)` | Specifically calls out null and undefined. Eg. let x = {a: '', b: 0, c: null}; x.a |  | 'Defalult' gives Default and not '' therefore, use x.a ?? 'Default' will give empty. Same is try for other falsy vales |

