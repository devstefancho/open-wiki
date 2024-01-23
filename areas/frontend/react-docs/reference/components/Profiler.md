---
published: false
id: Profiler
slug: Profiler
title: PRofiler
description: Profiler
tags: ["components"]
categories: ["components"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

# Profiler

리액트 트리의 랜더링 성능을 측정할 수 있습니다.

```tsx
<Profiler id="App" onRender={onRender}>
  <App />
</Profiler>
```

## Reference - Profiler
### Props
**id**
UI 측정시 구분하기 위한 id

**onRender**
Profiled 트리가 업데이트 될때마다 실행되는 callback 함수
랜더링되는데 걸리는 시간에 대한 정보를 갖고 있습니다.

### Caveats
Profiling은 추가적인 부담을 주기때문에, Production 빌드시에는 기본적으로 비활성화 됩니다.
프로파일을 Production에도 적용하고 싶다면 [특별한 build](https://gist.github.com/bvaughn/25e6233aeb1b4f0cdb8d8366e54a3977)를 해야합니다.

## Reference - onRender callback
```tsx
function onRender(id, phase, actualDuration, baseDuration, startTime, commitTime) {
  // Aggregate or log render timings...
}
```

### Parameters
**id**
방금 커밋된 `<Profiler>` 트리의 문자열 id입니다. 
여러 개의 프로파일러를 사용하는 경우 어떤 부분이 커밋되었는지 식별할 수 있습니다.

**phase**
"mount"는 처음으로 트리가 마운트되었음을 나타내고, 
"update" 또는 "nested-update"는 props, state, 또는 훅의 변경으로 인해 트리가 다시 렌더링되었음을 나타냅니다.

**actualDuration**
현재 업데이트에 대한 `<Profiler>` 및 해당 하위 요소들을 렌더링하는 데 소요된 시간(milliseconds)입니다. 
이 값은 메모이제이션 (예: memo와 useMemo)을 얼마나 잘 활용하는지를 나타냅니다. 
초기 마운트 이후에는 많은 하위 요소들이 특정 props가 변경되지 않는 한 다시 렌더링될 필요가 없으므로 이 값은 크게 감소해야 합니다.

**baseDuration**
최적화 없이 전체 `<Profiler>` 서브트리를 다시 렌더링하는 데 걸리는 시간(milliseconds)을 추정한 값입니다. 
이 값은 트리의 각 컴포넌트의 최근 렌더링 지속 시간을 합산하여 계산됩니다. 
이 값은 렌더링의 최악의 경우를 추정하며 (예: 초기 마운트 또는 메모이제이션 없는 트리), 
memoization이 작동하는지 확인하기 위해 actualDuration과 비교할 수 있습니다.

**startTime**
현재 업데이트의 렌더링이 시작된 시간(숫자 타임스탬프)입니다.

**endTime**
현재 업데이트가 커밋된 시간(숫자 타임스탬프)입니다. 
이 값은 커밋 내의 모든 프로파일러에서 공유되어 필요한 경우 그룹화할 수 있습니다.

## Usage

### 예시

아래 예시로 리액트 개발자도구에서 Profiler를 보면, Button을 랜더링하는데 1초가 걸리는 것을 확인할 수 있습니다.
```ts
const MyComponent = () => {
  return (
    <Profiler
      id="CouponButton"
      onRender={(id, phase, actualDuration, baseDuration, startTime, endTime) => {
        console.group('profiler test')
        console.log({
          id,
          phase,
          actualDuration,
          baseDuration,
          startTime,
          endTime,
        })
        console.groupEnd()
      }}
    >
      <Button />
    </Profiler>
  )
}

const Button = () => { // Button을 랜더링하는데 약 1초가 걸림
  for (let i = 0; i < 1000; i++) {
    let startTime = performance.now()
    while (performance.now() - startTime < 1) {}
  }
  return (...)
}
```

### 서로다른 부분을 측정하기
Profiler는 여러군데의 컴포넌트에서 사용할 수 있습니다.
Profiler는 중첩(nested)할 수 있습니다.

## Ref
- https://blog.bitsrc.io/profiling-performance-of-react-apps-using-react-profiler-d02d77f3c96a
