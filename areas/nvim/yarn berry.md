---
published: false
id: yarn berry
slug: yarn berry
title: Yarn Berry
summary: yarn berry
toc: true
tags: ["nvim"]
categories: ["nvim"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

# yarn-berry

## Problem
yarn berry에서는 node_modules를 .yarn 밑에 설치해두기 떄문에, import에서 node_modules를 찾지 못하는 문제가 있었음

### Option 1. project 마다 typescript-language-server를 설치
```shell
yarn add typescript-lanuage-server
```
nvim-lspconfig에 setup을 변경 (mason-lspconfig에서도 가능)
```lua
-- before
lspconfig.tsserver.setup{
  cmd = { "typescript-language-server", "--stdio" };
  on_attach = on_attach;
}

-- after
lspconfig.tsserver.setup{
  cmd = { "yarn", "typescript-language-server", "--stdio" };
  on_attach = on_attach;
}
```

### (Recommended) Option 2. 아래 package를 설치하면 해결됨
```shell
yarn dlx @yarnpkg/sdks vim
```

## Conclusion
Option 2가 더 좋은 방법인듯, Option 1은 프로젝트마다 typescript-lanuage-server를 설치해야해서 너무 번거롭ㄴ다.

## Ref
https://github.com/yarnpkg/berry/issues/170#issuecomment-1430869022
