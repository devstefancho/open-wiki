---
published: false
id: select
slug: select
title: Select
summary: select
toc: true
tags: ["react-dom-components"]
categories: ["react-dom-components"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

# select

## Props

multiple: boolean값으로, 여러개 선택할 수 있음
children: option, optgroup, datalist 사용가능
size: number값으로, multiple일때, item이 보이는 최대개수

## Caveats
- select에 defaultValue를 사용하면, uncontrolled이고, value를 사용하면 controlled select box이다.
- controlled와 uncontrolled 두가지를 동시에 가질 수 없다.
- 생애주기동안 controlled와 uncontrolled를 스위칭할 수 없다.
- controlled select box는 반드시 onChange를 갖고 있어야한다. (readOnly인 경우는 제외)
  - onChange가 없고 value만 있다면, 매번의 keystroke에서 리액트는 강제로 value값으로 되돌릴 것이다.

## Usages

### multiple selection
defaultValue에 여러개를 초기값으로 잡는 경우에 array로 되어있어야한다.

### form
onSubmit에서 e.target을 new FormData에 넣어서 사용한다.
`[...formData.entries()]` 혹은 `Object.fromEntriess(formData.entries())`로 데이터를 추출하여 사용할 수 있다.
URL 쿼리 형태로 만들때는 `new URLSearchParams(formData).toString()` 방식으로 처리하면 된다.



