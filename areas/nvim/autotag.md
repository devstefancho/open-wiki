---
published: false
id: autotag
slug: autotag
title: Autotag
description: autotag
tags: ["nvim"]
categories: ["nvim"]
createdDate: 2023-10-23
updatedDate: 2024-01-23
---


# Autotag

## Setup
```lua
  {
    "windwp/nvim-ts-autotag",
    opts = {
      enable_close_on_slash = false,
    },
    cond = require("devstefancho.plugins_status").plugins_status["autotag"],
  },
```

## Options
- self-close시에 [버그](https://github.com/windwp/nvim-ts-autotag/issues/125)로 인해서 enable_close_on_slash는 꺼두어야함
