---
published: false
id: personal-todo
slug: personal-todo
title: Personal Todo
summary: personal todo
toc: true
tags: ["projects"]
categories: ["projects"]
createdDate: 2023-09-25
updatedDate: 2024-01-27
---

# Personal Todo

## Blog
- blog는 issue로 할일을 관리
- [ ] API for blog list (with swagger)
- 참고링크
  - https://www.codyhiar.com/blog/
  - https://albertwalicki.com/cssart/lighthouse

## Not Listed
- empower music list 만들기
- github action self-hosted test
- nextjs ssr debugging: https://nextjs.org/docs/pages/building-your-application/configuring/debugging
- type error => quick fix list code
```lua
local function fill_quickfix()
  local files = {
    "src/client/components/templates/ErrorTemplate/components/AsyncBoundary.tsx:20",
    "src/client/components/templates/mainLayoutTemplate/components/MainHeader.tsx:69",
    "src/client/components/templates/mainLayoutTemplate/components/MainNavbar.tsx:20",
    "src/client/utils/auth/middleware.ts:12",
  }
  local items = {}
  for _, file in ipairs(files) do
    local split = vim.split(file, ":")
    local filename = split[1]
    local lnum = tonumber(split[2])
    table.insert(items, { filename = filename, lnum = lnum, text = "파일" })
  end

  vim.fn.setqflist({}, " ", { title = "특정 파일들", items = items })
  vim.api.nvim_command("copen")
end

vim.api.nvim_create_user_command("FillQuickfix", fill_quickfix, {})
```

## In 30mins
- [o] vimr_dashboard 개선 
  - [ ] work todo, personal todo 페이지 추가, 혹은 todo만 있는 todo_index를 만들어서 그걸 telescope로 띄우던지)
  - [X] daily tommorrow 넣기
- [ ] nvim-tree o key to open with intellij
- [X] wiki 분리 (project용, 기록용)
- [ ] fzf advanced 확인(https://github.com/junegunn/fzf/blob/master/ADVANCED.md#ripgrep-integration)
- [ ] marksman language server 설정
- [ ] syntax 적용: https://github.com/johngrib/vimwiki/blob/johngrib/syntax/vimwiki.vim
- [ ] match 적용: https://johngrib.github.io/wiki/vim/match/
- [ ] vimwiki 환경변수 적용 (tmuxinator 적용)
  ```
  project/기록용 위키 분리가 투두에 있길래 정보를 좀 드리자면, 저는 환경변수로 구분해서 사용하고 있어요. https://github.com/malkoG/dotfiles/blob/main/private_dot_config/nvim/lua/zettelkasten.lua#L4
  블로그에 올라가는 위키 시스템 / private repository 요런 구성으로 분리하거든요. 각각의 위키에 진입할때는 yaml config 파일에다가 환경변수를 다르게 세팅해놓고 tmuxinator 프로세스 띄우는 식으로 활용합니다.
  블로그용 - https://github.com/malkoG/dotfiles/blob/main/private_dot_config/tmuxinator/blogging.yml
  Journaling - https://github.com/malkoG/dotfiles/blob/main/private_dot_config/tmuxinator/default.yml
  ```
- [ ] fzf-lua 적용
  ```
  https://github.com/wookayin/dotfiles/blob/master/nvim/lua/config/fzf.lua#L317-L374 (cmd_fzf 함수)
  - fzf-lua의 command_history, search_history 검색을 부름 (텔레스코프도 가능요)
  - 근데 이게 기본적으로 화면 중앙에 팝업으로뜨는데 약간의 window opts과 fzf option을 깎아서 가장 힘든 부분 Ex mode statuseline에 딱 붙여버립니다. 그러면 cmp-cmdline이나 wilder 처럼 커맨드 라인에 팝업이 뜬것 같은 UI적 효과를 만들수 있죠
  - 기본 command_history류 액션이 동작이 바로 액션을 실행해버리는건데 저는 그냥 ex line 에 불러와서 수정하길 원하기때문에 require("fzf-lua.actions").ex_run 이런 별도의 action handler를 썼습니다. telescope..는 이런게 아마 잘 안될거에요
  - 간단하게 쓰시려면 그냥 화면 중앙에 띄우면 whatever 키맵 (q:, ::, <C-R> 등등) => Fzf/Telescope command_history
  ``` 

## 1hour
- [ ] why vim을 사용하고, vim으로 개발하려고 하고 wiki도 vim으로 쓰는건지에 대한 생각을 글로 정리해보기(문제 정의하기)
- [ ] lvim처럼 load된 lsp 서버 lualine에 표시하도록 적용하기
- [ ] null-ls 제거
- [ ] lsp-zero로 변경하기 (lsp 관리 심플하게 하기 위함)
  - https://github.com/VonHeikemen/lsp-zero.nvim
  - https://github.com/ThePrimeagen/init.lua/blob/97c039bb88d8bbbcc9b1e3d0dc716a2ba202c6d2/after/plugin/lsp.lua#L44

## 1hour ~ 2hour
- [ ] .config에 vifm 설정 추가 (08/11)
- [ ] chatgpt nvim 적용해보기 (08/11)
- [ ] mdx treesitter 적용 https://phelipetls.github.io/posts/mdx-syntax-highlight-treesitter-nvim/

## Over 2hour
- [ ] git diff, commit hash 효율적으로 운영할 방안 고민하기
- [ ] snippet 만들기

## Try new plugins
- [ ] oil.nvim
- [ ] folke/zen-mode
- [ ] folke/noice.nvim
- [ ] calendar vim
- [ ] conform.nvim
- [ ] https://github.com/rmagatti/auto-session

## Etc
- [ ] cmp-omni 적용할 수 있는지 확인
  - c-x c-o의 경우, `omnifunc`을 사용하고 있는데 자동완성에서 일부 단어검색으로 자동완성이 되지 않는 불편함이 있음
  - https://github.com/hrsh7th/cmp-omni
- [ ] setup.sh 스크립트 완성하기
  - [ ] setup.sh split zsh and zsh plugin manager function
  - [ ] setup.sh에서 echo -n 이 줄바꿈이 되는 이유 확인하기(줄바꿈이 안되어야함)

## Article
- [ ] [vimwiki](/nvim/vimwiki) 사용방법 정리 및 업로드
- [ ] [iVim](applications/iVim)
- [ ] cssmodule_ls 관련하여 config
- [ ] asdf
- [ ] .config setup.sh (08/13)


