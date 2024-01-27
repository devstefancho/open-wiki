---
published: false
id: lazy
slug: lazy
title: Lazy
summary: lazy
toc: true
tags: ["apis"]
categories: ["apis"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

# lazy

Date created: 2023-06-14

```tsx
const SomeComponent = lazy(load)
```

## Reference - lazy(load)
### Parameters
**load**
- load 함수는 Promise(혹은 thenable)를 반환해야 합니다.
- React는 컴포넌트를 최초로 랜더링할때까지 load를 실행하지 않습니다.
- load가 실행되면, resolve가 되기를 기다리고 resolve의 결과로 컴포넌트를 랜더링합니다.
- resolve된 Promise 결과값은 캐시되므로, load는 다시 실행되지 않습니다.
- Promise가 reject 되면, 가장 가까운 Error Boundary에rejection 이유를 throw 합니다.

### Returns
- 컴포넌트를 반환합니다.
- lazy 컴포넌트가 로딩중이라면, `<Suspense>`를 사용하여 loading indicator를 표시할 수 있습니다.

## Reference - load 함수
### Parameters
파라미터 없음

### Returns
Promise(혹은 thenable)를 반환해야 합니다.

## Usage
```ts
// 일반적인 경우
import MarkdownPreview from './MarkdownPreview.js';

// lazy 사용하는 경우
import { lazy } from 'react';

const MarkdownPreview = lazy(() => import('./MarkdownPreview.js'));

// Suspense 적용
<Suspense fallback={<Loading />}>
  <h2>Preview</h2>
  <MarkdownPreview />
 </Suspense>
```

## Trouble Shooting
lazy 선언은 module의 top-level에서 사용하세요.
```ts
import { lazy } from 'react';

function Editor() {
  // 🔴 Bad: This will cause all state to be reset on re-renders
  const MarkdownPreview = lazy(() => import('./MarkdownPreview.js'));
  // ...
}

// ✅ Good: Declare lazy components outside of your components
const MarkdownPreview = lazy(() => import('./MarkdownPreview.js'));

function Editor() {
  // ...
}
```
