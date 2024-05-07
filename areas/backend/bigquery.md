---
published: false
id: bigquery
slug: bigquery
title: Bigquery
summary: bigquery
toc: true
tags: ["backend"]
categories: ["backend"]
createdDate: 2024-03-20
updatedDate: 2024-03-20
---

# BigQuery

https://console.cloud.google.com/bigquery에 접속함
왼쪽 Panel에서는 DataSet, Database, Table을 선택할 수 있다.
햄버거 버튼을 누르면 "콘텐츠 새로고침"을 할 수 있음

프로젝트 선택하기
프로젝트 선택을 하기 위해서는Google Cloud 오른쪽에 있는 버튼을 눌러준다.
최근 프로젝트가 보이는데, 전체를 보고 싶으면 전체탭을 선택하면 된다.

자동완성을 하기 위해서는 backtic으로 감싸야한다.
Query는 아래처럼 작성한다.
줄바꿈을 할때 앞쪽에 콤마를 쓰는 이유는 한줄을 쉽게 삭제할 수 있기 때문이다.
```sql
SELECT
  empno
  , ename
  , job
FROM `hidden-outrider-390504.knit.emp`
WHERE job = 'SALESMAN'
```

실행한 쿼리를 저장할 수 있다.
쿼리 결과를 저장할 수도 있다.
쿼리 결과는 CSV, JSON, BigQuery, Google Sheets, 클립보드에 저장할 수 있다.
