---
published: false
id: Suspense
slug: Suspense
title: SUspense
summary: Suspense
toc: true
tags: ["components"]
categories: ["components"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

# Suspense

Date created: 2023-06-10

```tsx
<Suspense fallback={<Loading />}>
  <SomeComponent />
</Suspense>
```

자식 컴포넌트의 loading이 끝날때까지 fallback을 보여줍니다.

## Reference
### Props
**children**
render할 UI로 랜더링에 시간이 걸리는 경우 fallback으로 전환됩니다.

**fallback**
실제 UI를 대체하는 UI로 ReactNode를 받을 수 있습니다.
placeholder 역할을 하고, 스켈레톤이나 로딩 스피너를 주로 사용합니다.
가장 가까운 부모의 Suspense Boundary가 활성화됩니다.

## Usage

### 로딩되는 동안, stale 컨텐츠로 보여주기
children 컴포넌트에 useDeferredValue를 사용하면, fallback이 실행되지 않게 할 수 있습니다.
transition과 useDeferredValue는 fallback을 보여주는 것을 피하고 싶을때 사용할 수 있습니다.

### 서버에서 fallback 제공하기

서버에서 에러를 발생시키는 경우 fallback이 서버에서 생성되는 HTML에 포함되게 됩니다.
그래서 유저는 fallback(예시에서는 Loading 컴포넌트)를 가장 먼저 보게 됩니다.
(서버에서 에러가 났다고 server render를 중단하지 않습니다.)
즉 Suspense는 서버에서 에러 핸들링을 해줍니다.

클라이언트에서 똑같은 에러가 발생하면 가장가까운 error boundary를 보여주게 됩니다.

```tsx
<Suspense fallback={<Loading />}>
  <Chat />
</Suspense>

function Chat() {
  if (typeof window === 'undefined') {
    throw Error('Chat should only render on the client.');
  }
  // ...
}
```

## Trouble Shooting

