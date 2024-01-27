---
published: false
id: StrictMode
slug: StrictMode
title: STrictMode
summary: StrictMode
toc: true
tags: ["components"]
categories: ["components"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

# StrictMode

Date created: 2023-06-10

```tsx
<StrictMode>
  <App />
</StrictMode>
```

## Reference
추가적인 개발기능과 컴포넌트 트리의 warning을 활성화해줌

- Deprecated API를 체크
- 버그를 찾기 위해서 re-render에 추가시간이 발생함
- Effect의 cleanup 누락으로 인한 버그를 찾기 위해서 추가시간이 발생함

### Props
Props를 받지 않음

### Caveats
StrictMode로 감싸진 트리 내에서는 Strict Mode를 비활성화하는 방법이 없습니다. 
이는 모든 컴포넌트가 StrictMode 내부에서 체크되는 것을 확신하게 해줍니다. 
만약 한 제품을 개발하는 두 팀이 해당 체크가 유용한지에 대해 이견이 있다면, 
그들은 합의에 이르거나 StrictMode를 트리에서 더 아래로 이동시켜야 합니다.

## Usage

### StrictMode를 전체앱에 적용하기
새로운 Application에서는 항상 StrictMode로 감싸는 것을 권장합니다.
개발모드에서만 StrictMode가 동작합니다.
Production에서는 재현이 잘 되지 않는 버그를 찾고 수정하는데에 도움이 됩니다.

### 컴포넌트 일부에 적용하기
StrictMode는 컴포넌트 일부에만 감쌀 수도 있습니다.

### 2번 랜더링하여 버그 찾기
함수의 순수성을 확인하기 위해서 아래 요소들을 2번 랜더링을 시킵니다.
- 컴포넌트 function의 body (최상단 레벨만 해당됩니다. event handler내의 코드는 포함되지 않습니다.)
- useState, useState의 set 함수, useMemo, useReducer에 전달하는 함수들
- class 컴포넌트의 [메소드들](https://legacy.reactjs.org/docs/strict-mode.html#detecting-unexpected-side-effects)(constructor, render, shouldComponentUpdate 등)

### Deprecated warning
StrictMode내에 있는 컴포넌트에서 deprecated API를 사용하면 warning 해줍니다.

