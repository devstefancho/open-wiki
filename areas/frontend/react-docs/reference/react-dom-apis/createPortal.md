---
published: false
id: createPortal
slug: createPortal
title: CreatePortal
description: createPortal
tags: ["react-dom-apis"]
categories: ["react-dom-apis"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

# createPortal

children을 DOM의 다른 부분에 랜더링할 수 있다.
```tsx
<div>
    <SomeComponent />
    {createPortal(children, domNode, key?)}
</div>
```

## Reference
### Parameters
**children**
React를 사용하여 랜더될 수 있는 어떤것 (div, Component, Fragment, string, number, array)

**domNode**
document.getElementById() 같은 것으로 return된 어떤 DOM Node
DOM Node는 반드시 존재해야함, 다른 DOM Node를 넘기는 것은 portal의 content의 재생성을 발생시킴

**key**
string 혹은 number로 unique key

### Return
- JSX를 포함할 수 있는 React Node 혹은 React Component를 반환
- React가 랜더된 output을 만나면, 제공된 children을 domNode안에 배치시킨다.

### Caveat
- Portal의 이벤트 버블은 DOM tree가 아닌, React tree에 따라 발생한다.
- 예를 들어 portal을 클릭하면, portal을 감싸고 있는 div의 onClick이 실행된다.
- 만약 이것이 문제라면, stopPropagation을 해주거나, portal 자체를 다른 React tree로 이동시켜야 한다.

## Usage
### 다른곳에 랜더링 시키기
Portal은 컴포넌트를 DOM의 다른 위치에 랜더시킬 수 있게 해준다.
컴포넌트 특정부분을 container 외부로 escape 시켜주는 것이다.
예를 들어, modal dialog나 tooltip을 page 외부에서 보여주도록 할 수 있다.

Portal을 만들기 위해서는 어떤 JSX와, 위치시킬 DOM node가 필요하다.
Portal은 요소를 teleported 시켜준다.

Portal은 DOM node의 물리적 위치만 변경시켜준다. 나머지 방식은 portal이 특정 JSX안의 children인것처럼 동작한다.
예를들어, child(portal)은 parent tree의 context에 접근할 수 있고, React tree에 따라 이벤트 버블링이 발생한다.

웹 접근성을 고려하기 위해
Modal을 만들때는 [WAI-ARIA Modal Authoring Practices](https://www.w3.org/WAI/ARIA/apg/patterns/dialog-modal/)를 참고하자.
