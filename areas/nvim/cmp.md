---
published: false
id: cmp
slug: cmp
title: Cmp
description: cmp
tags: ["nvim"]
categories: ["nvim"]
createdDate: 2023-10-22
updatedDate: 2024-01-23
---

# Cmp

> This is Auto Completion [[neovim-plugin]]
> Auto Completion need `luasnip` for better performance

## Setup
```lua
cmp.setup({
	snippet = {
		expand = function(args)
			require("luasnip").lsp_expand(args.body)
		end,
	},
 -- Other Config..
  sources = cmp.config.sources({
    { name = "luasnip" },
    { name = "copilot" },
    { name = "nvim_lsp" },
    { name = "path" },
    { name = "buffer", keyword_length = 5 },
  }),
 })
```
### Setup Options
- keyword_length: don't show buffer if is shorter than length
- `["<CR>"] = cmp.mapping.confirm({ select = true })` select가 true이면, 명확히 첫번째 메뉴항목을 선택하지 않아도 엔터입력시 자동완성이 된다.


## Functions
scroll_docs : move up and down in the right popup window docs
![[cmp-scroll_docs.png]]


## Ref
- [Youtube About LuaSnip](https://youtu.be/puWgHa7k3SY?t=4059)
