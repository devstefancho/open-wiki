---
published: false
id: iVim
slug: iVim
title: IVim
description: iVim
tags: ["resources"]
categories: ["resources"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

# iVim
vim을 사용할 수 있는 앱이다.

## Install
App Store에서 iVim을 다운받기

## iVim 설정

### Ctrl키 변경
설정 > 일반 > 키보드 > 하드웨어 키보드 > 보조키 > capslock, ctrl키 설정을 서로 바꾸기

### 항상 nerdtree 시작
1. 설정 > iVim
2. LAUNCH OPTIONS
  - Argument: ./my-vault/index.md
  - Always: on

## vimrc
```bash
:e .vimrc # .vimrc를 생성하여 수정
```

```vim
let mapleader = ' '
set background=light
colorscheme gruvbox

" For Vimwiki
set nocompatible
filetype plugin on
syntax on

" Setter
set rnu
set number
set hlsearch
set clipboard+=unnamed
set ignorecase
set shiftwidth=2

" Map
inoremap jk <Esc>
nnoremap <C-,> :VimwikiToggleListItem<cr>
nnoremap sv :vsplit<cr><C-w>w
nnoremap ss :split<cr><C-w>w
nnoremap <leader>n :noh<cr>
inoremap <C-h> <Left>
inoremap <C-j> <Down>
inoremap <C-k> <Up>
inoremap <C-l> <Right>
cnoremap <C-h> <Left>
cnoremap <C-j> <Down>
cnoremap <C-k> <Up>
cnoremap <C-l> <Right>
nnoremap <leader>h <C-w>h
nnoremap <leader>j <C-w>j
nnoremap <leader>k <C-w>k
nnoremap <leader>l <C-w>l

" Variables
let g:vimwiki_listsym_rejected = 'X'
let g:vimwiki_listsyms = "✗○◐●✓"

let g:ctrlp_working_path_mode = 'ra'

" Highlight
hi MarkdownHeaderTitle cterm=bold ctermfg=136 ctermbg=223 guifg=#b57614 guibg=#ebdbb2 gui=bold
hi! link Title MarkdownHeaderTitle 
```

## Sync with iCloud
```bash
:idocuments # open iCloud file (저장하면 iCloud에 바로반영된다)
:iexdir add /private # 탐색할 directory 추가 (이걸 꼭 해줘야 nerdtree에서 파일 검색이 가능해진다)
:iexdir list # directory index 확인하기
:iexdir goto ,0 # 현재 디렉토리 경로를 0번 index 경로로 변경한다
```

## Plugin Manager
iplug add는 유료기능으로 6,600 won을 지불하였다.
(Plugin 설치를 위해서 유료 기능이 필수는 아니지만, 이게 편할 것 같아서 구매했다)

```bash
iplug add https://github.com/<owner>/<repo> # 새로운 플러그인 추가
iplug list # 설치된 플러그인 리스트 확인
iplug remove https://github.com/<owner>/<repo> # 플러그인 지우기
```

- 현재 사용중인 플러그인들 (`let @*=execute('iplug list')`로 리스트 가져옴)
  - [vimwiki]  https://github.com/vimwiki/vimwiki.git : Note
  - [gruvbox]  https://github.com/morhetz/gruvbox.git : Theme
  - [ctrlp.vim]  https://github.com/kien/ctrlp.vim : File Search (Not useful)
  - [nerdtree]  https://github.com/preservim/nerdtree : File Explorer
  - [tabular]  https://github.com/godlygeek/tabular : Dependency for vim-markdown plugin
  - [vim-markdown]  https://github.com/preservim/vim-markdown : Markdown syntax highlighting

## Trouble Shooting

### 디렉토리가 비어있는 경우
간혹 NerdTree에서 디렉토리 밑에 있는 파일이 보이지 않는 경우가 있는데, idocuments로
해당경로에 있는 파일 하나를 열어주면 나머지 파일도 보인다.

### :q의 문제점
윈도우가 하나만 열려있을때 :q를 하면 완전히 꺼지기 때문에 :bd를 활용하자

### VimwikiToggleListItem
`<Ctrl-Space>` 는 `VimwikiToggleListItem` keymap인데, 아이패드에서는 기본적으로 `<Ctrl-Space>`가 언어전환으로 매핑되어있다.
이건 바꿀 수 없기 때문에, `VimwikiToggleListItem`를 `<Ctrl-,>`로 매핑해서 사용하고 있다.

