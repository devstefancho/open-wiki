---
published: false
id: nvim-ufo
slug: nvim-ufo
title: Nvim Ufo
summary: nvim ufo
toc: true
tags: ["nvim"]
categories: ["nvim"]
createdDate: 2023-12-03
updatedDate: 2024-01-23
---

# nvim-ufo

## config

```lua
local status, ufo = pcall(require, "ufo")
if not status then
  return
end

local keymap = require("devstefancho.utils").createKeymap("NvimUfo")

ufo.setup({
  provider_selector = function(bufnr, filetype, buftype)
    return { "treesitter", "indent" }
  end,
})

vim.o.foldcolumn = "1" -- '0' is not bad
vim.o.foldlevel = 99 -- Using ufo provider need a large value, feel free to decrease the value
vim.o.foldlevelstart = 99
vim.o.foldenable = true

-- Using ufo provider need remap `zR` and `zM`. If Neovim is 0.6.1, remap yourself
keymap("zR", ufo.openAllFolds, "open all folds")
keymap("zM", ufo.closeAllFolds, "close all folds")
```
