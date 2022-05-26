---
layout: post
title:  Docker mac에 설치 (sample schema 포함)
date:   2021-08-11
author: Degan
categories: DOCKER
tags: oracle Mac docker
comments: true
---


## docker pull image
```bash
docker pull deepdiver/docker-oracle-xe-11g

docker run -d -p 49160:22 -p 49161:1521 deepdiver/docker-oracle-xe-11g

docker ps
```

## Oracle SQL Developer
- Name : 이름
- 사용자 이름: system
- 비밀번호: oracle
- 포트넘버: 49161
- SID: xe

---

## HR 계정 
```sql
alter user hr account unlock;

alter user hr identified by (설정할 비밀번호);
```

- 사용자 이름: hr
- 비밀번호: 위에서 설정한 비밀번호
- 포트넘버: 49161
- SID: xe

---

## Reference
[Docker를 이용해 MacOS에서 Oracle 11g 설치하기]( https://purplecow1997.tistory.com/76)
