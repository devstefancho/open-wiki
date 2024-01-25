---
published: true
id: nextjs-env
slug: nextjs-env
title: Nextjs env
description: nextjs-env
tags: ["nextjs-env"]
categories: ["nextjs"]
createdDate: 2023-09-04
updatedDate: 2024-01-26
---


# Next.js 환경변수가 갖는 특징에 관하여

## [process.env][1]
- 환경변수를 포함한 객체를 반환한다
- process.env는 객체(object)이기 때문에
  - 직접 값을 넣어서 변경할 수 있다
  - 속성을 delete 할 수 있음

- Nextjs는 .env.local의 값들을 process.env에 넣어주게 된다.


예를들어, 아래 env.local 값들이 있다고 하자.
`process.env.DB_HOST, process.env.DB_USER, and process.env.DB_PASS` 이렇게 노드 환경에 로드된다.

```
DB_HOST=localhost
DB_USER=myuser
DB_PASS=mypassword
```

위 값들은 data fetching method, API routes, getStaticProps와 같은 곳에서 사용할 수 있다.

### $ 사용방법

- 다른 Variables로 referencing 한다.
- shell script처럼 `$`가 붙은 변수를 expand해준다.

```
TWITTER_USER=nextjs
TWITTER_URL=https://twitter.com/$TWITTER_USER
```

위에서 `TWITTER_URL`는 `https://twitter.com/nextjs`가 된다.


## Bundling Env for the Browser

- `NEXT_PUBLIC_`가 붙지 않은 것만 Node 환경에서 사용가능하다. 즉 browser에서 접근할 수 없다.
- browser에서 환경변수로 셋팅한 값을 사용하기 위해서는 js에 인라인으로 넣어야하는데 `NEXT_PUBLIC_`를 붙여두면, js내에서 하드코딩 값으로 대체된다.

예를들어,

```javascript
console.log(process.env.NEXT_PUBLIC_ANALYTICS_ID);
```
NEXT_PUBLIC_ANALYTICS_ID가 123456으로 설정되어있고, 라면, 로컬에서는 변수의 값으로 123456이라는 값을 출력하게 된다.
빌드시에는 `process.env.NEXT_PUBLIC_ANALYTICS_ID`자체를 123456으로 변경한다.

따라서 빌드를 한 뒤에는 이런 환경변수들에 대해서 대응할 수가 없게 된다.
모든 `NEXT_PUBLIC_`값은 build time에 계산된 값으로 frozen 되기 때문이다.
만약 값을 변경하고 싶다면, 그냥 별도의 API로 client에 값을 제공해주어야 한다.

dynamic lookups은 inline으로 대체 되지 않는다.
```javascript
// This will NOT be inlined, because it uses a variable
const varName = 'NEXT_PUBLIC_ANALYTICS_ID'
setupAnalyticsService(process.env[varName])
 
// This will NOT be inlined, because it uses a variable
const env = process.env
setupAnalyticsService(env.NEXT_PUBLIC_ANALYTICS_ID)
```

## Default Env
일반적으로 .env.local만 있으면 되지만, 환경별로 .env가 필요한 경우가 있다.

예를들면,
next dev에서는 development, next start에서는 production 환경변수를 기본값으로 사용하고 싶을 것이다.

| filename         | role                                                 |
|------------------|------------------------------------------------------|
| .env             | 모든 환경값                                          |
| .env.development | 개발환경                                             |
| .env.production  | production 환경                                      |
| .env*.local      | 보통 .gitignore에 포함되며, secrets 값들이 저장된다. |
| .env.test        | testing 환경                                         |


## Env Load Order (우선순위)
환경변수는 아래 순서로 찾게되며, 일단 찾으면 거기서 멈춘다.
NODE_ENV는 production, development, test값이 허용된다.

1. process.env
2. .env.$(NODE_ENV).local
3. .env.local (NODE_ENV가 test일 때는 체크하지 않음)
4. .env.$(NODE_ENV)
5. .env

테스트해보니까, .env 파일 하나만 사용하는 것이 아니라, 각 환경변수별로 1~5 순서로 변수를 찾는것이다.
그래서 yarn dev시에 .env, .env.local, .env.development의 환경변수들이 모두 섞여서 들어갈 수 있다.


## Good to know
- .env는 무조건 root directory에 있어야한다.
- NODE_ENV 변수가 없으면 next는 자동으로 next dev일때 `NODE_ENV=development`
  다른 커맨드일때는 `NODE_ENV=production`으로 값을 할당해준다.

## Link
- [환경변수 docs](https://nextjs.org/docs/pages/building-your-application/configuring/environment-variables)
- [Example Code](https://github.com/vercel/next.js/tree/canary/examples/environment-variables)


[1]: https://nodejs.org/dist/latest-v8.x/docs/api/process.html#process_process_env "nodejs 환경변수"
