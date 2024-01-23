---
published: false
id: startTransition
slug: startTransition
title: StartTransition
description: startTransition
tags: ["apis"]
categories: ["apis"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

# startTransition

Date created: 2023-06-11

```tsx
startTransition(scope)
```

## Reference
### Parameters
**scope**
함수로써 내부에서 1개 이상의 useState의 set 함수를 호출합니다.
이 함수에는 파라미터가 없습니다.

### Returns
return 값이 없습니다.

### Caveats
- pending 상태에 대한 값을 제공하지 않습니다.
  pending indicator가 필요하면 useTransition을 사용하세요
- set 함수에 대한 것만 사용하세요, props나 custom Hook의 return 값에 대해서는 useDeferredValue를 사용하세요.
- startTransition에 synchronous 함수만 전달되어야 합니다.
  이 함수를 바로 실행해서 상태 업데이트가 transition 으로 실행되도록 모든 상태를 표시하기 때문입니다.
- transition으로 표시된 상태는 다른 어떤 상태업데이트에 의해서 방해받을 수 있습니다.
- transition 업데이트는 text input에서 사용할 수 없습니다.
- 동시에 여러 transition이 있다면, 일괄처리(batch) 할 것입니다.

## Usage

startTransition 호출안에 상태업데이트를 둠으로써, non-blocking transition으로 동작하게 됩니다.
```ts
import { startTransition } from 'react';

function TabContainer() {
  const [tab, setTab] = useState('about');

  function selectTab(nextTab) {
    startTransition(() => {
      setTab(nextTab);
    });
  }
  // ...
}
```


