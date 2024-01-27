---
published: false
id: javascript-sicp
slug: javascript-sicp
title: Javascript Sicp
summary: javascript sicp
toc: true
tags: book
categories: book
createdDate: 2024-01-07
updatedDate: 2024-01-23
---

# JavaScript SICP

## 1. 함수를 이용한 추상화

### 1.1 프로그래밍의 기본 요소

- 자바스크립트는 저수준언어로 컴파일이 요구되는 C와 달리, 웹 브라우저가 해석하는 식으로 실행됨
- 모든 언어는 단순한 아이디어를 조합하여 복잡한 아이디어를 만들어내는데 3가지 메커니즘이 있음
  - 원시 표현식(primitive expression)
  - 조합수단(combination)
  - 추상화 수단(abstraction)
- 조합 평가의 과정은 트리로 표현할 수 있다. (복잡한 사칙연산을 하는 과정을 트리로 표현하면 쉬움)
- 매개변수는 함수의 인수들을 지칭하는 지역이름이다.
- 함수를 적용하려면 함수표현식과 인수표현식들을 각각 평가하고, 함수표현식을 인수표현식에 적용한다.
- 정상 순서 평가(normal-order evaluation): 완전히 전개한다음에 평가하는 방법
- 인수우선 평가 혹은 적용적 순서평가(applicative-order evaluation): 치환모형으로 평가할 수 있다.
- 자바스크립트의 해석기: 자바스크립트는 인수 우선 평가 방식을 사용한다.
```javascript
// 아래 코드를 돌려보면 인수우선 평가이기 때문에
// Maximum call stack size exceeded 가 발생하는것을 확인할 수 있다.
function p() { return p(); }
function test(x, y) {
  return x === 0 ? 0 : y;
}
test(0, p());
```
- 매개변수는 함수에 묶이게 된다. (binding)
- 함수에 묶이지 않은 이름은 자유롭다고 말한다.
- scope: 주어진 이름의 바인딩이 유효하게 유지되는 문장들의 집합

- 내부선언과 블록 구조의 중요성
아래 함수의 문제는 실제 사용자에게 중요한 함수는 sqrt인데, 나머지 함수들이 혼란만 준다.
대형 시스템에서는 이런 보조함수들로 인해 다른 함수들과 공존의 문제가 생긴다.
실수로 다른 계산을 하는 improve를 사용하게 될 수도 있다.
```javascript
// 개선전
function sqrt(x) { return sqrt_iter(1, x); }
function sqrt_iter(guess, x) {
  return is_good_enough(guess, x) ? guess : sqrt_iter(improve(guess, x), x); 
}
function is_good_enough(guess, x) {
  return abs(square(guess) - x) < 0.001;
}
function improve(guess, x) {
  return average(guess, x/guess);
}
```

아래와 같이 scope를 제한함으로써 공존의 문제를 해결할 수 있다.
```javascript
// 블록구조로 개선
function sqrt(x) { 
  function is_good_enough(guess, x) {
    return abs(square(guess) - x) < 0.001;
  }
  function improve(guess, x) {
    return average(guess, x/guess);
  }
  function sqrt_iter(guess, x) {
    return is_good_enough(guess, x) ? guess : sqrt_iter(improve(guess, x), x); 
  }
  return sqrt_iter(1, x); 
}
```

여기서 조금 더 개선할 수 있다. x는 sqrt에 바인딩되고 scope내에서 모두 사용되기 때문에
x를 명시적으로 전달하지 않고 x를 내부 선언들 안에서 하나의 자유이름으로 사용할 수 있다.
```javascript
// 함수 단순화
function sqrt(x) { 
  function is_good_enough(guess) {
    return abs(square(guess) - x) < 0.001;
  }
  function sqrt_iter(guess) {
    return is_good_enough(guess, x) ? guess : sqrt_iter(improve(guess, x), x); 
  }
  function improve(guess) {
    return average(guess, x/guess);
  }
  return sqrt_iter(1); 
}
```

### 1.2 함수와 과정

전문 프로그래머가 되기 위해서는 다양한 종류의 함수가 생성하는 과정을 시각화할 수 있어야 한다.

재귀함수에서는 과정에 따라 두가지로 나눌 수 있다.
선형 재귀 과정(linear recursive process)와 선형 반복적 과정(linear iterative process)이다.
선형 반복적 과정은 반복문과 같은 구조를 갖는다.
꼬리 재귀과정(tail recursive process)는 반복정 과정을 구현한 것을 표현한 것이다.
이러한 과정들은 치환 모형을 이용하여 시각화할 수 있다.

가장 간단한 비교방법은 factorial을 이용한 구현이다.
