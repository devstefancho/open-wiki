---
published: false
id: managing-state
slug: managing-state
title: Managing State
summary: managing state
toc: true
tags: ["react-docs"]
categories: ["react-docs"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

# Reacting to Input with State

## How declarative UI compares to imperative

declarative UI vs Imperative UI
(선언형 vs 명령형)

차를 타고가는데 동승자가 길을 가르쳐준다고 생각해보자
명령형은 좌회전, 우회전 등 모든 경로를 다 알려주는것이고,
선언형은 어떤 장소로 가면 되는건지 알려주는 것이다. (사사건건 다 알려주지 않음)

UI에서 각각의 동작에 대해서 말하는 것이 아니라, 무엇을 하고자 하는가에 초점을 맞춰야 한다.

## Thinking about UI declaratively

UI를 아래 방식으로 재구성할 수 있다.
1. 컴포넌트의 다른 비주얼 상태를 정의한다.
2. 상태를 변경하는게 무엇인지 정한다.
3. useState로 state를 표현한다.
4. 불필요한 상태를 제거한다.
5. 이벤트 핸들러와 state를 연결한다.

### Step 1: Identify your component's different visual states

computer science에서는 state machine이라는 것을 들어봤을 것이다.
여러가지 state가 있을때, 디자이너와 함께 일한다면 서로다른 상태에 대해서 디자인(visual) mockup을 만들것이다.

visualize 가능한 서로 다른 state들은 이렇게 생각할 수 있다.
- Empty: Form has a disabled “Submit” button.
- Typing: Form has an enabled “Submit” button.
- Submitting: Form is completely disabled. Spinner is shown.
- Success: “Thank you” message is shown instead of a form.
- Error: Same as Typing state, but with an extra error message.

이런 상태들을 통해서 mockup을 먼저 만들어본다.
naming은 중요하지 않다. 각 상태값을 바꿔보면서 원하는 화면이 나오는지 확인해본다.
(여러가지 상태들을 페이지들에 쭉 나열해서 보는것을 living styleguides 또는 storybooks이라고 한다.)

### Step 2: Determine what triggers those state changes

상태를 업데이트 하는방식은 2가지가 있다.
1. Human Inputs (버튼클릭, 필드입력, 링크이동)
2. Computer Inputs (네트워크 응답이 도착했을때, 타임아웃일때, 이미지가 로드될때)

이러한 경우에 state를 새로 정해서 UI를 업데이트 해야한다.
각각의 input이 어떤 state를 변경시키는지 생각해본다.
이 flow를 visualize하기 위해서 label을 달아둔 동그라미(state)들을 화살표로 연결해서 그려본다.
많은 flow를 스케치할 수 있고, 구현하기 전에 버그를 가려낼 수 있다.
(아래에서 괄호안에 있는게 상태이다.)

```
(Empty) --Start typing--> (Typing) --Press submit--> (Submitting)
--Network Error--> (Error)
--Network Success--> (Success)
```

### Step 3: Represent the state in memory with useState

useState로 state를 만들어볼 시간이다. 이때 중요한 key는 simplicity이다.
moving piece를 최소화 해야한다. (복잡할수록 버그가 많이 생긴다!)

초반에는 모든 경우를 커버할 수 있을정도의 충분한 숫자의 state를 만들어봐라
(당연히 이게 베스트는 아니다, 리팩토링을 할 것이다!)

### Step 4: Remove any non-essential state variables

중복을 제거하고 불필요한 state를 삭제한다.
여기서 목표는 UI상에서 발생할 수 없는 케이스를 방지하는 것이다.
(예를 들어 에러메시지와 input disable은 동시에 일어날 수 없다, 동시에 일어나면 사용자가 에러를 수정할 수 없기 때문에)

1. 모순이 있는가? (Does this state cause a paradox?)

- isTyping과 isSubmitting의 true는 공존할 수 없다. 2\*2 = 4로 총4가지 경우가 있는데 가능한 경우는 3가지이다.
  따라서 'typing', 'submitting', 'success' 세가지 값을 갖고 있는 status를 state로 만들어서 사용하도록 한다.

2. 다른 state에 같은 정보가 있는가? (Is the same information available in another state variable already?)

- isEmpty와 isTyping은 동시에 true일 수 없다. state를 분리함으로써, sync가 맞지 않는 리스크가 발생한다.
- 따라서 isEmpty는 answer.length === 0 으로 체크할 수 있으므로 제거한다.

3. inverse를 해서 같은 정보의 state로 표현할 수 있는가? (Can you get the same information from the inverse of another state variable?)

- isError는 error가 있으면 필요하지 않다. error !== null로 대신 사용한다.

4. reducer를 활용해서 impossible한 state들을 제거한다.

- https://react.dev/learn/extracting-state-logic-into-a-reducer

### Step 5: Connect the event handlers to set state

state를 업데이트할 이벤트 핸들러를 만들어준다.

### 예시

backgroundClassName과 pictureClassName을 변수빼서 처리한다.
(나는 className 곳곳에 삼항연산자를 넣었는데 이게 훨씬 깔끔해보임)

```typescriptreact
import { useState } from "react";

export default function Picture() {
  const [isActive, setIsActive] = useState(false);

  let backgroundClassName = "background";
  let pictureClassName = "picture";
  if (isActive) {
    pictureClassName += " picture--active";
  } else {
    backgroundClassName += " background--active";
  }

  return (
    <div className={backgroundClassName} onClick={() => setIsActive(false)}>
      <img
        onClick={(e) => {
          e.stopPropagation();
          setIsActive(true);
        }}
        className={pictureClassName}
        alt="Rainbow houses in Kampung Pelangi, Indonesia"
        src="https://i.imgur.com/5qwVYb1.jpeg"
      />
    </div>
  );
}
```

또는 아예 if문을 분리할 수도 있다. 이 경우에는 JSX tree 구조가 일치하도록 신경써야한다.

```typescriptreact
import { useState } from "react";

export default function Picture() {
  const [isActive, setIsActive] = useState(false);
  if (isActive) {
    return (
      <div className="background" onClick={() => setIsActive(false)}>
        <img
          className="picture picture--active"
          alt="Rainbow houses in Kampung Pelangi, Indonesia"
          src="https://i.imgur.com/5qwVYb1.jpeg"
          onClick={(e) => e.stopPropagation()}
        />
      </div>
    );
  }
  return (
    <div className="background background--active">
      <img
        className="picture"
        alt="Rainbow houses in Kampung Pelangi, Indonesia"
        src="https://i.imgur.com/5qwVYb1.jpeg"
        onClick={() => setIsActive(true)}
      />
    </div>
  );
}
```

# Choosing the State Structure

## Principles for structuring state

component를 작성할때는 몇개의 state를 사용할 것인가 데이터 shape은 무엇을 사용할 것인가 고려해야한다.
완전 최적화는 아니더라도 몇가지 가이드는 준수하는 것이 좋다.
아래 5가지의 Principles에 대해서 설명한다.

## Group related state

연관된 state들은 하나로 묶어라
한번에 2~3개의 state를 동시에 업데이트 해야하는 상황이라면 하나로 묶는것을 고려해라
하나의 필드만 업데이트 하게 되면 sync가 맞지 않게 되는데, 이런 실수를 방지하기 위함이다.
(복잡할수록 동시에 업데이트 하는것을 놓치기 쉽다.)

```typescriptreact
const [position, setPosition] = useState({ x: 0, y: 0 });
```

## Avoid contradictions in state

불가능한 state가 나오는것을 제거해라
isSending, isSent 두개의 state를 사용하면 둘다 true인 경우가 나오는데, 이건 불가능한 케이스임
이런것을 제거하기 위해서는 하나의 status로 묶는게 좋다.
status를 'typing'(initial), 'sending', 'sent' 이런식으로 만들면 됨

```typescriptreact
const isSending = status === "sending";
const isSent = status === "sent";
```

## Avoid redundant state

필요없는 state를 없애라
랜더링중에 다른 state에 의해 계산될수 있는 state는 필요없는 state이다.
예를들어, firstName, lastName이 있으면 fullName state는 필요가 없다. rendering중에 계산하면 된다.

## Don't mirror props in state

props를 state로 넘기지 마라
useState의 초기값은 한번만 실행되므로, props가 변경되어도 useState의 초기값은 바뀌지 않는다.

## Avoid duplication in state

state의 중복을 피해라
만약 객체가 들어있는 배열으로 items(`Array<Item>`)라는 state가 있으면
selectedItems(`Item`)같은걸 만들면 안된다.
결론은 selectedItemId(`Item['id']`)를 state로 만들어야한다. selectedItemId와 같이 id만 뽑아야한다.
selectedItem으로 만들면 items에 있는 객체 하나를 넣게 되는건데, 이건 state가 중복되는 것이다.
state 중복으로 인해서 sync가 맞지 않는 문제가 발생할 수 있다.

```typescriptreact
const selectedItem = items.find((item) => item.id === selectedId);
return (
    ...
)
```

위와 같이 id만 저장하고 실제 선택된 객체는 rendering중에 계산해야지 정확하게 원하는 결과로 랜더링이 된다.
(You didn’t need to hold the selected item in state, because only the selected ID is essential. The rest could be calculated during render.)

## Avoid deeply nested state

너무 깊은 구조는 피해라
flat([normalize](https://learn.microsoft.com/en-us/office/troubleshoot/access/database-normalization-description)) 시켜라

아래 구조처럼 childIds로 id만 참고해라

```typescriptreact
export const initialTravelPlan = {
  0: {
    id: 0,
    title: "(Root)",
    childIds: [1, 43, 47],
  },
  1: {
    id: 1,
    title: "Earth",
    childIds: [2, 10, 19, 27, 35],
  },
  2: {
    id: 2,
    title: "Africa",
    childIds: [3, 4, 5, 6, 7, 8, 9],
  },
  3: {
    id: 3,
    title: "Botswana",
    childIds: [],
  },
  4: {
    id: 4,
    title: "Egypt",
    childIds: [],
  },
};
```

## Recap

- If two state variables always update together, consider merging them into one.
- Choose your state variables carefully to avoid creating “impossible” states.
- Structure your state in a way that reduces the chances that you’ll make a mistake updating it.
- Avoid redundant and duplicate state so that you don’t need to keep it in sync.
- Don’t put props into state unless you specifically want to prevent updates.
- For UI patterns like selection, keep ID or index in state instead of the object itself.
- If updating deeply nested state is complicated, try flattening it.

# Sharing State Between Components

공통 parent로 state를 올리는 것을 'lifting state up'이라고 한다.

## Lifting state up by example

단계

1. child component에서 state를 제거한다.
2. common parent로부터 hardcoded data를 넣는다.
3. common parent에 state를 추가한다.

### Controlled vs Uncontrolled

parent가 child에 영향을 줄 수 없는것을 `Uncontrolled`라고 한다.
`Controlled`가 되었다는건 local state보다는 props에 의해 driven 된다는 것을 의미한다.
Uncontrolled Component는 configuration이 덜 필요하므로 사용하기가 편리하다.
하지만 여러가지를 coordinate하는 경우에는 유연성이 좋지못하다.
Controlled는 max flexible하다. 하지만 fully configuration이 필요하다.
Controlled와 Uncontrolled는 strict tech terms은 아니지만,
(각 컴포넌트를 작성할 때 local state와 props가 mixed 되므로)
컴포넌트를 디자인할때 어떻게 컴포넌트를 디자인했는지에 대해서 논의할 때 유용한 용어이다.

## A single source of truth for each state

tree의 가장하단에 있는 컴포넌트를 leaf Component라고 한다.

### single source of truth

유니크한 state들에 대해서 어떤 컴포넌트에 그것을 둘것인지 선택해야한다.
이런 원칙을 single source of truth라고 한다.
이것은 모든 state들이 한곳에 있어야한다는 것을 의미하는 것은 아니다.
하지만 특정한 state(information)는 특정한 component가 갖고 있어야 한다.
state를 중복으로 생성하기 보다는, lift up을 해서 필요한 children에 내려줘야한다.

# Preserving and Resetting State

## The UI tree

Browser에서는 많은 tree구조가 있다.
[DOM](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction)은 HTML elements를 표현한다.
[CSSOM](https://developer.mozilla.org/en-US/docs/Web/API/CSS_Object_Model)은 CSS를 표현한다.
[Accessiblility tree](https://developer.mozilla.org/en-US/docs/Glossary/Accessibility_tree)라는 것도 있다.

React를 JSX로부터 UI tree를 만든다.
그리고 React DOM은 UI tree에 맞게 browser DOM을 업데이트한다.
(React Native는 mobile platform에 맞는 element로 변환한다.)

## State is tied to a position in the tree

state는 tree의 위치에 얽혀있다.
state는 component안에 살아있다고 생각할수도 있다. 하지만 사실 state는 React 안에서 잡고 있다.
React는 각각의 state를 UI tree에서의 위치 기반으로 정확한 컴포넌트에 연관시키고 있다.

[codesandbox 예시](https://codesandbox.io/embed/react-remember-state-by-position-p7qlpi?fontsize=14&hidenavigation=1&theme=dark)
(내가 만든 예시를 보면 충격적이게도 BuggyCounter에서는 click 버튼을 눌렀을 때, 중간에 있는 Counter 값이 유지가 된다. )

## Same component at the same position preserves state

React는 "the same component at the same position" 조건에서 state를 유지시킨다.
(React는 UI tree 구조에서 그 자리에 랜더링이 계속 되어있는한 state가 유지된다.)
만약 그 자리에서 컴포넌트가 제거가 되거나 다른 컴포넌트가 랜더링 된다면 state를 버린다(discard)

기억해야하는 것은 UI tree에서의 위치(position)이라는 것이다. 이것은 JSX markup 코드에서의 위치을 말하는 것이 아니다.

## Different components at the same position reset state

만약 re-render간에 state를 유지하고 싶다면, tree의 구조(structure)이 "match up" 되어야 한다.
만약 tree 구조가 다르면, state는 destroyed 된다

이러한 이유로 nest component function definition를 해서는 안된다.
(새로운 랜더링이 일어날때마다 안쪽에 nest 되어있는 새로운 함수가 생성되고, 이는 다른 컴포넌트이기 때문이다.)

## Resetting state at the same position

state를 reset 하는방법은 2가지가 있다.

1. Render components in different positions

Counter가 isPlayerA에 따라 위치가 다르다. (사실 이건 정확하게 이해가 안되긴함 UI Tree상에서는 같은거 아닌가 싶은 느낌)
(This solution is convenient when you only have a few independent components rendered in the same place.
In this example, you only have two, so it’s not a hassle to render both separately in the JSX.)

```typescriptreact
export default function Scoreboard() {
  const [isPlayerA, setIsPlayerA] = useState(true);
  return (
    <div>
      {/* state reset이 된다. */}
      {isPlayerA && <Counter person="Taylor" />}
      {!isPlayerA && <Counter person="Sarah" />}

      {/* state reset이 안된다. */}
      {/* {isPlayerA ? <Counter person="Taylor" /> : <Counter person="Sarah" />} */}
      <button
        onClick={() => {
          setIsPlayerA(!isPlayerA);
        }}
      >
        Next player!
      </button>
    </div>
  );
}
```

2. Give each component an explicit identity with key

key는 list에서만 사용하는게 아니다!
어떤 컴포넌트간에서 구분을 하기위해서 사용할 수 있다.
key는 global하게 unique한게 아니다. parent 기준으로 특정 위치에서만 unique하면 된다.

```typescriptreact
export default function Scoreboard() {
  const [isPlayerA, setIsPlayerA] = useState(true);
  return (
    <div>
      {isPlayerA ? (
        <Counter key="Taylor" person="Taylor" />
      ) : (
        <Counter key="Sarah" person="Sarah" />
      )}
      <button
        onClick={() => {
          setIsPlayerA(!isPlayerA);
        }}
      >
        Next player!
      </button>
    </div>
  );
}
```

아래처럼 Chat 화면을 사용자에 따라 reset 하기위해서 key를 줄 수 있다.

```typescriptreact
import { useState } from "react";
import Chat from "./Chat.js";
import ContactList from "./ContactList.js";

export default function Messenger() {
  const [to, setTo] = useState(contacts[0]);
  return (
    <div>
      {/* 새로운 contact(사용자)를 선택할 때마다 to 값이 바뀐다. */}
      <ContactList
        contacts={contacts}
        selectedContact={to}
        onSelect={(contact) => setTo(contact)}
      />
      {/* key가 변하므로 Chat이 reset 된다. */}
      <Chat key={to.id} contact={to} />
    </div>
  );
}

const contacts = [
  { id: 0, name: "Taylor", email: "taylor@mail.com" },
  { id: 1, name: "Alice", email: "alice@mail.com" },
  { id: 2, name: "Bob", email: "bob@mail.com" },
];
```

위와 같은 경우에서는 Chat이 reset되지만, 실제 Chat app에서는 input 값을 복구(recover)하고 싶을수가 있다.
이 경우에는 아래 방식으로 처리하면 된다.

1. CSS로 chat을 숨긴다. 이 경우는 tree에서 제거되는게 아니라서 local state가 유지가 된다.
   simple한 UI에서는 이런방식이 괜찮다. 하지만 hidden tree가 많을수록 갖고있는 DOM이 많아서 app은 느려질것이다.

2. lift state up을 해서 state를 parent에서 유지하고 있는 방식이 있다.
   이 방식은 children component가 제거되더라도 state가 유지될 수가 있다. 가장 일반적인 solution이다.

3. React가 아닌 다른 source를 사용하는 방식이 있다. localStroage 같은것을 사용할 수 있다.

- [이미지 관련해서 로딩예시](https://codesandbox.io/s/preserving-and-resetting-state-clear-an-image-while-its-loading-m43rvn?file=/App.js)
  - image에 key가 없으면 loading이 될 동안 이전 이미지가 그대로 떠 있으므로, 텍스트와 이미지의 불일치가 발생한다. 이것을 해결하려면 key를 사용하면 된다.

# Extracting State Logic into a Reducer

## Consolidate state logic with a reducer

component가 복잡하게 커지면 한눈에 state가 어떻게 업데이트 되는지 확인하기 어렵다.
이런경우 Reducer를 통해서 state를 핸들링할 수 있다.
useState를 useReducer로 migrate 하는 과정

### 1. state를 setting하는 로직을 dispatch action 하는것으로 변경

```typescriptreact
function handleAddTask(text) {
  setTasks([
    ...tasks,
    {
      id: nextId++,
      text: text,
      done: false,
    },
  ]);
}
// 아래처럼 변경
function handleAddTask(text) {
  dispatch({
    // dispatch에 pass한 object를 "action"이라고 부른다.
    type: "added",
    id: nextId++,
    text: text,
  });
}
```

일반적인 javascript object와 같지만, 최소한의 정보만을 포함해야한다.

### 2. reducer function 작성

아래처럼 reducer 함수를 작성한다. 일적인 컨벤션은 switch문을 사용하는것이다.
(switch문이 한눈에 보기가 쉽다.)
switch문을 쓸때는 `{}`로 case를 감싼다. 그리고 반드시 return을 잊지 않도록 한다.

```typescriptreact
function tasksReducer(tasks, action) {
  switch (action.type) {
    case "added": {
      return [
        ...tasks,
        {
          id: action.id,
          text: action.text,
          done: false,
        },
      ];
    }
    case "changed": {
      return tasks.map((t) => {
        if (t.id === action.task.id) {
          return action.task;
        } else {
          return t;
        }
      });
    }
    case "deleted": {
      return tasks.filter((t) => t.id !== action.id);
    }
    default: {
      throw Error("Unknown action: " + action.type);
    }
  }
}
```

reducer라고 불리게 된 이유는 reduce 함수에서 비롯된 것이다.
(따라서 reduce함수로 reducer를 구현할 수도 있다.)

### 3. component에서 reducer 사용

useReducer Hook을 사용한다.
reducer 파일을 별도로 분리하는 것도 방법이다.

## Comparing useState and useReducer

Reducer의 장단점

1. Reducer로 인해 Code Size가 커진다. (reducer 함수, dispatch action 두가지 모두 작성해야하기 때문에)
2. 가독성(코드가 짧은 경우 useState가 더 읽기 쉽다)
3. 디버깅(useState는 디버깅이 어렵다. dispatch action에 문제가 없으면 reducer 함수에서만 console을 찍어보면 되므로, 디버깅이 편리하다)
4. 테스트(reducer는 pure function이다. 즉 isolated 환경에서 import해서 테스트를 해볼 수 있다.)
5. 개인선호도에 따라서 reducer를 싫어하는 사람들도 있다.

잘못된 state update로 인해서 버그가 발생하는 경우에는 reducer를 사용해보는것을 추천한다.
반드시 reducer만 사용할 필요는 없고 같은 컴포넌트내에서 useState와 useReducer를 섞어서 사용해도 된다.
(ex. reducer를 사용하면 과거상태를 따로 관리하면 상태를 완전히 뒤로 되돌릴 수도 있기때문에 체스같은 게임에서 사용하기 좋다.)

## Writing reducers well

reducer는 pure 해야한다. 즉 same input, same output이 지켜져야한다.
request를 요청해서는 안된다. timeout 또한 안된다. 다른 side effect(컴포넌트 밖에 영향을 주는것들)도 사용하면 안된다.
반드시 object, array를 mutate 없이 업데이트해야한다.

하나의 action은 하나의 user interaction에 적용되어야한다.
즉 Form에서 Reset 버튼을 누르는 경우, set_field action이 아니라 reset_form action이 dispatch 되어야한다.
예를들어 버튼 클릭시 메시지를 보내고 빈값으로 리셋하는 경우
아래처럼 사용할 수도 있지만,

```typescriptreact
<button
  onClick={() => {
    alert(`Sending "${message}" to ${contact.email}`);
    dispatch({
      type: "edited_message",
      message: "",
    });
  }}
>
  Send to {contact.email}
</button>
```

아래처럼 별도의 action을 만들어주는 것이 더 좋다. (메시지를 보내는 것은, 필드를 수정하는 것과는 다르기 때문)
(keep in mind that action types should ideally describe “what the user did” rather than “how you want the state to change”)

```typescriptreact
export function messengerReducer(state, action) {
  switch (action.type) {
    case "edited_message": {
      return {
        ...state,
        message: action.message,
      };
    }
    case "sent_message": {
      // action을 추가함
      return {
        ...state,
        message: "",
      };
    }
    default: {
      throw Error("Unknown action: " + action.type);
    }
  }
}
```

## Writing concise reducers with Immer

useImmerReducer를 사용하면 mutate 없이 더 쉽게 작성할 수 있다.

## useReducer 직접 만들어보기

```typescriptreact
import { useState } from "react";

export function useReducer(reducer, initialState) {
  const [state, setState] = useState(initialState);

  function dispatch(action) {
    setState((s) => reducer(s, action));
  }

  return [state, dispatch];
}
```

# Passing Data Deeply with Context

props를 넘기는 것은 props 구조가 깊거나 많은 컴포넌트가 같은 prop을 필요로 한다면
결국 비대해지고(verbose) 그리고 불편하게 된다.
가장 가까운 common parent가 멀어지고 lifting state up이 높아지면 결국 "prop drilling"의 상황이 발생한다.

## Context: an alternative to passing props

### 1. Create the context

context 파일을 만들고 createContext를 한것을 export 한다.
(컴포넌트에서 사용할 것이다.)

### 2. Use the context

useContext를 사용한다. 이때 createContext한것을 useContext의 인자로 넣어준다.

### 3. Provide the context

context provider로 컴포넌트를 wrap 한다.
만약 LevelContext.Provider라는걸로 감쌌는데, 같은 Provider로 여러번 감싸게 되었다면,
가장 가까운 Provider의 값을 사용하게 된다.

## Using and providing context from the same component

같은 컴포넌트에서 context를 사용하고 제공할수도 있다.
[codesandbox 예시](https://codesandbox.io/s/j0w3hj?file=/Section.js&utm_medium=sandpack)

## Context passes through intermediate components

서로다른 React context는 서로를 override하지 않는다.
하나의 컴포넌트는 많은 다른 context들을 아무문제없이 사용(use)하거나 제공(provider)할 수 있다.

## Before you use context

단순히 props를 여러 level에 내려줘야하는 이유때문에
context는 과도하게 사용(overuse) 하는 것은 좋지 않다.

context를 적용하기 아래 2가지를 먼저 시도해보자

1. passing props를 먼저해보자. 만약 중요하지 않은 컴포넌트라면 여러개의 props를 내려주고 여러컴포넌트들이
   사용하는 것이 그리 이상하지는 않다. 오히려 명확하게 보이기 때문에 유지보수하는 사람이 고마워할 것이다.
2. 컴포넌트를 extract해서 children으로 내려주는것을 해보자. `<Layout><Posts posts={posts} /></Layout>` 이런 경우
   Layout은 데이터를 받을 필요가 없다. 따라서 children prop으로 적용할 수 있다.
   데이터를 명시적으로 사용함으로써 component간의 layer의 숫자를 줄여줄 수있다.

## Use cases for context

아래와 같은 경우는 일반적으로 context를 사용하면 된다.

1. Theming: 예를 들어 darkMode 같은 것을 적용하는 경우
2. Current Account: 많은 컴포넌트들이 로그인한 유저정보를 필요로 한다.
3. Routing: 많은 routing solution들이 내부적으로 context를 사용한다.
4. Managing state: app이 커지면서, 많은 state들이 top에 위치할 것이다. 멀리떨어진 컴포넌트에서 state를
   변경하고 싶을수도 있다. 이런 경우 [reducer와 함께 context를 사용](https://react.dev/learn/scaling-up-with-reducer-and-context)하는 것이 일반적이다.

# Scaling Up with Reducer and Context

## Combining a reducer with context

App이 커지면서, reducer의 dispatch같은것을 하위컴포넌트에서 사용하려면 [pass down](https://react.dev/learn/passing-props-to-a-component) 해줘야한다.
이때 context랑 같이 쓰면 된다.
(context-reducer pair라고 말함)

reducer와 context를 combine은 3가지 단계로 진행한다.

### 1. Create the context.

context는 아래처럼 2개를 생성해준다.

```typescriptreact
import { createContext } from "react";

export const TasksContext = createContext(null);
export const TasksDispatchContext = createContext(null);
```

### 2. Put state and dispatch into context.

두개의 Provider로 감싼다.
(순서는 크게 상관없는데 예시에서는 Dispatch를 좀더 아래로 감싼듯)

```typescriptreact
import { TasksContext, TasksDispatchContext } from "./TasksContext.js";

export default function TaskApp() {
  const [tasks, dispatch] = useReducer(tasksReducer, initialTasks);
  // ...
  return (
    <TasksContext.Provider value={tasks}>
      <TasksDispatchContext.Provider value={dispatch}>
        ...
      </TasksDispatchContext.Provider>
    </TasksContext.Provider>
  );
}
```

3. Use context anywhere in the tree.

useContext로 dispatch를 바로 사용해본다.

```typescriptreact
export default function AddTask() {
  const [text, setText] = useState('');
  const dispatch = useContext(TasksDispatchContext);
  // ...
  return (
    // ...
    <button onClick={() => {
      setText('');
      dispatch({
        type: 'added',
        id: nextId++,
        text: text,
      });
    }}>Add</button>
    // ...
```

## Moving all wiring into a single file

이것은 반드시 할필요는 없지만, reducer와 context를 컴포넌트에서 분리시켜서 더 깔끔하게 만들수 있다.

분리한 파일에서는 3가지 역할을 갖게된다. [codesandbox 예시](https://codesandbox.io/s/2ro1iu?file=/TasksContext.js&utm_medium=sandpack)

1. reducer 관리
2. context 제공(provide)
3. children prop으로 JSX를 넘겨줄 수 있음

또한 context 사용하는 function을 export해서 바로 사용할 수도 있다.

```typescriptreact
// Custom Hook 작성
export function useTasks() {
  return useContext(TasksContext);
}

export function useTasksDispatch() {
  return useContext(TasksDispatchContext);
}
```

```typescriptreact
// 컴포넌트에서 사용
const tasks = useTasks();
const dispatch = useTasksDispatch();
```
