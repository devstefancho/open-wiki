---
published: false
id: describing-the-ui
slug: describing-the-ui
title: Describing The Ui
summary: React 컴포넌트 이해하기
toc: true
tags: ["react-docs"]
categories: ["react-docs"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

## React 컴포넌트 이해하기

### 컴포넌트 개요

React는 마크업, CSS, 자바스크립트를 재사용 가능한 사용자 정의 컴포넌트로 조합합니다.

#### JSX란 무엇인가?


JSX는 마크업이 반환되는 부분의 문법입니다. React 컴포넌트는 자바스크립트 함수에 불과합니다. 그러나 반드시 대문자로 시작해야 합니다.

```typescriptreact
export default function Gallery() {
  // Gallery를 parent component
  return (
    <section>
      <h1>Amazing</h1>
      <Profile /> {/* Profile을 child */}
    </section>
  );
}
```

JSX에서 'section'은 소문자로 시작하므로 HTML 태그라는 것을 React가 인식합니다. 
반면에 'Profile'은 대문자 P로 시작하므로 컴포넌트를 사용하려는 의도라는 것을 React가 인식합니다.

#### 컴포넌트 정의하기

- 최상위 위치에 두기: 컴포넌트는 최상위 위치에 두는 것이 좋습니다. 여러 컴포넌트를 같은 파일에 두는 것은 문제가 없습니다.
- 중첩하지 않기: 컴포넌트는 다른 컴포넌트를 렌더링할 수 있지만, 중첩하여 정의하지 않아야 합니다. 이는 성능 문제와 버그를 유발할 수 있습니다.

```typescriptreact
export default function Gallery() {
  // 🔴 컴포넌트내에서 Nesting 하여 정의하지 않기
  function Profile() {
    // ...
  }
  // ...
}

export default function Gallery() {
  // ...
}

// ✅ Top Level에서 컴포넌트 정의하기
function Profile() {
  // ...
}
```

## 컴포넌트 Import와 Export

### 루트 컴포넌트란?

- CRA: 보통 CRA로 프로젝트를 생성하면 src/App.js가 "루트 컴포넌트"가 됩니다.
- Nextjs: Nextjs와 같이 파일 기반 라우팅 프레임워크를 사용하면, 루트 컴포넌트는 페이지마다 달라집니다.

### Export 방식

Export에는 크게 두 가지 종류가 있습니다.
- named export, default export

export 하는 패턴은 다음과 같습니다.

- 하나의 default export (보통 파일하나에 하나의 export가 있는 경우)
- 여러 named exports
- 하나 이상의 named export와 하나의 default export

어떤 패턴을 사용해야 할까요?
- 사용하는 패턴은 팀이나 프로젝트에 따라 달라집니다. 이는 개인의 선호나 프로젝트의 요구 사항에 따라 결정됩니다.
- 혼동을 줄이기 위해 일부 팀들은 하나의 스타일을 정하곤 합니다.
- 이름이 의미있게 지어지는 것이 중요합니다.
- export default () => {}와 같은 이름이 없는 함수는 권장되지 않습니다. 이는 디버깅을 어렵게 만들기 때문입니다.

### Import 방식

import 할때는 './Gallery.js' or './Gallery' 둘중에서 아무거나 사용해도 되는데
'./Gallery.js' 방식이 [native ES Modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules)가 동작하는것에 가깝습니다.


## JSX 활용하기

JSX는 Javascript의 syntax extension(문법 확장)입니다.
Javascript 내에서 HTML와 유사하게 작성할 수 있습니다.

### Javascript에서 마크업 사용하기

과거에는 내용(HTML), 디자인(CSS), 로직(JavaScript)이 분리되어 있었습니다. 
하지만 지금은 로직이 내용을 생성하게 되면서 JavaScript 내에 마크업을 넣는 것이 더 효율적이게 되었습니다. 
이 때문에 JSX가 등장하게 되었는데, JSX는 HTML보다 엄격하며 동적 정보를 표현할 수 있습니다. 
(예: JSX는 닫는 태그를 반드시 필요로 합니다.)

React는 JSX는 어떤 관계일까?
- JSX(syntax extension)와 React(javascript library)는 별개의 개념입니다.
- JSX를 사용하지 않더라도 아래와 같이 React를 사용할 수 있습니다.

```javascript
import React from "react";

const App = () => {
  return React.createElement("div", null, "Hello, World!");
};

export default App;
```

### HTML을 JSX로 변환하기

JSX는 단일 부모 태그를 필요로 합니다. 
이는 HTML처럼 보이지만 실제로는 JavaScript 객체로 변환되기 때문입니다. 
함수의 반환값은 하나의 객체만 가능합니다. 여러 개의 객체를 반환하려면 객체들을 배열에 넣어서 반환해야 합니다. 
이것이 바로 JSX에서 하나의 태그만 반환할 수 있는 이유입니다.

### JSX 규칙

- 닫는 태그가 필요: JSX는 닫는 태그를 반드시 필요로 합니다.
- 변수명에 제한이 있음: JavaScript에서는 -를 변수명으로 사용할 수 없습니다. 그래서 대부분의 속성은 camelCase를 사용합니다. (예: strokeWidth) 또한, 예약어인 'class'도 사용할수 없습니다. (예: className) 그러나 `aria-*`, `data-*`와 같은 속성은 역사적인 이유로 HTML과 동일하게 사용합니다.

## `{}`를 사용하는 JSX

curly braces`{}`는 언제 사용할까요?
- javascript 로직을 markup 사이에 넣고 싶은 경우
- 이것을 자바스크립트 창을 연다고 표현합니다. ("open a window to Javascript")
- `{}` 사이에서는 표현식(expression)만 사용할 수 있습니다.
- markup의 attribute 부분에서 `{}` 사용할 수 있습니다. (attribute부분에서 `=` 다음에 사용)

```typescriptreact
export default function Avatar() {
  const avatar = "https://i.imgur.com/7vQD0fPs.jpg";
  const description = "Gregorio Y. Zara";
  // attribute에 사용하기
  return <img className="avatar" src={avatar} alt={description} />;
}
```

```typescriptreact
// tag 내에서 사용하기
<h1>{name}'s TODO</h1>
```


object를 JSX에서 사용하는 경우는 어떨까요?
- `{{}}` 형태로 사용하게 됩니다.
- 하나의 object에 expression들을 넣어놓고 꺼내쓰는 방식이 있습니다.

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

## 컴포넌트에서 Props 사용하기

### props란?

props는 HTML의 attribute와 대응됩니다.
그리고 ReactDOM [HTML standard](https://html.spec.whatwg.org/multipage/embedded-content.html#the-img-element)을 따릅니다.

### 컴포넌트에 props 넘기기

props의 데이터 타입은 object 입니다. 함수형 컴포넌트는 단 하나의 argument를 받습니다. (props를 read할 때는 destructing syntax를 주로 사용)
부모 컴포넌트는 props을 넘기고, 자식 컴포넌트는 props를 읽습니다.

### props의 기본값

```typescriptreact
// 기본값 적용하기
function Avatar({ person, size = 100 }) {
  // ...
}
```

기본값이 적용되는 경우는 missing (props가 누락된 경우)와 `undefined`를 넣는 경우입니다. (예를 들어, `size={undefined}`) 
`null`이나 `0`을 pass하는 경우는 기본값이 적용되지 않습니다.

### spread syntax(`...`)로 props 넘기기

JSX에서 spread syntax로 props를 넘기는 것을 Forwarding props 이라고 합니다.

spread syntax 사용시 유의사항은 - 반복적인 것을 줄일때 간결함을 주고자 할 때는 좋지만, 제한적으로 사용해야 합니다.
Forwarding을 해야하는 상황은 컴포넌트를 나누거나 children으로 넘겨야한다는 신호입니다.

### children props 사용하기

children props는 부모 컴포넌트 입장에서는 뭐가 들어가는지 알 필요가 없기 때문에 flexible pattern 이다.
children prop은 visual wrappers(panel, grids 등)에서 종종 사용한다.

### props는 어떻게 변하는가?

특정 시점의 props값은 불변입니다.(즉, immutable, unchangeable)
props는 읽기 전용(read-only snapshots)입니다. props 변경이 필요한 경우 항상 parent로부터 새로운 객체전달을 받습니다.
즉 랜더링 할때마다 새로운 버전의 props를 받게 됩니다.
부모 컴포넌트는 항상 다른 props를 내려줄 것이고, old props는 나중에 자바스크립트 엔진에 의해 memory에서 제거됨

## 조건부 렌더링(Rendering)

JSX에서 조건에 따른 Rendering을 하는 방법으로는 `if statement`, `&&`, `?:` operator가 있습니다.
ternary operator는 코드를 DRY하게 만들어 줍니다. 단 과도하게 사용하면 유지보수가 어려워질 수 있습니다.
아무것도 필요없다면 null을 리턴하면 됩니다.

```typescriptreact
// 아래 2개 컴포넌트는 완전히 동일함
// className을 바꿔야하는 상황이라면 2번이 Good
// 1.
if (isPacked) {
  return <li className="item">{name} ✔</li>;
}
return <li className="item">{name}</li>;

// 2.
return <li className="item">{isPacked ? name + " ✔" : name}</li>;
```

OOP로 생각하면 서로다른 두개로 return하는게 각각의 instance를 생성하기 때문에 다르다고 생각할 수 있으나,
각각의 JSX는 instance가 아니며, internal state를 갖고있지 않고, real DOM도 아닙니다. 그냥 blueprint일 뿐입니다.


조건부 markup에서 주의할 점은 과도한 중첩은 하지 않도록 합니다.
이 경우 child 컴포넌트로 추출하는것을 고려할 수 있습니다.

### 논리연산자 AND(`&&`)

일반적인 사용방법으로는 아래와 같습니다.
```typescriptreact
// isPacked가 false이면 아무것도 표시하지 않는다.
return (
  <li className="item">
    {name} {isPacked && "✔"}
  </li>
);
```

`&&` 왼쪽에 number를 넣으면, 0인경우 React는 0값을 그대로 표시할 것이기 때문에 주의가 필요합니다.

```typescriptreact
messageCount && <p>New Messages</p>; // X
messageCount > 0 && <p>New Messages</p>; // O
```

### let을 사용한 값 할당

- 짧게 쓰는 문법이 단순한 코드를 작성하는것을 방해할때 variable(`let`)과 if statement를 사용할 수 있습니다.

```typescriptreact
function Item({ name, isPacked }) {
  let itemContent = name;
  if (isPacked) {
    itemContent = name + " ✔";
  }
  return <li className="item">{itemContent}</li>;
}
```

## List 렌더링하기

- 표현하고자 하는걸 array 변수에 담고, map, filter를 사용해서 랜더링할 수 있습니다.
- 예시에서는 map, filter를 한번에 chaining하지 않고, 변수로 할당해줍니다.

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

### key를 사용하여 list내에서 item 위치 유지하기

- key는 array items에서 inserted, deleted, moved 될 때 DOM을 정확하게 업데이트 하기위해서 중요한 요소입니다.
- key는 map을 실행할때 만들지 말고 data에 있는 값을 사용하도록 합시다.

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

key를 위한 Fragment 활용하기
- 여러 element를 묶어야하는 경우 `<></>`에는 key 사용불가합니다.
- div로 묶는 것보다는 `<Fragment></Fragment>`로 명시적으로 사용합시다.

어떤값을 key로 사용하는가?
- DB에서 가져온 데이터라면 DB의 unique id를 사용합니다.
- Local 데이터라면 uuid로 생성합니다.

key 사용규칙
- key는 sibiling간에 유일값이어야 합니다. (서로 다른 array에서 same key를 사용하는것은 괜찮음)
- key는 변경되거나 용도외에 사용하면 안됩니다. (rendering할 때 생성하지 마라!)

key에 대한 추가적인 내용들
- key를 사용할때 array의 index를 사용하는 경우가 있는데, 사실 react에서는 key값을 명시하지 않으면 내부적으로 그 값을 사용한다.
- 아이템의 순서는 매번 바뀔수 있다. (insert, delete, move에 의해서)
- 순서가 재 정렬되면 작은버그가 발생수 있다.
- 유사하게 키를 render할때 random값으로 생성하면 이전 key와 매치가 안된다.
- 이로인해서 component가 전체 다시 랜더링 되거나 DOM이 매번 새로 생성될수가 있다.
- 결과적으로 속도가 느려지게 되고 input 데이터가 갑자기 날라갈 수도 있다.
- key는 props로 받아지지 않는다. key는 오직 React 자체의 힌트일 뿐이다.
- 만약 id가 필요하다면 별도의 prop(`<Profile key={id} userId={id} />`)으로 넘겨줘라

## 컴포넌트 순수성(purity)

pure function은 다음과 같은 특징을 갖습니다.
- 실행되기전에 있던 다른값들을 변경시키지 않습니다. (It minds its own business)
- 같은 입력에는 같은 결과가 나와야 합니다. (Same Inputs, same output)


### Side Effects

- React Component는 무조건 JSX를 리턴하는것만 해야 합니다.
- 랜더링과 상관없는 위치에 있는 변수나 객체를 변경시키면 안됩니다.

local mutation의 특징
- 순수성 지키지 않아도 됩니다.
- 컴포넌트가 랜더링 되는 동안, 내부에서 variable과 object를 변경하는것은 괜찮습니다.
- 아래처럼 같은 render내에서 컴포넌트의 작은비밀(component's little secret)을 만드는것은 괜찮습니다.

```typescriptreact
function Cup({ guest }) {
  return <h2>Tea cup for guest #{guest}</h2>;
}

export default function TeaGathering() {
  let cups = [];
  for (let i = 1; i <= 12; i++) {
    cups.push(<Cup key={i} guest={i} />); // 랜더링 되는동안 내부 variable의 값이 변경되는 것은 괜찮음
  }
  return cups;
}
```

### StritMode 사용법
- `<React.StrictMode>`로 root component를 감싸기

StrictMode에서 컴포넌트 두번 호출
- pure한것을 보장하기 위해서 컴포넌트를 두번 호출하게 됩니다.
- 두번호출하는 것으로 이러한 rule들을 위반했는지 찾을 수 있습니다.
- Strict Mode는 production에는 아무 영향을 주지 않는다. 따라서 app을 느리게 만들지 않습니다.

### Side Effects는 무엇인가?

일반적 예시
- updating screen, starting an animation, changing the data와 같은것들을 말합니다.

React에서 예시
- event handler는 side effect이다. 컴포넌트 안에 있지만 rendering될때 실행되지 않기 때문에 pure하지 않아도 됩니다.
- 모든 옵션을 알아보고 적합한 event handler를 찾을 수 없다면 side effect의 마지막 대안으로 useEffect가 있습니다.

왜 purity가 중요한가?
- React가 추구하는 패러다임을 유지하기 위해서 순수성을 지켜야 합니다.
- 이를 통해 rendering component를 제외하고 성능개선을 할 수 있습니다. pure한 컴포넌트면 같은입력 같은결과이기 때문에 캐시여부를 신경쓰지 않아도 됩니다.
- 여러 사용자환경에서 서버상태에 상관없이 같은 입력이면 같은 결과가 나와야 합니다.

