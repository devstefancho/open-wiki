---
published: false
id: github-action
slug: github-action
title: Github Action
summary: github action
toc: true
tags: ["infra"]
categories: ["infra"]
createdDate: 2024-01-07
updatedDate: 2024-01-23
---

# GitHub Action

## GITHUB_TOKEN
github workflow가 정상적으로 실행되지 않을때 아래와 같이 설정해야함
Settings > Actions > General > Workflow permissions
- `Read and write permissions` 선택
- `Allow GitHub Actions to create and approve pull requests` 체크

## labeler
- 자동으로 라벨을 적용해주는 [github action](https://github.com/marketplace/actions/labeler)이다.
- 파일두개가 필요하다

`.github/workflows/labeling_pr.yml`는 labeler를 실행하는 용도
```yaml
name: "Pull Request Labeler"
on:
- pull_request_target

### develop target인 PR에만 적용하는 경우 ###
# on:
#   pull_request:
#     branches:
#       - develop

jobs:
  labeler:
    permissions:
      contents: read
      pull-requests: write
    runs-on: [self-hosted, HRA-control, dockerd, small]
    steps:
    - uses: actions/checkout@v4 # checkout 필수
    - uses: actions/labeler@v5
```

`.github/labeler.yml`은 적용조건 및 라벨이름
```yaml
# add learning label when apps/learning files changed
learning:
- changed-files:
  - any-glob-to-any-file: 'apps/learning/**/*'

# add member label when apps/member files changed
member:
- changed-files:
  - any-glob-to-any-file: 'apps/member/**/*'

# add reading label when apps/reading files changed
reading:
- changed-files:
  - any-glob-to-any-file: 'apps/reading/**/*'

# add webview label when apps/webview files changed
webview:
- changed-files:
  - any-glob-to-any-file: 'apps/webview/**/*'

# add www label when apps/www files changed
www:
- changed-files:
  - any-glob-to-any-file: 'apps/www/**/*'
```
