---
published: false
id: lua
slug: lua
title: Lua
summary: lua
toc: true
tags: ["nvim"]
categories: ["nvim"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

# Lua
## require
require load once when entering nvim (not reload)

## EOF
You can run lua in vim file with EOF
```vim
lua << EOF
  print('test')
EOF
```

## source lua file
```
:luafile %
```

## how to re source lua config
확인 더 필요함
```bash
:lua package.loaded.bashbunni = nil <cr>:source ~/.config/nvim/init.lua <cr>
```ㅓ
