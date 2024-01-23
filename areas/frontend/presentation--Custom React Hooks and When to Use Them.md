---
published: false
id: presentation--Custom React Hooks and When to Use Them
slug: presentation--Custom React Hooks and When to Use Them
title: Presentation  CUstom REact HOoks And WHen To USe THem
description: presentation  Custom React Hooks and When to Use Them
tags: ["frontend"]
categories: ["frontend"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

# Custom React Hooks and When to Use Them

[https://thoughtbot.com/blog/custom-react-hooks](https://thoughtbot.com/blog/custom-react-hooks)

---

# What are custom hooks

custom hook은 React의 추상화 방법 중 하나.

React 공식 문서

> A custom Hook is a JavaScript function whose name starts with "use" and that may call other Hooks.

> custom hook은 이름이 use로 시작하고 다른 hook을 호출할 수 있는 JavaScript 함수

---

# Reusable custom hooks

##  언제 훅을 만들어야 할까?

- 함수를 만들어야 할 때
- 추출된 코드에 JSX가 없을 때
  * JSX가 포함된다면 component
* 추출된 코드에 다른 훅에 대한 호출이 포함되어 있는 경우

---

# Reusable custom hooks

```tsx
function HomePage {
  useEffect(() => {
    document.title = 'Home'
  }, [])

  return <div>Home Page...</div>
}
```

---

# Reusable custom hooks

```tsx
function useTitle(title) {
  useEffect(() => {
    document.title = title;
  }, [title]);
}
```

```tsx
function HomePage {
  useTitle('Home')

  return <div>Home Page...</div>
}
```

---

# Non-reusable custom hooks to extract functionality

* 컴포넌트를 만드는 이유와 동일하게, 부모 컴포넌트를 더 쉽게 이해할 수 있도록 기능을 추출하려고 custom hook을 만들기도 함
* 재사용할 수 없는 hook을 만들 때의 기준
  * 부모 컴포넌트가 더 이해하기 쉬워졌는가?
  * hook 자체로 충분히 의미가 있는가?
* 둘 중 하나만 NO라도 깔끔한 hook이 아닐 가능성이 높다.
* hook으로 분리하지 않았을 때 더 이해하기 쉬울 수도 있다.
* hook을 추가하여 생기는 복잡성을 감수할 가치가 있는지 다시 생각해보자.

---

# Determining how much to extract

```tsx
function MyComponent({ displayTitle }) {
  const user = useSelector((state) => state.user);

  const formattedName = _.compact([
    displayTitle ? user.title : null,
    user.firstName,
    user.middleName,
    user.lastName,
  ]).join(" ");

  return (
    <>
      <h1>Hello, {formattedName)}</h1>
      <p>Some text</p>
    </>
  );
}
```

---

# Determining how much to extract

```tsx
function formatUserName(user, { displayTitle }) {
  return _.compact([
    displayTitle ? user.title : null,
    user.firstName,
    user.middleName,
    user.lastName,
  ]).join(" ");
}

function MyComponent({ displayTitle }) {
  const user = useSelector((state) => state.user);

  // extracted logic to a function
  const formattedName = formatUserName(user, { displayTitle });

  return (
    <>
      <h1>Hello, {formattedName}</h1>
      <p>Some text</p>
    </>
  );
}
```

---

# Determining how much to extract

```tsx
// extracted everything, including the global store access, so this is a hook
function useFormattedUserName({ displayTitle }) {
  const user = useSelector((state) => state.user);
  const formattedName = _.compact([
    displayTitle ? user.title : null,
    user.firstName,
    user.middleName,
    user.lastName,
  ]).join(" ");

  return formattedName;
}

function MyComponent({ displayTitle }) {
  const formattedName = useFormattedUserName({ displayTitle });

  return (
    <>
      <h1>Hello, {formattedName}</h1>
      <p>Some text</p>
    </>
  );
}
```

---

# Determining how much to extract

```tsx
function UserGreeting({ displayTitle }) {
  const user = useSelector((state) => state.user);
  const formattedName = _.compact([
    displayTitle ? user.title : null,
    user.firstName,
    user.middleName,
    user.lastName,
  ]).join(" ");

  return <h1>Hello, {formattedName}</h1>;
}

function MyComponent({ displayTitle }) {
  return (
    <>
      <UserGreeting displayTitle={displayTitle} />
      <p>Some text</p>
    </>
  );
}
```

---

# Determining how much to extract

* 약간의 코드 중복이 있더라도 작은 추상화를 만드는 것이 가장 좋다.
* 이 경우에는 formatUserName 함수를 만들고 여러 위치에서 useSelector 호출 및 JSX 출력의 중복을 허용하는 것이 가장 좋다.

---

# The cost of abstractions

* 추상화는 coupling을 만들고, layer를 만들고, 방향을 만든다.
* 추상화를 수정할 때는 사용되는 모든 곳을 고려해야 한다.

---

# The cost of abstractions

React 공식 문서

> Try to resist adding abstraction too early. Now that function components can do more, it's likely that the average function component in your codebase will become longer. This is normal — don't feel like you have to immediately split it into Hooks. But we also encourage you to start spotting cases where a custom Hook could hide complex logic behind a simple interface, or help untangle a messy component.

> 너무 일찍 추상화를 추가하는 것은 자제하세요. 이제 함수 컴포넌트가 더 많은 작업을 수행할 수 있으므로 코드베이스의 평균 함수 컴포넌트 길이가 길어질 가능성이 높습니다. 이는 정상적인 현상이므로 즉시 Hook으로 분할해야 한다고 생각하지 마세요. 하지만 커스텀 Hook이 단순한 인터페이스 뒤에 복잡한 로직을 숨기거나 복잡한 컴포넌트를 정리하는 데 도움이 될 수 있는 경우를 발견하는 것도 좋습니다.

---

# The cost of abstractions

Sandi Metz says, **"duplication is far cheaper than the wrong abstraction".**

---

# 수고하셨습니다!

![bg right:50% w:300]([https://drive.google.com/uc?export=view&id=1HjitD4keiSj_CTZRjz1OYO_BMdzYpBg-](https://drive.google.com/uc?export=view&id=1HjitD4keiSj_CTZRjz1OYO_BMdzYpBg-))
