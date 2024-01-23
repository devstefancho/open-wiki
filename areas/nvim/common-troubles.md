---
published: false
id: common-troubles
slug: common-troubles
title: Common Troubles
description: common troubles
tags: ["nvim"]
categories: ["nvim"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

## ftplugin error
Why this happend?
I did `vi -u /Users/stefancho/.local/share/lunarvim/lvim/init.lua src` to just -u option test then my nvim command is broken For example when I do `vi src/test.tsx`, this error message shows, I don't have lvim in my .config/nvim. so I have no idea why this happened. deleting neovim and reinstall is not working

![[nvim-error-ftplugin.png]]

How Solve it?
remove .local/share/nvim/site/after/ftplugin

## Too slow cursor movement
추정되는 여러가지 이유들이 있는데,
버벅대는 현상은 :help 페이지에서도 마찬가지였다

### 너무 많은 플러그인들이 load되는 경우
packer -> lazy.nvim으로 변경했음

### 과도한 syntax highlight
tokyonight -> rose-pine으로 변경

### tmux 환경
tmux 환경에서 jk로 위아래 cursor 이동시 lag 현상이 심하다는게 느껴졌다.

### scrolloff=999의 문제
파일 가장아래 라인에서 zz했을때 위아래 999줄이 있어야해서 마지막라인에서는 중앙위치가 적용되지 않는다.
scrolloff=8정도로 하면 문제가 없다.
재현 방법은 scrolloff=999로 하고, G 키로 파일 맨 아래로 이동후 zz를 해본다음에 어떤것을 입력하려고 하면 커서 위치가 화면 맨 아래로 재이동 된다.

### 그외
cursorline
cursorcolumn
relativenumber

