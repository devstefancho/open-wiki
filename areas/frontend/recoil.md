---
published: false
id: recoil
slug: recoil
title: Recoil
summary: recoil
toc: true
tags: ["frontend"]
categories: ["frontend"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

# Recoil

## Atom과 Selector

Todo를 만들때 input 입력은 Atom에 저장하고, Selector에서 타입별로 정리해서 저장한다.
(저장: Atom, 랜더링용 리스트: Selector)
한곳(Atom)에다가 저장해야 LocalStorage 같은곳에 쉽게 저장해둘 수 있다.
TO_DO -> DOING으로 변경하면 Selector에서 리스트를 업데이트한다.

Atom에서는 최초에 status: 'TO_DO' 상태로 저장
리스트에 아래 형태로 데이터를 분류하고 화면에 랜더링한다.

```typescript
// atom에 저장되는 데이터 형태
const toDos = [
  {
    value: "밥먹기",
    status: "TO_DO",
  },
  {
    value: "밥먹기",
    status: "TO_DO",
  },
  {
    value: "밥먹기",
    status: "TO_DO",
  },
  {
    value: "밥먹기",
    status: "TO_DO",
  },
];

// 랜더링 시킬 list
const list = [
  toDos.filter((todo) => todo.status === "TO_DO"),
  toDos.filter((todo) => todo.status === "DOING"),
  toDos.filter((todo) => todo.status === "DONE"),
];
```
