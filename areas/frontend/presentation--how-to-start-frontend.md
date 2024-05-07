---
published: false
id: presentation--how-to-start-frontend
slug: presentation--how-to-start-frontend
title: Presentation  How To Start Frontend
summary: presentation  how to start frontend
toc: true
tags: ["frontend"]
categories: ["frontend"]
createdDate: 2024-05-04
updatedDate: 2024-05-06
---

# Frontend 개발 시작하기

## ide 설정

## Nextjs 설치
https://nextjs.org/docs/getting-started/installation
- nodejs 18.17 이상으로 설치
- npx create-next-app@latest
```
What is your project named? my-app
Would you like to use TypeScript? Yes
Would you like to use ESLint? Yes
Would you like to use Tailwind CSS? Yes
Would you like to use `src/` directory? Yes
Would you like to use App Router? (recommended) No
Would you like to customize the default import alias (@/*)? No
```

## nextjs 디렉토리 구조설명
- pages 디렉토리
- api route

## eslint, prettier 적용 (lint는 어렵다면 패스)
- 정상적으로 prettier가 설정되어있다면, 저장했을때 prettierrc에 따라 적용되야함

## husky, lint-staged 적용

## package.json 설명 (scripts, dependency, version)
- scripts (start, build, dev)
- dependency 추가하기 (yarn add)

## env 파일 및 script 설정
- cp env/.env.beta .env
- cp env/.env.production .env

## yarn build && yarn start
- 실행해보기

## ts, tsx 설명 (jsx 문법설명)
- 기본적인 page 파일 생성
- ts 파일의 용도 (util, hooks, constants, interface)

## functional component 설명 (props와 state)
- props로 component간 데이터 전달
- state로 component 내부 데이터 관리

## event handler 설명
- button의 onClick에 event handler 적용
- event handler에서 http request 보내기 (axios 설명)

## api router를 proxy로 사용하기
- CORS 이슈 해결


## vercel 배포
## 배포스크립트 작성

