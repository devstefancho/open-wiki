---
published: false
id: asdf
slug: asdf
title: Asdf
description: asdf
tags: ["resources"]
categories: ["resources"]
createdDate: 2023-08-16
updatedDate: 2024-01-23
---

# asdf

## 설치된 버전확인
```bash
asdf current # 현재 버전들
asdf list nodejs # 설치된 버전들
cat ~/.tool-versions # 버전확인
```

## node 설치하기
```bash
asdf plugin add nodejs https://github.com/asdf-vm/asdf-nodejs.git # plugin 설치
asdf install nodejs 18.14.2 # 특정 버전으로 설치
asdf global nodejs 18.14.2 # 노드 버전 설정
node -v # node 버전확인

asdf install nodejs latest # 최신버전 nodejs 설치
asdf global nodejs latest # 최신버전 nodejs 적용
asdf current # 현재 설치된 버전
```

## yarn 설치하기
```bash
brew install gpg pinentry # 두개 설치해야 정상적으로 yarn 설치됨
asdf plugin-add yarn # plugin 설치
asdf install yarn 1.22.18 # 특정버전으로 설치
asdf global yarn 1.22.18 # yarn 버전 설정
yarn -v
```

## which <something>가 다른경로를 보고 있는 경우
1. which 커맨드로 실행 경로 확인
```bash
which node # node가 어떤경로 보고 있는지 확인
which yarn # yarn이 어떤경로 보고 있는지 확인 
```

2. `/opt/homebrew/bin`를 보고 있다면 아래과정으로 진행
```bash
echo $PATH # 경로 확인(경로에는 ~/.asdf/shims 가 포함되어 있어야함 )
```

3. .zshrc에 asdf 경로 추가(아래 코드 추가 후 `source ~/.zshrc`)
```bash
# asdf path
export PATH="$HOME/.asdf/shims:$PATH"
```
