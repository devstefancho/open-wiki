---
published: true
id: nextjs-app-router
slug: nextjs-app-router
title: Nextjs App Router
description: nextjs app router
tags: ["nextjs"]
categories: ["nextjs"]
createdDate: 2024-01-16
updatedDate: 2024-01-26
---

# Next.js App Router 방식에 관하여

> App Router 방식의 특징들에 대해서 간략하게 정리해봄

## Server Component
- app 밑에 있는 컴포넌트는 모두 Server Component로 동작하기 때문에 async를 붙일 수 있다.

## serverless function의 cache
- 캐시 처리를 따로 해주지 않으면, serverless 함수결과값이 캐싱이 되기 때문에, 아래와 같이 처리필요

```tsx
/** Disable Vercel Data Cache */
export const fetchCache = 'force-no-store';

const result = await fetch(baseUrl + '/contents', {
  cache: 'no-cache',
});
```

## serverless function
- async function 을 그대로 작성하기만 하면 serverless 로 동작한다. (즉 api route 랑 똑같이 동작)

```tsx
async function getBlog(slug: string): Promise<{
  html: string;
  frontmatter: Frontmatter;
  content: string;
}> {
  const result = await fetch(
    `${process.env.API_BACKEND_BASE_URL}/content/${slug}`
  );
  return await result.json();
}
```

## Page 컴포넌트의 params
- url 파라미터 사용할때 바로 파라미터로 받아서 사용함

```tsx
type Params = {
  params: {
    slug: string;
  };
};

export default async function Page({ params: { slug } }: Params) {
```

## layout.tsx 파일
- `_app.tsx`는 없어졌고, layout 컴포넌트로 대체됨

```tsx
export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  const themeFromCookie =
    cookies().get('x-theme')?.value === Theme.DARK ? Theme.DARK : Theme.LIGHT;

  return (
    <html lang="en" className={themeFromCookie}>
      <body className={inter.className}>
        <ThemeProvider xTheme={themeFromCookie}>
          <NavBar />
          {children}
          <Footer />
        </ThemeProvider>
      </body>
    </html>
  );
}
```

## loading.tsx 파일
- 모든 페이지에서 사용할 수 있는 loading 컴포넌트가 생김 (이거 안쓰는 페이지는 Suspense로 감싸면 됨)

```tsx
export default function Loading() {
  return <div>Loading 중입니다!</div>;
}
```

## 'use client'
```js
'use client'; // 페이지 최상단에 써주면 됨
```
- [use client][2]를 하더라도 서버에서 static HTML 생성할때는 컴포넌트가 사용된다.
- To optimize the initial page load, Next.js will use React's APIs 
  to render a static HTML preview on the server for both Client and Server Components.


## file convention
- 파일명에 대해서 [컨벤션][1]이 생겼다.

[1]: https://nextjs.org/docs/app/building-your-application/routing#file-conventions
[2]: https://nextjs.org/docs/app/building-your-application/rendering/client-components#full-page-load
