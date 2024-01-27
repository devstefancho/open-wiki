---
published: false
id: input
slug: input
title: Input
summary: input
toc: true
tags: ["react-dom-components"]
categories: ["react-dom-components"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

# input

## 속성

[form](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input#form)
- form id가 명시적으로 표시되어 있지 않다면, 포함하고 있는 가장 가까운 form에 연관된다.

[list](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/datalist)
- 자동완성에 들어갈 `<datalist>`의 id를 넣음

onInvalid
- React에서는 onInvalid 함수가 버블링됨

multiple
- file과 email에서 사용되는데 email의 경우 콤마로 구분하고 각 이메일 주소는 별도의 유효성 검사를 받게 됨

onInput
- React에서는 onChange가 onInput 역할을 대체할 수 있도록 설계되었음, 따라서 사용할일이 거의 없음

## Caveat
- checked 혹은 value prop을 사용한다면 controlled input 이다.
- controlled와 uncontrolled 사이에서 스위칭될 수 없다.
- controlled와 uncontrolled를 동시에 가질 수 없다.
- controlled input은 항상 onChange event를 가져야한다.

## Usage

### Providing a label for an input 

- label안에 input을 넣는 것(nested)이 일반적이다.
- 그렇게 할 수 없는 경우에는 htmlFor로 id를 명시해준다. (예시에서는 useId 사용함)

### form에서 button 동작

- 어떤 버튼이든 form 내에서는 기본적으로 submit으로 동작한다.
- 따라서 type을 명시적으로 써주도록 한다.

### controlled인데, 의도적으로 onChange를 넣지 않는 경우

- readOnly prop을 추가하는 경우 controlled이면서 onChange를 쓰지 않을 수 있다.
  (단, 의도적으로 값이 항상 고정되게 하는 경우)

```tsx
// ✅ Good: readonly controlled input without on change
<input value={something} readOnly={true} />

// ✅ Good: readonly controlled input without on change
<input type="checkbox" checked={something} readOnly={true} />
```

### value, checked에는 null이나 undefined가 들어가면 안됨

- component에 value를 제공할때는 항상 string(checked인 경우 boolean)으로 남아있어야 한다.
- 만약 초기에 값을 알 수 없거나 API가 호출되는 동안 null이나 undefined로 초기화 되는 경우라면
  반드시 빈 문자열(`''`)로 세팅을 해야한다. (checked의 경우 boolean으로)

```tsx
value={someValue ?? ''}
```
