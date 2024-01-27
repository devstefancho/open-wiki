---
published: false
id: bun
slug: bun
title: Bun
summary: bun
toc: true
tags: ["resources"]
categories: ["resources"]
createdDate: 2024-01-22
updatedDate: 2024-01-23
---

# bun

## Performance

| package     | install                | install from cache   |
| ----------- | ---------------------- | -------------------- |
| yarn 1.2.21 | yarn install (44.61s)  | yarn.lock (14.36s)   |
| yarn 4.0.2  | yarn install (30.902s) | .yarnrc.yml (7.723s) |
| bun 1.0.25  | bun install (20.57s)   | bun.lockb (2.19s)    |


| package     | build      | time   |
|-------------|------------|--------|
| yarn 1.2.21 | yarn build | 41.94s |
| bun 1.0.25  | bun build  | 38.86s |

## Dev Server for Next.js
```bash
bun --bun run dev # use bun
bun run dev # use nodejs (nextjs App Router는 아직 내부적으로 nodejs만 지원함)
```

## Bun init
bun init으로 프로젝트 기본 세팅이 생성됨
```bash
bun init
```

## Typescript
타입스크립트를 바로 실행할 수 있음
```bash
bun run index.ts
```

## Example with React

server-side-rendering 코드 만들어보기
```jsx
// Test.tsx
import React from "react";

export default function Component(props: { message: string }) {
  return <body><h1 style={{ color: "orangered" }}>{props.message}</h1></body>;
}

// index.tsx
import { renderToReadableStream } from "react-dom/server";
import Test from "./Test";

Bun.serve({
  async fetch(request) {
    const stream = await renderToReadableStream(<Test message="Hello world!" />);
    return new Response(stream, { headers: { "Content-Type": "text/html" } });
  }
});

console.log("Listening ...");
```

```bash
bun init
touch index.tsx Test.tsx # 위 예시코드를 복사
bun index.tsx
```

## install to start
```bash
bun install
bun run build
bun start
```

## Print dependency
```bash
bun pm ls # print dependencies excluding nth-order dependencies
bun pm ls --all # print all dependencies
```

## 1.0.25 기준 한계점

- nodejs와 호환성이 100%가 아님
- nextjs에서는 App Router 지원하지 않음
