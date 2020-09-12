# Promise Methods

## 1. Create a class that extends another class using ES 5 syntax.

```javascript
function Animal(noOfLegs, color) {
  this.noOfLegs = noOfLegs
  this.color = color
}
Animal.prototype.walk = function() {
  console.log('Animal Walk')
}
Animal.prototype.sound = function() {
  console.log('Animal Is making sound')
}

function Dog(type, color) {
  Animal.call(this, 4, color)
  this.type = type
}
Dog.prototype = Object.create(Animal.prototype)
Dog.prototype.constructor = Dog
Dog.staticProperty = 'Some Name'
Dog.staticMethod = function() {
  console.log('an static method')
}

Dog.prototype.run = function() {
  console.log(`It can run`)
}
Dog.prototype.sound = function() {
  console.log('It barks')
}
```

## 2. Implement Promise.race, Promise.all and Promise.allSettled

### For Promise.race

Returns the first resolved or rejected promises.

```javascript
Promise.race = function(promises) {
  return new Promise((resolve, reject) => {
    promises.forEach(promise => {
      promise
      .then(resolve, reject)
    });
  })
}
```

### Promise.all

Accepts array of promises, and returns when all the promises are resolved with array of data corresponding to the index of promise. In case of any rejection of promises, it would reject with the reason of the first rejected promise.

```javascript
Promise.all = function(promises) {
  let totalResolved = 0
  const totalToResolve = promises.length
  const resolvedPromises = new Array(totalToResolve)
  return new Promise((resolve, reject) => {
    if(totalToResolve === 0) {
      resolve(resolvedPromises)
    }
    for(let i = 0; i < totalToResolve; i++) {
      promises[i].then(data => {
        totalResolved += 1
        resolvedPromises[i] = data
        if(totalResolved === totalToResolve) {
          resolve(resolvedPromises)
        }
      }).catch(err => {
        reject(err)
      })
    }
  })
}

Promise.all = async function(promises) {
  const resultArr = new Array(promises.length)
  for(let i = 0; i < promises.length; i++) {
    resultArr[i] = await promises[i]
  }
  return resultArr
}
```

### Promise.allSettled

For a given array of promises, returns array of values/reasons for the promises along with their status. It would return only when all the promises of processed irrespective of the status ie. fullfilled or rejected

```javascript
Promise.allSettled = function(promises) {
  let totalResolved = 0
  let totalToResolve = promises.length
  const dataToResolve = new Array(totalToResolve)
  return new Promise((resolve) => {
    if(totalToResolve === 0) {
      resolve(resolvedPromises)
    }
    for(let i=0; i < totalToResolve; i++) {
      promises[i]
      .then(value => {
        dataToResolve[i] = {
          status: 'fulfilled',
          value
        }
      })
      .catch(reason => {
        dataToResolve[i] = {
          status: "rejected",
          reason
        }
      })
      .finally(() => {
        totalResolved += 1
        if(totalResolved === totalToResolve) {
          resolve(dataToResolve)
        }
      })
    }
  })
}
```

