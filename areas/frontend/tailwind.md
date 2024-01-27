---
published: false
id: tailwind
slug: tailwind
title: Tailwind
summary: tailwind
toc: true
tags: ["frontend"]
categories: ["frontend"]
createdDate: 2023-08-03
updatedDate: 2024-01-23
---


# Tailwind

## tailwind.config.js
```javascript
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: ["./dist/**/*.{html,js}"],
  theme: {
    extend: {
      colors: {},
    },
  },
  plugins: [],
};
```

The content section of your tailwind.config.js file is where you configure the paths to all of your HTML templates, JavaScript components, and any other source files that contain Tailwind class names.
(content에 내가 사용하는 파일위치가 있어야함, 예를들어 index.html이 dist에 있으면 위 예제와 같이 사용해야한다.)

 
## nextjs 13에서 자동완성 되지 않은 문제
 
### TL;DR
- 재설치 후 문제 사라짐 ()
```bash
# https://github.com/tailwindlabs/tailwindcss-intellisense/tree/HEAD/packages/tailwindcss-language-server#readme
npm install -g @tailwindcss/language-server
```

### 원인파악
- nextjs12는 문제가 없고(npx create-next-app@12) nextjs13(npx create-next-app@latest)에서만 문제가 발생함
- tailwind.config.js의 차이점이 nextjs12는 commonJS를 사용했고, nextjs13은 타입스크립트 때문에 ECMAScript(ESM) 모듈을 사용했음
- nextjs13 프로젝트에 commonJS를 사용하니까 자동완성이 정상적으로 되었음
- 우선 문제는 tailwindcss-language-server가 제대로 로드되지 않은게 문제였던거 같음 lvim으로 확인해봤을때 ESM 사용한 nextjs13은 root Directory가 Not Found로 떴음
  나의 nvim에서는 tailwindcss-language-server가 정상적으로 로드되는것으로 표시 되었음 (로드는 되었지만, 정상동작하지 않은거 같음)

### 앞으로 주의할점
- nextjs13이 새로나온 다음에 tailwindcss-language-server가 정상동작하는지 확인하지 않았음, framework의 새로운 major버전이 나오면 language-server를 최신버전으로 업데이트 해야할듯


## 특정파일에서 tailwind class 로드되지 않는 문제

아래와 같이 파일 path가 추가되면 tailwind.config.js를 수정해야한다.
```js
// tailwind.config.js
  content: [
    './src/pages/**/*.{js,ts,jsx,tsx,mdx}',
    './src/components/**/*.{js,ts,jsx,tsx,mdx}',
    './src/app/**/*.{js,ts,jsx,tsx,mdx}',
    './src/layouts/**/*.{js,ts,jsx,tsx,mdx}',
  ],
```
