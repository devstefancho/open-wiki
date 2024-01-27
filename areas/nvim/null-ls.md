---
published: false
id: null-ls
slug: null-ls
title: Null Ls
summary: null ls
toc: true
tags: ["nvim"]
categories: ["nvim"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

## Prettier 사용하지 않는 경우
.prettierignore을 사용하는 파일의 root directory에 추가한다.

## Trouble Shooting
if default stylua indentation has 8, then add `.stylua.toml` under `~/.config/nvim`
```toml
indent_type = "Spaces"
indent_width = 2
```

## eslint_d

Failed to load라는 에러가 발생하는 경우 [.eslint_d를 제거](https://github.com/mantoni/eslint_d.js/issues/235)하고 프로젝트를 다시 실행해본다.
주로 파일 첫줄 import에서 에러가 발생함
```bash
cat ~/.eslint_d # 파일있는지 확인해보기
rm ~/.eslint_d # 제거하기
```
