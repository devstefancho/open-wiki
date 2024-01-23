---
published: false
id: nextjs-image
slug: nextjs-image
title: Nextjs Image
description: nextjs image
tags: ["nextjs"]
categories: ["nextjs"]
createdDate: 2024-01-07
updatedDate: 2024-01-23
---


# Next.js Image

## Version History
v14.0.0
- onLoadingComplete prop and domains config deprecated.

v13.4.14
- placeholder prop support for data:/image...

v13.2.0
- contentDispositionType configuration added.

v13.0.6
- ref prop added.

v13.0.0
- `<span>` wrapper removed.
- layout, objectFit, objectPosition, lazyBoundary, lazyRoot props removed.
- alt is required.
- onLoadingComplete receives reference to img element.
- Built-in loader config removed.

v12.3.0
- remotePatterns and unoptimized configuration is stable.

## Props

src
- 외부 URL 사용시에는 반드시 next.config.js 에서 remotePatterns

width
- static import 혹은 fill 속성 사용시에는 필요없음

height
- width와 동일함

alt
- 이미지를 보여주는 용도가 아니라면 empty string으로 처리

loader
- 이미지 url 처리를 위한 custom function, 파라미터는 src, width, quality로 받는다.

fill
- parent element에 완전히 채울때 사용, width와 height가 불분명한 경우
- parent element에는 반드시 relative, fixed, absolute로 적용 되어있어야함
- img element는 기본적으로 absolute가 적용됨
- 채워지는 방식에 따라 object-fit을 contain 혹은 cover로 적용하면 됨

size
- breakpoints에 따라 어떤 wide로 보여줄 것인지 정보를 넣는 것
- fill을 사용할때 sizes의 값에 따라 성능차이가 발생한다.
- next/image는 자동으로 srcset을 생성하고 다운받음
- 어떤 사이즈를 다운받아야할지 모르기때문에 viewport와 같거나 큰 사이즈로 선택함
- 브라우저에게 full screen보다 작아야한다고 말해줄 수 있다.
- size를 지정하지 않고 fill을 사용하면 기본적으로 100vw가 들어감
```js
// 33vw로 지정하지 않으면
// 유저는 3배 더 넓은 이미지를 받게 되고, 결과적으로 9배용량의 이미지를 받게됨 (3 x 3 = 9)
import Image from 'next/image'
 
export default function Page() {
  return (
    <div className="grid-element">
      <Image
        fill
        src="/example.png"
        sizes="(max-width: 768px) 100vw, (max-width: 1200px) 50vw, 33vw"
      />
    </div>
  )
}
```

quality
- 최적화된 이미지의 품질을 결정하며, 1~100 사이로 지정할 수 있고 기본값은 75이다.

priority
- true이면 preload가 된다.
- priority를 사용하면 lazy loading은 자동으로 disable됨
- image가 LCP인 경우에는 priority를 사용하라
- above the fold인 경우에는 priority를 사용하면 됨

placeholder
- static import는 blurDataURL이 자동으로 적용됨
- dynamic image에는 blurDataURL를 반드시 넣어줘야함
- https://github.com/joe-bell/plaiceholder를 사용해서 base64 generation하는 것도 방법임
- 작은 이미지의 경우 `data:image/...`를 사용할 수 있다.
- empty로 하면 로딩중에 빈 공간만 생김

style
- intrinsic aspect를 유지하기 위해 width를 수정하게 되면, height는 반드시 auto로 적용해야한다. 

onLoadingComplete
- Nextjs 14부터는 onLoad를 대신 사용한다.
- 이미지가 로드되고, placeholder가 제거 되었을때 invoke된다.

onLoad
- 이미지가 로드되고, placeholder가 제거 되었을때 invoke된다.
- 1개의 인자와 함께 호출되며, `e.target`으로 img element를 사용할 수 있음

onError
- 이미지 로드가 실패한 경우 호출
- 1개의 인자와 함께 호출되며, 예시에서는 `e.target.id`로 에러 로그 출력

loading
- lazy와 eager가 있으며, eager로 설정하는 것은 성능에 악영향을 준다.
- loading대신에 priority 속성을 사용하는 것을 권장한다.

blurDataURL
- placeholder가 blur일때만 효과가 있음
- base64로 인코딩된 이미지만 사용할 수 있으며, 10px 미만의 작은 사이즈를 권장함

