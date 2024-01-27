---
published: false
id: colorscheme
slug: colorscheme
title: Colorscheme
summary: colorscheme
toc: true
tags: ["nvim"]
categories: ["nvim"]
createdDate: 2023-09-28
updatedDate: 2024-01-23
---

# Nvim Colorscheme

> I use catppuccin instead of tokyonight

## steps
1. install catppuccin with lazy.nvim
2. change lualine config
3. change bufferline config
4. customize color with palette
  a. see https://github.com/catppuccin/nvim/blob/main/lua/catppuccin/palettes


## install catppuccin

```lua
return {
  "catppuccin/nvim",
  name = "catppuccin",
  priority = 1000,
  config = function()
    vim.cmd("colorscheme catppuccin") -- set colorscheme
  end,
}
```


## bufferline example

```lua
return {
  {
    "akinsho/bufferline.nvim",
    version = "v3.*",
    dependencies = {
      "nvim-tree/nvim-web-devicons",
    },
    config = function()
      -- mocha colors {https://github.com/catppuccin/nvim/blob/main/lua/catppuccin/palettes/mocha.lua}
      local mocha = require("catppuccin.palettes").get_palette("mocha")

      require("bufferline").setup({
        options = {
          mode = "buffers",
          separator_style = "thin",
          show_close_icon = false,
          show_buffer_close_icons = false,
          color_icons = true,
          truncate_names = false,
          diagnostics = "nvim_lsp",
        },
        -- fg is font-color, bg is background-color
        highlights = require("catppuccin.groups.integrations.bufferline").get({
          styles = { "italic", "bold" },
          custom = {
            all = {
              fill = { bg = mocha.base }, -- background color for rest area of bufferline
            },
            mocha = {
              background = {
                fg = mocha.surface1, -- inactive buffer font color
              },
            },
            latte = {
              background = { fg = "#000000" },
            },
          },
        }),
      })
    end,
  },
}
```


## lualine example
```lua
return {
  "nvim-lualine/lualine.nvim",
  config = function()
    require("lualine").setup({
      options = {
        icons_enabled = true,
        theme = "catppuccin",
        section_separators = { left = "", right = "" },
        component_separators = { left = "", right = "" },
        disabled_filetypes = {},
      },
      sections = {
        lualine_a = { "mode" },
        lualine_b = { "branch" },
        lualine_c = {
          {
            "filename",
            file_status = true, -- displays file status (readonly status, modified status)
            path = 1, -- 0 = just filename, 1 = relative path, 2 = absolute path
          },
        },
        lualine_x = {
          {
            "diagnostics",
            sources = { "nvim_diagnostic" },
            symbols = { error = " ", warn = " ", info = " ", hint = " " },
          },
          "encoding",
          "filetype",
        },
        lualine_y = { "progress" },
        lualine_z = { "location" },
      },
      inactive_sections = {
        lualine_a = {},
        lualine_b = {},
        lualine_c = {
          {
            "filename",
            file_status = true, -- displays file status (readonly status, modified status)
            path = 1, -- 0 = just filename, 1 = relative path, 2 = absolute path
          },
        },
        lualine_x = { "location" },
        lualine_y = {},
        lualine_z = {},
      },
      tabline = {},
      extensions = { "fugitive" },
    })
  end,
}
```
