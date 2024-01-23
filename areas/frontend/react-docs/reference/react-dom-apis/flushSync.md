---
published: false
id: flushSync
slug: flushSync
title: FlushSync
description: flushSync
tags: ["react-dom-apis"]
categories: ["react-dom-apis"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

# flushSync

flushSync를 사용하는 것은 일반적이지 않으며, 앱 성능을 해칠 수 있다.

flushSync안에 있는 callback은 동기적으로 먼저 실행된다.

```ts
import { flushSync } from 'react-dom'
flushSync(() => {
    setSomething(123)
})
```

## Reference
### Returns
flushSync는 undefined를 반환함

### Caveats
- 성능상 문제가 될 수 있으므로, 마지막 수단으로 사용해야 한다.
- flushSync 사용은 Suspense boundaries의 fallback을 강제로 지연시킬 수도 있다.

## Usages
flushSync는 third-party 라이브러리(browser API나 UI 라이브러리)와 통합할때 사용할 수 있다.
React API만 사용하는 경우에는, flushSync는 완전 불필요하다.

아래 예시에서는 프린트 API가 실행되기 전에, flushSync로 강제로 먼저 yes를 랜더링 시킨다.
아래의 경우에는 flushSync가 없다면, 상태가 업데이트 되기전에 먼저 print dialog를 보여지게 되고, 
setIsPrinting(true)와 setIsPrinting(false)가 batch로 실행된다.

```ts
import { useState, useEffect } from 'react';
import { flushSync } from 'react-dom';

export default function PrintApp() {
  const [isPrinting, setIsPrinting] = useState(false);
  
  useEffect(() => {
    function handleBeforePrint() {
      flushSync(() => {
        setIsPrinting(true);
      })
    }
    
    function handleAfterPrint() {
      setIsPrinting(false);
    }

    window.addEventListener('beforeprint', handleBeforePrint);
    window.addEventListener('afterprint', handleAfterPrint);
    return () => {
      window.removeEventListener('beforeprint', handleBeforePrint);
      window.removeEventListener('afterprint', handleAfterPrint);
    }
  }, []);
  
  return (
    <>
      <h1>isPrinting: {isPrinting ? 'yes' : 'no'}</h1>
      <button onClick={() => window.print()}>
        Print
      </button>
    </>
  );
}
```

