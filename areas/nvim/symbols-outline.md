---
published: false
id: symbols-outline
slug: symbols-outline
title: Symbols Outline
summary: symbols outline
toc: true
tags: ["nvim"]
categories: ["nvim"]
createdDate: 2023-10-15
updatedDate: 2024-01-23
---

# Symbols Outline

파일내의 symbol 구조를 보여주는 plugins

## plugins
- https://github.com/simrat39/symbols-outline.nvim (`선택`)
- https://github.com/stevearc/aerial.nvim
- https://github.com/liuchengxu/vista.vim

## aerial
vim의 기본 fold 단축키와 유사하여 괜찮음
zm 동작이 제대로 되지 않는 문제가 있음(nvim-ufo를 삭제한 이유와 동일함)

## symbols-outline
선택한 이유: kemap이 심플하고 nvim-tree와 비슷함, 내부 구조가 자세하게 보임

```lua
return {
  "simrat39/symbols-outline.nvim",
  config = function()
    require("symbols-outline").setup({
      highlight_hovered_item = true,
      show_guides = true,
      auto_preview = false,
      position = "right",
      relative_width = true,
      width = 25,
      auto_close = false,
      show_numbers = false,
      show_relative_numbers = false,
      show_symbol_details = true,
      preview_bg_highlight = "Pmenu",
      autofold_depth = nil,
      auto_unfold_hover = true,
      fold_markers = { "", "" },
      wrap = false,
      keymaps = { -- These keymaps can be a string or a table for multiple keys
        close = { "<Esc>", "q" },
        goto_location = "<Cr>",
        focus_location = { "o", "<Tab>" },
        hover_symbol = "<C-space>",
        toggle_preview = "K",
        rename_symbol = "r",
        code_actions = "a",
        fold = "h",
        unfold = "l",
        fold_all = "W",
        unfold_all = "E",
        fold_reset = "R",
      },
      lsp_blacklist = {},
      symbol_blacklist = {},
      symbols = {
        File = { icon = "", hl = "@text.uri" },
        Module = { icon = "M", hl = "@namespace" },
        Namespace = { icon = "Ns", hl = "@namespace" },
        Package = { icon = "", hl = "@namespace" },
        Class = { icon = "𝓒", hl = "@type" },
        Method = { icon = "ƒ", hl = "@method" },
        Property = { icon = "", hl = "@method" },
        Field = { icon = "Field", hl = "@field" },
        Constructor = { icon = "", hl = "@constructor" },
        Enum = { icon = "E", hl = "@type" }, -- typescript enum
        Interface = { icon = "I", hl = "@type" }, -- typescript interface
        Function = { icon = "", hl = "@function" },
        Variable = { icon = "T", hl = "@constant" }, -- typescript type
        Constant = { icon = "", hl = "@constant" },
        String = { icon = "𝓐", hl = "@string" },
        Number = { icon = "#", hl = "@number" },
        Boolean = { icon = "⊨", hl = "@boolean" },
        Array = { icon = "", hl = "@constant" },
        Object = { icon = "⦿", hl = "@type" },
        Key = { icon = "🔐", hl = "@type" },
        Null = { icon = "NULL", hl = "@type" },
        EnumMember = { icon = "", hl = "@field" },
        Struct = { icon = "𝓢", hl = "@type" },
        Event = { icon = "T", hl = "@type" },
        Operator = { icon = "+", hl = "@operator" },
        TypeParameter = { icon = "𝙏", hl = "@parameter" },
        Component = { icon = "C", hl = "@function" }, -- react component
        Fragment = { icon = "Fr", hl = "@constant" },
      },
    })
  end,
}
```
