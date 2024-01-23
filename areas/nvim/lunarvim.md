---
published: false
id: lunarvim
slug: lunarvim
title: Lunarvim
description: lunarvim
tags: ["nvim"]
categories: ["nvim"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

# Lunarvim

## How to Contribute?
[config](https://discord.com/channels/701530051140780102/704077577920446636/1066076538128306216)

`lvim` `lvimd` (dev) `lvimc` (check prs)

`cp -r ~/.local/share/lunarvim ~/.local/share/lunarvimd && cp -r ~/.local/bin/lvim ~/.local/bin/lvimd` and then edit the contents of `~/.local/bin/lvimd` to be

```bash
#!/bin/sh

export LUNARVIM_RUNTIME_DIR="${LUNARVIM_RUNTIME_DIR:-"$HOME/.local/share/lunarvimd"}"
export LUNARVIM_CONFIG_DIR="${LUNARVIM_CONFIG_DIR:-"$HOME/.config/lvimd"}"
export LUNARVIM_CACHE_DIR="${LUNARVIM_CACHE_DIR:-"$HOME/.cache/lvimd"}"
   
exec nvim -u "$LUNARVIM_RUNTIME_DIR/lvim/init.lua" "$@"
```
   
then you can go to `~/.local/share/lunarvimd/lvim` and `gh pr checkout <PR>` or anything else you want to do
