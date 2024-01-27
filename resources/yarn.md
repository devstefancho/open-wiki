---
published: false
id: yarn
slug: yarn
title: Yarn
summary: yarn
toc: true
tags: ["resources"]
categories: ["resources"]
createdDate: 2024-01-07
updatedDate: 2024-01-23
---

# Yarn

## Yarn 1.x to berry
yarn 1 버전은 npm을 통해서 설치할 수 있다.
현재 yarn 1 버전은 더이상 업데이트가 되지 않는다. (최소한의 유지보수만 되고 있는 듯 하다.)
yarn 2 버전이상부터는 global 설치를 할 수 없다.
프로젝트내에서 설치를 해야한다.

```bash
## corepack 설치 (https://github.com/nodejs/corepack?tab=readme-ov-file)
npm uninstall -g yarn pnpm
npm install -g corepack

## yarn berry migration (https://yarnpkg.com/migration/guide)
corepack enable
cd ~/project # project로 이동
yarn set version berry
# .npmrc 와 .yarnrc 파일을 .yarnrc.yml 로 변경
yarn install # yarn.lock migration
git commit . # commit all changes

yarn --version # 버전확인
```

