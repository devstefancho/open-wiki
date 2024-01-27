---
published: false
id: treesitter
slug: treesitter
title: Treesitter
summary: treesitter
toc: true
tags: ["nvim"]
categories: ["nvim"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

Syntax Highlight and more

## highlights.scm

highlight의 구조에 따라서 색상을 변경할 수 있다.

- https://github.com/ikatyang/tree-sitter-markdown/issues/23
- https://www.reddit.com/r/neovim/comments/kxa56x/treesitter_multilevel_headings_in_different_colors/
- https://alpha2phi.medium.com/neovim-101-tree-sitter-usage-fa3e8bed921a

- YT: https://www.youtube.com/watch?v=twrNV_BZ2jU
- DotFiles: https://github.com/duboisf/.dotfiles/blob/64430770959b10376bef2e8f0b000ab4d1079c24/nvim/.config/nvim/lua/core/colorscheme/theme.lua#L47

## Configuration

```lua
local status, ts = pcall(require, "nvim-treesitter.configs")
if not status then
  return
end

-- How to Setup (https://github.com/nvim-treesitter/nvim-treesitter#modules)
ts.setup({
  ensure_installed = {
    "lua",
    "tsx",
    "json",
    "css",
    "javascript",
    "typescript",
    "help",
    "vim",
    "markdown",
    "html",
  },
  highlight = {
    enable = true,
    disable = {},
  },
  indent = {
    enable = true,
    disable = {},
  },
  context_commentstring = {
    enable = true,
  },
  incremental_selection = { -- This will do incremental selection
    enable = true,
    keymaps = {
      init_selection = "<c-space>",
      node_incremental = "<c-space>",
      scope_incremental = "<c-s>",
      node_decremental = "grm",
    },
  }
})

```


## nvim-treesitter-context
show the function context preview on the top


## nvim-treesitter-textobjects
- customize inside and around selection
- movement step by textobject (i.e. go to next function)


## Trouble Shooting
### Slow Response
- 현상: 특정 파일내에서 엄청나게 느려짐
- 해결: `cd ~/.local/share/nvim/lazy` 에서 nvim-treesitter를 삭제한후에 다시 Lazy로 설치되니까 괜찮아짐
- 후속조치: 아래코드 추가함([nvim-treesitter 설정에서 가져옴](https://github.com/nvim-treesitter/nvim-treesitter#modules))
```lua
    disable = function(lang, buf)
        local max_filesize = 100 * 1024 -- 100 KB
        local ok, stats = pcall(vim.loop.fs_stat, vim.api.nvim_buf_get_name(buf))
        if ok and stats and stats.size > max_filesize then
            return true
        end
    end,
```
`~/.local/state/nvim/`에 log들이 있는데 다음에 문제 생기면 여기 확인해보라고함([discord link](https://discord.com/channels/701530051140780102/863276893059153971/1167861192723804180))
`~/.local/state/nvim/`에 있는 log들만 지워도 better performance를 줄 수있다고 함, 간혹 엄청 큰 log 파일이 생성되기도 하기 때문에


## Playground
- playground is deprecated, instead use Inspect, InspectTree command
