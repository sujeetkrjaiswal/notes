# Short Program

## 1. Check Prime

```javascript
function isPrime(n) {
  const limit = Math.sqrt(n); // just need to test till this limit
  if (n === 2 || n === 3) return true; // basic cases
  if (n % 2 === 0) return false; // if even its false
  for (let d = 3; d <= limit; d++) {
    if (n % d === 0) return false; // is divisible by any no. between 3 and the limit. is not prime
  }
  return true;
}
```

## 2. Nearest Prime Number less than the given number n \(3 &lt;= n &lt; 10^6 \)

It uses `Sieve of Eratosthenes` algorithm to find all the prime numbers below `n`.

In general Sieve of Sundaram produces prime numbers smaller than \(`2x + 2`\) for a given number `x`. For us, the value of x = `(n-2)/2`

```javascript
function allPrimesBelow(n) {
  const arr = new Array(n-2).fill(true)
  for(let i = 0; i< n; i++) {
    if(arr[i]) {
      const prime = i+2
      let nextMark = 2*prime - 2
      while(nextMark < arr.length) {
        arr[nextMark] = false
        nextMark += prime
      }
    }
  }

  return arr
    .map((v,i) => [i, v])
    .filter(([,v]) => v)
    .map(([n]) => n+2)
}
```

## 3. Find all prime factors for a given number

```javascript
function allPrimeFactors(n) {
  const factors = []
  const limit = Math.sqrt(n)
  for(let d = 2; d <= limit; d+= d===2 ? 1 : 2) {
    while(n % d === 0) {
      n /= d
      factors.push(d)
    }
  }
  if(n > 2) {
    factors.push(n)
  }
  return factors
}
```

## 4. Get nth Fibonacci number

```javascript
function nthFibonacci(n) {
  let [a,b] = [0, 1] // 1st and 2nd fibonacci numbers
  for(let i = 2; i<n; i++) {
    [a,b] = [b, a+b]
  }
  return b
}
```

For getting the series

```javascript
function* nthFibonacci(n) {
  let [a,b] = [0, 1] // 1st and 2nd fibonacci numbers
  if(n >= 1) yield a
  if(n >=2) yield b
  for(let i = 2; i<n; i++) {
    [a,b] = [b, a+b]
    yield b
  }
}
```

## 4. Get the Greatest Common Divisor for given a & b

```javascript
function gcd(a,b) {
  // Using Eucladian algorithm
  [a,b] = a > b ? [a,b] : [b,a]
  while(b !== 0) {
    [a,b] = [b, a%b]
  }
  return a
}
```

For computing GCD for more than two numbers

```javascript
function gcdVarArgs(a,b,...args) {
  if(args.length === 0) {
    return gcd(a,b)
  }
  return gcdVarArgs(
    gcd(a,b),
    ...args
  )
}
function gcdIter(...args) {
  let processed = [...args]
  while(processed.length > 1) {
    const processing = []
    for(let i =0; i< processed.length; i+=2) {
      if(processed.length === i+1) {
        processing.push(processed[i])
      } else {
        processing.push(gcd(processed[i], processed[i+1]))
      }
    }
    processed = processing
  }
  return processed[0]
}
```

## 5. Merge Two sorted array

```javascript
function merge(arr1, arr2) {
  const output = new Array(arr1.length + arr2.length)
  aRef = 0
  bRef = 0
  while(aRef !== arr1.length -1 || bRef !== arr2.length -1) {
    if(arr1[aRef] >= arr2[bRef]) {
      output[aRef+bRef] = arr2[bRef]
      bRef++
    } else {
      output[aRef+bRef] = arr1[aRef]
      aRef++
    }
  }
  if(aRef !== arr1.length) {
    output[aRef+bRef] = arr1[aRef]
    aRef++
  }
  if(bRef !== arr2.length) {
    output[aRef + bRef] = arr2[bRef]
    bRef++
  }
  return output
}

a = [1,5,7,8,11,56]
b = [0, 2, 6,9,10,22,44,66]
console.log(merge(a,b))
```

## 6 Reverse String Prototype method

```javascript
Object.defineProperty(String.prototype, 'reverse', {
  value: function() {
    return this.split('').reverse().join('')
  }
})
```

Reverse words in place in a sentance. eg. `I am good` =&gt; `I ma doog`

```javascript
function reverseWordInPlace(sentence) {
  return sentence.split(' ').map(
    word => word.split('').reverse().join('')
    ).join(' ')
}
```

## 7 First non-repeating character

```javascript
function firstNonRepChar(str) {
  const charMap = {}
  for(let i = 0; i < str.length; i++) {
    const char = str[i]
    if(char in charMap) {
      charMap[char] = -1 // if its repeating its use-less
    } else {
      charMap[char] = i
    }
  }
  const nonRepChars = Object.entries(charMap)
    .filter(([char, val]) => val >= 0)
    .sort(([, i1],[, i2]) => i1 - i2)
  console.log('non-rep', nonRepChars)
  return nonRepChars[0] && nonRepChars[0][0]
}
console.log(firstNonRepChar(`the quick brown fox jumps then quickly blow air`))
```

Modify the solution to get First non-rep char, last non-rep char, first highest-rep char, last highest-rep char and highest-rep char through out.

```javascript
function firstNonRepChar(str) {
  const charMap = {};
  for (let i = 0; i < str.length; i++) {
    const char = str[i];
    if (char === " ") {
      continue;
    }
    if (char in charMap) {
      charMap[char].count += 1;
      charMap[char].lastIndex = i;
    } else {
      charMap[char] = {
        char,
        count: 1,
        firstIndex: i,
        lastIndex: i
      };
    }
  }

  const all = Object.values(charMap);

  all.sort((a, b) =>
    a.count > b.count ? 1 : b.count > a.count ? -1 : a.firstIndex - b.firstIndex
  );
  console.log(all);
  const res = {
    minRepFirst: all[0],
    minRepLast: all.find(
      (elem, i, arr) =>
        elem.count === arr[0].count &&
        (i + 1 === arr.length || arr[i + 1].count !== elem.count)
    ),
    maxRepFirst: all.find(
      (obj, i, arr) => obj.count === arr[arr.length - 1].count
    ),
    maxRepLast: all[all.length - 1],
    all
  };
  return res;
}

console.log(firstNonRepChar(`the quick brown fox jumps then quickly blow air`));
```

