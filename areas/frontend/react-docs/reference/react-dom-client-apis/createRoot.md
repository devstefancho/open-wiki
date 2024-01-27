---
published: false
id: createRoot
slug: createRoot
title: CreateRoot
summary: createRoot
toc: true
tags: ["react-dom-client-apis"]
categories: ["react-dom-client-apis"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

# createRoot

browser DOM node에 React Component를 표시하기 위해서 root를 생성해줌

```ts
const root = createRoot(domNode, options?)
```

domNode에 React root를 생성하면, DOM의 관리를 위임받게 된다.
root를 생성한 후에는 React Component를 DOM안에 표시하기 위해서, render 함수를 실행해야 한다.

보통 React로만 만들어진 app은 createRoot를 한번만 호출한다.

## createRoot
### Parameters
**domNode**
render하는 컨텐츠가 표시될 DOM element

**options**
object로 2가지 옵션을 갖고있다.
- onRecoverableError: 리액트가 자동으로 에러를 회복할때 실행되는 Callback
- identifierPrefix: multiple root에서 useId로 생성된 string 충돌을 막기위해서 사용

### Returns
render, unmount 메소드가 있는 object

### Caveats
- 만약 서버사이드 랜더링을 하는 경우라면, hydrateRoot를 사용하라
- child 컴포넌트가 아닌 DOM tree의 다른 부분에 JSX 코드를 render하고 싶은 경우, createPortal을 사용하라
  - 예를들면, modal이나 tooltip


## root.render
### Parameters
**reactNode**
표시하려고 하는 React Node (보통 `<App />`)

### Returns
undefined를 반환

### Caveats
- 최초에 root.render를 실행하면, 리액트는 기존에 있던 HTML을 제거하고 React Component를 랜더한다.
- 같은 root의 render함수를 한번이상 호출하면, React는 DOM을 최신 JSX로 업데이트한다.

## root.unmount
대체적으로 React로만 만들어진 App에서는 unmount를 호출하지 않음
만약 다른 framework(예를 들면 jQuery)에서 React 코드를 완전히 제거할때는 유용하다.
root.unmount를 실행하면, 같은 root에 대해서는 render를 다시 실행할 수 없다. (“Cannot update an unmounted root” 이런 에러가 발생할 것이다.)
하지만 이전의 root를 unmount한 다음에, 같은 DOM node에 새로운 root를 생성할 수는 있다.



