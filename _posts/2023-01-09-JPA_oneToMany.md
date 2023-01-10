---
layout: post
title:  "JPA 1 대 다 관계"
author: Degan
categories: spring boot
comments: true
tags: spring
---

## JPA 1 대 다 관계


- @OneToMany
	- 뒤가 many이면 Set 등 여러개
	- 단방향으로 만들면 관계 테이블 추가 됨
		- ex) 
		- account table
		- study table 
		- account_studies table

```java
// Account.class

@OneToMany
private Set<Study> studies = new HashMap<>();
```

- @ManyToOne
	- 뒤가 One이라서 하나
	- 단방향으로 만들면 외래키 컬럼 추가 됨

```java
// Study.class

@ManyToOne
private Account owner;
```

- 양방향에서 참조가 필요할 때
	- 두 개가 같은 참조라고 명시해줘야 함
	- 아래 경우 Study.class가 주인
		- mappedBy가 없는 곳이 주인
		- 또는 외래키가 있는 곳이 주인

```java

// Account.class
@OneToMany(mappedBy = "owner")
private Set<Study> studies = new HashSet<>();

// Study.class
@ManyToOne
private Account owner;

```


- 주인과 데이터 입력
	- 주인이 아닌 곳에만 데이터를 넣고 주인인 곳에 안 넣으면 데이터 안 들어감
	- 주인만 넣고 주인이 아닌곳은 안 넣어도 데이터 들어감
	- 하지만 객체지향에 따라 둘 다 넣는 것이 좋음
		- 편의 메소드를 만들어서 사용
		- 지우는 메소드도 꼭 추가해야 함

```java
//Account.class

public void addStudy(Study study) {
	study.setOwner(this);
	this.getStudies().add(study);
}

public void removeStudy(Study study) {
	this.getStudies().remove(study);
	study.setOwner(null);
}
```

---
## Reference

[[스프링 데이터 JPA 강의]]
