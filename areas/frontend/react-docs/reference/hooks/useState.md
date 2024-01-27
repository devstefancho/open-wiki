---
published: false
id: useState
slug: useState
title: UseState
summary: useState
toc: true
tags: ["hooks"]
categories: ["hooks"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

# useState

```ts
const [state, setState] = useState(initialState)
```

## Reference
### Parameters
**initialState**
초기 랜더링 이후에 이 인자는 무시됩니다.
함수로 넘길 수도 있는데, initializer 함수로써 동작하고, 순수함수이어야 합니다.
인자는 없어야하고, 이 함수의 반환값이 store에 초기값으로 저장됩니다.

### Returns
1. 현재 상태값로 첫번째 랜더동안은 initialState와 동일한 값이 됩니다.
2. set 함수로 상태를 업데이트 할 수 있고, re-render를 트리거할 수 있습니다.

### Caveats
- Strict Mode의 개발모드에서는 함수의 순수성을 보장하기 위해서 두번 실행됩니다.
- set 함수에 의한 상태업데이틑 일괄처리(batch) 합니다. 모든 이벤트 핸들러안에 있는 set 함수가 모두 호출된 후에 스크린을 업데이트 합니다. 
  이것은 하나의 이벤트에 의해 여러번 랜더링되는 것을 방지합니다.
- 간혹 스크린 업데이트를 강제로 먼저해줘야할 때가 있는데, 이 경우에는 flushSync를 사용하면 됩니다.
- set 함수의 인자에 상태 설정함수(Updater function)를 넣는 경우는 보류상태(pending state)를 인자로 받습니다.
  이때 보류상태는 마지막 랜더링 상태인 이전상태(Old state)와는 다를 수 있습니다.

## Usage

### Updater 함수

set 함수에 Updater 함수를 넣을때, Updater함수의 인자의 네이밍 컨벤션은 아래와 같습니다.
- 상태 변수명의 첫번째 글자 (상태명이 age라고 하면 a)
- prev를 prefix로 붙일 수 있음 (상태명이 age라고 하면 prevAge)

### 초기값을 재생성하지 않기

Bad case처럼 초기값에서 함수를 실행하면, 매 랜더링마다 createInitialTodos가 실행됩니다.
따라서 createInitialTodos를 initializer 함수로 넣어야 합니다.
initializer 함수로 넣어두면, React는 initialization 동안만 실행할 것 입니다.
```ts
// Bad
function TodoList() {
  const [todos, setTodos] = useState(createInitialTodos());
  // ...

// Good
function TodoList() {
  const [todos, setTodos] = useState(createInitialTodos);
  // ...
```

### key 값을 reset할때 State를 사용
key 값을 바꾸면 컴포넌트는 reset 됩니다.
이 경우 key값을 state로 관리할 수 있습니다.

```tsx
import { useState } from 'react';

export default function App() {
  const [version, setVersion] = useState(0);

  function handleReset() {
    setVersion(version + 1);
  }

  return (
    <>
      <button onClick={handleReset}>Reset</button>
      <Form key={version} />
    </>
  );
}

function Form() {
  const [name, setName] = useState('Taylor');

  return (
    <>
      <input
        value={name}
        onChange={e => setName(e.target.value)}
      />
      <p>Hello, {name}.</p>
    </>
  );
}
```

### props의 이전값과 비교해야하는 경우

useEffect를 쓰는 것보다는, State를 만들어서 비교하는 것이 낫습니다.

```tsx
import { useState } from 'react';

export default function CountLabel({ count }) {
  const [prevCount, setPrevCount] = useState(count);
  const [trend, setTrend] = useState(null);
  if (prevCount !== count) {
    setPrevCount(count);
    setTrend(count > prevCount ? 'increasing' : 'decreasing');
  }
  return (
    <>
      <h1>{count}</h1>
      {trend && <p>The count is {trend}</p>}
    </>
  );
}
```

## Trouble Shooting

### State에 함수를 저장하려는 경우
State에는 함수를 저장할 수 없습니다.
- initialState에 함수를 넣으면, initializer 함수로 인식할 것입니다.
- set 함수의 인자에 함수를 넣으면, Updater 함수로 인식할 것입니다.
