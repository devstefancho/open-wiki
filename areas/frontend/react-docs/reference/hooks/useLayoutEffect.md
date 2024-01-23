---
published: false
id: useLayoutEffect
slug: useLayoutEffect
title: UseLayoutEffect
description: useLayoutEffect
tags: ["hooks"]
categories: ["hooks"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

# useLayoutEffect

브라우저가 스크린에 repaint하기전에 실행되는 useEffect 입니다

```tsx
useLayoutEffect(setup, dependencies?)
```

## Reference

### Parameters

**setup**
함수로써 Effect's logic를 가짐, cleanup 함수도 추가할 수 있습니다.
DOM에 컴포넌트가 추가되기전에 실행됩니다.
컴포넌트가 DOM에서 제거되기전에 cleanup 함수를 실행합니다.

**dependencies**
setup code에 있는 reactive values

### Returns
undefined

### Caveats
useLayoutEffect 내부의 코드와 그로부터 예정된 모든 상태 업데이트는 브라우저가 화면을 다시 그리는 것을 차단합니다.
과도하게 사용될 경우, 이는 앱의 속도를 느리게 만듭니다. 가능하다면 useEffect를 선호하세요

## Usage

랜더링이 되기전에 element의 사이즈(예를들어 높이)를 알아야하는 경우 사용합니다.
예를 들어, Tooltip의 경우 Tooltip이 랜더링 되기전에 높이를 알아야 랜더링될 정확한 위치를 지정할 수 있습니다.
(만약 useEffect를 사용한다면 Tooltip이 랜더링되면서 위치를 조정하는것을 사용자가 보게 될 것 입니다.)

아래와 같은 step으로 동작합니다.
1. Tooltip은 처음에는 높이가 기본값(`0`)으로 랜더링 됩니다.
2. React는 DOM에 Tooltip을 배치하고, useLayoutEffect을 실행합니다.
3. useLayoutEffect에서 텍스트 컨텐츠를 포함한 Tooltip의 높이를 계산하고, re-render를 발생시킵니다.
4. Tooltip은 진짜 높이와 함께 다시 랜더됩니다.
5. React는 DOM을 업데이트하고, 마침내 Tooltip이 화면에 표시됩니다.

useLayoutEffect는 서버사이드 랜더링에서는 동작하지 않습니다.
최초 랜더링이 서버에서 발생하는데, 서버에서는 아무런 레이아웃이 없기때문입니다.

만약 이것을 해결하고 싶으면 아래방법들이 있습니다.
- 초기 랜더링 결과가 표시되어도 괜찮다면, useLayoutEffect대신에 useEffect를 사용합니다.
- Suspense를 사용하여 컴포넌트가 client-only라는 것을 표시합니다.
- isMounted같은 state를 사용(초기값은 false)하여 `return isMounted ? <RealContent /> : <FallbackContent />` 형태로 사용합니다.
  서버와 hydration중에는 FallbackContent가 보여지고 useLayoutEffect를 실행하지 않습니다.
  React가 RealContent로 변경한 후에 client에서 useLayoutEffect이 실행되게 됩니다.
- layout 계산목적이 아닌, 외부에 저장된 데이터와 sync를 맞추기 위해서라면 useSyncExternalStore를 사용합니다.
