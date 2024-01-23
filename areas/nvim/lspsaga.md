---
published: false
id: lspsaga
slug: lspsaga
title: Lspsaga
description: lspsaga
tags: ["nvim"]
categories: ["nvim"]
createdDate: 2023-10-16
updatedDate: 2024-01-23
---

# Lspsaga

관련 커밋: `3e9069e1681eebc1b6ceebe888f914476216bc28`

## Lazy
```lua
  {
    "glepnir/lspsaga.nvim",
    event = "LspAttach",
    config = function()
      require("devstefancho.lspsaga.setup")
      require("devstefancho.lspsaga.maps")
    end,
    dependencies = {
      { "nvim-tree/nvim-web-devicons" },
      { "nvim-treesitter/nvim-treesitter" },
    },
    cond = false,
  }, -- LSP performant UI
```

## Key Mapping
lua/devstefancho/lspsaga/maps.lua
```lua
local keymap = require("devstefancho.utils").createKeymap("Lspsaga")

-- Diagnostic
keymap("<leader>do", "<Cmd>Lspsaga show_line_diagnostics<CR>", "[D]iagnostics [O]pen") -- same as vim.diagnostic.open_float
keymap("[d", "<Cmd>Lspsaga diagnostic_jump_prev<CR>", "[d]iagnostic [p]rev") -- same as vim.diagnostic.goto_prev
keymap("]d", "<Cmd>Lspsaga diagnostic_jump_next<CR>", "[d]iagnostic [n]ext") -- same as vim.diagnostic.goto_next

-- Definition
keymap("<leader>pd", "<Cmd>Lspsaga peek_definition<CR>", "[p]eek [d]efinition") -- Peek

-- See `:help K` for why this keymap
keymap("K", "<Cmd>Lspsaga hover_doc<CR>", "hover doc") -- same as vim.lsp.buf.hover
```

## Setup
lua/devstefancho/lspsaga/setup.lua
```lua
local status, lspsaga = pcall(require, "lspsaga")
if not status then
  return
end

lspsaga.setup({
  symbol_in_winbar = {
    enable = false,
  },
  finder = {
    max_height = 0.5,
    keys = {
      jump_to = "p",
      edit = { "o", "<CR>" },
      vsplit = "<c-v>", -- same as telescope vsplit keymap
      split = { "i", "<c-x>" }, -- same as telescope split keymap
      tabe = { "t", "<c-t>" }, -- same as telescope split keymap
      quit = { "q", "<ESC>" },
      close_in_preview = "<ESC>",
    },
  },
})
```
