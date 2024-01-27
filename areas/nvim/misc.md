---
published: false
id: misc
slug: misc
title: Misc
summary: misc
toc: true
tags: ["nvim"]
categories: ["nvim"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

## How to check nvim startup speed
```bash
nvim --startuptime startup.log -c exit && tail -5 startup.log
```

## How to autocomplete to be case insensitive in command mode
[Reddit](https://www.reddit.com/r/neovim/comments/pfm84z/how_to_get_autocomplete_to_be_case_insensitive/)
```
set ignorecase
```
