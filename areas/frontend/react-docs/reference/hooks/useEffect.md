---
published: false
id: useEffect
slug: useEffect
title: UseEffect
description: useEffect
tags: ["hooks"]
categories: ["hooks"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

# useEffect

external system과 synchronize 해주는 hook

```tsx
useEffect(setup, dependencies?)
```

## Reference

### Parameters

**setup**
Effect's logic 부분으로, optional로 cleanup function을 return할 수 있다.
DOM에 component가 add되면 실행되고, component가 DOM에서 제거되면 cleanup function이 실행된다.

**dependencies**
setup code안에 있는 reactive value들의 list
reactive value는 props, state, component body안에서 선언된 모든 variables와 functions

### Returns

undefined를 리턴함

### Caveats

- Effect는 client에서만 동작하므로, server rendering 중에는 실행되지 않는다.
- 효과가 시각적인 작업을 수행하는 경우(ex. 툴팁 위치지정) 지연이 눈에 띄게 된다면, useLayoutEffect로 대체한다.
- 다만 useLayoutEffect는 랜더링 직후에 동기적으로 실행되므로 성능에 영향을 줄 수 있다.

## Usage

cleanup code는 old props, state와 실행된다.
setup code는 new props, state와 실행된다.

bug를 찾는데 도움을 주기 위해서, 개발모드에서는 setup전에 setup, cleanup이 추가로 한번더 실행된다.
이것은 stress-test로써 logic이 정확하게 실행되는것을 보증해주기 위함이다.

Effect에서 말하는 external system은 아래와 같다. (예시)

- timer 형태 `setInterval()`, `clearInterval()`
- event 구독 형태 `window.addEventListener()`, `window.removeEventListener()`
- third-party의 animation library 형태 `animation.start()`, `animation.reset()`

class 사용하여 DOM node의 ref값을 저장해서 DOM node를 조작하는 경우
React Component가 tree에서 제거되면, DOM node도 제거되기 때문에
class instance는 Javascript engine에 의해 자동으로 garbage-collected 된다.

fetch data를 Effect에서 하는 경우
race condition을 고려해야 한다.
더 추천하는 방식은 framework의 data fetching mechanism을 사용하는 것이다.
추가 고려사항

- Effect는 server에서 실행되지 않으므로, server-rendered HTML에 data가 빠지게 될 것이다. (효율성 문제)
- network waterfalls를 발생시킬 수 있다.
- Effect에서 fetch하는 것은 preload나 cache를 하지 않는 것을 의미한다. (unmount되면 fetch를 다시 할것이기 때문에)
- ergonomic 하지 않다.(사용자에게 편리하지 않다)
  위와 같은 문제를 해소하기 위해서는

- framework를 사용한다면 built-in data fetching mechanism을 사용한다.
  (Nextjs에서의 getStaticProps나 getServerSideProps 같은 데이터 fetching 함수)
- 혹은 client-side cache를 고려한 React Query, useSWR, React Router 6.4+ 등을 사용한다.

### Effect Event를 사용하기

useEffectEvent를 사용하면 non-reactive value를 dependencies에서 제외할 수 있다.

### server와 client에서 서로다른 content를 보여주는 경우

server rendering을 사용하는 경우 두가지 다른 환경에서 render가 된다.
server에서는 initial HTML을 생성하고 client에서는 code를 다시 생성하면서 event handler를 붙이게 된다.
(이것이 hydration을 위해서 client와 server의 initial render 결과가 같아야하는 이유이다.)
그런데 localStorage의 경우 server에서 읽을 수 없다.
이 경우에는 useEffect에서 didMount에 대한 state를 true로 만들면서 client-only JSX를 보여줄 수 있다.

## Trouble Shooting

### Effect가 두번 실행되는 경우

개발모드에서는 stress-test를 위해서 한번 더 setup, cleanup을 실행한다.
중요한 룰은 user는 setup이 한번 실행되는 것과 setup -> cleanup -> setup 으로 실행되는 것을 구분할 수 없어야한다.

## 무한 cycle이 실행되는 경우

아래 경우를 체크한다.

- Effect에서 어떤 state를 update하는지
- 그 state가 re-render를 발생시키고, 그것이 업데이트를 발생시키는 Effect의 dependencies인지

## cleanup 관련

- setup code없이 cleanup을 단독으로 사용하지 마라
- cleanup과 setup logic은 symmetrical 해야한다. (setup된 것을 stop하거나 undo 해야함)
