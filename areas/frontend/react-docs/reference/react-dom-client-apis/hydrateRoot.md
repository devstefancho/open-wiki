---
published: false
id: hydrateRoot
slug: hydrateRoot
title: HydrateRoot
description: hydrateRoot
tags: ["react-dom-client-apis"]
categories: ["react-dom-client-apis"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

# hydrateRoot

browser DOM node(react-dom/server에 의해 미리 생성된 HTML content)에 React Component를 표시하기 위함

```ts
const root = hydrateRoot(domNode, reactNode, options?)
```

## hydrateRoot
기존의 HTML(서버에서 React를 랜더링하여 생성)에 React를 attach 해준다.
### Parameters
**domNode**
서버에서 랜더링된 root element

**reactNode**
React node로 주로 `<App />`

**options**
object로 2가지 옵션을 갖고있다.
- onRecoverableError: 리액트가 자동으로 에러를 회복할때 실행되는 Callback
- identifierPrefix: multiple root에서 useId로 생성된 string 충돌을 막기위해서 사용

### Returns
render, unmount 메소드가 있는 object

### Caveats
- server에서 render된 content와 완전히 일치해야한다. 만약 일치하지 않으면 버그이기 때문에 고쳐야한다.
- 개발모드에서는 불일치가 있으면 React가 경고를 해준다.
- 만약 framework를 사용한다면, 그 내부에서 hydrateRoot를 호출하고 있을것이다.
- HTML이 render된것이 없는 client-render 앱이라면 hydrateRoot는 지원하지 않는다. createRoot를 사용하라.

## root.render
```ts
root.render(<App />)
```
hydrate된 root내에서 `<App />`으로 업데이트할 것이다.
### Parameters
**reactNode**
update하고 싶은 React node로서 보통 `<App />`이다.

### Returns
undefined를 반환

### Caveats
만약 hydrating이 끝나기전에, root.render를 호출하면, 기존의 server-rendered HTML을 제거하고
전체 root를 client rendering으로 교체하게 된다.

## root.unmount
대체적으로 React로만 만들어진 App에서는 unmount를 호출하지 않음
만약 다른 framework(예를 들면 jQuery)에서 React 코드를 완전히 제거할때는 유용하다.
root.unmount를 실행하면, 같은 root에 대해서는 render를 다시 실행할 수 없다. (“Cannot update an unmounted root” 이런 에러가 발생할 것이다.)
하지만 이전의 root를 unmount한 다음에, 같은 DOM node에 새로운 root를 생성할 수는 있다.

## Usage
### 피하기 어려운 hydration 불일치 에러에 대해서 Suppress 하는 방법
suppressHydrationWarning를 true로 두면 불일치 에러를 silent 시킬 수 있다.
```tsx
export default function App() {
  return (
    <h1 suppressHydrationWarning={true}>
      Current Date: {new Date().toLocaleDateString()}
    </h1>
  );
}
```

### client와 server content가 다른경우 핸들링하는 방법

아래와 같이 클라이언트에서 값이 변경되는 경우, server의 값과 initial render값을 일치시켜주면 된다.
`Is Server`라는 text로 일치가 된 후에(hydration이 완료된 후에), 추가적인 랜더가 발생하게 된다.
```html
<div id="root"><h1>Is Server</h1></div>
```

```js
import './styles.css';
import { hydrateRoot } from 'react-dom/client';
import App from './App.js';

hydrateRoot(document.getElementById('root'), <App />);
```

```js
import { useState, useEffect } from "react";

export default function App() {
  const [isClient, setIsClient] = useState(false);

  useEffect(() => {
    setIsClient(true);
  }, []);

  return (
    <h1>
      {isClient ? 'Is Client' : 'Is Server'}
    </h1>
  );
}
```

위와 같은 방식은 render를 두번하는 것이기 때문에, hydration을 느려지게 만든다.
초기 HTML이 랜더링보다 Javascript 코드가 로드가 상당히 뒤에 일어날 수 있기 때문에, 
hydration 직후에 다른 UI를 랜더링하는 것은, user 입장에서 부조화를 느낄 수 있다.
