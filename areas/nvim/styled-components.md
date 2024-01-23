---
published: false
id: styled-components
slug: styled-components
title: Styled Components
description: styled components
tags: ["nvim"]
categories: ["nvim"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---


## Add styled-component intelliSense in typescript project

- [참고링크](https://github.com/styled-components/typescript-styled-plugin)

add the plugin in tsconfig and `yarn add --dev @styled/typescript-styled-plugin` in the  terminal of project

**tsconfig.json**
```json
{
  "compilerOptions": {
    "plugins": [
      {
        "name": "@styled/typescript-styled-plugin"
      }
}
```

**Before**
![[styled-component-no-intellisense.png]]

**After**
![[styled-component-with-intellisense.png]]

## 추가참고
- [Coc 사용하기](https://github.com/neoclide/coc.nvim/issues/660)
