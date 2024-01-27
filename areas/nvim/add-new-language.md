---
published: false
id: add-new-language
slug: add-new-language
title: Add New Language
summary: add new language
toc: true
tags: ["nvim"]
categories: ["nvim"]
createdDate: 2023-09-29
updatedDate: 2024-01-23
---

# Add New Language

## Rust 추가하기

- [git commit](https://github.com/devstefancho/.config/commit/00ce375db39868de0c3cc36f99544957a5cca72d)

1. language server 추가를 위해 mason에 추가(자동완성)
2. treesitter에 추가
3. formatter 적용(rust.vim 패키지 설치함)

```lua
-- mason-lspconfig.lua
  function M.setup()
     "lua_ls",
     "cssmodules_ls",
     "bashls",
     "rust_analyzer",
   },
 })
 
-- plugins/rust.lua
  return {
    "rust-lang/rust.vim",
    init = function()
      -- setup rust formatter {https://github.com/rust-lang/rust.vim#formatting-with-rustfmt}
      vim.g.rustfmt_autosave = 1
    end,
  }

-- plugins/treesitter.lua
   "html",
   "graphql",
   "yaml",
   "rust",
```
