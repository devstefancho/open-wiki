---
published: false
id: telescope
slug: telescope
title: Telescope
description: telescope
tags: ["nvim"]
categories: ["nvim"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

## Keymaps
[[quick-fix-list]]
```
<C-t> # open file in a new tab
<C-q> # open quick fix list
```

## Commands
```bash
:Telescope # show all commands
:Telescope commands # command details
:Telescope command_history # all command history
:Telescope find_files cwd=~/.config # search files from current working directory
:Telesope current_buffer_fuzzy_find # search in current file
:Telesope current_buffer_fuzzy_find sorting_strategy=ascending prompt_position=top # search in current file with ascending order, prompt position is top
```

## Speedup Search
find_files do fuzzy find (with definition search) so it's little slow for only searching string
so first do grep_string, then do find_files
1. grep_string
```bash
:Telesope grep_string search=Home # grep search for Home
```
2. find_files 
- grep_string will automatically open prompt to search file which is result from grep_string

## Trouble Shooting
```lua
:Telescope current_buffer_fuzzy_find
```
에서 [에러](https://github.com/nvim-telescope/telescope.nvim/issues/2279)가 발생함, 원인은 Treesitter 패키지와의 충돌 때문이었음
tag를 0.1.0 -> 0.1.1로 업데이트 및 PackerSync 하여 해결함

## Telescope oldfiles 분석

- telescope.nvim에서 `/lua/telescope/builtin/__internal.lua`에 있음
- 우선적으로 `:buffers!`와(열려있는 buffer들로 lsp 제외, lsp는 line 0으로 끝나면 lsp에 의해 열린파일이다.) 
- 다음으로는 위 buffer에 이미 있는것을 제외한 `vim.v.oldfiles`의 리스트들을 사용함 (`:help v:oldfiles`)

## Options
- no single preview line: https://github.com/nvim-telescope/telescope.nvim/issues/2121

## Experimental (git status)

```lua
function M.git_status_grep()
  -- git status로 변경된 파일 목록을 가져옵니다.
  local handle = io.popen("git status --porcelain | awk '{print $2}'")
  local git_files = handle:read("*a")
  handle:close()

  P(git_files)
  -- 파일 목록을 배열로 변환합니다.
  local file_list = {}
  for file in git_files:gmatch("[^\r\n]+") do
    table.insert(file_list, file)
  end

  P(file_list)
  -- telescope의 live_grep을 사용하여 파일 목록 내에서 검색합니다.
  builtin.live_grep({
    prompt_title = "Git Status Grep",
    search_dirs = file_list,
  })
end
```
