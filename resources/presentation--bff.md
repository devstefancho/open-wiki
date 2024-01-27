---
published: false
id: presentation--bff
slug: presentation--bff
title: Presentation  Bff
summary: presentation  bff
toc: true
tags: ["resources"]
categories: ["resources"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

# BFF

프론트엔드를 위한 중간서버 구현


---

# BFF를 필요로 하는 경우

- MSA로 인해 여러 Endpoint에서 요청을 해야하는 경우
- 프론트엔드에서 필요한 데이터만 받고 싶은 경우
- 여러플랫폼에 API를 무리하게 맞춰야하는 경우
- 일관된 에러메시지 전달

---

# General Purpose API vs BFF

<img src="https://samnewman.io/pattern-img/bff/general-purpose-api-teams.jpg" width="200"/>

---

# BFF 패턴 N-to-1

- N개의 플랫폼 (IOS, Android), 1개의 BFF
  <img src="https://samnewman.io/pattern-img/bff/generic-mobile-bff.jpg" width="200" />

---

# BFF 패턴 1-to-1

- 1개의 플랫폼, 1개의 BFF
  <img src="https://samnewman.io/pattern-img/bff/one-bff-per-mobile.jpg" width="200" />

---

# 핏펫몰에서 사용할 수 있는 BFF 패턴

## N-to-1

- Desktop과 Mobile에서 사용하는 데이터가 같으므로 하나의 BFF만 사용
- 중복되는 코드들을 줄일 수 있음
- BFF 서버가 다운되면 모든곳에 장애가 발생

## 1-to-1

- Repository 1개당 1개의 BFF를 사용
- 플랫폼 분기처리가 필요하지 않으므로 복잡도를 줄일 수 있음
- 중복 코드에 대해서 관리해야함

---

# 주의할 점

- BFF는 프론트엔드 개발자가 관리하는 것이 이상적
- 새로운 서버를 만들지 않아야함
  - 백엔드 받는 데이터를 사용하기 쉽게 가공하는것이 아닌 추가로 만드는것은 피해야함

---

# 참고자료

## Article

- [카카오페이지는 BFF(Backend For Frontend)를 어떻게 적용했을까?](https://fe-developers.kakaoent.com/2022/220310-kakaopage-bff/)
- [Pattern: Backends For Frontends](https://samnewman.io/patterns/architectural/bff/)
- [Using NextJS API Routes as a BFF](https://medium.com/codex/using-nextjs-api-routes-as-a-bff-4c5065d2dbae)

## Video

- [What Is A Backend For A Frontend (BFF) Architecture Pattern](https://www.youtube.com/watch?v=SSo-z16wEnc)
