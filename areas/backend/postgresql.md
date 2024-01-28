---
published: false
id: postgresql
slug: postgresql
title: Postgresql
summary: postgresql
toc: true
tags: ["backend"]
categories: ["backend"]
createdDate: 2024-01-19
updatedDate: 2024-01-21
---

# postgresql

## Installation
```bash
brew install postgresql # 설치
brew services start postgresql # 실행

psql -h localhost -d test # test database 접속
psql -U admin -d blog # blog database에 admin 계정으로 접속
```

## Database 기본 명령어
```sql
\l -- database 목록
\d -- table 목록
\dt -- table 목록
\q -- 종료
\d visit -- visit table의 column 정보
\dt visit -- visit table의 owner 정보
```

## Table 조작하기
```sql
-- table 생성
CREATE TABLE test(
  id serial PRIMARY KEY,
  title VARCHAR ( 50 ) NOT NULL,
);

-- table 삭제
DROP TABLE test;

-- record 추가
INSERT INTO test (title) VALUES('hello_world');

-- table colunm 이름수정
ALTER TABLE test
RENAME COLUMN title TO title2;

-- table column 타입 변경
ALTER TABLE test
ALTER COLUMN title2 TYPE VARCHAR(100);
```

## 권한 설정
```sql
GRANT ALL PRIVILEGES ON DATABASE test TO test;
```

## Railway 원격 접속하기
- `{}` 부분은 실제값으로 변경하면 됨
```bash
PGPASSWORD={password} psql -h {hostname} -U {username} -p {port_number} -d {database}
```

