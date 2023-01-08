---
layout: post
title:  "JPA 스프링 데이터 원리"
author: Degan
categories: spring boot 
comments: true
tags: spring boot
---

## JPA 스프링 데이터 원리

- JpaRepository<Entity, Id> interface
	- @Repository가 없어도 bean으로 등록
	
- 원래는 application.java main에
- @EnableJpaRepositories 있어야 함
	- @Import(JpaRepositoriesRegistrar.class)
	- 핵심은 ImportBeanDefinitionRegistrar
	- 프로그래밍으로 bean을 등록 가능

---
## Reference


