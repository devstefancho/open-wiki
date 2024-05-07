---
published: false
id: jira-cli
slug: jira-cli
title: Jira Cli
summary: jira cli
toc: true
tags: ["resources"]
categories: ["resources"]
createdDate: 2024-01-23
updatedDate: 2024-02-21
---

# Jira Cli
[jira-cli repo](https://github.com/ankitpokhrel/jira-cli)

## 설치
- [brew로 설치하기](https://github.com/ankitpokhrel/jira-cli/wiki/Installation#homebrew)

```bash
brew tap ankitpokhrel/jira-cli
brew install jira-cli
```

### 환경변수
[API token 페이지](https://id.atlassian.com/manage-profile/security/api-tokens) 페이지에서 token 발행
.zshrc에 환경변수 추가
```bash
export JIRA_API_TOKEN=<YOUR_API_TOKEN>
```

### 초기설정
```bash
jira init # default project를 변경하고 싶을때도 사용한다.
## 선택항목 ##
# Installation Type: Cloud
# Link to Jira server: https://mydomain.atlassian.net/
# Login email: devstefancho@mydomain.co.kr
# Default project: READINGTF
# Default board: Scrum FMP
```

init으로 설정된 내용들은 `~/.config/.jira/.config.yml`에서 확인하면 된다.

## 활용방법

모든 이슈 리스트 확인하기
```bash
jira issue list
```

내가 보고 있는 이슈 리스트 확인하기
```bash
jira issue list -w
```

특정 이슈 확인하기
```bash
jira issue view <ISSUE_KEY> # jira issue view FMP-1
```

이슈 상태 이동하기
```bash
jira issue move FMP-1 "In Progress"
```

스프린트에서 내 작업 찾기
```bash
jira sprint list -a$(jira me)
```

다음 스프린트 작업 확인하기
```bash
jira sprint list --state future
```

내가 여태 열어본 issue history
```bash
jira issue list --history
```

QA 항목만 보기
```bash
jira issue list --history -a$(jira me) -t"Subtask-in-qa"
```

이슈 생성
```bash
jira issue create -tTask -s"제목을 쓰세요" -P<EPIC NAME>-1234
jira issue create -tTask -s"제목을 쓰세요" -P<epic name>-1234 # 대소문자 상관없음
jira issue create -tTask -s"제목을 쓰세요" -PFrontEnd-1234 # 프로젝트명이 Frontend인 경우의 예시 (Frontend-1234 하위에 만듦)
```
