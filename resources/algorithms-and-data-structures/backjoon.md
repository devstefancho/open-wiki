---
published: false
id: backjoon
slug: backjoon
title: Backjoon
summary: backjoon
toc: true
tags: ["algorithms-and-data-structures"]
categories: ["algorithms-and-data-structures"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

##  이항정리(2775)
이항정리에서 이항계수의 패턴이 나오는 문제

이항정리 없이 풀었을 때 (3중 for문)
```js
const [len, ...rest] = require('fs').readFileSync('./dev/stdin').toString().trim().split('\n').map(s => Number(s))

let o = {
  0: Array(14).fill('').map((_, index) => index + 1)
}
// k: 층수, n: 호수, i: 1~n호수
for (let k = 0; k < 14; k++) {
  for (let n = 0; n < 14; n++) {
    let sum = 0
    for (let i = 0; i <= n; i++) {
      sum += o[k][i]
    }
    o[k + 1] = [...(o[k + 1] ?? []), sum]
  }
}

for (let p = 0; p < len; p++) {
  const k = rest[p * 2]
  const n = rest[(p * 2) + 1]
  console.log(o[k][n - 1])
}
```

이항정리로 풀었을 때 (2중 for문)
```js
const [len, ...rest] = require('fs').readFileSync('./dev/stdin').toString().trim().split('\n').map(s => Number(s))

for (let i = 0; i < len; i++) {
  // k 층, n 호
  const k = rest[i * 2]
  const n = rest[i * 2 + 1]

  let result = 1
  /*
  k = 6, n = 4 이라면 실제로는 7C3 조합을 구하는 것임 (10C0 부터 시작이기 때문에)
  따라서 분모에서는 k+n * ... * {k+n - (n - 2)} 까지 곱하고, 분자에서는 1 * ... * (n - 1) 까지 나눔
  */
  for (let j = 0; j < n - 1; j++) {
    result *= k + n - j
  }

  for (let j = 1; j < n; j++) {
    result /= j
  }
  console.log(result)
}
```

## 소수(Prime Number) 찾기 (에라토스테네스의 체)
1978 소수찾기1
```js
const [len, input] = require('fs').readFileSync('./dev/stdin').toString().trim().split('\n')

const nums = input.split(/\s/).map(s => Number(s))
let m = nums[0]
for (let i = 0; i < len; i++) {
  m = Math.max(m, nums[i])
}

if (m < 2) {
  console.log(0)
} else {
  const arr = [false, false, ...new Array(m - 1).fill(true)]  // index 0, 1 제외
  for (let i = 2; i <= Math.sqrt(m); i++) {
    if (arr[i]) {
      for (let j = i * i; j <= m; j = i + j) {
        arr[j] = false
      }
    }
  }
  console.log(nums.filter(x => arr[x]).length)
}
```

2581 소수찾기2
```js
const [_m, _n] = require('fs').readFileSync('./dev/stdin').toString().trim().split('\n').map(s => Number(s))

function getPrime(m, n) {
  if (n < 2) {
    return console.log(-1)
  }

  const arr = [false, false, ...new Array(n - 1).fill(true)]
  for (let i = 2; i * i <= n; i++) {
    if (arr[i]) {
      for (let j = i * i; j <= n; j += i) {
        arr[j] = false
      }
    }
  }
  const primes = arr.map((x, idx) => x && idx >= m && idx).filter(x => x)
  primes.length ? [primes.reduce((acc, el) => acc + el), primes[0]].forEach(r => console.log(r)) : console.log(-1)
}

getPrime(_m, _n)
```

1929 소수 구하기3
```js
const [n, m] = require('fs').readFileSync('./dev/stdin').toString().trim().split(/\s/).map(s => Number(s))

const arr = [false, false, ...new Array(m - 1).fill(true)]
for (let i = 2; i <= Math.sqrt(m); i++) {
  if (arr[i]) {
    for (let j = Math.pow(i, 2); j <= m; j += i) {
      arr[j] = false
    }
  }
}

console.log(arr.map((x, i) => x && i >= n && i).filter(a => a).join('\n'))
```

4948 소수구하기 4
```js
const input = require('fs').readFileSync('./dev/stdin').toString().trim().split('\n').map(s => Number(s))

function getPrimes(n) {
  if (n === 0) {
    return
  }

  const arr = [false, false, ...new Array(n - 1).fill(true)]
  for (let i = 2; i <= Math.sqrt(n); i++) {
    if (arr[i]) {
      for (let j = Math.pow(i, 2); j <= n; j += i) {
        arr[j] = false
      }
    }
  }
  console.log(arr.slice((n / 2) + 1, n + 1).filter(x => x).length)
}

for (let i = 0; i < input.length; i++) {
  getPrimes(2 * input[i])
}
```

9020 소수 구하기 5 (골드바흐의 추측)
```js
const input = require('fs').readFileSync('./dev/stdin').toString().trim().split('\n').map(s => Number(s))

let m = input[0]
for (let i = 0; i < input.length; i++) {
  m = Math.max(m, input[i])
}

let arr = [...new Array(2).fill(false), ...new Array(m - 1).fill(true)]
for (let i = 2; i <= Math.sqrt(m); i++) {
  if (arr[i]) {
    for (let j = Math.pow(i, 2); j <= m; j += i) {
      arr[j] = false
    }
  }
}

// change true(prime number) to real number
arr = arr.reduce((acc, el, i) => {
  if (el) acc.push(i)
  return acc
}, [])

function getNums(i, half) {
  const pivot = arr.findIndex(x => x >= half)
  for (let j = pivot; j >= 0; j--) {
    for (let k = pivot; k < arr.length; k++) {
      if (arr[j] + arr[k] === input[i]) {
        return console.log(arr[j] + ' ' + arr[k])
      }
    }
  }
}

for (let i = 0; i < input.length; i++) {
  let half = input[i] / 2
  if (arr.find(x => x === half)) {
    console.log(half + ' ' + half)
  } else {
    getNums(i, half)
  }
}
```

## Array에 값 집어넣기
2563 색종이 문제
내 풀이
```js
const [len, ...rest] = require("fs")
  .readFileSync("./dev/stdin")
  .toString()
  .trim()
  .split("\n");

const arr = new Array(100).fill("").map((_) => new Array(100).fill(""));
let count = 0;

for (let i = 0; i < len; i++) {
  const [x, y] = rest[i].split(/\s/).map(Number);
  for (let j = x; j < x + 10; j++) {
    for (let k = y; k < y + 10; k++) {
      if (!arr[j][k]) {
        arr[j][k] = true;
        count++;
      }
    }
  }
}

console.log(count);
```
다른사람 풀이 (https://www.acmicpc.net/source/45383965)
- Array 사이즈를 지정하지 않고, 그냥 push한다음에 Set으로 필터함
```js
const input = require('fs').readFileSync('/dev/stdin').toString().trim().split('\n')
let allXY = []
let numSqa = Number(input[0])

for(let i = 1 ; i <= numSqa ; i++){
    let [a,b] = input[i].split(' ').map(Number)
    for(let j = a+1 ; j <= a+10 ; j++){
        for(let k = b+1 ; k <= b+10 ; k++){
            allXY.push(`${j} ${k}`)
        }
    }
}
console.log(new Set(allXY).size)
```
