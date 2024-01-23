---
published: false
id: useCallback
slug: useCallback
title: UseCallback
description: useCallback
tags: ["hooks"]
categories: ["hooks"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

---
id: "useCallback"
aliases:
  - "Reference"
tags: []
---

useCallback은 re-render간에 function definition을 cache 해주는 React Hook이다.

```typescript
const cachedFn = useCallback(fn, dependencies);
```

## Reference

### Parameter

**fn**
cache 하려는 function value로 arguments, return 을 가질 수 있다.
initial render에서는 function 그대로를 return하게 된다. (call 하지는 않는다.)
next render부터는 dependencies이 바뀌지 않으면 same function을 주게 된다.

**dependencies**
fn 내에 있는 reactive value가 들어가야하며, dependency는 고정된 갯수만 들어갈 수 있다.

### Caveats

- useCallback은 hook이기 때문에 component top level에서 사용할 수 있다. loop나, 조건문안에서는 사용할 수 없다. (만약 그렇게 사용해야한다면 component를 분리해서 사용해라.)
- development mode에서는 파일이 수정되면 cache를 버릴 것이다. (throw away)

## Usage

rendering 성능 최적화를 위해서 가끔 cache 된 function을 child component에 넘겨야할 때가 있다.
기본적으로 component가 re-render될때, 하위 컴포넌트들은 recursive하게 모두 re-render된다.
이때 같은 props에 대한 render를 skip하기 위해서 memo를 사용한다.
그런데 함수가 props로 내려가게 되면 부모컴포넌트에서 re-render될때마다 함수를 매번 새로 생성이 된다. 따라서 이때는 memo로는 최적화가 되지 않는다.
이때 useCallback을 사용하면 된다.

useCallback은 성능 최적화에서만 사용해야한다.
만약 코드가 useCallback없이 제대로 동작하지 않는다면, 먼저 그 문제부터 해결하고나서 useCallback을 적용해라

useMemo는 function 호출의 결과값을 cache하고, useCallback은 function 자체를 cache 한다.

useCallback은 모든곳에 추가되어야하는가?
만약 app이 페이지이동이 많은 경우라면, memoization은 불필요하다.
하지만 만약 app이 drawing editor이거나 세밀한 interaction 이라면 도움이 될 것이다.

useCallback이 유용한 경우는 몇가지 케이스만 있다.

- memo를 사용하는 컴포넌트에 prop으로 넘기는 경우
- 다른 hook에 function을 넘기는 경우
  (함수를 custom hook에 넘기는데 이게 custom hook의 useEffect에서 사용되는 경우 useCallback으로 한번 wrap하고 넘기면 됨)

다른 경우에는 useCallback을 사용해서 얻는 이득이 없다. 오히려 가독성이 떨어지는 단점이 있다.
모든 memoization이 효과적인 것은 아니다. 하나의 새로운 값이 모든 component의 memoization을 break할 수도 있기 때문이다.

useCallback은 함수생성 자체를 막아주는 것은 아니다. 항상 함수를 생성해야하고, cache된 함수를 주는 것 뿐이다.

아래 몇가지 원칙을 따르면 많은 불필요한 memoization을 줄일 수 있다.

- JSX children을 사용하는것, wrapper component가 그것만의 state로 update되면, React는 children을 re-render하지 않는다. (React가 알고 있다.)
- lift state up 보다는 local state를 선호할 것
- rendering logic을 pure하게 만들 것 (re-render 버그가 있는 경우 memoization하지 말고, 먼저 고쳐라)
- state를 업데이트하는 용도의 불필요한 Effect를 사용하지 말것 (많은 성능문제는 Effect chaining에 의해서 발생한다.)
- Effect에서 불필요한 dependency를 제거할 것

만약 특정 interaction이 laggy하게 느껴진다면, React Developer Tools의 Profile 기능을 사용해보자.

몇가지 useCallback 관련 예시

- useCallback에서 state update할 때는 updater function을 사용해서 dependency를 줄이자.
- Effect에서 사용하는 함수를 useCallback으로 감싸기 보다는, Effect내부에서 선언하자
- custom Hook에서 return하는 어떠한 function이라도 useCallback으로 감싸는것을 추천한다.

## Troubleshooting

만약 render마다 useCallback이 다른 function을 return 한다면
dependency를 체크해보고, Object.is를 console.log에서 찍어서 확인해본다.
(`console.log`로 변수를 console에 찍은다음에 "Store as a global variable"를 사용해서 temp1, temp2에 저장하고 아래 방식으로 비교해봄)

```javascript
Object.is(temp1[0], temp2[0]); // Is the first dependency the same between the arrays?
Object.is(temp1[1], temp2[1]); // Is the second dependency the same between the arrays?
Object.is(temp1[2], temp2[2]); // ... and so on for every dependency ...
```

useCallback을 map과 같은 loop안에서 사용하고 싶다면
별도의 함수로 추출해서 useCallback을 사용해야 한다.
