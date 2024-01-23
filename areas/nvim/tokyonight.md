---
published: false
id: tokyonight
slug: tokyonight
title: Tokyonight
description: tokyonight
tags: ["nvim"]
categories: ["nvim"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

# Tokyo Night

## Setup

```lua
{
"folke/tokyonight.nvim",
config = function()
  require("tokyonight").setup({
    style = "night",
    styles = {},
    on_colors = function(colors)
      colors.hint = colors.orange
      colors.error = "#ff0000"
    end,
    -- See color code (https://github.com/folke/tokyonight.nvim/blob/main/lua/tokyonight/theme.lua)
    on_highlights = function(hl, c)
      hl.Title = { fg = c.magenta }
    end,
  })
  vim.cmd("colorscheme tokyonight-night")
end,
priority = 1000,
},
```

## Change highlight

change color for heading

```
:hi Title
:hi Title guifg=blue
```

## Italic

italic will only work when italic font installed
(https://github.com/folke/tokyonight.nvim/issues/190)