unoptimized
- true이면 소스 이미지가 있는 그대로 표시됨 (quality, size, format이 변경되지 못함)
- Next.js 12.3.0부터는 next.config.js에서 모든이미지에 unoptimized를 적용할 수 있음

srcSet
- deviceSizes를 대신 사용할 것

decoding
- 항상 async임

## Configuration Options
next.config.js에서 설정하는 것

remotePatterns
- wildcard pattern으로 외부 이미지 주소를 지정해 줄 수 있음 (`**`의 경우 중간에는 못씀)
- protocol, hostname, port, pathname으로 상세하게 지정가능

domains
- Next.js 14 부터는 remotePatterns을 사용하면 됨

loaderFile
- Next.js의 빌트인 최적화를 사용하지 않고, cloud provider를 사용하기를 원하는 경우 사용
- https://nextjs.org/docs/app/api-reference/next-config-js/images#example-loader-configuration 참고
```js
/** next.config.js */
  images: {
    loader: "custom",
    loaderFile: "./my/image/loader.js",
```
```js
/** ./my/image/loader.js */
export default function myImageLoader({ src, width, quality }) {
  // external image는 loader 함수를 사용하게 됨
  return `${src}?w=${width}&q=${quality || 75}&hello=world`;
}
```

## Advanced

deviceSizes
- device width breakpoints를 알고 있다면 next.config.js에서 설정할 수 있다.
- 기본값 `[640, 750, 828, 1080, 1200, 1920, 2048, 3840]`

imageSizes
- device size와 연결하여 srcset을 생성하게 된다.
- 모든 imageSizes는 가장작은 deviceSizes보다 작아야한다.
- 기본값 `[16, 32, 48, 64, 96, 128, 256, 384]`

formats
- 이미지 최적화 api는 요청의 Accept를 기반으로 최적화된 이미지를 반환한다.
- Accept header가 formats에 지정된 값과 2개이상 일치하면, array에서 첫번째 매칭된 값으로 반환한다.
- 기본값 `['image/webp']`

## Caching Behavior

요청에 의해 dynamic하게 최적화된 이미지는 `<distDir>/cache/images`에 저장된다.
최적화된 이미지는 cache가 만료되기까지 제공된다.
캐시 상태는 `x-nextjs-cache` 응답 헤어의 값으로 판단할 수 있다.
- MISS : path에 대해서 cache된것이 없음
- STALE : path에 캐시는 있지만, revalidate 시간초과
- HIT : path에 캐시가 있고, revalidate time을 초과하지 않음

만료 시간은 Math.max(minimumCacheTTL, Cache-Control)로 결정된다.

minimumCacheTTL
- 캐시에 대한 초(sec)를 설정할 수 있다.
- static import하는 경우에는 자동으로 file content를 hash하여 무제한 캐시를 만든다.
- 현재는 cache를 invalidate하는 매커니즘은 없음
  - minimunCacheTTL을 낮게 설정하는것이 최선이다.
  - 혹은 src 값을 변경하거나 `<distDir>/cache/images`를 제거할 수도 있다.
- next.config.js에서 headers를 설정하여 Cache-Control을 변경할 수 있다.
  - https://nextjs.org/docs/app/api-reference/next-config-js/headers

disableStaticImages
- static file을 직접 import해서 사용할 수 있지만, 특정 plugin과 충돌나는 경우에는 끌 수 있다.

dangerouslyAllowSVG
- 기본적으로 loader는 svg를 최적화 하지 않지만, 강제적으로 최적화하고 싶은 경우 사용한다.
- 이 경우 contentDispostionType과 contentSecurityPolicy를 설정해야한다.

## Animated Images

- loader는 기본적으로 animated image는 bypass 한다.
- 자동감지를 통해서 GIF, APNG, WebP는 최대한 지원을 한다.
- 다만 명시적으로 animated image를 bypass하고 싶으면 unoptimized를 적용한다.

## Responsize Images

- srcset은 기본적으로 1x, 2x 이미지를 포함한다.
- viewport에 맞는 이미지를 원한다면, sizes 값과 style값(또는 className)을 설정하는 것이 필요하다.
- static import인 경우는 `sizes=100vw`, `style={{ widht: '100%', height: 'auto' }}`을 설정한다.
- dynamic import인 경우는 sizes, style, width, height까지 설정한다.
  - width, height는 aspect ratio를 맞추기 위해서 설정해야한다.
  - asepct ratio를 모르는 경우 fill을 적용한다.
    - parent에 relative를 적용, object-fit는 stretch할건지 crop할건지에 따라서 결정하면 된다.

