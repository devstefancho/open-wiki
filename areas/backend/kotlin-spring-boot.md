---
published: false
id: kotlin-spring-boot
slug: kotlin-spring-boot
title: Kotlin Spring Boot
summary: kotlin spring boot
toc: true
tags: ["backend"]
categories: ["backend"]
createdDate: 2024-06-09
updatedDate: 2024-06-09
---

# Kotlin Spring Boot

## jdk 설치
```bash
brew search openjdk
brew install openjdk@17
brew reinstall openjdk@17 # 다시 설치하는 경우
java --version # confirm java version

# java --version 했을때 20 버전으로 나온다면, 아래와 같이 경로 설정 해줌
echo 'export PATH="/opt/homebrew/opt/openjdk@17/bin:$PATH"' >> ~/.zshrc
```

## intellij 에서 설정

```bash
# java 경로 확인
which java # /opt/homebrew/opt/openjdk@17/bin/java
```
`/opt/homebrew/opt/openjdk@17/libexec/openjdk.jdk/Contents/Home/` 와 같이 Home 경로로 경로입력해준다.
sdk 추가 : File -> Project Structure -> Project -> Project SDK -> Add SDK -> JDK -> 경로입력
