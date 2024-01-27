---
published: false
id: rds
slug: rds
title: Rds
description: rds
tags: ["infra", "rds"]
categories: ["infra"]
createdDate: 2024-01-24
updatedDate: 2024-01-24
---

# rds

AWS Aurora DB 클러스트 엔드 포인트 종류

## 인스턴스를 엔드포인트로 사용하지 않는 이유
[엔드포인트]는 총 4가지가 있음
클러스터 엔드포인트는 인스턴스 역할에 맞춰서 알아서 매핑해주기 때문에 
RDS에서 장애조치(failover)를 해도 잠시 순단만 발생
클러스터 엔드포인트 예시
```
mydbcluster.cluster-123456789012.us-east-1.rds.amazonaws.com:3306
```

인스턴스 엔드포인트를 사용하면 RDS에서 장애조치시에 쓰기역할 인스턴스가 읽기역할을
읽기역할 인스턴스가 쓰기 역할을 하면서 쓰기역할이 안되면서 장애가 확산됨
인스턴스 엔드포인트 예시
```
mydbinstance.123456789012.us-east-1.rds.amazonaws.com:3306
```


[엔드포인트]: https://repost.aws/ko/articles/ARuYsuzQNoRUmO_pKbkKNFIA/aws-aurora-db-%ED%81%B4%EB%9F%AC%EC%8A%A4%ED%84%B0-%EC%97%94%EB%93%9C%ED%8F%AC%EC%9D%B8%ED%8A%B8-%EC%97%B0%EA%B2%B0-%EC%A2%85%EB%A5%98-4-%EA%B0%80%EC%A7%80

