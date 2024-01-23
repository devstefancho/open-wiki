---
published: false
id: adding-interactivity
slug: adding-interactivity
title: Adding Interactivity
description: adding interactivity
tags: ["react-docs"]
categories: ["react-docs"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

# Responding to Events

event handler 이름은 handle + event name 형식으로 짓는다.

```typescriptreact
// arrow function을 쓰거나 함수를 별도로 선언해도 무방하다.
<button onClick={function handleClick() {
  alert('You clicked me!');
}}>
```

event handler는 called 되면 안된다. pass되어야한다.
called되면 rendering될때 마다 실행될것이다.
`{}`는 Javascript inside the JSX 이다. 따라서 js 스크립트가 랜더링되면서 바로 실행될 것이다.

## event handler와 props

event handler는 component의 props를 직접 받을 수 있다.
[디자인시스템](https://uxdesign.cc/everything-you-need-to-know-about-design-systems-54b109851969)에서는 button같은거는 스타일링만 포함하기 때문에 특정한 동작에 대한 코드가 없어야한다.
대신 PlayButton, UploadButton 같은 컴포넌트가 event handler props를 처리해줘야한다.

```typescriptreact
function Button({ onClick, children }) {
  return <button onClick={onClick}>{children}</button>;
}

function PlayButton({ movieName }) {
  function handlePlayClick() {
    alert(`Playing ${movieName}!`);
  }

  return <Button onClick={handlePlayClick}>Play "{movieName}"</Button>;
}

function UploadButton() {
  return <Button onClick={() => alert("Uploading!")}>Upload Image</Button>;
}

export default function Toolbar() {
  return (
    <div>
      <PlayButton movieName="Kiki's Delivery Service" />
      <UploadButton />
    </div>
  );
}
```

event handler props의 naming
div나 button은 onClick 핸들러만 있기 때문에 여러개의 button을 가진컴포넌트에
핸들러를 커스텀을 하기위해서 다양한 이름의 핸들러를 만들어서 내려줄 수 있다.
prefix를 `on`으로 붙인 네이밍을 쓴다.
(props로 내려줄때는 handlerPlayMovie가 아니라 onPlayMovie가 적합한듯)

```js
export default function App() {
  return (
    <Toolbar
      onPlayMovie={() => alert("Playing!")}
      onUploadImage={() => alert("Uploading!")}
    />
  );
}

function Toolbar({ onPlayMovie, onUploadImage }) {
  return (
    <div>
      <Button onClick={onPlayMovie}>Play Movie</Button>
      <Button onClick={onUploadImage}>Upload Image</Button>
    </div>
  );
}

function Button({ onClick, children }) {
  return <button onClick={onClick}>{children}</button>;
}
```

## Stopping Propagation

event propagation
bubble과 propagate는 같은 의미로 사용된다.
onScroll은 전파되지 않고, 직접 적용한 JSX tag에서만 동작한다. 다른 이벤트들은 전파된다.

```typescriptreact
function Button({ onClick, children }) {
  return (
    <button
      onClick={(e) => {
        e.stopPropagation();
        onClick();
      }}
    >
      {children}
    </button>
  );
}

export default function Toolbar() {
  return (
    <div
      className="Toolbar"
      onClick={() => {
        alert("You clicked on the toolbar!");
      }}
    >
      <Button onClick={() => alert("Playing!")}>Play Movie</Button>
      <Button onClick={() => alert("Uploading!")}>Upload Image</Button>
    </div>
  );
}
```

event handler는 기본적으로 1개의 object를 argument로 받는다. 이것은 주로 `e`라고 부르고 event를 의미한다.
간혹 stopPropagation을 한 상태에서 elements의 모든 이벤트를 catch하고 싶을 때가 있다.
이 경우에는 Capture를 이벤트명 끝에 붙이면 된다.

```typescriptreact
<div
  onClickCapture={() => {
    /* this runs first */
  }}
>
  <button onClick={(e) => e.stopPropagation()} />
  <button onClick={(e) => e.stopPropagation()} />
</div>
```

Capture는 routers나 analytics 같은 것을 처리할때 유용하다. 하지만 app code에서는 사용하지 않아야한다.
여기서 router랑 app code를 ChatGPT는 이렇게 설명한다.
라우터(router)는 웹 애플리케이션에서 페이지간의 이동을 관리하는 코드를 말한다.
라우터에서 이벤트 캡처를 사용하는 경우, 이벤트가 전파되지 않더라도 해당 이벤트에 대한 정보를 추적하고, 페이지 이동과 관련된 로직을 실행할 수 있다.
앱 코드(app code)는 일반적인 웹 애플리케이션의 코드를 의미한다.

## Preventing default behavior

`<form></form>`같은것은 submit을 하면 page를 reload한다. 이런것처럼 몇개의 event들은 기본 브라우저 동작을 갖고 있는데
이를 실행하지 않도록 해주는 것이 preventDefault이다.

## Can event handlers have side effects?

rendering 함수들과 달리 이벤트 핸들러는 pure할 필요가 없다.
그래서 뭔가를 바꾸기에 가장 좋은 곳이다. 예를 들어 input값이 바뀔때 state를 업데이트 한다거나, button 클릭시 list를 바꾼다거나
그러나 정보를 바꿀때는 그것을 어딘가(state, 즉 component's memory)에 저장해야한다.

# State: A Component's Memory

In React, component-specific memory is called state

## When a regular variable isn't enough

Local variable 문제점

1. Local variable은 render간에 유지되지 않는다.
2. Local variable 값을 변경하는 것은 render를 trigger 하지 않는다. (React doesn't realize it needs to render the components again with the new data)

새로운 데이터로 업데이트 하기 위해서는 두가지가 되어야한다.

1. render간에 데이터를 계속 보유하고 있어야함
2. React가 새로운 데이터로 render할 수 있게 trigger가 발생해야함

## Adding a state variable, Meet your first Hook

리액트에서는 `use`로 시작하는 Hook 이라는 것을 사용한다.
Hook은 special function으로 rendering이 일어나는 동안만 사용할 수 있다.
Hook은 컴포넌트 top level에서 호출되어야한다. (조건문, loop, nested function안에서 사용할 수 없다.)

## Anatomy of useState

매번 컴포넌트가 render 될때, useState는 2가지 값을 array에 준다.

1. The state variable (값이 저장되어있음)
2. The state setter function (state를 새로운 값으로 업데이트할 수 있고, 리액트가 다시 render되도록 trigger해줌)

## Giving a component multiple state variables

state를 여러개 사용해도 되지만, 만약 state간에 연관성이 있다면 object 구조의 state를 사용하는 것도 좋다.

How does React know which state to return?
useState는 아무런 구분정보(identifier)도 받지 않는다. 그런데 React는 어떻게 각 state를 구분할까?
Hooks는 호출되는 순서로 이것을 기억한다. 이것이 제대로 동작할 수 있는 이유는 top level에서 호출하기 때문이다.
[linter plugin](https://www.npmjs.com/package/eslint-plugin-react-hooks) 으로 순서를 실수하지 않도록 잡아 줄 수 있다.
내부적으로 리액트는 state pairs를 array로 hold 한다. 랜더링 될때마다 새로운 state pair를 전달해준다. ([참고](https://medium.com/@ryardley/react-hooks-not-magic-just-arrays-cd4f1857236e))
내부로직은 [codesandbox](https://codesandbox.io/s/hqrhcm?file=/index.js&utm_medium=sandpack)를 참고하자

## State is isolated and private

리액틑 각 컴포넌트 instance에 대해서 독립적인 상태를 갖는다.
그래서 상태를 공유(sync)하기 위해서는 가장 가까운 shared parent component에 state를 넣어줘야한다.

# Render and Commit

## Step 1: Trigger a render

주문이 들어오고 주문을 Component kitchen에 넘긴다.

1. 컴포넌트 초기랜더링: createRoot로 target DOM에 initial render를 한다.
2. state가 업데이트 되면 re-render 한다.

## Step 2: React renders your components

초기랜더링 때는 root Component를 호출한다.
연달아 일어나는 랜더링은 state가 업데이트된 function Component를 호출한다.
이 과정은 recursive하다. 만약 업데이트되는 component가 다른 component를 return한다면
React는 다음 component를 계속 return하고 nested된 component가 없을때까지 이 과정을 계속할 것이다.

initial render에서는 DOM nodes를 생성한다. (section, h1, img 같은것들)
re-render에서는 속성값들을 계산해서 변경이 있으면 commit phase로 넘어간다.
(rendering과정은 pure calculation이어야 한다. 같은 input에는 같은 output이 나오고, 자기것만 하고 다른걸 바꾸면 안된다.)

rendering의 기본 동작은 최적화가 된것이 아니다.
(특히 상위 컴포넌트가 업데이트가 되는 경우, 아래에 있는것들이 다 다시 랜더링 되기때문에 최적화된것이 아님)
최적화 이슈는 [Performance section](https://legacy.reactjs.org/docs/optimizing-performance.html)에서 다룬다.

## Step 3: React commits changes to the DOM

rendering(calling)이 일어난 후에, react는 DOM을 수정한다.

initial render에서는 appendChild DOM API를 사용해서 모든 DOM nodes를 screen에 표시한다.
re-render에서는 계산된 최소한의 operation을 실행한다.
(React는 render간에 변화가 있는 DOM nodes만 변경한다.)

## Epilogue: Browser paint

rendering과 DOM update가 끝난 후에, browser는 screen을 repaint한다.
이것을 browser rendering이라고 부른다. (또는 painting이라고 부름)

# State as a Snapshot

## Setting state triggers renders

user click이 일어나면 user interface에 바로 변화가 생길 것 같지만,
react는 사실 그렇게 동작하지 않는다.

## Rendering takes a snapshot in time

state를 변경하는것은 새로운 render를 요청한다.
Rendering은 React가 component를 호출하는 것을 의미한다. (component는 function이다.)
function이 return한 JSX는 그 시간에 있는 UI의 snapshot이다.
props, event handler, local variables들은 그 시각의 state로 계산된다.

snapshot은 사진이나 영화와 다르게 interactive하다.
event handler같이 input에 응답을 주는 로직들을 포함하기 때문이다.
그 결과로 버튼을 클릭하면 JSX에 click handler를 실행할 것이다.

Rendering 과정

1. React가 function을 호출한다. (React executing the function)
2. function이 JSX snapshot을 return한다. (Calculating the snapshot)
3. React가 snapshot에 맞는 screen으로 업데이트한다. (Updating the DOM tree)

state는 component의 memory이다. rendering에 따라서 사라지지 않는다.
component 외부에 React 그 자체에 살아있는 것이다.(as if on a shelf)
특정 render에 대해서 state snapshot을 제공한다.
결과적으로 UI snapshot을 return하는데 여기 JSX 안에는 props, event handler가 있다.
(variables, event handler는 re-render에서 survive하지 못한다.)

## State over time(Substitute method)

각 예시들을 살펴보자.
mentally substituting state 를 하면 주석처럼 된다.

```typescriptreact
// 아래는 결과적으로 +1만 된다.
export default function Counter() {
  const [number, setNumber] = useState(0);

  return (
    <>
      <h1>{number}</h1>
      <button
        onClick={() => {
          setNumber(number + 1); // setNumber(0 + 1)
          setNumber(number + 1); // setNumber(0 + 1)
          setNumber(number + 1); // setNumber(0 + 1)
        }}
      >
        +3
      </button>
    </>
  );
}
```

```typescriptreact
import { useState } from "react";

export default function Counter() {
  const [number, setNumber] = useState(0);

  return (
    <>
      <h1>{number}</h1>
      <button
        onClick={() => {
          setNumber(number + 5); // setNumber(0 + 5)
          alert(number); // alert(0)
        }}
      >
        +5
      </button>
    </>
  );
}
```

```typescriptreact
// setTimout을 하더라도 schedule에 들어갈때 이미 snapshot의 state를 사용한다.
import { useState } from 'react';

export default function Counter() {
  const [number, setNumber] = useState(0);

  return (
    <>
      <h1>{number}</h1>
      <button onClick={() => {
        setNumber(number + 5); // setNumber(0 + 5)
        setTimeout(() => {
          alert(number); // alert(0)
        }, 3000);
      }}>+5</button>
    </>
  )
```

한번의 render에 있는 event handler에서는 그때의 state가 고정(fixed)되어 사용된다.
(React keeps the state values "fixed" within one render's event handlers)
그래서 코드가 도는동안 state가 바뀌었는지 체크할 필요가 없는 것이다.

# Queueing a Series of State Updates

## React batches state updates

react는 event handler의 모든 코드가 실행될때까지 기다리고, state를 한번에 update 한다.
음식점의 웨이터가 주문을 받을때, 첫음식을 듣자마자 kitchen으로 달려가지 않을 것이다.
다른 테이블에서 주문이 있더라도, 현재 테이블에서 모든 주문(혹은 주문변경까지)을 다 들을때까지 기다릴 것이다.
이 방식덕분에 너무 많은 re-render가 발생하지 않는다.
이런 동작 방식을 batching이라고 부른다. 이것으로 React는 속도가 빨라지고, half-finished 같은 혼동이 없도록 해준다.

React는 여러개의 의도적인 이벤트(ex. Click 이벤트)를 batch 처리하지 않는다.
각각의 클릭은 분리되어 처리된다.
이런 동작은 첫번째 클릭이 Form을 disable시켜서 두번째 클릭에서 submit을 하지 않도록 해준다.

## Updating the same state multiple times before the next render

### 첫번째 예시

일반적이지 않지만 같은 state를 한번에 여러번 업데이트하고 싶으면 이렇게 할 수있다.
(function을 인자값으로 넘기는것이다.)

```typescriptreact
import { useState } from "react";

export default function Counter() {
  const [number, setNumber] = useState(0);

  return (
    <>
      <h1>{number}</h1>
      <button
        onClick={() => {
          setNumber((n) => n + 1);
          setNumber((n) => n + 1);
          setNumber((n) => n + 1);
        }}
      >
        +3
      </button>
    </>
  );
}
```

react는 이 함수들을 queue에 넣고 다음 render전에 순차적으로 처리하는데
이 return값은 updater의 인자값으로 들어가게 된다.

substitute method를 사용하면 이렇게 표현된다.

```
queued update | n |  returns
n => n + 1    | 0 |  0 + 1 = 1
n => n + 1    | 1 |  1 + 1 = 2
n => n + 1    | 2 |  2 + 1 = 3
```

### 두번째 예시

```typescriptreact
import { useState } from "react";

export default function Counter() {
  const [number, setNumber] = useState(0);

  return (
    <>
      <h1>{number}</h1>
      <button
        onClick={() => {
          setNumber(number + 5); // setNumber(0 + 5) 이므로 replace with 5 가 된다. (여기서는 n이 사용되지 않았다!)
          setNumber((n) => n + 1);
        }}
      >
        Increase the number
      </button>
    </>
  );
}
```

substitute method를 사용하면 이렇게 표현된다.

```
queued update  | n |  returns
replace with 5 | 0 |  5
n => n + 1     | 5 |  5 + 1 = 6
```

### 세번째 예시

```typescriptreact
import { useState } from "react";

export default function Counter() {
  const [number, setNumber] = useState(0);

  return (
    <>
      <h1>{number}</h1>
      <button
        onClick={() => {
          setNumber(number + 5); // replace with 5
          setNumber((n) => n + 1); // n => n + 1
          setNumber(42); // replace with 42
        }}
      >
        Increase the number
      </button>
    </>
  );
}
```

substitute method를 사용하면 이렇게 표현된다.

```
queued update   | n         |  returns
replace with 5  | 0(unused) |  5
n => n + 1      | 5         |  5 + 1 = 6
replace with 42 | 6(unused) |  42
```

## Naming conventions

updater function의 argument는 state variable의 첫글자로 만든다.
(혹은 state이름을 그대로 쓰기도 한다.)

```typescriptreact
setEnabled((e) => !e); // 줄여서 사용
setLastName((ln) => ln.reverse());
setFriendCount((fc) => fc * 2);
setEnabled((enabled) => !enabled); // 그대로 사용
setEnabled((prevEnabled) => !prevEnabled); // prefix를 붙이는 경우
```

# Updating Objects in State

multiple fields에 대해서는 single event handler를 사용하자

```typescriptreact
import { useState } from "react";

export default function Form() {
  const [person, setPerson] = useState({
    firstName: "Barbara",
    lastName: "Hepworth",
    email: "bhepworth@sculpture.com",
  });

  function handleChange(e) {
    setPerson({
      ...person,
      [e.target.name]: e.target.value,
    });
  }

  return (
    <>
      <label>
        First name:
        <input
          name="firstName"
          value={person.firstName}
          onChange={handleChange}
        />
      </label>
      <label>
        Last name:
        <input
          name="lastName"
          value={person.lastName}
          onChange={handleChange}
        />
      </label>
      <label>
        Email:
        <input name="email" value={person.email} onChange={handleChange} />
      </label>
      <p>
        {person.firstName} {person.lastName} ({person.email})
      </p>
    </>
  );
}
```

object는 실제로 nested가 된것이 아니다.
아래 경우를 보면 obj1은 obj2에만 nested되어 있는 구조가 아니다.
object를 pointing 하고 있는 것이다.

```typescriptreact
let obj1 = {
  title: "Blue Nana",
  city: "Hamburg",
  image: "https://i.imgur.com/Sd1AgUOm.jpg",
};

let obj2 = {
  name: "Niki de Saint Phalle",
  artwork: obj1,
};

let obj3 = {
  name: "Copycat",
  artwork: obj1,
};
```

Immer를 사용하자.
[Immer](https://github.com/immerjs/use-immer)는 `draft`라는 특별한 타입의 object를 제공한다. 이것은 Proxy로 동작하는데
실제로 내부에서는 완전히 새로운 object를 만들어서 전달해준다.

```typescriptreact
updatePerson((draft) => {
  draft.artwork.city = "Lagos";
});
```

# Updating Arrays in State

## Updating arrays without mutation

Mutation을 하지 않기 위해서 피하거나 선호해야하는 array operations

- avoid : push, unshift, pop, shift, splice, `arr[i] = ...` assignment, reverse, sort
- prefer : concat, `[...arr]`, filter, slice, map, copy the first array(for using reverse, sort)

prefer operation들을 활용하면 아래 예시들처럼 사용할 수 있다.

### Adding to an array

```typescriptreact
setArtists(
  // Replace the state
  [
    // with a new array
    ...artists, // that contains all the old items
    { id: nextId++, name: name }, // and one new item at the end
  ]
);
```

### Removing from an array

```typescriptreact
setArtists(artists.filter((a) => a.id !== artist.id));
```

### Transforming an array

```typescriptreact
let initialShapes = [
  { id: 0, type: 'circle', x: 50, y: 100 },
  { id: 1, type: 'square', x: 150, y: 100 },
  { id: 2, type: 'circle', x: 250, y: 100 },
];
(...)
function handleClick() {
  const nextShapes = shapes.map((shape) => {
    if (shape.type === "square") {
      // No change
      return shape;
    } else {
      // Return a new circle 50px below
      return {
        ...shape,
        y: shape.y + 50,
      };
    }
  });
  // Re-render with the new array
  setShapes(nextShapes);
}
```

### Replacing items in an array

```typescriptreact
function handleIncrementClick(index) {
  const nextCounters = counters.map((c, i) => {
    if (i === index) {
      // Increment the clicked counter
      return c + 1;
    } else {
      // The rest haven't changed
      return c;
    }
  });
  setCounters(nextCounters);
}
```

### Inserting into an array

```typescriptreact
function handleClick() {
  const insertAt = 1; // Could be any index
  const nextArtists = [
    // Items before the insertion point:
    ...artists.slice(0, insertAt),
    // New item:
    { id: nextId++, name: name },
    // Items after the insertion point:
    ...artists.slice(insertAt),
  ];
  setArtists(nextArtists);
  setName("");
}
```

### Making other changes to an array

```typescriptreact
function handleClick() {
  const nextList = [...list];
  nextList.reverse();
  setList(nextList);
}
```

## Updating objects inside arrays

array안에 object를 수정할때는 shallow copy를 주의해야한다.
아래 예시를 보면 객체의 값에 직접접근해서 수정하면 안된다.

```typescriptreact
// Don't
const myNextList = [...myList];
const artwork = myNextList.find((a) => a.id === artworkId);
artwork.seen = nextSeen; // Problem: mutates an existing item
setMyList(myNextList);

// Do
setMyList(
  myList.map((artwork) => {
    if (artwork.id === artworkId) {
      // Create a *new* object with changes
      return { ...artwork, seen: nextSeen };
    } else {
      // No changes
      return artwork;
    }
  })
);

// Immer
updateMyTodos((draft) => {
  const artwork = draft.find((a) => a.id === artworkId);
  artwork.seen = nextSeen;
});
```

직접 예시를 만들어봤다.
newList의 객체를 mutate하면, listA와 listB에 동시에 영향을 미친다.

```typescriptreact
import React, { useState } from "react";

const defaultList = [
  { id: 1, name: "Stefan" },
  { id: 2, name: "Cho" },
  { id: 3, name: "Dev" },
  { id: 4, name: "Steven" },
];

const isGood = true;

const Test = () => {
  const targetId = 3;
  const [listA, setListA] = useState(defaultList);
  const [listB, setListB] = useState(defaultList);

  const handleClickA__Bad = () => {
    const newList = [...listA];
    newList[targetId].name = "AAA";
    setListA(newList);
  };
  const handleClickB__Bad = () => {
    const newList = [...listB];
    newList[targetId].name = "BBB";
    setListB(newList);
  };

  const handleClickA__Good = () => {
    const newList = listA.map((item) => {
      if (item.id === targetId) {
        return { ...item, name: "AAA" };
      }
      return item;
    });
    setListA(newList);
  };
  const handleClickB__Good = () => {
    const newList = listB.map((item) => {
      if (item.id === targetId) {
        return { ...item, name: "BBB" };
      }
      return item;
    });
    setListB(newList);
  };

  return (
    <div>
      <h1>Test</h1>
      <ul>
        {listA.map((item) => (
          <li key={item.id}>{item.name}</li>
        ))}
      </ul>
      <button onClick={isGood ? handleClickA__Good : handleClickA__Bad}>
        Update
      </button>
      <ul>
        {listB.map((item) => (
          <li key={item.id}>{item.name}</li>
        ))}
      </ul>
      <button onClick={isGood ? handleClickB__Good : handleClickB__Bad}>
        Update
      </button>
    </div>
  );
};

export default Test;
```
