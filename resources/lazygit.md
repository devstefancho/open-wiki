---
published: false
id: lazygit
slug: lazygit
title: Lazygit
summary: lazygit
toc: true
tags: ["resources"]
categories: ["resources"]
createdDate: 2024-01-07
updatedDate: 2024-01-23
---

# Lazygit

git terminal UI 이다.

## Key Binding
나는 몇가지 설정을 아래와 같이 수정했다.

```yaml
    quit: "<esc>" # YarnDev용 터미널을 닫을때 쓰는 키와 일치시킴
    return: "h" # 목록으로 빠져나오는 키
    togglePanel: "" # 불필요하여 제거
    prevBlock-alt: "" # 불필요하여 제거
    nextBlock-alt: "" # 불필요하여 제거
    select: "-" # Git Fugitivie에서 사용하는 키와 일치시킴
    goInto: "l" # 상세로 들어가는 키
```

## .config로 Path 설정하기

yml 파일을 읽는 path를 수정할 수 있다.
.zshrc에 아래 코드를 추가하고 `source ~/.zshrc`를 실행하자.
```bash
export XDG_CONFIG_HOME="$HOME/.config"
```

`lazygit/state.yml`은 tracking이 필요하지 않다고 판단되어 `.gitignore`에 추가했다

## Ref
상세 설정은 [링크](https://github.com/jesseduffield/lazygit/blob/master/docs/Config.md)를 참고
