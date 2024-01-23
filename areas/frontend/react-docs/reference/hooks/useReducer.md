---
published: false
id: useReducer
slug: useReducer
title: UseReducer
description: useReducer
tags: ["hooks"]
categories: ["hooks"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

# useReducer

```tsx
const [State, dispatch] = useReducer(reducer, initialArg, init?)
```

## Reference
### Parameters
**reducer**
State를 업데이트하기 위한 reducer 함수로 순수함수 이어야 한다.
State와 Action을 인자로 받는다, 반드시 다음 State를 반환해야한다.

**initialArg**
초기 State

**init**
init용 함수로, initial State를 반환해야한다.
만약 없다면, initialArg 값이 세팅된다. 
사용한다면, 초기값은 init(initialArg)로 세팅된다.

### Returns
1. 현재 State
2. dispatch 함수(State를 다른 값으로 업데이트해서 다시 랜더할 수 있게 해준다)

### Caveats
Strict Mode의 개발모드에서는 순수성을 보장하기 위해서 reducer가 두번 호출된다.

## dispatch 함수
dispatch함수는 Action만을 인자로 받는다.
reducer 함수에는 현재 State와 dispatch에 넘긴 Action이 제공되고,
React는 reducer함수를 실행한 결과를 다음 State로 설정한다.

dispatch는 return value가 없다.
dispatch 함수로 인한 상태 업데이트는 다음 랜더링 사이클에서 반영된다.
dispatch 호출하고나서 바로 상태변수를 읽으면, 그 상태변수는 여전히 dispatch 호출 이전의 값을 갖고 있을 것이다.

## Usage

### reducer 작성하기
보편적으로 switch문을 사용한다.
Action은 어떤 형태도 가능하지만, 보편적으로 object로 넘기고, type 속성으로 Action을 식별한다.
Action은 다음 상태를 계산하기 위한 최소한의 정보만 포함해야한다.
각각의 Action은 하나의 상호작용을 의미해야 한다. (만약 여러값을 바꾸더라도)
State의 형태는 보통 object 혹은 array 이다.
State는 읽기전용이기 때문에, Mutate 하면 안된다

### 초기값 재생성 피하기

초기값을 위한 함수를 사용할 때, 3번째 인자에 함수를 넣으면 함수가 재생성 되지 않는다.
아래 Bad case 처럼 넣어두면 매번 랜더링마다 함수가 재생성되어 비효율적이다.

```typescript
function TodoList({ username }) {
// Bad
  const [state, dispatch] = useReducer(reducer, createInitialState(username));

function TodoList({ username }) {
// Good
  const [state, dispatch] = useReducer(reducer, username, createInitialState);
```

## Trouble Shooting

### 상태가 이전값을 사용하는 경우

dispatch 호출후에 바로 state를 사용하면 dispatch 호출이전 값으로 사용하게 된다.
```typescript
function handleClick() {
  console.log(state.age);  // 42

  dispatch({ type: 'incremented_age' }); // Request a re-render with 43
  console.log(state.age);  // Still 42!

  setTimeout(() => {
    console.log(state.age); // Also 42! (스냅샷이기 때문에 state.age는 42이다.)
  }, 5000);
}
```

### 화면 업데이트가 되지 않는 경우
dispatch를 했음에도 화면이 업데이트 되지 않는다면 reducer에서 Mutate를 한곳이 있는지 점검해보자.
