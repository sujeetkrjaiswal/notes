---
description: Generate fibonacci series. a series whose defination is given by
---

# Fibonacci

## Problem Statement

**Variation 1:** Write a program to output first n numbers of fibonacci series.  
**Variation 2:** Write a program to get Nth number in fibonacci series.

Fibonacci series is defined by below mathematical equation.

$$
n_i = \begin{cases} \text{not defined} & \text{if $i < 0$} \\ 0 & \text{if $i = 1$} \\ 1 & \text{if $i = 2$} \\   n_{i-1} + n_{i-2} & \text{otherwise} \end{cases}
$$

## Example

for n = 1 =&gt; 0 0  
for n = 2 =&gt; 0 1 1  
for n = 3 =&gt; 0 1 1 1  
for n = 5 =&gt; 0 1 1 2 3 3

### Implementation

#### Using Divide and Conquer

```typescript
function fibonacciNRecur(n: number): number {
  if (n <= 0) throw "Invalid valud of n";
  if (n <= 2) return n - 1;
  return fibonacciNRecur(n - 1) + fibonacciNRecur(n - 2);
}
```

#### Using Dynamic Programming \(Top - down / Memoization\)

```typescript
function fibonacciNusingDP(n: number, memoized = [0, 0, 1]): number {
  if (n <= 0) throw "Invalid valud of n";
  if(typeof memoized[n] === 'number') return memoized[n]
  const Nth = fibonacciNusingDP(n - 1, memoized) + fibonacciNusingDP(n - 2, memoized);
  memoized[n] = Nth
  return Nth
}
```

#### Using Dynamic Programming \(Bottom - up / Tabular\)

```typescript
function fibonacciSeriesTabular(n: number) {
    if (n <= 0) throw "Inavlid value of n"
    if (n <= 2) return n - 1
    const tabular = new Array(n + 1)
     tabular[1] = 0
     tabular[2] = 1
     for(let i = 3; i <= n; i+=1) {
      tabular[i] = tabular[i-1] + tabular[i-2] 
     }
     return tabular[n]
}
```

#### Using Iterator \(for above implementation\)

```typescript
/**
* Generate Fibonacci Series using Iterator
**/
function* fibonacciSeriesIter(n: number) {
  if (n <= 0) throw "Invalid valud of n";
  if (n >= 1) {
    yield 0;
  }
  if (n >= 2) {
    yield 1;
  }
  let last2 = 0;
  let last1 = 1;
  for (let i = 3; i <= n; i += 1) {
    [last2, last1] = [last1, last1 + last2];
    yield last1;
  }
  return;
}

/**
* Get Nth Fibonacci number using Iterator
**/
function fibonacciNIter(n: number) {
  if (n <= 0) throw "Invalid valud of n";
  if (n <= 2) return n - 1;
  let last2 = 0;
  let last1 = 1;
  for (let i = 3; i <= n; i += 1) {
    [last2, last1] = [last1, last1 + last2];
  }
  return last1;
}
```

#### 

