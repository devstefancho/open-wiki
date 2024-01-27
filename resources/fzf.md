---
published: false
id: fzf
slug: fzf
title: Fzf
summary: fzf
toc: true
tags: ["resources"]
categories: ["resources"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

# fzf

## trouble
문제
`vi **<tab>`이 동작하지 않는 문제
해결
`vi **<Ctrl-t>`를 시도한 후에 정상동작함

## Ctrl + R
command history를 보여주는듯하다


## select git status from fzf and open with vim

```bash
vi $(git status --short | fzf | awk '{print $2}')
```


