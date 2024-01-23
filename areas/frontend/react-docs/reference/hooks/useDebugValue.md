---
published: false
id: useDebugValue
slug: useDebugValue
title: UseDebugValue
description: useDebugValue
tags: ["hooks"]
categories: ["hooks"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

React DevTools에서 custom hook에 label을 붙일 수 있는 hook이다.
```tsx
useDebugValue(value, format?)
```
## Reference

### Parameters
**value**
React DevTools에서 표시하고 싶은 값 (어떤 타입도 가능하다)
**format**
formatting function을 사용하는 경우, value가 argument로 들어감, 만약 없다면 value가 표시됨

### Returns
return 되는 것 없음

## Usage
읽을 수 있는 debug value를 표시하는 용도이다.

useDebugValue를 모든 custom hook에 넣지마라, 공유되고 복잡한 구조를 가지고 있어서 inspect하기가 어려운 경우에 사용하면 가장 도움이 될 것이다.

format 함수는 개발자도구(React DevTools)에서 검사(inspect)될 때만 실행되므로, 비싼 formatting logic을 format function에 넣어두면, inspect할때만 실행되므로, 매 랜더링마다 계산이 발생하는 것을 피할 수 있다.
