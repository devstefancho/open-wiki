---
published: false
id: useDeferredValue
slug: useDeferredValue
title: UseDeferredValue
summary: useDeferredValue
toc: true
tags: ["hooks"]
categories: ["hooks"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

UI의 일부 업데이트를 지연하는 경우 사용
```tsx
const deferredValue = useDeferredValue(value)
```

## Reference
### Parameters
**value**
defer하고 싶은 value로 어떤 타입이든 가능함

### Returns
initial render에 return 되는 deferred value는 처음에 주입한 값과 동일할 것이다.
update가 되는 동안, React는 old value로 re-render하고, 그 다음에 background에서 new value로 re-render을 시도한다.

### Caveats
- useDeferredValue에 넣는 값은 primitive value이거나 rendering과 무관한 objects이어야한다.
- 만약 rendering과정에서 생겨나는 object를 넣으면, 매번 랜더링 될때마다 달라지므로, 불필요한 background re-render를 발생시킨다.
- 만약 다른 값을 useDeferredValue에서 받게 되면 background에서 re-render를 schedule한다.
background re-render은 interruptible 하다.
- 만약 다른 새로운 값이 들어온다면 scratch부터 re-render를 시작할 것이다.
- 예를 들어 chart가 defer value를 받고 있고, user가 이때 typing을 하고 있다면 chart의 re-render는 user typing이 끝난 후에 시작된다.
- useDeferredValue는 Suspense랑 integrated되어 있기 때문에,  UI 업데이트 지연동안은 Suspense의 fallback를 표시하지 않는다. 따라서 기존화면을 보면서 다음화면을 기다릴 수 있다.
- useDeferredValue는 network requests를 막지 못한다.
- useDeferredValue가 갖고 있는 고정된 delay값은 없다. 기존의 re-render가 끝나면 즉시 새로운 deferred value로 re-render를 시작한다. 어떠한 이벤트이든 이것보다 우선순위를 갖고 interrupt할수가 있다.
- useDeferredValue에 대한 Effects는 deferred value가 완전히 화면에 commit된 후에 실행된다.

## Usage

### 새로운 content(fresh content)가 로딩될 동안 기존의 content(stale content)를 보여주려는 경우
업데이트가 될 동안 deferred value는 최신 값에 대해서 "lag behind" 되어있다.
React는 deferred value를 업데이트 하지 않고 첫 re-render를 할 것이고, 그 다음 background에서 새로운 값으로 re-render를 시도한다.

### Indicating that the content is stale
css를 사용하여 데이터가 loading이라는 것을 표시할 수 있다.
```tsx
export default function App() {
  const [query, setQuery] = useState('');
  const deferredQuery = useDeferredValue(query);
  const isStale = query !== deferredQuery;
  return (
      <Suspense fallback={<h2>Loading...</h2>}>
        <div style={{
          opacity: isStale ? 0.5 : 1,
          transition: isStale ? 'opacity 0.2s 0.2s linear' : 'opacity 0s 0s linear'
        }}>
          <SearchResults query={deferredQuery} />
        </div>
      </Suspense>
  );
}
```

### Deferring re-rendering for a part of the UI
성능 최적화를 위해서 사용할 수 있는데, 특정 UI가 느린 경우에 유용하다.
최적화를 할 수 있는 쉬운방법이 없을때, 이 느린 부분이 다른 UI를 blocking하는 것을 방지할때 사용할 수 있다.
1. 느린 Component는 memo로 감싼다.
2. useDeferredValue를 props로 내려준다.

useDeferredValue는 검색창에서 타이핑할때 debounce 대신에 사용하기 좋은 것 같다.
(예시도 검색창 처럼 되어있음)

debouncing, throttling과 deferring value의 차이점
rendering 최적화를 위해서는 deferring이 더 적합하다. 
왜냐면 React와 깊게 통합되어 있고, 사용자 기기에 적응하기 때문에 최적화에 더 적합하다.

deboucing, throttling과 다른점 
fixed delay를 선택할 필요가 없다. 빠른 디바이스에서는 거의 즉시 deferred value가 re-render 된다.
(유저가 거의 눈치채지 못할 정도일 것이다.)
deferring은 interruptible하다. 이 말은 유저가 typing을 하면 re-render를 버리고, keystroke을 핸들링한 다음에 다시 background에서 re-render를 시도한다.
만약 최적화가 rendering에서 발생하지 않는다면 debounce와 throttle도 여전히 유용할 것이다.
예를 들어 network 요청을 최소화 하려고 한다면 debounce, throttle을 같이 사용하면 된다.
