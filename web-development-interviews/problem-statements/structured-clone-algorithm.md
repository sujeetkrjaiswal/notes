---
description: >-
  Used to copy complex javascript object and used internally to transfer data
  between Workers via postMessage, storing objects with IndexedDB and other
  APIs.
---

# Structured Clone Algorithm

#### Difficulty: Low

Read more about Algorithm here [https://developer.mozilla.org/en-US/docs/Web/API/Web\_Workers\_API/Structured\_clone\_algorithm](https://developer.mozilla.org/en-US/docs/Web/API/Web\_Workers\_API/Structured\_clone\_algorithm)



Implement a method named `clone` and the minimum cases it should handle are below:

1. Nested Objects / and Array of Objects & Primitives
2. Support Cyclic Objects/Array
3. Min Data Types to support: All primitives except Symbols, RegEx, Date, Set, Map, TypedArrays

