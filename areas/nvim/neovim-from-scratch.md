---
published: false
id: neovim-from-scratch
slug: neovim-from-scratch
title: Neovim From Scratch
summary: neovim from scratch
toc: true
tags: ["nvim"]
categories: ["nvim"]
createdDate: 2022-12-30
updatedDate: 2024-01-23
---

## lua 파일 생성 순서
base.lua : set 관련
highlights.lua : visual mode highlight 관련인듯 (연구필요)
maps.lua : keymap 관련

## Clipboard
By Default, Vim doesn't sync yanked text with the system clipboard, and vice versa
Clipbard function은 init.lua에 추가

## Install Packer
plugins.lua
```
:PackerSync # 현재 설치한 package의 sync를 맞출때
```

## Install Packer
https://github.com/wbthomason/packer.nvim#quickstart
```
git clone --depth 1 https://github.com/wbthomason/packer.nvim\
 ~/.local/share/nvim/site/pack/packer/start/packer.nvim
```
plugin/neosolarized.rc.lua : neosolarized와 colorbuddy 설치


## Color Scheme
after/plugin/neosolarized.rc.lua : neosolarized와 colorbuddy로 editor 색상 적용

## Status Line
FILE: after/plugin/lualine.lua
https://github.com/nvim-lualine/lualine.nvim


## [[lsp]]
FILE: after/plugin/lspconfig.lua
이걸 설치해야지 auto complete, error check 같은게 된다.
사용할 language를 추가할떄마다 여기서 setup 설정을 해줘야한다.

[neovim/nvim-lspconfig](https://github.com/neovim/nvim-lspconfig)

아래와 같은 에러가 보이면 [lua-language-server](https://github.com/sumneko/lua-language-server)를 설치
```
Spawning language server with cmd: `lua-language-server` failed. The language server is either not installed, missing from PATH, or not executable
```
brew install lua-language-server

아래와 같은 에러가 보이면 [typescript-language-server]()를 설치
```
Spawning language server with cmd: `typescript-language-server` failed. The language server is either not installed, missing from PATH, or not executable
```
npm install -g typescript-language-server typescript

여기까지 설정하면 init.lua에서 Ctrl-n을 입력하면 자동완성을 테스트해볼수 있다.

## AutoComplete
FILE: lspkind.lua
FILE: cmp.lua

[nvim-cmp](https://github.com/hrsh7th/nvim-cmp) : completion engine
[lspkind](https://github.com/onsails/lspkind.nvim) : vscode 형태의 자동완성 ui

> You need a snippet engine to get cmp.nvim to work
cmp 가 동작하기 위해서는 snippet에 있는 engine이 필요하다 (즉 packer로 설치하고, setup할떄 같이 해줘야함)
[luasnip](https://github.com/L3MON4D3/LuaSnip#keymaps) : snippet을 등록하기 위함 (Tab, Shift Tab 입력시 자동완성에서 어떻게 움직일것인지에 대한 예시가 있음)

## TreeSitter
Syntax highlightings
```
brew install tree-sitter
```

TreeSitter가 적용된(설치된) 언어에 대한 정보를 보려면
```
:TSInstallInfo
:TSModuleInfo
```

- Trouble Shooting
  - PackerInstall 할때 에러가 한번 발생한다면 [Installation](https://github.com/nvim-treesitter/nvim-treesitter/wiki/Installation)을 참고한다.
  - vim.cmd에서 에러가 나는 경우, TSModuleInfo에 vim이 설치되어 있다고 하더라도 `:TSInstall vim`으로 다시 설치해준다.

## TreeSitter Playground
[playground](https://github.com/nvim-treesitter/playground)

## Auto Tag, Auto Pair
함수나 element를 만들때 반대쪽 tag를 자동으로 만들어줌

예를 들어
```
<h1> 이렇게 입력하면 <h1></h1>이 완성됨
```

## Telescope (검색기)
FILE: after/plugin/telescope.lua
brew install rg (이게 속도가 빠름)

## Telescope-File-Browser
[telescope 와 비교](https://www.libhunt.com/compare-telescope-file-browser.nvim-vs-telescope.nvim)

## Web Devicons
file icon 표시

## BufferLine
FILE: after/plugin/bufferline.lua
tab, buffer, window에 대한 플러그인
tab의 색상이나 탭이동 액션에 대해서 mapping 할 수 있다.

## Colorizer
hash color의 색상표시하기

## Lspsaga
LSP UI 제공해줌
hint, 사용하는 곳 찾아주는것 등을 해줌

## Code Formatting
LSP내에서 각 언어에 대해서 code formatting을 제공해주지는 않는다.
따라서 null-ls를 사용해야한다. (null-ls 자체가 LSP)

```
npm install -g @fsouza/prettierd
```

eslint는 mason을 통해서 설치해야 정상적으로 동작했다.
```
 -- managing & installing lsp servers, linters & formatters
  use("williamboman/mason.nvim") -- in charge of managing lsp servers, linters & formatters
  use("williamboman/mason-lspconfig.nvim") -- bridges gap b/w mason & lspconfig

-- formatting & linting
  use("jose-elias-alvarez/null-ls.nvim") -- configure formatters & linters
  use("jayp0521/mason-null-ls.nvim") -- bridges gap b/w mason & null-ls
```

## Git
[gitsigns](https://github.com/lewis6991/gitsigns.nvim)
[git]()


## reference blog
- how to setup neovim (https://dev.to/craftzdog/my-neovim-setup-for-react-typescript-tailwind-css-etc-58fb)
- about lua (https://github.com/krapjost/nvim-lua-guide-kr)
- About LSP Configuration (https://levelup.gitconnected.com/a-step-by-step-guide-to-configuring-lsp-in-neovim-for-coding-in-next-js-a052f500da2)

----------------
## vi tips

Search keymap
```
:map # custom keymap list
:h ^w # Ctrl-w in normal mode
:h i^w # Ctrl-w in insert mode
:h v^w # Ctrl-w in visual mode
:h c^w # Ctrl-w in command mode
:tab h ^w # open help in another tab
```

Clipboard
```
:h 'clipboard' # about clipboard
```

Replace
```
:'<,'>:s/abc/123 # visual block 한 상태에서 command mode(:) 들어가서 :s abc를 123으로 replace
```

Buffer
```
:bf # go to prev buffer (gf 이후에 돌아갈때)
```

Tab
```
Ctrl-w gf # gf를 새탭으로 열어줌
gt # next tab
gT # prev tab
N gt # N번째 tab

:tab h telescope.nvim # 새로운 탭에서 helper page 열기
```
