---
published: false
id: textarea
slug: textarea
title: Textarea
summary: textarea
toc: true
tags: ["react-dom-components"]
categories: ["react-dom-components"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

# textarea

## Props

autoComplete: 'on' 혹은 'off'로 자동완성 동작에 대한 명시
children: 리액트에서 textarea는 children을 허용하지 않는다. defaultValue를 사용해야한다.
cols: 기본값 20
onSelect: textarea내에서 텍스트 선택이 발생하면 실행됨 `e.target.value.substring(e.target.selectionStart, e.target.selectionEnd)`
readOnly: 읽기만 가능함
rows: 기본값 2
wrap: 텍스트 wrap에 관한것으로 'hard', 'soft', 'off'가 있으며 기본값은 'soft'


## Caveats
- html 표준과 다르게, children을 받을 수 없다.
- value가 있으면 controlled textarea 이다.
- controlled와 uncontrolled 두가지를 동시에 가질 수 없다.
- 생애주기동안 controlled와 uncontrolled를 스위칭할 수 없다.
- controlled textarea는 반드시 onChange를 갖고 있어야한다. (readOnly인 경우는 제외)
  - onChange가 없고 value만 있다면, 매번의 keystroke에서 리액트는 강제로 value값으로 되돌릴 것이다.

## Trouble Shooting

### readOnly인 경우 onChange가 없어도 된다.
- value가 있더라도, readOnly인 경우에는 onChange가 없어도 된다.
```typescriptreact
// ✅ Good: readonly controlled text area without on change
<textarea value={something} readOnly={true} />
```

### controlled일때는 value에 e.target.value 값이 그대로 들어가야한다.
- 아래와 같이 변형하여 들어가면 안된다.
```typescriptreact
// 🔴 Bug: updating an input to something other than e.target.value
function handleChange(e) {
  setFirstName(e.target.value.toUpperCase());
}

// 🔴 Bug: updating an input asynchronously
function handleChange(e) {
  setTimeout(() => {
    setFirstName(e.target.value);
  }, 100);
}

// ✅ Updating a controlled input to e.target.value synchronously
function handleChange(e) {
  setFirstName(e.target.value);
}
```

### value에는 반드시 string이 들어가야한다.
- 초기값으로 `value={undefined}`로 설정할 수 없다. controlled component는 반드시 string value를 받아야한다. (null이나 undefined는 안됨)
- 따라서 초기값이 명확하지 않은 경우 빈 문자열로 세팅해줘야한다. `value={someValue ?? ''}`
