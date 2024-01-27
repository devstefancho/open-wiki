---
published: false
id: Fragment
slug: Fragment
title: FRagment
summary: Fragment
toc: true
tags: ["components"]
categories: ["components"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

# Fragment

```ts
<>
  <OneChild />
  <AnotherChild />
</>
```

## Reference
shorthand 형태인 `<></>`를 더 많이 사용하게 된다. (따로 import가 필요하지 않음)

### Caveats
- `<Fragment></Fragment>`는 key가 필요한 경우에만 사용한다.
- State는 `<><Child /></>` to `<Child />` 경우에는 유지된다.
  더 많은 케이스는 [여기](https://gist.github.com/clemmy/b3ef00f9507909429d8aa0d3ee4f986b)에서 확인할 수 있다.

## Usage
Fragment는 요소들을 그룹하면서 레이아웃이나 스타일에 영향을 주지 않아야할 때 유용하다.
