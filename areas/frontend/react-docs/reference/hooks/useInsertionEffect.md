---
published: false
id: useInsertionEffect
slug: useInsertionEffect
title: UseInsertionEffect
summary: useInsertionEffect
toc: true
tags: ["hooks"]
categories: ["hooks"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

# useInsertionEffect

useEffect의 다른 버전으로 DOM mutations 발생하기전에 실행되는 Effect

```tsx
useInsertionEffect(setup, dependencies?)
```

## Reference

### Parameters

**setup**
Effect's logic으로 cleanup로 optional하게 사용할 수 있음

**dependencies**
setup code에 있는 reactive values

### Returns

undefined를 return함

## Usage

만약 style tag를 Runtime에 injection 해야하는 경우 반드시 useInsertionEffect를 사용한다.
이것은 CSS-in-JS 라이브러리나 툴을 위한 것이다.

CSS-in-JS는 자바스크립트 코드 내에서 CSS 스타일을 작성하는 방식을 의미합니다.

1. 정적 추출: 컴파일러를 사용하여 CSS 파일로 추출 (ex. Nextjs에서 scss를 module로 만들어서 import 하는 방식)
   이 방식에서는 CSS-in-JS 라이브러리나 도구를 사용하여 작성된 스타일을 CSS 파일로 컴파일하고 추출합니다. 이 방식은 웹사이트의 초기 로드 성능을 향상시키며, CSS를 직접 작성하는 방식과 유사한 퍼포먼스를 제공합니다.

2. 인라인 스타일: `<div style={{ opacity: 1 }}>`와 같은 방식
   이 방식은 자바스크립트 객체를 이용하여 스타일을 직접 HTML 요소에 적용하는 방식입니다. 이는 주로 동적인 스타일을 적용할 때 사용되며, 컴포넌트의 상태나 props에 따라 스타일이 변할 때 유용합니다.

3. 런타임 `<style>` 태그 삽입
   이 방식에서는 CSS 스타일을 JavaScript에서 생성하고, 이를 `<style>` 태그를 통해 런타임에 DOM에 삽입합니다.
   그러나 이 중에서도 React 팀은 정적 추출과 인라인 스타일의 조합을 추천하며, 런타임 `<style>` 태그 삽입은 권장하지 않습니다.
