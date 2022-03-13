---
description: Multiple Question based on one or more features of Event loop
---

# Event Loop Based Snippets



#### Dom Click Event listener

Why is the behavior when executing event listeners on btn1 is different when clicked via user (HTML rendered button) and when clicked via javascript.

{% embed url="https://jsfiddle.net/sujeetkrjaiswal/4kvwhoyL/embedded/js,html,result/dark/" %}

### Infinite Loop differences&#x20;

* How each of these below loops is different?
* Which one is preferred by which use-cases (assuming the limit to stop is n = 1milllion)

#### Iterative loop

```
let n = 1
while(n) {
    console.log(`printing: ${n}`)
    n+=1
}
```

#### Recursive Loop

```
function loop(n) {
  console.log(`printing: ${n}`)
  loop(n+1)
}

loop(1)
```

#### Macro-task Queue based loop

```
function loop(n) {
    console.log(`printing: ${n}`)
    setTimeout(loop.bind(null, n+1), 0)
}
loop(1)
```

#### Animation Queue Based Loop

```
function loop(n) {
    console.log(`printing: ${n}`)
    requestAnimationFrame(loop.bind(null, n+1))
}
loop(1)
```

#### Micro-Task Based Loop

```
function loop(n) {
    console.log(`printing: ${n}`)
    Promise.resolve(n+1).then(loop)
}
loop(1)
```

