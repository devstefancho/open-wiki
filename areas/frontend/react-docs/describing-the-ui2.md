---
published: false
id: describing-the-ui2
slug: describing-the-ui2
title: Describing The Ui2
description: describing the ui2
tags: ["react-docs"]
categories: ["react-docs"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

# Your First Component

## Components: UI building blocks

React -> markup, CSS, javascript를 custom components로 combine해주는 것, 또한 reusable 하다.

## Defining a component

### JSX

React component는 단지 javascript 함수이다.
단, 반드시 capital letter로 시작해야한다.
markup이 return 되는 부분 syntax를 JSX라고 부른다.

```typescriptreact
export default function Gallery() {
  return (
    <section>
      <h1>Amazing</h1>
      <Profile />
    </section>
  );
}
```

## Using Component

section은 lowercase로 시작하므로 HTML tag인 것으로 React가 알고 있다.
Profile은 capital P로 시작하므로 Component를 사용하려는 의도를 React가 알고 있다.

### 명명법

Gallery를 parent component
Profile을 child 라고 부른다.

### Component defintion

Component는 최상위로 위치해라 (같은 파일에 두는건 상관없다.)
Component는 다른 component를 render할 수 있지만, definition을 nest하지마라. (성능상에 문제, 버그 발생할 수 있다.)

```typescriptreact
export default function Gallery() {
  // 🔴 Never define a component inside another component!
  function Profile() {
    // ...
  }
  // ...
}

export default function Gallery() {
  // ...
}

// ✅ Declare components at the top level
function Profile() {
  // ...
}
```

# Importing and Exporting Components

## The root component file

보통 CRA로 만들면 src/App.js가 "root component"일 것이다.
Nextjs처럼 file-based routing framework를 사용하면 root component는 page마다 다를것이다.

## Exporting and importing a component

export는 named export, default export 두개의 방식이 있다.
3가지 방식이 있다.

- one default export (보통 파일하나에 하나의 export가 있는 경우)
- multiple named exports
- named export(s) and one default export

뭐를 사용하던 상관없다. 다만 meaningful name을 사용하고,
다만 `export default () => {}` 처럼 이름이 없는건 discourage 이다. (debugging을 어렵게 만들기 때문)

import 할때는 './Gallery.js' or './Gallery' 둘중에서 아무거나 사용해도 되는데
'./Gallery.js' 방식이 [native ES Modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules)가 동작하는것에 가깝다

potential confusion을 줄이기 위해서 어떤팀들은 한개의 스타일을 정한다.

## Exporting and importing multiple components from the same file

# Writing Markup with JSX

JSX is a syntax extension for Javascript that lets you write HTML-like markup inside a Javascript file

## JSX: Putting markup into Javascript

rendering 로직이 group되어 있는 이유
과거에는 content(HTML), design(CSS), logic(Javascript)으로 분리되어 있었다.
현재는 logic이 content를 만들게 되면서 javascript안에 markup을 넣는게 효율적이게 되었다.
이것 때문에 JSX가 나온것인데, HTML보다 좀더 strict하고, dynamic information을 표현할 수 있다.
(strict 하다는 것은 close tag가 반드시 필요한 것으로 예를 들 수 있음)

JSX(syntax extension)와 React(javascript library)는 다른 concept 이다.
JSX를 사용하지 않더라도 아래처럼 React를 사용할 수 있다.

```javascript
import React from "react";

const App = () => {
  return React.createElement("div", null, "Hello, World!");
};

export default App;
```

## Converting HTML to JSX

jsx는 single parent tag로 감싸야한다.
jsx는 HTML처럼 보이지만, 사실은 javascript object로 변환되는 것이다.
function의 return은 하나의 object만 가능하다.
여러개의 object를 리턴하려면 array에 object들을 넣어서 return 해야한다.
이게 바로 하나의 tag로만 return이 가능한 이유이다.

## The Rules of JSX

jsx는 explict하게 close tag가 필요하다.

jsx에서 attribute는 javascript object의 key로 들어간다.
javascript에서는 `-`를 변수명으로 사용할 수 없다. 그래서 대부분의 attribute는 camelCase를 사용하게 된 것이다. (ex. `strokeWidth`)
그리고 class같이 reserved word 또한 사용할 수 없다. (ex. `className`)
다만 `aria-*`, `data-*`는 historical reason들로 인해서 HTML과 똑같이 사용한다.

# Javascript in JSX with Curly Braces

javascript 로직을 markup 사이에 넣고 싶은 경우가 있는데 그때 curly braces를 사용한다. (아래예시)
이것을 자바스크립트 창을 연다고 표현한다. (open a window to Javascript)
중괄호 사이에서는 표현식(expression)만 사용이 가능하다.

## Passing strings with quotes

markup 내에서 curly brace를 사용할 수 있다.
(attribute부분에서 `=` 다음에 사용)

```typescriptreact
export default function Avatar() {
  const avatar = "https://i.imgur.com/7vQD0fPs.jpg";
  const description = "Gregorio Y. Zara";
  return <img className="avatar" src={avatar} alt={description} />;
}
```

## Using curly braces: A window into the Javscript world

jsx내에서 curly brace를 사용할 수 있다.

```typescriptreact
<h1>{name}'s TODO</h1>
```

## Using "double curlies": CSS and other objects in JSX

obejct를 넘길때는 `{{}}`가 필요하다.

## More fun with Javascript objects and curly braces

하나의 object에 expression들을 넣어놓고 꺼내쓰는 방식도 있다.

```typescriptreact
const person = {
  name: "Gregorio Y. Zara",
  theme: {
    backgroundColor: "black",
    color: "pink",
  },
};

export default function TodoList() {
  return (
    <div style={person.theme}>
      <h1>{person.name}'s Todos</h1>
      <img
        className="avatar"
        src="https://i.imgur.com/7vQD0fPs.jpg"
        alt="Gregorio Y. Zara"
      />
      <ul>
        <li>Improve the videophone</li>
        <li>Prepare aeronautics lectures</li>
        <li>Work on the alcohol-fuelled engine</li>
      </ul>
    </div>
  );
}
```

# Passing Props to a Component

## Familiar props

props는 HTML의 attribute라고 생각하면 된다.
ReactDOM conforms to the [HTML standard](https://html.spec.whatwg.org/multipage/embedded-content.html#the-img-element)

## Passing props to a component

Parent Component는 props을 pass 하고
Child Component는 props를 read 한다.
React Component function은 props라는 single argument를 받는다. props는 object이다.
props를 read할 때는 destructing syntax를 사용하자.

## Specifying a default value for a prop

```typescriptreact
function Avatar({ person, size = 100 }) {
  // ...
}
```

default props는 missing, undefined(`size={undefined}`) 두가지 상황에서만 사용된다.
null이나 0을 pass하면 사용되지 않는다.

## Forwarding props with the JSX spread syntax

JSX에서 spread syntax로 props를 넘기는 것을 Forwarding props라고 한다.
반복적인 것을 줄일때 간결함을 주고자 할 때는 좋지만, 제한적으로 사용해야 한다.
모든 컴포넌트에 적용하면 뭔가 잘못될 수가 있다. Forwarding을 해야하는 상황은 컴포넌트를 나누거나 children으로 넘겨야한다는 신호이다.

## Passing JSX as children

children으로 넘기는 건 flexible pattern 으로 사용하고자 할때이다.
parent 입장에서는 뭐가 들어가는지 알 필요가 없다.
children prop은 visual wrappers(panel, grids 등)에서 종종 사용한다.

## How props change over time

props는 immutable 이다. (unchangeable)
컴포넌트가 사용자 상호작용이나 새로운 데이터에 대한 응답으로 props를 변경해야하는 경우,
props를 변경해야할 때는 parent component에게 새로운 객체 전달을 요청 해야한다.
(같은 값이라도 항상 새로운 props를 받는 것이기 때문에 랜더링을 최소화하기 위해서 memo 같은 것을 사용하는 듯)
props는 읽기 전용이다.(read-only snapshots)

parent component는 different props를 내려줄 것이고, old props는 다른곳으로 보내졌다가 javascript engine에 의해 memory에서 제거된다.
props는 읽기 전용이다. 즉 랜더링 할때마다 새로운 버전의 props를 받는 것이다.

# Conditional Rendering

JSX에서 조건에 따라서 랜더링해야하면 if statement, &&, ?: operator를 사용할 수 있음

## Conditionally returning JSX

if statement로 조건에 따라 다르게 랜더링하거나 아무것도 필요없다면 null을 리턴할 수도 있다.

## Conditionally including JSX

ternary operator로 DRY하게 만들 수 있다.
(반복하는게 잘못된것은 아니지만 이것으로 인해 유지보수가 힘들어질 수가 있다.)

className을 바꿔야하는 상황이라면 2번이 좋을 것이다.

```typescriptreact
// 1.
if (isPacked) {
  return <li className="item">{name} ✔</li>;
}
return <li className="item">{name}</li>;

// 2.
return <li className="item">{isPacked ? name + " ✔" : name}</li>;
```

위 2개의 컴포넌트는 완전같은가?
OOP로 생각하면 서로다른 두개로 return하는게 각각의 instance를 생성하기 때문에 다르다고 생각할 수 있으나,
JSX는 instance가 아니고 internal state를 갖고있지 않고, real DOM도 아니다. 그냥 blueprint일 뿐이기 때문에
두개는 완벽하게 동일하다.

과도한 nested conditional markup을 주의하라.
이런경우에는 child Component로 추출하는것을 고려해라.

## Logical AND operator (&&)

isPacked가 false이면 아무것도 표시하지 않는다.

```typescriptreact
return (
  <li className="item">
    {name} {isPacked && "✔"}
  </li>
);
```

&& 왼쪽에 number를 넣지마라, 0인경우 React는 0값을 그대로 표시할 것이다.

```typescriptreact
messageCount && <p>New Messages</p>; // X
messageCount > 0 && <p>New Messages</p>; // O
```

## Conditionally assigning JSX to a variable

짧게 쓰는 문법이 단순한 코드를 작성하는것을 방해할때 variable(let)과 if statement를 사용해라
let으로 사용해서 재할당해도 된다.

```typescriptreact
function Item({ name, isPacked }) {
  let itemContent = name;
  if (isPacked) {
    itemContent = name + " ✔";
  }
  return <li className="item">{itemContent}</li>;
}
```

# Rendering Lists

## Rendering data from arrays

표한하고자 하는걸 array 변수에 담고, map, filter를 사용해서 랜더링할 수 있다.

## Filtering arrays of items

예시에서는 map, filter를 한번에 chaining하지 않고, 변수로 할당해준다.

```typescriptreact
import { people } from "./data.js";
import { getImageUrl } from "./utils.js";

export default function List() {
  const chemists = people.filter((person) => person.profession === "chemist");
  const listItems = chemists.map((person) => (
    <li>
      <img src={getImageUrl(person)} alt={person.name} />
      <p>
        <b>{person.name}:</b>
        {" " + person.profession + " "}
        known for {person.accomplishment}
      </p>
    </li>
  ));
  return <ul>{listItems}</ul>;
}
```

## Keeping list items in order with key

key는 array items에서 inserted, deleted, moved 될 때 DOM을 정확하게 업데이트 하기위해서 중요한 요소이다.
key는 map을 돌릴때 만드는 것 보다 data에 포함시켜라 (Rather then generating keys on the fly, you should include them in your data)

```typescriptreact
export const people = [
  {
    id: 0, // Used in JSX as a key
    name: "Creola Katherine Johnson",
    profession: "mathematician",
    accomplishment: "spaceflight calculations",
    imageId: "MK3eW3A",
  },
  {
    id: 1, // Used in JSX as a key
    name: "Mario José Molina-Pasquel Henríquez",
    profession: "chemist",
    accomplishment: "discovery of Arctic ozone hole",
    imageId: "mynHUSa",
  },
  {
    id: 2, // Used in JSX as a key
    name: "Mohammad Abdus Salam",
    profession: "physicist",
    accomplishment: "electromagnetism theory",
    imageId: "bE7W1ji",
  },
];
```

여러 element를 묶어야하는 경우 `<></>`에는 key를 줄 수 없다.
이 때는 div로 묶는 것보다는 `<Fragment></Fragment>`로 명시적으로 사용해주는게 좋다.

어디서 key를 얻는가?

- DB에서 가져온 데이터라면 DB의 unique id를 사용
- Local 데이터라면 uuid로 생성

key 사용규칙

- key는 sibiling간에서만 unique하면 된다. (서로 다른 array에서 same key를 사용하는것은 괜찮음)
- key는 변경되거나 용도외에 사용하면 안된다. rendering할 때 생성하지 마라!

key에 대한 더 많은 정보
key를 사용할때 array의 index를 사용하는 경우가 있는데, 사실 react에서는 key값을 명시하지 않으면 내부적으로 그 값을 사용한다.
아이템의 순서는 매번 바뀔수 있다. (insert, delete, move에 의해서)
순서가 재 정렬되면 작은버그가 발생수 있다.
유사하게 키를 render할때 random값으로 생성하면 이전 key와 매치가 안된다.
이로인해서 component가 전체 다시 랜더링 되거나 DOM이 매번 새로 생성될수가 있다.
결과적으로 속도가 느려지게 되고 input 데이터가 갑자기 날라갈 수도 있다.
key는 props로 받아지지 않는다. key는 오직 React 자체의 힌트일 뿐이다.
만약 id가 필요하다면 별도의 prop(`<Profile key={id} userId={id} />`)으로 넘겨줘라

# Keeping Components Pure

## Purity: Components as formulas

pure function은 다음과 같은 특징을 갖는다.

- It minds its own business : 실행되기전에 있던 다른값들을 변경시키지 않음
- Same Inputs, same output : 같은 입력에는 같은 결과가 나옴

## Side Effects: (un)intended consequences

React Component는 무조건 JSX를 리턴하는것만 해야한다. 랜더링과 상관없는 위치에 있는 변수나 객체를 변경시키면 안된다.
prop에 의존해서만 JSX를 리턴해야한다.

StritMode란?
pure한것을 보장하기 위해서 컴포넌트를 두번 호출하게 된다.
두번호출하는 것으로 이러한 rule들을 위반했는지 찾을 수 있다.
Strict Mode는 production에는 아무 영향을 주지 않는다. 따라서 app을 느리게 만들지 않는다.
`<React.StrictMode>`로 root component를 감싸면 적용된다.

Local Mutation
컴포넌트가 랜더링 되는 동안 내부에서 variable과 object를 변경하는것은 괜찮다.
아래처럼 same render에서 component's little secret을 만드는것은 괜찮다.(이것을 local mutation이라고 부름)

```typescriptreact
function Cup({ guest }) {
  return <h2>Tea cup for guest #{guest}</h2>;
}

export default function TeaGathering() {
  let cups = [];
  for (let i = 1; i <= 12; i++) {
    cups.push(<Cup key={i} guest={i} />);
  }
  return cups;
}
```

## Where you can cause side effects

side effect는 updating screen, starting an animation, changing the data와 같은것들을 말한다.
리액트에서는 event handler가 side effect이다.
컴포넌트 안에 있지만 rendering될때 실행되지 않기 때문이다. 그래서 event handler는 pure할 필요가 없다.
당신이 모든 옵션을 알아보고 적합한 event handler를 찾을 수 없다면 마지막 대안으로 useEffect가 있다.
useEffect는 리액트에서 나중에(랜더링이후에) 실행하라고 말한다.

왜 purity가 중요한가? (keeping components pure unlocks the power of the React paradigm)

- 왜냐면 다른 여러가지 환경에서 컴포넌트가 실행되기 때문이다. 여러 사용자환경에서 서버상태에 상관없이 같은 입력이면 같은 결과가 나와야한다.
- rendering component를 제외하고 성능개선을 할 수있다. 왜냐면 pure한 컴포넌트면 같은입력 같은결과이기 때문에 캐시여부를 신경쓰지 않아도 된다.
- 만약 컴포넌트 tree를 생성하는 중간에 데이터가 바뀐다면 리액트는 랜더링을 바로 재시작할 수 있다.
  (나머지 랜더링을 다 하고 나서 데이터 바뀌어서 그때부터 랜더링을 다시한다면 시간낭비이다!)
