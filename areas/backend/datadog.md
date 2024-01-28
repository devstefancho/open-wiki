---
published: false
id: datadog
slug: datadog
title: Datadog
summary: datadog
toc: true
tags: ["backend"]
categories: ["backend"]
createdDate: 2023-08-03
updatedDate: 2024-01-22
---

# Datadog

## Infrastructure > Map

Container, K8s 환경에 대해서 모니터링 가능

- 정상: 초록색
- 장애가까울수록 빨간색

## Upload Source Maps

소스맵을 업로드하면, Error 발생지점이 어디인지 확인 할 수 있다
업로드하기 위해서는 빌드시에 sourcemap을 생성해야한다
그리고 datadog-ci로 sourcemap을 업로드해야한다
최종 배포시에 소스맵은 production에 올라가면 안되므로 빌드후에 삭제 혹은 ignore 처리해야한다.

## Step

### build script
```bash
## build시에 sourcemap을 생성
yarn run build
```

### datadog에 *.js.map 파일 업로드

- upload 경로는 `.next`로 시작하고 path-prefix는 `_next`로 시작한다.
- service는 datadog에 등록한 application name과 일치해야한다.
- release-version은 Rum의 init에 있는 version과 일치해야한다.

```bash
npx datadog-ci sourcemaps upload .next/static \
  --service=my-service-name \
  --release-version="1.0.0" \
  --minified-path-prefix="https://my-domain.co.kr/_next/static"
```

### Option 1) *.js.map 파일삭제
datadog에 업로드후 삭제하는 방법
```bash
find .next/static/chunks -name '*.map' -exec rimraf {} +
```

### Option 2) set .dockerignore
소스맵 파일을 도커 이미지 빌드시에 제외하는 방법
아래는 .dockerignore 예시
```
.idea
.git
.gitignore
.dockerignore
Dockerfile
**/*.js.map
```

## SourceMap visualization
소스맵 업로드가 안되어있는 경우 sourcemap visualization으로 `*.js`와 `.js.map` 파일을 업로드하여 직접 코드 위치를 확인할 수 있음

## Ref
- source map visualization : https://sokra.github.io/source-map-visualization/#custom
- what are source maps : https://web.dev/articles/source-maps
- datadog docs : https://docs.datadoghq.com/ko/real_user_monitoring/guide/upload-javascript-source-maps/?tab=webpackjs
- DD_VERSION : https://docs.datadoghq.com/ko/tracing/trace_collection/library_config/nodejs/
- example : https://github.com/search?q=datadog-ci+.next%2Fstatic+lang%3Ayaml&type=code
