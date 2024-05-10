---
published: false
id: dadbob
slug: dadbob
title: Dadbob
summary: dadbob
toc: true
tags: ["nvim"]
categories: ["nvim"]
createdDate: 2024-05-09
updatedDate: 2024-05-09
---

# Dadbob

database를 연결하여 sql 쿼리를 실행할 수 있고, ui도 제공함

## Installation with Lazy nivm

```lua
return {
  "kristijanhusak/vim-dadbod-ui",
  dependencies = {
    { "tpope/vim-dadbod", lazy = true },
    { "kristijanhusak/vim-dadbod-completion", ft = { "sql", "mysql", "plsql" }, lazy = true },
  },
  cmd = {
    "DBUI",
    "DBUIToggle",
    "DBUIAddConnection",
    "DBUIFindBuffer",
  },
  init = function()
    -- Your DBUI configuration
    vim.g.db_ui_use_nerd_fonts = 1
  end,
}
```

자동완성을 위해서 hrsh7th/nvim-cmp 설정쪽에 코드를 추가해준다.

```lua
local source_names = {
  luasnip = "[snip]",
  copilot = "[Copilot]",
  nvim_lsp = "[lsp]",
  path = "[path]",
  buffer = "[buf]",
  "vim-dadbod-completion" == "[db]", --> add this line!!
}

cmp.setup({
  -- ... 생략
  sources = cmp.config.sources({
    { name = "luasnip", keyword_length = 3 },
    { name = "copilot" },
    { name = "nvim_lsp" },
    { name = "path" },
    { name = "buffer", keyword_length = 5 },
    { name = "vim-dadbod-completion" }, --> add this line!!
  }),
  formatting = {
    fields = { "kind", "abbr", "menu" },
    max_width = 0,
    format = function(entry, vim_item)
      -- See :help complete-items
      vim_item.menu = source_names[entry.source.name]
      local kind = require("devstefancho.icons").kind[vim_item.kind] or vim_item.kind
      vim_item.kind = kind
      vim_item.dup = duplicates[entry.source.name] or duplicates_default
      return vim_item
    end,
  },
})
```

## DB Connection

```bash
:DBUIAddConnection
```
명령어를 치면 url을 입력하라고 나온다. 이때 로컬로 접속하려면
`postgresql://user_name@localhost/database_name`로 입력한다.

예를들어 psql -U admin -d blog 로 접속할 수 있다면(비밀번호 없는 경우)
`postgresql://admin@localhost/blog`로 입력하면 된다.


## Usage

쿼리 일부만 실행하고 싶다면 visual mode로 감싸고, `DB` 명령어를 넣어주면 된다.
```
:'<,'>DB
```
