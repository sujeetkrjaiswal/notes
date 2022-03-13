---
description: Snippets to understand promises
---

# Promise based Snippets

#### Infinite loop using promises

* What is the output?
* Is it render blocking? - why?

```javascript
function loop(n = 0) {
    console.log(`loop iteration: ${n}`)
    Promise.resolve(n+1).then(loop)
}
loop()
```

```javascript
function test() {
  console.log('TODO')
}
```

