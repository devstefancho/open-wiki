---
published: false
id: ec2-image-builder
slug: ec2-image-builder
title: Ec2 Image Builder
summary: ec2 image builder
toc: true
tags: ["infra"]
categories: ["infra"]
createdDate: 2024-01-23
updatedDate: 2024-01-23
---

# AMI

## AMI란?

AMI는 Amazon Machine Image의 약어로, EC2 인스턴스를 시작하기 위한 이미지

chatgpt
Question) AMI는 EC2 생성을 바로해주는 boilerplate 같은건가?
Answer)
AMI는 EC2 인스턴스를 쉽게 생성할 수 있도록 미리 구성된 템플릿 또는 보일러플레이트 역할을 합니다.
AMI를 사용하면 EC2 인스턴스를 시작할 때 미리 정의된 운영 체제(OS)와 소프트웨어, 설정 등을 가지고 인스턴스를 바로 생성할 수 있습니다.
AMI를 사용하면 보일러플레이트 코드를 작성할 필요 없이,
미리 구성된 운영 체제 및 소프트웨어를 포함하는 EC2 인스턴스를 빠르게 배포할 수 있습니다.
AMI를 사용하면 인스턴스의 배포 및 관리를 간소화하고, 보안과 일관성을 유지할 수 있습니다.
따라서 AMI는 개발자 및 IT 운영팀에게 시간과 비용을 절약하는 효과적인 솔루션으로 작용합니다.

## EC2에서 AMI 사용

EC2 > Instance Launch Instances > My AMIs 에서 미리 생성해둔것을 선택할 수 있음

## 테스트

1. EC2 Image Builder 서비로 진입

2. Component를 생성

- 이미지 생성시 실행시킬 yaml 파일을 작성
- 어떤 OS를 사용할것인지 선택

3. Image Recipe 생성

- 사용할 Component 선택(위에서 작성한 component를 사용하거나 aws에서 기본으로 제공하는것을 사용해도 됨)
- Test Component 선택(이미지 생성후 Test를 돌리는 것)

참고사항

- EC2에서 AMI를 사용하여 인스턴스 생성하는 경우 총 15~20분정도 소요된다고함 (유튜브영상 참고)

## Ref

- [AMI 생성 및 관리를 자동화 - EC2 Image Builder 소개](https://www.wisen.co.kr/pages/blog/blog-detail.html?idx=11975)
- [EC2 Image Builder - Youtube](https://www.youtube.com/watch?v=S1eUw8ztAm8)
