---
published: false
id: vercel
slug: vercel
title: Vercel
summary: vercel
toc: true
tags: ["infra"]
categories: ["infra"]
createdDate: 2023-08-03
updatedDate: 2024-01-27
---

# Vercel

## How to rollback to specific deployment
1.  global로 vercel cli 설치
  ```
  yarn global add vercel
  ```
2. .zshrc 에 아래 코드 추가 (yarn global 에 설치된 패키지 사용하기 위해서 필요함)
  ```
  export PATH="$(yarn global bin):$PATH"
  ```
3. 아래 실행되는지 확인
  ```
  vercel --version
  ```
4. 버셀 로그인
  ```
  vercel certs
  ```
5. 만약 프로젝트가 여러개인경우 my-mall로 변경해야함 (아래 커맨드로 변경가능)
  ```
  vercel switch
  ```
6. alias 등록
  ```
  vercel alias mymall-feature-branch.vercel.app mymall.vercel.app
  ```
(이렇게 되면 vercel alias A B 인경우 B url 접속시 A로 접속됨)  
(A는 롤백하고 싶은 custom 도메인, B는 실제 접속 도메인) 7. alias list 확인하기
7. alias 확인
  ```
  vercel alias ls
  ```
추가로 `vercel rollback`이라는 커맨드가 있는것 같은데, 이거 용도는 확인필요 (edited)
8. rollback to latest deployment
  ```
  vercel alias mymall-feature-branch.vercel.app mymall.vercel.app
  ```
`vercel alias A B`
![[vercel-deployment-domain.png]]
A(pink area) : Always up-to-date this git branch domain
B : specific deployment domain

## deploy nestjs to vercel
- `vercel build`로 local에서 build 결과물을 확인할 수 있음 (`.vercel` 디렉토리 생성됨)
- `vercel deploy`(또는 `vercel .`)으로 현재 디렉토리를 바로 배포할 수 있음
- 배포 타겟을 바꾸고 싶으면 .vercel을 제거하고 다시 vercel deploy를 하면 됨
- yarn build 같은게 안돌아가므로 prebuild 같은거 만들어서 돌릴 수 없음
- 따라서 필요한 리소스는 prebuild에서 clone 받거나 할 수 없기 때문에, 배포할때 static file들 다 포함시켜서 배포해야함
- 배포완료후에 Vercel > Project 선택 > Deployments 탭 > Deployments 선택 > Source 탭에서 실제 배포된 결과물을 볼 수 있음

vercel.config 작성시 주의해야함
- vercel.config 작성할떄 경로 잘못넣으면 404 발생함
```javascript
{
  "version": 2,
  "builds": [
    {
      "src": "src/main.ts",
      "use": "@vercel/node"
    }
  ],
  "routes": [
    {
      "src": "/(.*)",
      "dest": "src/main.ts",
      "methods": ["GET", "POST", "PUT", "DELETE"]
    }
  ]
}
```

## Serverless function cached

- Purge Cache
  - serverless function은 cache가 될 수 있는데, 이 경우 api 호출하더라도 이미 cache된 function 결과를 가져오기 때문에 api 적용 결과와 다를 수 있음
  - Project > Settings > Data Cache > Purge Everything 버튼을 클릭해서 cache를 지워주면 됨

- 기본적으로 캐시 끄는 방법 [링크](https://vercel.com/docs/infrastructure/data-cache/manage-data-cache#disabling-vercel-data-cache)
```js
// 1. page 마다 cache 무효화
export const fetchCache = 'force-no-store';

// 2. 개별 요청마다 cache 무효화
const res = await fetch('https://example.com', { cache: 'no-store' });
```


