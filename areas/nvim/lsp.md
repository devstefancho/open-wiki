---
published: false
id: lsp
slug: lsp
title: Lsp
summary: lsp
toc: true
tags: ["nvim"]
categories: ["nvim"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

neovim has a built in LSP
but you need to configuration to use LSP

## Docs
[Microsoft LSP docs](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/)

## Package
### nvim-lspconfig
This is helper library for LSP setup ([[neovim-plugin]])
`on_attach` will be called when you open the related filtype

## Functions
vim.lsp.buf.rename
- do rename by lsp engine, after renaming You must do `:wa` for save. then confirm all changes with lazygit


## TroubleShooting

### filter react/index.d.ts
when go to definition it recommend a file in the node_modules.
![[lsp-go-to-definition-trouble-shooting.png]]
[reddit](https://www.reddit.com/r/neovim/comments/vfc7hc/lsp_definition_in_tsserver/)
[discussion](https://github.com/typescript-language-server/typescript-language-server/issues/216)
add this in lspconfig.lua
```lua
local function filter(arr, fn)
  print("type::", vim.inspect(type(arr))) -- For Debugging
  if type(arr) ~= "table" then
    return arr
  end

  local filtered = {}
  for k, v in pairs(arr) do
    if fn(v, k, arr) then
      print("value::", vim.inspect(v)) -- For Debugging
      print("key::", k) -- For Debugging
      print("arr::", vim.inspect(arr)) -- For Debugging
      table.insert(filtered, v)
    end
  end

  return filtered
end

local function filterDefinitionFile(value)
  -- See {https://github.com/typescript-language-server/typescript-language-server/issues/216}
  local uri = value.uri or value.targetUri -- uri for Telescope 0.1.0, targetUri for Telescope 0.1.1
  if not uri then
    return false
  end
  return string.match(uri, "index.d.ts") == nil -- Change if you need more specific filtering pattern
end

-- Setup LSP
nvim_lsp.tsserver.setup({
  on_attach = function(client, bufnr)
    on_attach(client, bufnr)
    enable_format_on_save(client, bufnr)
  end,
  filetypes = { "javascript", "typescript", "typescriptreact", "typescript.tsx" },
  cmd = { "typescript-language-server", "--stdio" },
  handlers = {
    ["textDocument/definition"] = function(err, result, method, ...)
      print("------------------------") -- For Debugging
      print(vim.inspect(vim.lsp.handlers["textDocument/definition"])) -- For Debugging
      print("------------------------") -- For Debugging
      if vim.tbl_islist(result) and #result > 1 then
        local filtered_result = filter(result, filterDefinitionFile)
        return vim.lsp.handlers["textDocument/definition"](err, filtered_result, method, ...)
      end

      vim.lsp.handlers["textDocument/definition"](err, result, method, ...)
    end,
  },
})
```

### Did not appear React Component props
Problem
- React props is not show on auto-complete list
How to solve?
- add capability to mason-lspconfig
```lua
-- enable mason
mason.setup()

local capabilities = vim.lsp.protocol.make_client_capabilities()
capabilities = require("cmp_nvim_lsp").default_capabilities(capabilities)
mason_lspconfig.setup_handlers({
  function(server_name)
    require("lspconfig")[server_name].setup({
      capabilities = capabilities,
      on_attach = on_attach,
      settings = servers[server_name],
    })
  end,

  -- Override tsserver lsp config
  ["tsserver"] = function()
    require("lspconfig")["tsserver"].setup({
      capabilities = capabilities,
      -- See {https://github.com/typescript-language-server/typescript-language-server/issues/216}
      on_attach = function(client, bufnr)
        on_attach(client, bufnr)
        enable_format_on_save(client, bufnr)
      end,
      filetypes = { "javascript", "typescript", "typescriptreact", "typescript.tsx" },
      cmd = { "typescript-language-server", "--stdio" },
      handlers = {
        ["textDocument/definition"] = function(err, result, method, ...)
          if vim.tbl_islist(result) and #result > 1 then
            local filtered_result = filter(result, filterDefinitionFile)
            return vim.lsp.handlers["textDocument/definition"](err, filtered_result, method, ...)
          end

          vim.lsp.handlers["textDocument/definition"](err, result, method, ...)
        end,
      },
    })
  end,
})
```

## How to work styled-component IntelliSense
[[styled-components#Add styled-component intelliSense in typescript project]]

## Add html
add 'html' to treesitter
add 'html' to mason-lspconfig
add 'emmet-vim' plugin for auto-complete
see this commit (`4286edba3e796324`)

### How to use emmet
1. [create Doctype](https://github.com/mattn/emmet-vim#quick-tutorial)
```
html:5
```
then `<c-y>,`
