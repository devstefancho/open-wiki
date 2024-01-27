---
published: false
id: intellij
slug: intellij
title: Intellij
description: intellij
tags: ["resources"]
categories: ["resources"]
createdDate: 2023-08-31
updatedDate: 2024-01-23
---

# IntelliJ

## Command line 사용하기
> command line launcher를 실행해야 한다.

1. [Toolbox](https://www.jetbrains.com/toolbox-app/)를 설치한다.
2. 터미널에서 프로젝트경로에 들어가서 `idea .`를 입력하면 열린다.
3. ideaVim 플러그인을 설치
4. ~/.ideavimrc 설정하기
```
""""""""""""""""""""""""
"" Default
""""""""""""""""""""""""
set hlsearch
set scrolloff=3
set ignorecase smartcase
imap jk <esc>
nnoremap <space>n :nohlsearch<cr>

""""""""""""""""""""""""
"" Action
""""""""""""""""""""""""
" built-in navigation to navigated items works better
nnoremap <c-o> :action Back<cr>
nnoremap <c-i> :action Forward<cr>
" but preserve ideavim defaults
nnoremap g<c-o> <c-o>
nnoremap g<c-i> <c-i>

" built in search looks better
nnoremap / :action Find<cr>
" but preserve ideavim search
nnoremap g/ /

""""""""""""""""""""""""
"" Plugin
""""""""""""""""""""""""
Plug 'preservim/nerdtree'
Plug 'tpope/vim-surround'
Plug 'tpope/vim-commentary'
nnoremap <space>e :NERDTreeToggle<cr>

```
5. Preference에서 prettier format on Save 설정하기