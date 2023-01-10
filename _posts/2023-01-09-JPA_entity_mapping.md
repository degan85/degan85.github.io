---
layout: post
title:  "엔티티 맵핑"
author: Degan
categories: spring 
comments: true
tags: spring
---

## 엔티티 맵핑

- @Entity
	- 객체 세상에서 쓰는 이름
	- 보통 class와 같은 이름을 사용
	- JQL에서 쓰임

- @Table
	- 릴레이션 세상에서 쓰임
	- @Entity의 이름이 기본값
	- 테이블의 이름은 SQL에 쓰임

- @Column
	- 생략하면 기본 값
	- nullable
	- unique...

- @Temporal
	- TemporalType
		- TIMESTAMP
		- DATE
		- TIEM

- @Transient
	- 객체로만 사용하고 DB에 반영 안 됨
	
---
## Reference

[스프링 데이터 JPA 강의](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EB%8D%B0%EC%9D%B4%ED%84%B0-jpa/dashboard)
