---
published: false
id: indent-blankline
slug: indent-blankline
title: Indent Blankline
summary: indent blankline
toc: true
tags: ["nvim"]
categories: ["nvim"]
createdDate: 2023-10-21
updatedDate: 2024-01-23
---

# indent-blankline

Pros
- guideline for indentation

Cons
- nvim performance issue in highly nested code structure

```lua
return {
  "lukas-reineke/indent-blankline.nvim",
  main = "ibl",
  config = function()
    local ibl = require("ibl")

    ibl.setup({
      indent = {
        char = "▏",
      },
    })
  end,
  cond = false,
}
```
