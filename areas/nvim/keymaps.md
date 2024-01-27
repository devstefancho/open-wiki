---
published: false
id: keymaps
slug: keymaps
title: Keymaps
summary: keymaps
toc: true
tags: ["nvim"]
categories: ["nvim"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

# Keymap
## Keymap options
```lua
vim.keymap.set("n", "gd", vim.fn.something, { buffer = 0 })
```
for example, buffer means it is work only for current buffer 
if you `:vnew` (which is create empty new buffer) and go to new buffer `:nmap gd` then nothing show up.
because this keymap is only for current buffer

## Keymap tips
```lua
vim.keymap.set("n", "<leader>dn", vim.diagnostic.goto_next, { buffer = 0 })
```  

### Use leader key
if this below example left hand is "dn", then mistakenly delete word until next word in normal mode
so add leader key to avoid these inconvinient

## cmd vs colon(:)
cmd is better than colon(:)
because cmd execute without entering normal mode
```lua
vim.keymap.set("n", "<leader>ff", ":Telescope find_files<CR>", { buffer = 0 })
vim.keymap.set("n", "<leader>ff", "<cmd>Telescope find_files<CR>", { buffer = 0 })
vim.keymap.set("n", "<leader>ff", ":Telescope find_files<CR>:print('hello')", { buffer = 0 })
```

## multiple command in one keymap
This is not practical example, but you can use like this
```lua
vim.keymap.set("n", "<leader>fa", ":NvimTreeToggle <CR>:lua print('hello')<CR>", { buffer = 0 })
```
