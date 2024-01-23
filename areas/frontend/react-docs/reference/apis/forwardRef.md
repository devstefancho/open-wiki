---
published: false
id: forwardRef
slug: forwardRef
title: ForwardRef
description: forwardRef
tags: ["apis"]
categories: ["apis"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

# forwardRef

Date created: 2023-06-14

```tsx
const SomeComponent = forwardRef(render)
```

## 예시 
```ts
import { forwardRef } from 'react'

const MyInput = forwardRef(function MyInput(props, ref)) {
    // ...
})
```
## Reference - forwardRef(render)
### Parameters
**render**
render 함수는 props와 ref를 파라미터로 갖고 있습니다.
props와 ref는 부모 컴포넌트에서 전달됩니다.

### Returns
JSX에서 랜더링 할 수 있는 React 컴포넌트를 반환합니다.
일반적인 컴포넌트와의 차이점으로는 ref prop을 받을 수 있습니다.

### Caveats
Strict Mode에서는 render 함수가 두번 호출될 것입니다.

## Reference - render 함수
forwardRef는 render 함수를 인자로 받습니다.

### Parameters
**props**
부모컴포넌트에서 전달받은 props 입니다.

**ref**
부모 컴포넌트에서 전달받은 ref 속성입니다.
ref는 object나 함수일 수 있습니다. 만약 전달받지 못하면 null이 됩니다.
ref는 반드시 다른컴포넌트, html element, useImperativeHandle 중에 하나에 전달되어야 합니다.

### Returns
ref prop을 받을 수 있는 React 컴포넌트를 반환합니다.

## Usage

컴포넌트에서 기본적으로 DOM node는 private하게 사용되지만, 컴포넌트내에 있는 DOM node를 부모컴포넌트에 노출할 때 사용됩니다.
ref를 노출하는 것은 컴포넌트 내부동작을 어렵게 만들 수 있습니다.
따라서 low-level인 buttons, text inputs 에서만 노출하고, application-level인 avatar나 comment 같은곳에서는 노출하지 마세요.

refs를 남용하지 마세요. 만약 props로 전달이 가능하다면 props를 사용하세요.
그리고 Effect를 사용해서 props로 넘길수 있다면 props를 사용하세요.
ref는 props로 표현할 없는 곳에서만 사용해야 합니다.
보통 스크롤, 포커스, 애니메이션 트리거, 텍스트 선택 등에서 사용합니다.

