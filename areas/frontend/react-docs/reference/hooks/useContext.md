---
published: false
id: useContext
slug: useContext
title: UseContext
description: useContext
tags: ["hooks"]
categories: ["hooks"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

useContext hook으로 component에서 context를 subscribe하고 read할 수 있다.
```tsx
const value = useContext(SomeContex)
```
## Reference

### Parameters
**SomeContext**
`createContext`로 생성한 context이다.
context 자체로는 정보가 없다. 
단지, 이것은 component에서 provide하고 read 하는 것이 어떤 종류의 정보인지를 알려주는 것이다.

### Returns
context value를 return 한다. 
가장 가까운 SomeContext.Provider으로 제공된 value에 의해 결정된다.
만약 provider가 없다면, createContext에 있는 defaultValue를 return할 것이다.
return 값은 항상 최신값이다.
만약 읽고 있는 값이 바뀐다면 React는 자동으로 component를 re-render 한다.

### Caveats
같은 컴포넌트내의 provider는 useContext에 영향을 주지 않는다.
반드시 상위의 컴포넌트의 `<Context.Provider>` 상호 응답한다.
React는 특정 context의 value에 다른 값이 들어가면 자동으로 그것의 하위에 있는 children을 re-render한다. (이는 다른 값을 받는 provider부터 시작된다.)
이전 값 비교는 Object.is 비교를 사용한다.
re-render를 skip하기 위한 memo는 새로운 context value를 받는 것을 방지하지 않는다.

## Usage

context value를 결정하기 위해서, React는 component tree를 검색하고 이 특정 context에 맞는 가장 가까운 context provider를 찾는다.
중간에 얼마나 많은 component layer가 있던 상관없다.

만약 prent tree에서 특정 context provider를 찾지 못한다면, default value를 return 할 것이다.
default value는 절대 바뀌지 않는다.

context는 override도 할 수 있다.
```tsx
<ThemeContext.Provider value="dark">  
	...
	<ThemeContext.Provider value="light">  
		<Footer />  
	</ThemeContext.Provider>  
	...
</ThemeContext.Provider>
```

context value에 내려주는 값 중에서 
function은 불필요한 re-render를 줄이기 위해서 useCallback으로 wrap 해준다.
object는 useMemo로 wrap 해준다.

## Trouble Shooting

component가 provider의 value를 보고 있지 않은 경우
- Provider로 감싸는 것을 잊는게 아닌지 확인, 혹은 다른 tree에 감싸둔게 아닌지 확인한다.
(React DevTools로 확인할 수 있다.)

default value랑 다르게 항상 undefined 값을 받는 경우
만약 Provider로 감싸놓고 value prop을 넣지 않으면 undefined 값이 넘어간다.
아래와 같은 경우 `value={undefined}`와 동일하게 보기 때문이다.
따라서 어딘가에 Provider를 감싸놓기는 한 것이다.

```tsx
// 🚩 Doesn't work: no value prop  
<ThemeContext.Provider>  
	<Button />  
</ThemeContext.Provider>
```

(이런 경우 console에서 warning이 나올 것이다.)
default value는 match되는 provider가 없는 경우에만 사용된다.
