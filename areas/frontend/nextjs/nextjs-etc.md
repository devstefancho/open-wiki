---
published: false
id: nextjs-etc
slug: nextjs-etc
title: Nextjs Etc
summary: nextjs etc
toc: true
tags: ["nextjs"]
categories: ["nextjs"]
createdDate: 2024-01-25
updatedDate: 2024-02-07
---

# Next.js 이것저것

## npm 내부에 console을 넣었는데 console이 chrome console에 나오지 않을때
`rm -rf .next`로 제거후에 `yarn dev`를 시도

## rewrites
next.config.js에서는 rewrites 경로를 지정할 수 있다.
source는 진입경로, destination은 endpoint를 의미한다.
source로 진입하면 destination의 결과가 보이는 것이다.
이때 source는 static 하게 존재하지 않는 페이지어야 한다.

먼저 next.config.js에 rewrites를 설정한다.

```javascript
async rewrites() {
  return [
    {
      source: '/about',
      destination: '/news'
    }
  ]
}
```

아래 파일 구조에서 /about 으로 진입하면 그냥 about 페이지 컨텐츠가 보인다.
```markdown
pages
|- about/index.tsx
|- news/index.tsx
```

이번케이스에는 about 페이지가 없기때문에 동적주소인 slug로 진입된다.
이때는 rewrites가 정상동작하여 news의 content를 보여주게 된다.
```markdown
pages
|- [slug]/index.tsx
|- news/index.tsx
```

간단하게 위 과정을 테스트 하기 위한 nextjs [예시][1]도 있다.


[1]: https://github.com/vercel/next.js/tree/canary/examples/active-class-name

