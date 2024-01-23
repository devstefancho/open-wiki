---
published: false
id: gatsby
slug: gatsby
title: Gatsby
description: gatsby
tags: ["frontend"]
categories: ["frontend"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

# Gatsby

https://www.gatsbyjs.com/docs/tutorial/part-1/

## default README

````markdown
<p align="center">
  <a href="https://www.gatsbyjs.com/?utm_source=starter&utm_medium=readme&utm_campaign=minimal-starter-ts">
    <img alt="Gatsby" src="https://www.gatsbyjs.com/Gatsby-Monogram.svg" width="60" />
  </a>
</p>
<h1 align="center">
  Gatsby Minimal TypeScript Starter
</h1>
```

## 🚀 Quick start

1.  **Create a Gatsby site.**

    Use the Gatsby CLI to create a new site, specifying the minimal TypeScript starter.

    ```shell
    # create a new Gatsby site using the minimal TypeScript starter
    npm init gatsby -- -ts
    ```

2.  **Start developing.**

    Navigate into your new site’s directory and start it up.

    ```shell
    cd my-gatsby-site/
    npm run develop
    ```

3.  **Open the code and start customizing!**

    Your site is now running at http://localhost:8000!

    Edit `src/pages/index.tsx` to see your site update in real-time!

4.  **Learn more**

    - [Documentation](https://www.gatsbyjs.com/docs/?utm_source=starter&utm_medium=readme&utm_campaign=minimal-starter-ts)
    - [Tutorials](https://www.gatsbyjs.com/tutorial/?utm_source=starter&utm_medium=readme&utm_campaign=minimal-starter-ts)
    - [Guides](https://www.gatsbyjs.com/tutorial/?utm_source=starter&utm_medium=readme&utm_campaign=minimal-starter-ts)
    - [API Reference](https://www.gatsbyjs.com/docs/api-reference/?utm_source=starter&utm_medium=readme&utm_campaign=minimal-starter-ts)
    - [Plugin Library](https://www.gatsbyjs.com/plugins?utm_source=starter&utm_medium=readme&utm_campaign=minimal-starter-ts)
    - [Cheat Sheet](https://www.gatsbyjs.com/docs/cheat-sheet/?utm_source=starter&utm_medium=readme&utm_campaign=minimal-starter-ts)

## 🚀 Quick start (Netlify)

Deploy this starter with one click on [Netlify](https://app.netlify.com/signup):

[<img src="https://www.netlify.com/img/deploy/button.svg" alt="Deploy to Netlify" />](https://app.netlify.com/start/deploy?repository=https://github.com/gatsbyjs/gatsby-starter-minimal-ts)

## 설치하기

```
npm i -g gatsby-cli
gatsby new
```

- 레이아웃 컴포넌트(src/components/Layout.tsx)
- Head Component (src/components/Seo.tsx)
- gatsby-config.ts에 site meta data가 있음
- Server side에서는 useStaticQuery

## Plugin

### gatsby-source-filesystem

- ex. root/blog-posts/first-blog.md와 같이 파일시스템에 접근해서 사용가능
- 아래처럼 코드 쓰면 페이지에서 query 결과 바로 사용할 수 있음

  ```tsx
  export default function Blog({ data }: PageProps<Queries.BlogTitlesQuery>) {
    console.log(data); // query 결과 바로 사용가능
    return (
      <Layout title="Blog">
        <p>The Most Recent Blog Post.</p>
      </Layout>
    );
  }
  // query 타입은 자동으로 생성되어 있음
  export const query = graphql`
    query BlogTitles {
      allFiles {
        nodes {
          name
        }
      }
    }
  `;
  export const Head = () => <Seo title="Blog" />;
  ```

### gatsby-plugin-mdx

- `pages/blog/{mdx.frontmatter__slug}.tsx`

```tsx
export const query = graphql`
  query PostDetail($frontmatter__slug: String) {
    mdx(frontmatter: { slug: { eq: $frontmatter__slug } }) {
      frontmatter {
        author
        category
        date
        title
        slug
      }
    }
  }
`;
```

### gatsby-plugin-image

정적이미지

- webp로 변환시켜서 가져옴, height에 적합한 사이즈로 변환해줌

```tsx
<StaticImage height={200} src="" alt="" />
```

동적이미지

- getImage 함수 사용하기

```tsx
export default function Blog() {
  const image = getImage(
    data.mdx?.frontmatter?.headerImage?.childImageSharp?.gatsbyImageData!
  );
  return <GatsbyImage image={image} alt={data.mdx?.frontmatter?.title!} />;
}

export const query = graphql`
  query PostDetail($frontmatter__slug: String) {
    mdx(frontmatter: { slug: { eq: $frontmatter__slug } }) {
      frontmatter {
        author
        category
        date
        title
        slug
        # mdx의 headerImage 가져옴
        headerImage {
          childImageSharp {
            gatsbyImageData(height: 400) # height 필요한 경우 설정
          }
        }
      }
    }
  }
`;
```

### gatsby-plugin-google-gtag

Google Analytic 사용

- respectDNT
  - 이게 true인 경우, 내 크롬설정이 Do not track으로 되어있으면 개발모드에서 window.gtag is not a function이라는 에러가 발생한다.
  - 그래서 production인 경우에만 true로 설정하였음
  - https://support.google.com/chrome/answer/2790761

## gatsby-browser.ts

- 페이지에서 로드가 필요한것들을 모두 넣으면됨

```typescript
import "./src/styles.css";
```

[pico](https://picocss.com/docs)
기본적으로 괜찮은 css가 있는 패키지 (cdn으로 설치하면 됨)
tailwind 제대로 쓰기전까지는 이걸로 써도 괜찮을 듯

```css
@import url("https://cdn.jsdelivr.net/npm/@picocss/pico@1/css/pico.min.css");
```

## Deploy

### Netlify

Publish directory -> public으로 변경
