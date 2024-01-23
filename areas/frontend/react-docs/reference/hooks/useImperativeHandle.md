---
published: false
id: useImperativeHandle
slug: useImperativeHandle
title: UseImperativeHandle
description: useImperativeHandle
tags: ["hooks"]
categories: ["hooks"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

# useImperativeHandle

```tsx
useImperativeHandle(ref, createHandle, dependencies?)
```

## Reference

### Parameters

**ref**
forwardRef render function의 두번째 argument값을 ref로 받음

**createHandle**
argument는 없음, 노출하고 싶은 ref handle을 return 해주는 함수
주로 노출하고 싶은 method들을 object로 return함

**dependencies**
createHandle내의 reactive value
이 항목을 누락하거나, 특정 dependencies로 인해서 re-render되는 경우
createHandle이 re-execute 되고, ref에 새로운 handle이 할당됨

### Returns

undefined를 return

## Usage

기본적으로 forwardRef의 ref를 JSX에 있는 DOM node의 ref에 넣어주는데,
useImperativeHandle을 사용하면 DOM node에 넣지않고 useImperativeHandle의 첫번째 인자로 넣어줌

useImperativeHandle는 당신만의 imperative methods (명령형 메소드)가 필요할때 사용함
예를들면, 스크롤을 하면서 node를 포커싱하고 애니메이션을 실행하는 method는 없으므로
이런 경우 이것을 다 갖고 있는 method를 만들어서 ref로 노출해주는 것임

refs는 overuse 하지말고, props로 대체 가능하면 대체하는 것이 좋음
(ChatGPT말로는 React는 선언형 패러다임을 따르기 때문에, 명령형 프로그래밍은 가능한 피하는 것이 좋음)
