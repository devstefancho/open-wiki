---
published: false
id: winbar
slug: winbar
title: Winbar
description: winbar
tags: ["nvim"]
categories: ["nvim"]
createdDate: 2023-10-14
updatedDate: 2024-01-23
---

# Winbar

## plugins support winbar

- https://github.com/utilyre/barbecue.nvim
- https://github.com/Bekaboo/dropbar.nvim
- https://github.com/SmiteshP/nvim-navic
- https://github.com/nvimdev/lspsaga.nvim


## barbecue configuration

```lua
return {
  "utilyre/barbecue.nvim",
  name = "barbecue",
  version = "*",
  dependencies = {
    "SmiteshP/nvim-navic",
    "nvim-tree/nvim-web-devicons", -- optional dependency
  },
  opts = {
    -- configurations go here
    -- default configuration {https://github.com/utilyre/barbecue.nvim#-configuration}
    show_dirname = false,
  },
}
```


## lspsaga winbar configuration

```lua
symbol_in_winbar = {
  enable = true,
  separator = " ",
  ignore_patterns={},
  hide_keyword = true,
  show_file = true,
  folder_level = 2,
  respect_root = false,
  color_mode = true,
},
```
