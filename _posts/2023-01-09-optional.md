---
layout: post
title:  "optional과 비어있는 collection list"
author: Degan
categories: spring boot 
comments: true
tags: spring boot
---

## optional과 비어있는 collection list

- 하나의 객체를 받을 경우 Optional 사용 추천(java 1.8 이후)
- null 체크보다 메소드 사용 권장

- collection의 경우  jpa data에서는
- 빈 collection을 return

- 스프링 프레임워크 5.0부터 Null 어노테이션 지원
	- @NonNullApi
	- @NonNull
	- @Nullable

---
## Reference


