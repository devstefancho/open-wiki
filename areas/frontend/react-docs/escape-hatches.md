---
published: false
id: escape-hatches
slug: escape-hatches
title: Escape Hatches
summary: escape hatches
toc: true
tags: ["react-docs"]
categories: ["react-docs"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

# Referencing Values with Refs

새로운 render없이 어떤 정보를 기억해야하는 경우 Refs를 사용할 수 있다.

## Adding a ref to your component

ref는 current property를 갖고 있는 plain Javascript object이다.

## Example: building a stopwatch

stopwatch를 만들때는 interval의 id를 useRef에 저장한다.
(id는 render을 위해 사용되지 않기 때문에 ref에 저장한다.)
[codesandbox 예시](https://codesandbox.io/s/9trm60?file=/App.js&utm_medium=sandpack)

## Differences between refs and state

ref 값을 rendering 과정에 넣지 않아야 한다. (JSX 리턴하는 쪽에 넣으면 안됨)
ref는 useState를 사용하여 구현될 수 있다.
첫번째 render에서 useRef는 `{ current: initialValue }`를 리턴한다.
이 값은 React에 저장된다. 그리고 다음 render때에 같은 object가 return 된다.

```tsx
// Inside of React
function useRef(initialValue) {
  const [ref, unused] = useState({ current: initialValue });
  return ref;
}
```

ref는 항상 같은 object를 return 하기 때문에, setter가 필요 없는 것이다.
useRef는 충분히 실용적이기 때문에, React는 built-in 버전의 useRef를 제공한다.

## When to use refs

주로 refs는 React의 바깥(external APIs)과 통신(communicate)하기 위해서 필요하다.

- timeout ID 저장하기
- DOM Element를 저장하거나 수정하기
- JSX에서 계산할 필요없는 object들을 저장하기

만약 컴포넌트가 어떤 값의 저장이 필요한데, 그것이 rendering logic에 영향을 주지 않는다면 ref를 선택해라

## Best practices for refs

- Ref를 escape hatch(비상 탈출구)로서 다뤄라.
  - 만약 외부시스템 혹은 브라우저 APIs를 사용하는데 너무 많은 로직과 데이터흐름이 ref에 의존한다면 접근방법에 대해서 다시 고민이 필요하다
- ref.current 값을 rendering에서 읽거나 쓰지마라.
  - React는 ref.current의 변경을 알지 못한다. 만약 읽을 수 있더라도 컴포넌트 랜더링 로직을 예측하지 못하게 만들어버릴 수도 있다.
    한가지 경우에만 예외인데, `if (!ref.current) ref.current = new Thing()` 과 같이 첫번째 랜더링에서 딱한번만 ref값을 설정할때는 사용해도 된다.

## Refs and the DOM

아무값이나 ref를 사용할 수 있지만, 가장 일반적인것은 DOM element에 접근하기 위해서 사용하는 것이다.
JSX에서 ref는 ref attribute(`<div ref={myRef}>`)에 넘길 수 있는데, 이렇게 하면 React가 상응하는 DOM element를 myRef.current에 넣어줄 것이다.

## 예시

예시에서 3번 문제는 click event handler에 useRef id를 사용하지 않고, clearTimeout을 넣는 예시를 보여준다.

# Manipulating the DOM with Refs

## Getting a ref to the node

처음에 useRef Hook은 null 값인 current라는 property를 리턴한다.
React가 DOM node를 생성했을때 myRef.current에 reference를 넣을 것이다.

아래처럼 쓰는건 React에게 input의 DOM node를 inputRef.current에 넣으라고 말하는 것이다.

```tsx
<input ref={inputRef}>
```

useRef가 사용될 수 있는곳

- DOM manipulation
- React 외부의 것들 (timer IDs 같은 것)

ref는 render 간에도 남아있는다.
ref는 re-render를 trigger하지 않는다.

### How to manage a list of refs using a ref callback

1. parent element에 ref를 적용하고 querySelectAll로 각각의 child node를 찾는 것
2. [Map에 넣는 방식](https://codesandbox.io/s/useref-with-map-3qt9zm)

## Accessing another component’s DOM nodes

browser element(built-in component)에 ref를 넣으면 React는 ref의 current에 DOM node를 넣을 것이다.
Component는 ref가 없기 때문에 forwardRef를 사용해야한다.
그리고 Component의 children중의 하나에 forwardRef에서 받은 ref를 넘겨줘야한다.
디자인 시스템에서는 low-level component(button, input 등)에 일반적으로 사용하는 패턴이다.
하지만 high-level(form, list, page)에서는 DOM node를 expose하지 않는다. DOM node에 실수로 의존성을 주는 것을 피하기 위해서이다.

useImperativeHandle를 사용하면 expose 하고 싶은 함수들을 제한할 수 있다.
아래처럼 작성하면 ref접근시 focus만 사용할 수 있게 된다.

```tsx
const MyInput = forwardRef((props, ref) => {
  const realInputRef = useRef(null);
  useImperativeHandle(ref, () => ({
    // Only expose focus and nothing else
    focus() {
      realInputRef.current.focus();
    },
  }));
  return <input {...props} ref={realInputRef} />;
});
```

## When React attaches the refs

리액트에서 업데이트는 2가지 phase로 나뉜다.

1. render: Component를 호출하고 스크린에 무엇이 있어야하는지 알아낸다.
2. commit: 변경사항을 DOM에 적용한다.

첫번째 랜더링에서는 DOM node가 아직 생성되지 않는다. 그래서 ref.current값이 null이 된다.
ref.current는 commit 과정에서 설정된다. DOM을 업데이트하기 전에는 ref.current값이 0이다.
DOM 업데이트후, 리액트는 바로 그것을 DOM node로 셋팅한다.

DOM을 synchronously하게 update하는 방법

- flushSync를 react-dom에서 import해서 사용한다.
- 주로 상태업데이트 후 스크롤을 해야하는 경우 사용할 수 있을듯

```tsx
flushSync(() => {
  setTodos([...todos, newTodo]);
});
listRef.current.lastChild.scrollIntoView();
```

## Best practices for DOM manipulation with refs

Refs는 esacpe hatch이다. 반드시 React 영역 밖일때 사용해야한다.
가장 일반적인 예시로는 focus, scroll position, browser APIs 사용 등이 있다. (React가 직접할 수 없는 것들)

non-structive하게 접근하는건 (focus, scrolling) 문제가 되지 않지만, DOM을 직접 수정하는건 React가 변경하려는 것과 충돌이 일어날 수 있다.
(피해야한다는 뜻)

React에서 관리하는 DOM node를 변경하는것은 피해야한다.
React가 업데이트하지 않는 부분에 대해서는 안전하게 사용할 수 있다.
예를 들어 div가 있는데 JSX에서는 empty div이다. 이 경우 React가 그것의 children들을 건드릴 이유가 없으니, 안전하게 추가 삭제를 할 수 있다.

## 예제

video 관련해서 useRef를 활용할 수 있음([codesandbox](https://codesandbox.io/s/j42s0w?file=/App.js&utm_medium=sandpack))

```tsx
import { useState, useRef } from "react";

export default function VideoPlayer() {
  const [isPlaying, setIsPlaying] = useState(false);
  const ref = useRef(null);

  function handleClick() {
    const nextIsPlaying = !isPlaying;
    setIsPlaying(nextIsPlaying);

    if (nextIsPlaying) {
      ref.current.play();
    } else {
      ref.current.pause();
    }
  }

  return (
    <>
      <button onClick={handleClick}>{isPlaying ? "Pause" : "Play"}</button>
      <video
        width="250"
        ref={ref}
        onPlay={() => setIsPlaying(true)}
        onPause={() => setIsPlaying(false)}
      >
        <source
          src="https://interactive-examples.mdn.mozilla.net/media/cc0-videos/flower.mp4"
          type="video/mp4"
        />
      </video>
    </>
  );
}
```

list내에서 scroll 이동관련으로 활용 ([codesandbox](https://codesandbox.io/s/ddffoy?file=%2FApp.js&utm_medium=sandpack))

```tsx
// 이거는 내가 작성한것이고, Solution에서는 selectedRef 방식으로 처리했음
import { useState, useRef } from "react";

export default function CatFriends() {
  const [index, setIndex] = useState(0);
  const listRef = useRef(null);
  const scrollTo = (index) => {
    listRef.current.childNodes[index].scrollIntoView({
      behavior: "smooth",
      block: "nearest",
      inline: "center",
    });
  };

  return (
    <>
      <nav>
        <button
          onClick={() => {
            if (index < catList.length - 1) {
              setIndex(index + 1);
              scrollTo(index + 1);
            } else {
              setIndex(0);
              scrollTo(0);
            }
          }}
        >
          Next
        </button>
      </nav>
      <div>
        <ul ref={listRef}>
          {catList.map((cat, i) => (
            <li key={cat.id}>
              <img
                className={index === i ? "active" : ""}
                src={cat.imageUrl}
                alt={"Cat #" + cat.id}
              />
            </li>
          ))}
        </ul>
      </div>
    </>
  );
}

const catList = [];
for (let i = 0; i < 10; i++) {
  catList.push({
    id: i,
    imageUrl: "https://placekitten.com/250/200?image=" + i,
  });
}
```

custom component로 ref를 활용해야하는 경우 forwardRef를 활용 ([codesandbox](https://codesandbox.io/s/mhxlfn?file=%2FSearchInput.js&utm_medium=sandpack))

```tsx
// App.js
import { useRef } from "react";
import SearchButton from "./SearchButton.js";
import SearchInput from "./SearchInput.js";

export default function Page() {
  const inputRef = useRef(null);
  return (
    <>
      <nav>
        <SearchButton
          onClick={() => {
            inputRef.current.focus();
          }}
        />
      </nav>
      <SearchInput ref={inputRef} />
    </>
  );
}

// SearchButton.js
export default function SearchButton({ onClick }) {
  return <button onClick={onClick}>Search</button>;
}

// SearchInput.js
import { forwardRef } from "react";

export default forwardRef(function SearchInput(props, ref) {
  return <input ref={ref} placeholder="Looking for something?" />;
});
```

# Synchronizing with Effects

어떤 컴포넌트들은 external system과 sync를 맞추는 것이 필요하다.
analytic log를 보내거나, server connection을 하거나 그런것들이다.
Effects는 rendering 이후에 당신의 component와 다른 시스템들의 sync를 맞추기 위해서 코드를 돌리는 역할을 한다.

## What are Effects and how are they different from events?

- Rendering 로직은 pure해야하고, React의 top-level에 있어야한다.
- Event handler는 user interaction에 실행되는 side effect를 갖고 있다.

Effects는 side effect인데, rendering 그 자체에 의해 발생한다. (특정 event가 아니라)
예를 들어 server connection의 경우 interaction에 상관없이 화면이 나오면 발생해야 한다.
이런 경우가 외부 system과 React의 sync를 맞추기에 좋은 경우이다.

## You might not need an Effect

아마 Effect가 필요없을 수도 있으니, 성급하게 Effect를 적용하려고 하지마라.
browser APIs, third-party widgets, network 같은 external system과 맞춰야하는 경우인지를 먼저 생각해보자

## How to write an Effect

Effect를 작성하는 순서

1. Effect 함수를 작성
2. dependency를 명시하기
3. cleanup은 필요한 경우 추가하기

render는 pure 해야하기 때문에, side effect같은 것이 계산에 있으면 안된다.
예를들어 아래처럼 중간에 DOM 조작 같은게 있으면 안됨 (DOM 조작은 side effect이므로)

```tsx
function VideoPlayer({ src, isPlaying }) {
  const ref = useRef(null);

  // 아래 if문은 useEffect에서 호출해야한다.
  if (isPlaying) {
    ref.current.play(); // Calling these while rendering isn't allowed.
  } else {
    ref.current.pause(); // Also, this crashes.
  }

  return <video ref={ref} src={src} loop playsInline />;
}
```

useEffect안에 setState를 바로 하는 것은 전원 콘센트를 자신의 콘센트에 꽂는 것이다("pluggin a power outlet into itself")

### dependency

dependency를 적용하는 것은 useEffect에게 불필요한 re-running을 skip 해라고 하는 것이다.
(previous render와 비교했을때 같은 값이면 skip 한다.)
dependency는 array에 여러개를 포함할 수 있고, 모든것이 똑같으면 skip할 것이다.
비교할때는 Object.is 비교를 사용한다.
dependency는 아무거나 선택할 수 없다. React가 예상하는 것과 일치하지 않으면 lint error가 발생할 것이다.
만약 특정 code가 re-run할 필요가 없다면, dependency가 필요하지 않도록 Effect code를 수정해라.

ref는 dependency에 필요하지 않는데, 그 이유는 stable identity이기 때문이다.
React가 이것은 항상 같은 object를 갖는 것을 보장한다.
만약에 ref가 parent 에서 내려오는 것이면, 같은 ref를 넘기는 것인지 확실하게 보장하기 어려우므로 dependency에 넣어도 된다.

### cleanup

React는 개발모드에서 initial mount 이후에 즉시 한번 더 모든 컴포넌트들을 remount 한다.
(같은 동작을 보장하기 위해서)
이전 동작을 cleanup 하기 위해서 Effect는 cleanup function을 return 한다.
production 모드에서는 remount 하지 않는다. development 모드에서 똑같은 동작을 해보려면 Strict Mode를 끄면 된다. 하지만 Strict Mode는 항상 켜두는걸 권장한다. 많은 버그들을 찾을 수 있게 해주기 때문이다.

## How to handle the Effect firing twice in development?

development에서 두번 Effect가 발생하는 것을 어떻게 처리할 것이냐는 cleanup 함수를 구현하는 것이 답이 될 수 있다.
가장 중요한 룰은 setup -> cleanup -> setup의 순서로 실행되는것을 user는 구분할 수 없어야한다.

Effect내에서 두번 호출되는게 문제가 없는 경우는 cleanup이 필요하지 않다.
예를 들어 아래 코드 같은 경우는 같은 level로 zoom을 하는 것은 아무것도 하지 않는 것이기 때문에 상관없다.

```tsx
useEffect(() => {
  const map = mapRef.current;
  map.setZoomLevel(zoomLevel);
}, [zoomLevel]);
```

여기서는 Effect에서 cleanup을 사용하는 몇가지 예시를 들어준다.

1. Controlling non-React widgets (꼭 필요하지 않을 수 있다고 한다.)
2. Subscribing to events (구독중인 event를 지울 때)
3. Triggering animations (animation을 초기화 할때)
4. Fetching data (fetch를 중단할때)

fetch를 Effect내에서 직접 호출하는건 아래 단점(downside)이 있다.

- Effect는 server에서 동작하지 않으므로 client에서 모든 js를 다운로드 받고 app을 랜더링한 후에 data를 load할 것이다. 이것은 효율적이지 않다.
- network waterfalls을 만들 수 있다. parent 컴포넌트 아래에 있는 자식 컴포넌트들이 render가 될때마다 fetch가 시작되는 상황이면, 모든 data를 병렬로 가져오는 것보다 속도가 더 느릴 것이다.
- Effect에서 직접 fetch를 하는건 preload나 cache data를 사용하지 않는 것이다. 만약 component가 unmount되고 다시 mount된다면 데이터를 다시 가져와야 한다.
- ergonomic하지 않다. [race condition](https://maxrozen.com/race-conditions-fetching-data-react-with-useeffect)으로 인해서 버그에 시달릴 수 있다. race condition은 호출하는 속도에 의해서 먼저 호출했던 데이터가 마지막으로 응답이 와서 보이는게 달라지거나 그런것을 의미함

Fetching data를 할때는 Effect내에서 쓰는 것보다. framework에서 제공하는 built-in data fetching mechanism을 사용하는 것이 좋다. 그것이 효율적이고 숨겨진 위험에 고통받지 않는 방법이다.
아니면 client-side cache를 할 수 있는 open source 라이브러리를 사용하는 것도 좋다. (React Query, useSWR, React Router 6.4+)

Sending analytics
여기에서 중요한건 development모드에서 두번 호출할 것이라는 것이다.
이것을 수정하려고 할것인데, 그냥 이런 코드는 그대로 두는것을 추천한다. (개발모드에서 analytic이 두번찍히는 것은 그대로 둬도 된다는 것)
만약 production 모드로 테스트하고 싶으면 staging 환경에서 해보던가, 아니면 Strict Mode를 제거하고 테스트해보면 된다. 더 정교한 테스트를 위해서는 intersection observer를 활용해서 테스트해보는 것도 좋다.

App을 initialize하기 위한 것은 Effect가 아니다.
컴포넌트 바깥에 코드를 두고 한번만 실행하도록 해야한다.
이것은 browser가 load된 후에 한번만 로직이 실행되도록 보장한다.

만약 remount가 app의 로직을 망가뜨린다면, 보통 보이지 않는 버그가 존재하는 것이다.
(예를 들면 다른 페이지로 갔다가 다시 돌아왔을때 Effect가 실행되면서 문제가 생기는 경우)

## Putting it all together

React는 항상 다음 render Effect가 실행되기 전에 이전 render Effect를 clean up한다.
각각의 render는 자신만의 Effect를 갖고 있다.
(Effects from each render are isolated from each other)

# You Might Not Need an Effect

## How to remove unnecessary Effects

아래 두가지 경우에서 Effect가 필요하지 않을 것이다.

1. rendering을 위해서 data를 transform 하는 경우
2. user event에 대해서 처리하는 경우

### Updating state based on props or state

state 값을 바꿔야하는 경우 그냥 rendering에서 직접 계산하도록 하자

```tsx
function Form() {
  const [firstName, setFirstName] = useState("Taylor");
  const [lastName, setLastName] = useState("Swift");
  // ✅ Good: calculated during rendering
  const fullName = firstName + " " + lastName;
  // ...
}
```

### Caching expensive calculations

계산에 비용이 많이 드는경우 Effect 사용보다는 useMemo를 활용한다.
(useMemo로 expensive caculation을 cache 즉 memoize 한다.)
단, useMemo는 처음 render에서는 빠르게 만들어주지 않는다. update시에 불필요한 work만 skip 하는데 도움을 준다.
calculation이 expensive한지 판단하는 방법은 console.time을 사용하는 것이다.
다만 development에서 측정하는 것은 정확한 결과를 주지는 못한다. 정확한 타이밍을 위해서는 production으로 빌드해서 user device에서 테스트해야한다.

### Resetting all state when a prop changes

이 경우는 explicit하게 key를 바꿔줌으로써 state를 reset할 수있다.

### Adjusting some state when a prop changes

prop이 변경되었을때 state 일부만 변경하고 싶다면, 아래처럼 하는게 Effect를 쓰는것보다 낫다. (Better)

```tsx
function List({ items }) {
  const [isReverse, setIsReverse] = useState(false);
  const [selection, setSelection] = useState(null);
  // Better: Adjust the state while rendering
  const [prevItems, setPrevItems] = useState(items);

  if (items !== prevItems) {
    setPrevItems(items);
    setSelection(null);
  }
  // ...
}
```

아니면 selected item을 저장하지 않고 id를 저장하면 state 조정이 필요하지 않게 된다. (Best)

```tsx
function List({ items }) {
  const [isReverse, setIsReverse] = useState(false);
  const [selectedId, setSelectedId] = useState(null);
  // ✅ Best: Calculate everything during rendering
  const selection = items.find((item) => item.id === selectedId) ?? null;
  // ...
}
```

### Sharing logic between event handlers

이벤트 핸들러의 공통로직을 실행시키기 위해서 Effect를 쓰지말고, 공통로직은 그냥 일반 함수로 분리해서 호출해라

### Sending a POST request

이거는 event handler에서 처리하는게 맞다

### Chains of computations

state 변경이 chain되는 경우를 Effect로 처리하는 경우가 있는데, 이 경우에는 그냥 event handler에서 처리해야할 state를 모두 처리하는게 낫다.

```tsx
 function Game() {
  const [card, setCard] = useState(null);
  const [goldCardCount, setGoldCardCount] = useState(0);
  const [round, setRound] = useState(1);

  // ✅ Calculate what you can during rendering
  const isGameOver = round > 5;

  function handlePlaceCard(nextCard) {
    if (isGameOver) {
      throw Error('Game already ended.');
    }

    // ✅ Calculate all the next state in the event handler
    // 아래에 있는 state들을 다 분리해서 각각의 useEffect에 넣는것은 좋지 않다.
    setCard(nextCard);
    if (nextCard.gold) {
      if (goldCardCount <= 3) {
        setGoldCardCount(goldCardCount + 1);
      } else {
        setGoldCardCount(0);
        setRound(round + 1);
        if (round === 5) {
          alert('Good game!');
        }
      }
    }
  }
  // ...
```

### Initializing the application

아래처럼 app에서 load시에만 딱 한번만 돌아야하는 경우는 component 바깥에 둔다
대신 이런패턴은 overuse 하면 안된다. (속도저하 혹은 아무 component나 import 했는데 의도치않은 행동으로 놀라게 할수도 있음)
App.js처럼 root component에서만 사용해라

```tsx
if (typeof window !== "undefined") {
  // Check if we're running in the browser.
  // ✅ Only runs once per app load
  checkAuthToken();
  loadDataFromLocalStorage();
}
function App() {
  // ...
}
```

### Notifying parent components about state changes

state 변경이 있는 것을 부모 컴포넌트에게 알리려고 할때 사용하지 마라

### Passing data to the parent

리액트에서는 data를 parent에서 child로 flow한다. 만약 뭔가 잘못되었을 때 이 흐름으로 데이터를 추적해나가야하는데 ( child에서 parent로 타고 올라가면서 trace), child에서 parent로 데이터를 올려보내는 처리를 useEffect로 해버리면, data flow가 추적하기 어려워진다.
예측 가능하고 심플한 data flow를 지켜야한다.

### Subscribing to an external store

browser의 navigator.onLine 처럼 외부 data store를 구독해야하는 경우
Effect 대신에 useSyncExternalStore를 사용하면 된다.

### Fetching data

이 경우에는 Effect내에서 주로 사용한다.
user interaction과 상관없이 데이터를 불러들이는 것이라서 그렇다.
대신 주의해야하는 건 race condition이 있기 때문에, [cleanup function을 추가](https://react.dev/learn/synchronizing-with-effects#fetching-data)해줘야 한다는 것이다.
대신 이렇게 사용할 때는 custom hook(Effect를 갖고 있는)으로 따로 빼내서 사용하는게 좋다.

# Lifecycle of Reactive Effects

Effect는 component와는 다른 lifecycle을 갖고 있다.
start synchronizing, stop synchronizing 두가지만 갖고 있다. props나 state가 변하는것에 따라서 여러번 발생할 수도 있다.
React는 Effect의 dependency가 제대로 명시되어 있는지 확인하기 위해서 linter rule을 제공한다.
이것은 Effect가 최신의 props와 state에 대해서 sync를 지킬 수 있게 해준다.

## The lifecycle of an Effect

Effect는 Component lifecycle과 별개로 생각해야한다.
Effect는 외부 시스템을 현재 props, state와 sync를 맞추는 것을 설명한다.

cleanup 함수는 Effect의 return 되는 값이고, sync를 stop 하는방법을 명시한다.
만약 cleanup 함수가 없다면, React는 마치 empty cleanup 함수가 return 되는것처럼 행동할 것이다.

Effect에서 roomId가 dependency라고 하면,
roomId가 바뀌면 old value를 stop synchronizing을 하고,
new value로 start synchronizing 한다.
즉 다른 roomId로 re-render 하면, Effect는 re-synchronizing 한다.

Component의 시각에서 바라보지 마라.
기존에는 Effect를 특정 시간에 발생하는(예를 들면, after a render , before unmount) callback이나 lifecyle events로 생각했을 것이다.
이런식의 생각은 복잡도를 빠르게 올리므로 피하는게 가장 좋다.

React는 개발모드에서 강제로 re-synchronizing 하기 때문에(stress-test), cleanup 동작이 잘되는지 체크할 수 있다.

React는 dependency list를 보고 re-sync의 필요여부를 ㄹ알 수 있다.
예를 들어 component props인 roomId가 dependency라고 생각해보면,
roomId는 바뀔수 있는 값이다. 이 값은 Effect내에 있고, Effect는 이것을 읽는다. 바뀔수 있는 값이기 떄문에 dependency list에 포함시켜야 한다.

각각의 Effect는 분리된 synchronization process이다.
같은 시점에 동작해야한다는 이유로 logic을 하나의 Effect에 묶지마라.
예를들어, log 기록하는것과 server connection 로직이 있는데, 이게 둘다 roomId가 dependency라도, 각각 다른 Effect로 분리해야한다 (Effect를 2개를 써라)
(Each Effect in your code should represent a separate and independent synchronization process)

다른 Effect를 지웠을때, 또 다른 Effect가 망가지지 않아야한다. 이것은 분리를 잘했다는 것을 의미한다. 하지만 만약 일관성있는 코드를 단지 깔끔해보이기 위해서 분리하는 것은 유지보수를 힘들게 만든다.
(코드가 깔끔해보이는 것에 대해서만 초점을 맞추지 않아야함)

## Effects “react” to reactive values

reactive value 즉, 바뀔수 있는 값 (props, state, 컴포넌트 안에서 정의된 다른 값들)들은 dependency로 넣어야한다. (render시에 바뀌지 않는 값은 dependency에 필요하지 않음)
예를 들어 component 바깥에 있는 상수값은 dependency에 필요하지 않음

Effect관점에서 생각해야한다. mounting, unmouting은 생각할 필요가 없다. (시점은 React가 알아서 하는 것),중요한것은 무엇을 Effect에서 start, stop synchronizing 하는가이다.

component내에서 정의된 모든 값들은 reactive이다.
따라서 계산된 값도 Effect에서 사용한다면 dependency에 필요하다.

```tsx
function ChatRoom({ roomId, selectedServerUrl }) {
  // roomId is reactive
  const settings = useContext(SettingsContext); // settings is reactive
  const serverUrl = selectedServerUrl ?? settings.defaultServerUrl; // serverUrl is reactive
  useEffect(() => {
    const connection = createConnection(serverUrl, roomId); // Your Effect reads roomId and serverUrl
    connection.connect();
    return () => {
      connection.disconnect();
    };
  }, [roomId, serverUrl]); // So it needs to re-synchronize when either of them changes!
}
```

React는 어떤 value들은 Component안에 있더라도 바뀌지 않는 다는 것을 알고 있다.
useState의 set function과 useRef의 ref object는 stable하다. re-render간에 변경되지 않는 것을 보장한다. stable value는 reactive가 아니다. 그래서 dependency list에서 넣지 않아도 된다. 포함하는것도 허용은 되지만 변경되는 값이 아니므로 필요하지 않다

dependency에 포함하지 않기 위해서 값이 변경되지 않는 것을 Effect에게 알리고 싶다면 아래 두가지 방법이 있다. (linter에게 prove한다고 표현함)

1. Component 밖으로 값을 빼낸다.
2. Effect안에서 선언해서 사용한다.

user interaction이 있어야하는 event handler와 다르게, Effects는 reactive blocks of code이다. 필요하면 언제나 synchronization을 실행한다.
당신은 dependency를 선택할 수 없다. 모든 reactive value는 dependency에 포함되어야한다. (linter가 이것을 강제해준다.) 간혹 이것으로 인해서 무한 루프가 발생하거나 너무 잦은 실행이 발생할 수 있는데, 이 때문에 linter를 suppress 하려고 하지마라 대신 아래 몇가지를 체크해봐라

- Effect의 로직이 독립적인 synchronization process를 갖는지 체크해봐라 (너무 여러가지를 synchronization한다면 분리해라)
- 만약 reactive part와 non-reactive part를 분리하고 싶다면 Effect Event(useEffectEvent)를 사용하여 분리해라
- object와 function은 dependency에 넣지 마라. (새로운 render마다, object와 function은 새로 만들어지기 때문에 매번 re-synchronization이 발생할 수 있다)

linter의 power는 제한적이다. linter는 dependency가 잘못되었다는 것만 안다. 가장 좋은 방식의 해법을 알지는 못한다.
만약 linter가 제안하는 dependency가 추가적인 루프를 발생한다해도 이것은 linter를 무시해도 된다는 것은 아니다.
Effect에서 dependency가 필요하지 않도록 코드를 수정하는 것이 필요하다.

# Separating Events from Effects

## Choosing between event handlers and Effects

why the code needs to run을 생각해봐라
Event handler는 특정 interaction의 반응으로써 실행한다.
Effects는 synchronization이 필요할때마다 실행한다.
(server connection을 생각해보면, 필요할때마다 re-connect을 해야함)

## Reactive values and reactive logic

직관적으로 생각했을때, event handler는 항상 manually trigger 되야하고, (버튼같은 것으로)
Effect는 automatic하게 실행되어야한다. (sync를 맞춰야할때마다 re-run 해야함)

Props, state, 그리고 component안에서 선언된 variables들은 모두 reactive value라고 부른다.
이런 reactive value들은 render에 의해서 값이 바뀔 수 있다.

event handler안에 있는 Logic은 reactive가 아니다. user interaction이 없으면 다시 실행되지 않기 때문이다.
event handler는 reactive value의 변경에 반응하지 않고 reactive value를 읽을 수 있다.

Effects안에 있는 Logic은 reactive이다. 만약 reactive value를 Effect에서 읽는다면, 반드시 그것을 dependency에 명시해줘야한다.
re-render가 reactive value를 변경하면 Effect에 있는 Logic도 새로운 value로 다시 실행될것이다.

## Extracting non-reactive logic out of Effects

useEffectEvent를 사용하면 non-reactive value를 분리할 수 있다.
(Effect Event는 reactive가 아니기 때문에 dependency에서 제외해야한다.)

Effect event는 event handler와 매우 비슷하게 보이는데, 중요한 차이점은 event handler는 user interaction에 대한 반응으로 실행되는 반면, Effect Event는 Effects에 의해서 실행된다는 것이다.
Effect event는 Effect의 reactivity와 reactive하지 않아야하는 코드들 사이를 "break the chain"을 한다.

Effect event에서 사용하는 변수는 Effect에서 직접 인자로 넘겨주는것이 좋다.
인자로 넘겨주지 않고 직접 Effect Event에서 접근하면 실수로 dependency를 삭제해버릴 수도 있다.
(Note 예시 참고)

Effect event의 한계점

- Effects 안에서만 호출할 수 있음
- 다른 컴포넌트나 훅스에 넘길 수 없음

# Removing Effect Dependencies

간혹 dependency가 너무 잦은 Effect run을 발생시키거나, 무한루프를 발생시키기도 하는데
이때 불필요한 dependency를 제거하는 방법에 대한 가이드를 알아본다.

## Dependencies should match the code

당신은 사용할 dependency를 직접 선택할 수 없다. Effect안에 있는 모든 reactive value는 dependency가 되어야한다.
dependency list는 Effect와 관련된 코드(surrounding code)에 의해서 결정된다.

만약 없애고 싶은 dependency가 있다면 linter에게 prove 해야한다.
Component 밖에서 상수로 정의하는 방법이 있다.

dependency를 바꾸기 위해서는 코드를 바꿔야한다. 아래 스탭을 참고해본다.

1. reactive value가 선언되어 있는 코드를 바꾼다.
2. linter에 따라서 dependency를 맞춘다.
3. 만약 dependency가 만족스럽지 못하다면 1번부터 다시한다.

## Removing unnecessary dependencies

아래 이유들로 Effect를 실행하는 dependency가 합리적이지 않을 수가 있다.

- 서로 다른조건에서 Effect의 다른부분을 다시 실행하고 싶을 수 있다.
- 일부 dependency는 그냥 최신값만 읽고 싶을 수 있다. (reacting 하지 않고)
- object, function등의 dependency가 의도치 않게 자주 변경될 수 있다.

위와 같은 상황에 대한 해답을 찾는 방법

1. 이 코드는 event handler에 있어야 하는 코드인가?

- submit 로직을 Effect에 넣어둔 경우

2. Effect에서 연관되지 않은 여러가지 것들을 하고 있는가?

- 여러 fetch 로직을 하나의 Effect에 넣는 경우, 로직간에 불필요한 dependency가 늘어남

3. 다음 state를 계산하기 위해서 다른 state를 읽고 있지는 않은가?

- Effect안에서 state를 업데이트할때는 updater function을 사용하면 dependency를 줄일 수 있음

4. 값의 변경에 대한 reacting 없이 값을 읽기만 하고 싶은가?

- useEffectEvent를 사용하여 non-reactive 코드를 분리(extract)한다.
- props로 받는 event handler 함수는 useEffectEvent로 wrap 한다.

5. 의도치 않게 reactive value들이 바뀌는가?

- object, function을 dependency에 넣으면 render마다 재 생성되기 때문에 의도치않게 Effect가 실행될 수 있다. (따라서 dependency에 직접 넣는것은 피해야함)
- static object, function들을 component 밖으로 옮기는 방법이 있다.
- dynamic obejct와 function을 Effect 안으로 옮기는 방법이 있다.
- object의 primitive value를 사용할 수 있다. props로 object를 넘기는 경우, Component top level에서 destructure해서 Effect에서 사용할때는 속성값만 사용하면 된다. (function도 같은 방식으로 적용해본다.)

# Reusing Logic with Custom Hooks

## Custom Hooks: Sharing logic between components

hook을 분리함으로써, component들에서 repetitive logic이 줄어든다.
여기서 중요한 것은 hook은 어떻게 실행하는지가 아니라(how to do it, 예를들면 브라우저 이벤트 구독하는 것)
무엇을 하려고하는지(what they want to do, 예를 들면 온라인 상태를 사용)를 나타내야한다.
로직을 커스텀 훅으로 추출하면, 외부 시스템이나 브라우저 API와 관련된 복잡한 세부사항을 숨길수 있다. 구현방법이 아닌 수행하려는 작업에 대한 의도를 표현해야한다.

훅은 use로 시작하는 이름을 가져야한다.
regular function과 hooks를 구분하기 위해서 regular function에서는 use prefix를 피해라
예를 들면 getColor이 component에서 사용된다면, hook이 아니라고 생각할 수 있어야한다.
(내부에 useState나 useEffect같은 것이 없어야한다.)

만약 당장 내부에 hook이 없지만, 미래에 포함할 예정이라면 use를 붙여도 된다. (대신 꼭 TODO를 남겨주자)

custom hook은 stateful logic을 공유하고, state 자체는 공유하지 않는다.

## Passing reactive values between Hooks

component가 re-render 될때마다 custom hook 내부코드도 re-run 될 것이다.
이 때문에 custom hook은 pure 해야한다. custom hook을 component의 일부라고 생각해야한다.
항상 component와 같이 re-render되기 때문에 최신의 props와 state를 받게 된다.

hook을 다른 hook의 인자로 넘길수도 있다.
(useState의 state를 custom hook의 인자로 넘겨도 됨)

custom hook내에서 event handler를 인자로 받아서 useEffect에서 사용하는 경우,
useEffectEvent를 사용해서(event handler를 useEffectEvent로 wrap) dependency list에서 제외할 수도 있다.

## When to use custom Hooks

항상 custom hook으로 logic을 분리할 필요는 없지만, useEffect를 사용하는 경우 고려해보는게 좋다
(However, whenever you write an Effect, consider whether it would be clearer to also wrap it in a custom Hook)
custom hook으로 추출하는 것은 data flow를 명시적으로 만들어준다.
app에 있는 대부분의 Effect들은 custom hook이 될 것이다.

custom hook으로 분리했을때 장점은 만약에 내부 변경이 필요할때 component를 수정하지 않고,
custom hook내부에서만 바꾸면 된다는 것이다.
(예시에서는 custom hook이 return하는 것이 없이 `useFadeIn(ref, 1000)` 만 사용하기도 함)

Effect를 반드시 써야하는 건 아니다.

- Effect대신에 useSyncExternalStore를 사용할 수도 있다.
- animation 같은 것을 만들때 Effect를 여러개 사용해야한다면 Javascript class에 로직을 넣어두는 방법도 좋다.
- fade-in animation 같은 경우는 CSS animation을 사용하는 것이 더 효과적일 것이다.
