---
published: false
id: lualine
slug: lualine
title: Lualine
description: lualine
tags: ["nvim"]
categories: ["nvim"]
createdDate: 2023-10-17
updatedDate: 2024-01-23
---

# Lualine

## Setup
```lua
return {
  "nvim-lualine/lualine.nvim",
  config = function()
    local mocha = require("catppuccin.palettes").get_palette("mocha")

    require("lualine").setup({
      options = {
        icons_enabled = true,
        theme = "catppuccin",
        -- section_separators = { left = "", right = "" },
        -- component_separators = { left = "", right = "" },
        component_separators = "|",
        section_separators = "",
        disabled_filetypes = {},
      },
      sections = {
        lualine_a = {
          {
            "buffers",
            mode = 4, -- buffer number and name
            buffers_color = {
              active = { gui = "bold", bg = "#181825", fg = mocha.rosewater }, -- copy bg color from :hi lualine_c_inactive
              inactive = "lualine_c_inactive",
            },
          },
        },
        lualine_b = {
          -- "branch"
        },
        lualine_c = {
          {
            -- "filename",
            -- file_status = true, -- displays file status (readonly status, modified status)
            -- path = 1, -- 0 = just filename, 1 = relative path, 2 = absolute path
          },
        },
        lualine_x = {
          -- {
          --   "diagnostics",
          --   sources = { "nvim_diagnostic" },
          --   symbols = { error = " ", warn = " ", info = " ", hint = " " },
          -- },
          -- "encoding",
          -- "filetype",
          {
            "filetype",
            cond = nil,
            padding = { left = 1, right = 1 },
          },
          {
            function(msg)
              msg = msg or "LS Inactive"
              local buf_clients = vim.lsp.buf_get_clients()
              if next(buf_clients) == nil then
                -- TODO: clean up this if statement
                if type(msg) == "boolean" or #msg == 0 then
                  return "LS Inactive"
                end
                return msg
              end
              local buf_ft = vim.bo.filetype
              local buf_client_names = {}
              local copilot_active = false

              -- add client
              for _, client in pairs(buf_clients) do
                if client.name ~= "null-ls" and client.name ~= "copilot" then
                  table.insert(buf_client_names, client.name)
                end

                if client.name == "copilot" then
                  copilot_active = true
                end
              end

              local unique_client_names = vim.fn.uniq(buf_client_names)
              local language_servers = "[" .. table.concat(unique_client_names, ", ") .. "]"

              if copilot_active then
                language_servers = language_servers .. "%#SLCopilot#" .. " " .. lvim.icons.git.Octoface .. "%*"
              end

              return language_servers
            end,
            color = { gui = "bold" },
          },
        },
        lualine_y = {
          -- "progress"
        },
        lualine_z = {
          -- "location"
        },
      },
      inactive_sections = {
        -- lualine_a = {},
        -- lualine_b = {},
        -- lualine_c = {
        --   {
        --     "filename",
        --     file_status = true, -- displays file status (readonly status, modified status)
        --     path = 1, -- 0 = just filename, 1 = relative path, 2 = absolute path
        --   },
        -- },
        -- lualine_x = { "location" },
        -- lualine_y = {},
        -- lualine_z = {},
      },
      tabline = {},
      extensions = { "fugitive" },
    })
  end,
}
```

## Experimental (show lsp server)

```lua
lualine_x = {
  {
    "filetype",
    cond = nil,
    padding = { left = 1, right = 1 },
  },
  {
    function(msg)
      msg = msg or "LS Inactive"
      local buf_clients = vim.lsp.buf_get_clients()
      if next(buf_clients) == nil then
        -- TODO: clean up this if statement
        if type(msg) == "boolean" or #msg == 0 then
          return "LS Inactive"
        end
        return msg
      end
      local buf_ft = vim.bo.filetype
      local buf_client_names = {}
      local copilot_active = false

      -- add client
      for _, client in pairs(buf_clients) do
        if client.name ~= "null-ls" and client.name ~= "copilot" then
          table.insert(buf_client_names, client.name)
        end

        if client.name == "copilot" then
          copilot_active = true
        end
      end

      local unique_client_names = vim.fn.uniq(buf_client_names)
      local language_servers = "[" .. table.concat(unique_client_names, ", ") .. "]"

      if copilot_active then
        language_servers = language_servers .. "%#SLCopilot#" .. " " .. lvim.icons.git.Octoface .. "%*"
      end

      return language_servers
    end,
    color = { gui = "bold" },
  },
},
```

## Experimental (show buffers)

```lua
lualine_a = {
  {
    "buffers",
    mode = 4, -- buffer number and name
    buffers_color = {
      active = { gui = "bold", bg = "#181825", fg = mocha.rosewater }, -- copy bg color from :hi lualine_c_inactive
      inactive = "lualine_c_inactive",
    },
  },
},
```
