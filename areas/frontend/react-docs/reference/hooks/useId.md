---
published: false
id: useId
slug: useId
title: UseId
summary: useId
toc: true
tags: ["hooks"]
categories: ["hooks"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

useId로 unique id를 만들고 접근가능한 attribute들에 넘길 수 있음

```tsx
const id = useId()
```

## Reference

### Parameters
useId는 parameter를 받지 않는다.

### Return
unique id를 return한다. (이 값은 특정 컴포넌트에만 연관된다)

### Caveats
useId는 list의 key를 만드는데에 사용해서는 안된다.
key는 반드시 data를 통해서 생성되어야한다.

## Usage
aria-describedby 같은 attribute에 넣기 위해서 사용할 수 있다.

useId의 장점
hydration은 event handler를 생성된 HTML에 붙이는데, hydration이 제대로 작동하기 위해서 server와 client간에 HTML output만 같으면 된다.
useId는 호출하는 component의 parent path에 의해 생성된다. 따라서 client와 server tree가 동일하면 rendering order에 상관없이 항상 일치하게 된다.

그리고 form 내부에 있는 input들의 id의 prefix로도 사용할 수 있다.
(여러 연관된 elements를 위한 id를 생성하는 것)

만약 React app이 두개의 다른앱을 랜더링하는 상황이라면, createRoot나 hydrateRoot의 두번째 인자 옵션에 identifierPrefix를 지정해주면 된다.
그러면 useId는 identifierPrefix를 prefix로 갖고 있는 unique id를 생성한다.
