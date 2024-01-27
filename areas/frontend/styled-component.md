---
published: false
id: styled-component
slug: styled-component
title: Styled Component
summary: styled component
toc: true
tags: ["frontend"]
categories: ["frontend"]
createdDate: 2023-08-03
updatedDate: 2024-01-23
---

# Styled Component

as로 element를 변경할 수 있음
as="a" href="" 이런식으로 넣을수 있음

```tsx
const Button = styled.button`
  // ...something styled
`;

const Component = () => {
  return <Button as="a" href="https://google.com" />;
};
```

attrs로 기본값을 만들어줄 수 있음
모든 input들이 required가 true값을 갖게 됨

```typescript
const Input = styled.input.attrs({ required: true })`
  font-size: 20px;
`

const Component = () => {
	return (
	  <Input />
	  <Input />
	  <Input />
	  <Input />
	  <Input />
	)
}
```

모든 styled component에 이름을 줄 필요는 없다.

```tsx
const Box = styled.div`
  display: flex;
  div {
    font-size: 20px;
    &:hover {
      font-size: 24px;
    }
  }
`;
```

직접 타겟팅하는 방법

```typescript
const Emoji = styled.div`
  font-size: 20px;
`

const Box = styled.div`
  display: flex;
  ${Emoji} {
    &:hover {
      font-size: 30px;
    }
  }
`

const Component = () => {
	return (
		<div>
		  <Box>
		    <Emoji>😃</Emoji> --> hover이 적용된다.
		  </Box>
		  <Emoji>😃</Emoji> --> 여기 emoji는 hover이 적용되지 않는다
		</div>
	)
}
```

[createGlobalStyle](https://styled-components.com/docs/api#createglobalstyle)
전역으로 style을 적용하고 싶을떄

```tsx
const GlobalStyle = createGlobalStyle`
	body {
		 margin: 0;
	}
`;

const App = () => {
  return (
    <>
      <GlobalStyle />
      <MyComponent />
    </>
  );
};
```
