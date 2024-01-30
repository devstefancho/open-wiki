---
published: true
id: algolia-docsearch
slug: algolia-docsearch 
title: Algolia DocSearch 적용기
summary: docsearch로 블로그에 검색엔진 적용하기
toc: true
tags: ["frontend"]
categories: ["frontend"]
createdDate: 2024-01-30
updatedDate: 2024-01-30
---

# DocSearch 적용하기 

최근에 많은 블로그들이 algolia를 검색엔진으로 사용하고 있다.
UI가 깔끔하고 검색결과도 괜찮게 나와서 나도 적용해보기로 했다.
(현재 블로그의 우측상단에 있는 검색바가 docsearch 이다.)

## Pre-requisite

algolia 계정을 생성해둔다.


## crawler 신청하기

algolia에서 검색바를 적용하는 방법은 여러가지가 있는데 그중에서 가장 쉬운 방법이다.
docSearch의 crawler가 내 블로그들을 크롤링해서 검색엔진에 색인을 해준다.
크롤링할때 h1~h6까지 그리고 내용과 url을 모두 색인해주기때문에 나는 script 적용만 하면된다.

crawler를 사용하기 위해서는 유료로 사용하거나 신청을 해야한다.
문서형태의 블로그의 경우 [Apply Page][1]에서 신청을 하면된다.
신청을 하고, 약 3일뒤에 승인이 되었다.
승인이 되면 appId, appKey, indexName을 받을 수 있는데, 이걸 적용하면 된다.


## crawler admin 확인하기

https://crawler.algolia.com/admin 에서 crawler의 설정값을 확인할 수 있다.


## docsearch 컴포넌트 적용하기

나는 react를 사용하고 있기 때문에 `@docsearch/react` 패키지를 사용하였다.
아래처럼 DocSearch 컴포넌트를 import하고, 필요한 키값만 넘겨주면 끝이다!

```typescriptreact
// css 적용을 위해서 cdn 링크를 추가해준다. 
// import '@docsearch/css'를 하는 경우에는 스타일이 일부 적용되지 않아서 cdn을 사용하였다.
export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="en">
      <head>
        <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@docsearch/css@3"/>
      </head>
      <body className={inter.className}>{children}</body>
    </html>
  );
}
```

```typescriptreact
import { DocSearch } from '@docsearch/react';

function AlgoliaSearchBox({ appId, apiKey, indexName }) {
  return <DocSearch appId={appId} indexName={indexName} apiKey={apiKey} />;
}
```

## 후기

docsearch를 적용하는 방법은 매우 쉬웠으나, 그 과정이 쉽지는 않았다.
일단 이 검색바 ui가 docsearch라는 것 자체를 몰라서 꽤 헤매었다.

헤매는 과정에서 instant search와 auto complete 기능도 적용해보았다.
algolia의 이 두가지 기능은 적어도 현재의 내 블로그에는 적합하지 않았다.

그리고 docsearch를 적용하려고 했을때는 crawler를 사용할줄 몰라서 또 엄청 삽질을 했다.
무료로 쓰기 위해서는 직접 신청해야한다는걸 알게 되었고, 될지 안될지 모르는 상황에서 신청만 해두었다.
그리고 3일뒤에 승인이 되었고 설레는 마음으로 적용을 해보았다.
결과는 성공적이었다, 꽤나 뿌듯한 기분으로 이 글을 마무리한다.


[1]: https://docsearch.algolia.com/apply/
