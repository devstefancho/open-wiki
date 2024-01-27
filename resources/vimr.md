---
published: false
id: vimr
slug: vimr
title: Vimr
summary: vimr
toc: true
tags: ["resources"]
categories: ["resources"]
createdDate: 2023-08-30
updatedDate: 2024-01-23
---

# VimR

현재 alacritty에서는 한글입력시 키가 씹히는 현상 ([issue 참고](https://github.com/alacritty/alacritty/issues/6942))이 있다.
한글 개인문서 작성을 위해서 vimr을 사용하기로 했다. (vimr은 한국인이 만든 vim gui인것 같다.)


## Install

```bash
brew install --cask vimr
```


## Commands

```bash
vimr ~/Vault # Vault 경로에서 vimr을 실행한다.
```


## Preference setup

1. `command + ,` 으로 설정창을 연다.
2. Appearance > Default Font > JetBrains Mono NFM으로 변경한다.
  (nvim-tree의 devicon이 정상적으로 표시되기 위해서 필요함)
3. Keys 에서 Use Left Option as Meta Key를 체크한다.


## VimR 전용 vim config 설정하기
- [vimr 적용 commit](https://github.com/devstefancho/.config/commit/567e93e0bb7dd38fbc1d86de916c0dfe1d112c47)

