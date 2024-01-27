---
published: false
id: luasnip
slug: luasnip
title: Luasnip
summary: luasnip
toc: true
tags: ["nvim"]
categories: ["nvim"]
createdDate: 2023-10-22
updatedDate: 2024-01-23
---

# Luasnip

## Setup

I set LuaSnip as a dependencies of cmp
```lua
return {
  "hrsh7th/nvim-cmp",
  config = function()
    require("devstefancho.cmp.setup")
  end,
  dependencies = {
    {
      "L3MON4D3/LuaSnip",
      -- follow latest release.
      version = "v2.*", -- Replace <CurrentMajor> by the latest released major (first number of latest release)
      -- install jsregexp (optional!).
      build = "make install_jsregexp",
      dependencies = { "rafamadriz/friendly-snippets" },
      config = function()
        local ls = require("luasnip")
        require("luasnip.loaders.from_vscode").lazy_load()

        ls.config.set_config({
          history = true,
          updateevents = "TextChanged,TextChangedI",
        })

        vim.keymap.set({ "i" }, "<M-k>", function()
          ls.expand()
        end, { silent = true })
        vim.keymap.set({ "i", "s" }, "<M-l>", function()
          ls.jump(1)
        end, { silent = true })
        vim.keymap.set({ "i", "s" }, "<M-h>", function()
          ls.jump(-1)
        end, { silent = true })

        vim.keymap.set({ "i", "s" }, "<M-e>", function()
          if ls.choice_active() then
            ls.change_choice(1)
          end
        end, { silent = true })

      end,
    },
    "saadparwaiz1/cmp_luasnip",
    "hrsh7th/cmp-nvim-lua",
    "hrsh7th/cmp-nvim-lsp",
    "hrsh7th/cmp-path",
    "hrsh7th/cmp-buffer",
  },
  cond = require("devstefancho.plugins_status").plugins_status["cmp"],
}
```

### Options
- `require("luasnip.loaders.from_vscode").lazy_load()`: vscode snippets
- `ls.jump(1)`: select next snippet node, -1 is previous node


## Custom Snippet
- [Adding Snippets](https://github.com/L3MON4D3/LuaSnip/blob/master/DOC.md#adding-snippets)

### Example
```lua
ls.add_snippets("all", {
  ls.parser.parse_snippet("expand", "-- what is this??"),
})
```

## Snippet 작성하기
- snippet을 사용하는데 snipmate와 vscode꺼는 커스텀하기가 힘든거 같아서 node를 사용하여 직접 작성해보기로 함

### import lua
먼저 lazy nvim config에서 snippet 작성할 lua 파일을 import 한다.
```lua
require("devstefancho.snips.typescript_react")
```

### write snippets
- print는 파일이 제대로 load되는지 확인하기 위해서 넣었다.
- useState의 capitalize하는 코드는 [여기](https://github.com/L3MON4D3/LuaSnip/discussions/538)를 참고했다.
```lua
-- print("Snippets are being loaded!")
local ls = require("luasnip")
local s = ls.snippet
local i = ls.insert_node
local f = ls.function_node
local extras = require("luasnip.extras")
local l = extras.lambda
local fmt = require("luasnip.extras.fmt").fmt

local get_filename = function()
  return vim.fn.expand("%:t:r")
end

local component = s(
  "rfc",
  fmt(
    [[
import {{ FC }} from 'react';

interface PropTypes {{}}

const {componentName}: FC<PropTypes> = () => {{
  {finishPosition}
  return (
    <>
      {content}
    </>
  );
}};

export default {componentName};
    ]],
    {
      componentName = f(function()
        return get_filename()
      end),
      content = i(1, "Hello World"),
      finishPosition = i(0),
    }
  )
)

local use_state = s(
  "uses",
  fmt(
    [[
const [{1}, set{setter}] = useState<{3}>({2});
    ]],
    {
      i(1, "state"),
      i(2, "initialValue"),
      setter = l(l._1:sub(1, 1):upper() .. l._1:sub(2, -1), 1),
      i(3, "T"),
    }
  )
)

local console_log = s(
  "clo",
  fmt(
    [[
console.log({{{}}});
    ]],
    {
      i(1),
    }
  )
)

ls.add_snippets("typescriptreact", {
  component,
  use_state,
  console_log,
})

ls.add_snippets("typescript", {
  console_log,
})
```
