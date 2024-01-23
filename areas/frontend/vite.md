---
published: false
id: vite
slug: vite
title: Vite
description: vite
tags: ["frontend"]
categories: ["frontend"]
createdDate: 2023-08-03
updatedDate: 2024-01-23
---

# Vite

## Multiple Entries
Version 4.1.1
[Multiple Entries](https://github.com/vitejs/vite/discussions/1736)

vite.config.js
```js
import { defineConfig } from "vite";
import { resolve } from "path";

export default defineConfig({
  build: {
    outDir: "./dist",
    lib: {
      entry: [
        resolve(__dirname, "./src/content.ts"),
        resolve(__dirname, "./src/popup.ts"),
      ],
      fileName: (_, entryName) => {
        return `js/${entryName}.js`;
      },
    },
  },
});
```

package.json
```json
{
  "name": "chrome-extension-color-pallete",
  "private": true,
  "version": "0.0.0",
  "type": "module",
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview"
  },
  "repository": {
    "type": "git",
    "url": "git+https://github.com/devstefancho/chrome-extension-color-pallete.git"
  },
  "keywords": [],
  "author": "devstefancho",
  "license": "ISC",
  "bugs": {
    "url": "https://github.com/devstefancho/chrome-extension-color-pallete/issues"
  },
  "homepage": "https://github.com/devstefancho/chrome-extension-color-pallete#readme",
  "devDependencies": {
    "typescript": "^4.9.5",
    "vite": "^4.1.1"
  }
}
```
