---
published: false
id: diagnostic
slug: diagnostic
title: Diagnostic
description: diagnostic
tags: ["nvim"]
categories: ["nvim"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

#  Diagnostic

## How to hide diagnostic
https://github.com/neovim/nvim-lspconfig/issues/662

```lua
vim.diagnostic.config({
  virtual_text = { severity = "Error" }, -- 우측에 텍스트
  underline = { severity = "Error" }, -- 밑줄
  signs = true, -- number line 왼쪽에 아이콘
})
```

## with LSP handlers
https://www.reddit.com/r/neovim/comments/lciqhp/disable_annoying_underline_when_make_errors/
