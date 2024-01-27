---
published: false
id: useMemo
slug: useMemo
title: UseMemo
summary: useMemo
toc: true
tags: ["hooks"]
categories: ["hooks"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

# useMemo

re-render간에 계산결과를 캐시하는 hook 입니다.

```typescript
const cachedValue = useMemo(calculateValue, dependencies)
```

## Reference
### Parameters
**calculateValue**
함수이고 계산값을 캐시합니다. 순수함수이어야 합니다.
argument를 받지 않아야하고, 어떠한 타입이든 반환할 수 있습니다.
처음에는 함수를 실행하고, 그 다음 랜더링부터는 dependencies가 변하지 않는 다면 같은값을 반환합니다.

### Returns
첫번째 리턴은 calculateValue를 실행한 결과입니다.
그 다음부터는 미리 저장된 값을 반환합니다.

### Caveats
- Strict Mode의 경우 개발모드에서 2번 함수를 호출합니다. 이는 함수의 순수성을 검증하기 위한 것입니다.
- 개발모드에서는 파일이 변경되면 캐시값을 버리게 됩니다.
- 컴포넌트가 초기 마운트 과정에서 suspend(일시 중단)상태가 된다면 캐시값을 버릴 수 있습니다.
- 캐시된 값을 반환하는것을 memoization이라고 부릅니다.

## Usage

useMemo는 언제 사용하는가?
보통의 경우 계산이 빠르지만, 큰 Array를 필터링하거나 변형하는 경우, 혹은 다른 비싼 계산이 있는 경우
useMemo를 사용합니다.

useMemo은 성능최적화를 위한 것
useMemo는 반드시 성능 최적화(performance optimization)을 위해서만 사용해야 합니다.
만약 useMemo없이 코드가 정상동작 하지 않는다면, 우선 그 문제부터 해결해야 합니다.

비싼 계산인지 확인하는 방법
- console.time을 활용한다.
- 인위적으로 느린 환경에서 테스트한다. (Chrome에서 CPU Throttling 사용)
- 개발모드에서는 정확하게 측정할 수 없다. 예를들어 Strict Mode에서는 각 컴포넌트가 두번씩 랜더되기 때문

useMemo를 사용하는 경우
- 느린계산을 줄이고자 할 때
- memo로 감싼 컴포넌트에 prop으로 전달하고자 할때
- useMemo의 계산값을 다른 hook에 의존성으로 사용하는 경우

메모이제이션은 항상 효과적인것은 아닙니다. 
어떤 하나의 새로운 값에 의해 모든 컴포넌트의 메모이제이션을 깰수 있습니다.

몇가지 방법으로 useMemo를 줄일 수 있습니다.
- 컴포넌트가 다른 컴포넌트를 시각적으로 감싸는 경우, children으로 받을 수 있습니다.
- 상태를 lift up 시키지 않고, 로컬 상태를 사용합니다.
- 랜더링 로직을 순수하게 유지합니다.
- 상태를 업데이트하는 불필요한 useEffect를 피합니다.
- Effect에서 불필요한 종속성을 제거합니다. (객체나 함수에 대한 메모이제이션 대신에 Effect 내부에서 선언합니다.)


