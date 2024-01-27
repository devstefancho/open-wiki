---
published: false
id: google-fonts
slug: google-fonts
title: Google Fonts
summary: google fonts
toc: true
tags: ["frontend"]
categories: ["frontend"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

# Google Font

## Download Noto Serif

### Download Font from google fonts
download from "https://fonts.google.com/noto/specimen/Noto+Serif#styles"
```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@100;200;300;400;500;600;700;800;900&family=Noto+Serif&display=swap" rel="stylesheet">
```

### Download woff2 filetype from google fonts
1. go to "https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@100;200;300;400;500;600;700;800;900&family=Noto+Serif&display=swap" link
2. find "Noto Serif" font face
3. unicode-range를 참고하여 필요한 font face만 추출 (latin, latin-ext가 보통 [발음기호](https://www.ling.upenn.edu/courses/Fall_2014/ling115/phonetics.html)에 필요한 font face이다)
4. save font face
  download it using curl
  ```
  curl -o ./cyrillic-ext.woff2 "https://fonts.gstatic.com/s/notoserif/v23/ga6iaw1J5X9T9RW6j9bNVls-hfgvz8JcMofYTa32J4wsL2JAlAhZqFCTyscKpKrCzi0iNaA.woff2"
  curl -o ./cyrillic.woff2 "https://fonts.gstatic.com/s/notoserif/v23/ga6iaw1J5X9T9RW6j9bNVls-hfgvz8JcMofYTa32J4wsL2JAlAhZqFCTw8cKpKrCzi0iNaA.woff2"
  curl -o ./greek-ext.woff2 "https://fonts.gstatic.com/s/notoserif/v23/ga6iaw1J5X9T9RW6j9bNVls-hfgvz8JcMofYTa32J4wsL2JAlAhZqFCTy8cKpKrCzi0iNaA.woff2"
  curl -o ./greek.woff2 "https://fonts.gstatic.com/s/notoserif/v23/ga6iaw1J5X9T9RW6j9bNVls-hfgvz8JcMofYTa32J4wsL2JAlAhZqFCTxMcKpKrCzi0iNaA.woff2"
  curl -o ./vietnamese.woff2 "https://fonts.gstatic.com/s/notoserif/v23/ga6iaw1J5X9T9RW6j9bNVls-hfgvz8JcMofYTa32J4wsL2JAlAhZqFCTyMcKpKrCzi0iNaA.woff2"
  curl -o ./latin-ext.woff2 "https://fonts.gstatic.com/s/notoserif/v23/ga6iaw1J5X9T9RW6j9bNVls-hfgvz8JcMofYTa32J4wsL2JAlAhZqFCTyccKpKrCzi0iNaA.woff2"
  curl -o ./latin.woff2 "https://fonts.gstatic.com/s/notoserif/v23/ga6iaw1J5X9T9RW6j9bNVls-hfgvz8JcMofYTa32J4wsL2JAlAhZqFCTx8cKpKrCzi0i.woff2"
  ```

