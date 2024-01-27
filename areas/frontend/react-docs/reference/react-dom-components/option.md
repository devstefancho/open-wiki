---
published: false
id: option
slug: option
title: Option
summary: option
toc: true
tags: ["react-dom-components"]
categories: ["react-dom-components"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

# option

select 아래에서 사용하는 것이고 3가지 props을 가진다.

## Props

disabled: 선택할수 없고, dimmed 처리됨
label: option에 표시되는 이름을 나타냄, 명시되지 않으면 option안에 있는 text값이 사용됨
value: form안에 있는 select에 제출되는 값

## Usage

React는 option의 selected attribute를 지원하지 않음.
uncontrolled일때는 select의 defaultValue에 값을 설정
controlled일떄는 select의 value에 값을 설정
