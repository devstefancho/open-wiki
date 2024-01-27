---
published: false
id: useTransition
slug: useTransition
title: UseTransition
summary: useTransition
toc: true
tags: ["hooks"]
categories: ["hooks"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

# useTransition

```ts
const [isPending, startTransition] = useTransition()
```

## Reference

### useTransition
#### Parameters
파라미터가 없습니다.

#### Returns
1. isPending은 전환이 중지 상태인지를 나타냅니다.
2. startTransition 함수내의 상태업데이트는 전환으로 간주됩니다.

### startTransition 함수
#### Parameters
**scope**
한개이상의 상태를 업데이트하는 set 함수를 호출하는 함수
파라미터는 없고, 업데이트 되는 모든 상태는 동기적으로 예약됩니다.

#### Returns
반환하는 값 없음

#### Caveats
- useTransition는 훅이기 때문에 컴포넌트 안에서만 호출할 수 있습니다.
  컴포넌트 밖에서 사용하고 싶다면, startTransition만 단독으로 사용할 수 있습니다.
- prop이나 custom Hook의 값으로 전환을 하려고 한다면, useDeferredValue를 대신 사용하세요.
- startTransition에 넘기는 함수는 바로 실행되도록, synchronous 해야합니다.
  timeout 같은걸로 나중에 업데이트 하도록 하면, 전환되지 않을 것입니다.
- text input에서는 Transition을 사용할 수 없습니다.
- 여러개의 전환이 있다면, React는 batch 처리할 것입니다. 이것은 현재 한계로 인한것으로 미래에는 개선될 것입니다.

## Usage

### Tab 클릭
Tab 클릭에서 useTransition을 사용할 수 있습니다.
Tab 클릭시 로딩이 오래걸리는 경우, App이 freeze 될 수 있습니다. 이 경우 click을 전환표시해두면 다른작업을 막지 않습니다.
(즉 Tab 클릭으로 인한 전환이 자연스러워짐)

```ts
  function selectTab(nextTab) {
    startTransition(() => {
      setTab(nextTab);
    });
  }
```

### 부모 컴포넌트 상태를 전환
부모 컴포넌트의 상태도 전환표시를 할 수 있습니다.
```tsx
// 1. 부모 컴포넌트에서 onClick에 setTab을 넘기는 경우
<TabButton
  isActive={tab === 'about'}
  onClick={() => setTab('about')}
>

// 2. 자식 컴포넌트에서 startTransition으로 감싸면 setTab이 전환표시 됩니다.
export default function TabButton({ children, isActive, onClick }) {
  const [isPending, startTransition] = useTransition();
  if (isActive) {
    return <b>{children}</b>
  }
  return (
    <button onClick={() => {
      startTransition(() => {
        onClick();
      });
    }}>
      {children}
    </button>
  );
}
```

### Suspense의 loading indicator 막기
아래와 같은 경우에 fetch 중에는 fallback을 호출하지만, TabButton click에 startTransition로 처리되어 있으면,
fallback이 호출되지 않고 isPending 상태가 됩니다.
```tsx
// 부모 컴포넌트
<Suspense fallback={<h1>🌀 Loading...</h1>}>
  <TabButton
    isActive={tab === 'about'}
    onClick={() => setTab('about')}
  >
    About
  </TabButton>
  (...)
```

```tsx
// 자식 컴포넌트
import { useTransition } from 'react';

export default function TabButton({ children, isActive, onClick }) {
  const [isPending, startTransition] = useTransition();
  if (isActive) {
    return <b>{children}</b>
  }
  if (isPending) {
    return <b className="pending">{children}</b>;
  }
  return (
    <button onClick={() => {
      // startTransition으로 인해서, Suspense의 fallback이 호출되지 않음
      startTransition(() => {
        onClick();
      });
    }}>
      {children}
    </button>
  );
}
```

### Page 전환의 경우에 Transition을 사용

React 프레임워크 또는 router로 앱을 만든다면, 아래 이유로 page navigation을 transition으로 처리하는 것을 권장합니다.
- Transition은 interruptible 하기 때문에, 마지막 랜더링까지 기다리지 않고도 유저가 클릭으로 페이지를 넘어갈 수 있습니다.
- 페이지 이동에 따른 원치않는 loading indicator를 방지할 수 있습니다.

```tsx
function Router() {
  const [page, setPage] = useState('/');
  const [isPending, startTransition] = useTransition();

  function navigate(url) {
    startTransition(() => { // 페이지상태를 전환으로 처리
      setPage(url);
    });
  }
```

