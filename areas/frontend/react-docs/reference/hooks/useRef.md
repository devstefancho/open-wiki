---
published: false
id: useRef
slug: useRef
title: UseRef
summary: useRef
toc: true
tags: ["hooks"]
categories: ["hooks"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

# useRef

```tsx
const ref = useRef(initialValue)
```

## Reference
### Parameters
**initialValue**
ref object의 current 속성의 초기값으로 초기랜더링 이후에는 무시된다.

### Returns
current 속성만을 갖고 있는 object

### Caveats
ref.current을 읽고 쓰는 로직을 rendering 중에는 넣지말고,
Effect나 event handler내에서 작성한다.

## Usage

1. input DOM 조작
2. image scroll
3. video play와 pause
4. forwardRef를 사용해서 커스텀 컴포넌트의 ref에 사용

### Ref 초기값을 여러번 생성하지 않아야함

초기값은 재사용되지 않지만, 인자의 함수(`new VideoPlayer()`)는 실행되므로 비효율적이다.
null-check를 로직중간에서 하지 않으려면 Better case 처럼 함수로 분리해서 사용할 수 있다.
```ts
// Bad
function Video() {
  const playerRef = useRef(new VideoPlayer());

// Good
function Video() {
  const playerRef = useRef(null);
  if (playerRef.current === null) {
    playerRef.current = new VideoPlayer();
  }

// Better
function Video() {
  const playerRef = useRef(null);

  function getPlayer() {
    if (playerRef.current !== null) {
      return playerRef.current;
    }
    const player = new VideoPlayer();
    playerRef.current = player;
    return player;
  }
```
