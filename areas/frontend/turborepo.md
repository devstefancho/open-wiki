---
published: false
id: turborepo
slug: turborepo
title: Turborepo
description: turborepo
tags: ["frontend"]
categories: ["frontend"]
createdDate: 2023-08-15
updatedDate: 2024-01-23
---


# TurboRepo

## Start
1. [설치하기](https://turbo.build/repo/docs/installing#install-globally)
2. [예시 가져오기](https://turbo.build/repo/docs/getting-started/from-example)

실제로 실행한 커맨드는 아래와 같다.
```bash
yarn global add turbo # turbo command 설치
npx create-turbo -e basic # Nextjs 예시로 시작
yarn turbo dev # 개발 서버 실행
```


## Caveats
dependencies의 [`*`는 아무 버전](https://docs.npmjs.com/cli/v9/configuring-npm/package-json#dependencies)을 의미한다


## Test
- .prettierrc를 root dir에 생성하고, 저장해보니 /apps와 /packages에 있는 모든 파일들이 format 되었다.
