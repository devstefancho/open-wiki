---
published: false
id: nvimTree
slug: nvimTree
title: NvimTree
summary: nvimTree
toc: true
tags: ["nvim"]
categories: ["nvim"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

## Shortcuts
```markdown
`P` go to parent directory
```

## Commands
```markdown
`:NvimTreeToggle` Open or close the tree. Takes an optional path argument.
`:NvimTreeFocus` Open the tree if it is closed, and then focus on the tree.
`:NvimTreeFindFile` Move the cursor in the tree for the current buffer, opening folders if needed.
`:NvimTreeCollapse` Collapses the nvim-tree recursively.
```

## Trouble Shooting
gx not work because of netrw disable config
`disable_netrw` is disable all netrw features, so remove it
- [fix commit](https://github.com/devstefancho/init.lua/commit/0ec957f06550cbdab445e16b3f5c17cf0b9b50df)
- [more reference](https://github.com/nvim-tree/nvim-tree.lua/issues/47)