## Theme Detection

theme에 따라 Image를 보여주기 위해서는 2개의 Image를 사용하면 된다.

## 테스트해보기

- https://github.com/vercel/next.js/tree/canary/examples/image-component
- `.next/cache/images` 에서 이미지 확인 (삭제하면 캐시도 날라감, 단 브라우저 캐시제거는 하드리로드 필요)

`/fill`
- Cache-Control에서 immutable 값 확인
- Content-Type 확인 (next.config.js에서 formats 설정으로 webp로 변경할 수 있음)

`/background`
- object-fit을 cover로 설정하면, 이미지가 crop됨
- object-fit을 contain으로 설정하면, 이미지가 stretch됨 (aspect ratio 유지)

`/color`
- blurDataURL에 base64 값을 직접 넣음

`/placeholder`
- static import인 경우 blurDataURL이 자동으로 적용됨 -> svg 로 생성됨
- network tab에서 이미지가 두개 내려옴

`/responsive`
- sizes: 100vw, width: 100%, height: auto
  ```js
  sizes="100vw"
  style={{
    width: "100%",
    height: "auto",
  }}
  ``` 
- 100vw를 지우는 경우 1x, 2x로 srcset이 설정된다.

  ```bash
  ## 100vw로 sizes 지정한 경우
  /_next/image?url=%2F_next%2Fstatic%2Fmedia%2Fmountains.a2eb1d50.jpg&w=640&q=75 640w,
  /_next/image?url=%2F_next%2Fstatic%2Fmedia%2Fmountains.a2eb1d50.jpg&w=750&q=75 750w,
  /_next/image?url=%2F_next%2Fstatic%2Fmedia%2Fmountains.a2eb1d50.jpg&w=828&q=75 828w,
  /_next/image?url=%2F_next%2Fstatic%2Fmedia%2Fmountains.a2eb1d50.jpg&w=1080&q=75 1080w,
  /_next/image?url=%2F_next%2Fstatic%2Fmedia%2Fmountains.a2eb1d50.jpg&w=1200&q=75 1200w,
  /_next/image?url=%2F_next%2Fstatic%2Fmedia%2Fmountains.a2eb1d50.jpg&w=1920&q=75 1920w,
  /_next/image?url=%2F_next%2Fstatic%2Fmedia%2Fmountains.a2eb1d50.jpg&w=2048&q=75 2048w,
  /_next/image?url=%2F_next%2Fstatic%2Fmedia%2Fmountains.a2eb1d50.jpg&w=3840&q=75 3840w

  ## sizes 없는 경우
  /_next/image?url=%2F_next%2Fstatic%2Fmedia%2Fmountains.a2eb1d50.jpg&w=750&q=75 1x,
  /_next/image?url=%2F_next%2Fstatic%2Fmedia%2Fmountains.a2eb1d50.jpg&w=1920&q=75 2x
  ```
- srcset중에서 어떤 이미지를 로드했는지는 network tab에서 request url을 비교해보면 알 수 있다.
- DRP(Device Pixel Ratio) 확인하기
  ```js
  console.log(window.devicePixelRatio)
  ```
- sizes가 100vw로 설정한 경우
  - 물리적 너비에 맞는 이미지를 로드한다.
  - DRP가 2 이상인 경우, 물리너비가 750px이라면 w=1920를 가져옴
- sizes가 100vw가 아닌 경우
  - srcset은 1x, 2x로 설정된다.
  - DRP가 2인 경우에는 2x로 받아온다.

`/theme`
- dynamic import인 경우 Cache-Control에서 must-revalidate 값이 있음 (minimumCacheTTL 로 설정필요)
- '/' 경로에서 '/theme' 경로로 왔다갔다 하면 X-Nextjs-Cache에서 HIT값 확인가능
  ```css
  .imgDark {
    display: none;
  }

  @media (prefers-color-scheme: dark) {
    .imgLight {
      display: none;
    }

    .imgDark {
      display: unset;
    }
  }
  ```

`next.config.js`
- deviceSizes, imageSizes 설정갯수에 따라 초기 로딩속도가 차이남
- 아래로 했을때 기본값에 비해서 10% ~ 최대 50%까지 빨라짐
  ```js
  deviceSizes: [640, 1024, 1440],
  imageSizes: [30, 50, 100],
  ```
