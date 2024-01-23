---
published: false
id: memo
slug: memo
title: Memo
description: memo
tags: ["apis"]
categories: ["apis"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

# memo

Date created: 2023-06-15

```tsx
const MemoizedComponent = memo(SomeComponent, arePropsEqual?)
```

memo는 성능 최적화를 위해서만 사용되어야 합니다.
기능이 동작하지 않거나 버그가 있는 경우, 버그를 수정하고 memo를 적용해야 합니다.

## Reference
메모 버전의 컴포넌트를 만들기 위해서 memo로 감싸줍니다.
이때 props가 바뀌지 않는다면, 부모컴포넌트가 re-render 되더라도 memo 컴포넌트는 re-render 되지 않습니다.

### Parameters
**Component**
memo하고 싶은 컴포넌트로, memo는 컴포넌트를 수정하지 않습니다.
새로운 memoized 컴포넌트를 반환하게 됩니다.
어떠한 리액트 컴포넌트라도 받을 수 있습니다. (forwardRef 컴포넌트도 물론 됩니다.)

**arePropsEqual**
prev props와 new props를 인자로 받습니다.
old 값과 new값이 같다면 true를 리턴해야합니다.

### Returns
새로운 리액트 컴포넌트를 반환합니다.

## Usage
memo는 props의 변경사항에 대해서만 동작합니다.

### context로 memo 컴포넌트 업데이트하기
memo 컴포넌트 내에서 useContext를 사용하면, context 값이 변경될때마다 re-render가 됩니다.
이를 막으려면 부모 컴포넌트에서 context를 호출하고, memo 컴포넌트에 props로 전달받도록 해야 합니다.

### props 변경 최소화 하기
props로 함수나 객체를 내려주면 매번 새롭게 생성되기 때문에 memo 컴포넌트가 새로 랜더링 됩니다.
이를 최소화 하기 위해서는 부모컴포넌트에 있는 객체(props로 전달하는 값)에 useMemo를 사용하면 됩니다.
혹은 객체로 전달하지않고, 필요한 데이터만 props로 내려주도록 합니다. 

### comparison 함수를 커스텀하기
2번째 파라미터인 arePropsEqual를 커스텀할 수 있습니다.
다만 비교를 하는 경우에는 내부에 있는 모든 props를 비교해줘야합니다.
따라서 props의 구조가 복잡하거나 깊은 경우에는 혼란스러운 버그를 만들수 있으므로 사용하지 않는 것이 좋습니다.

리액트는 old와 new props에 대해서 shallow equality를 사용합니다.
