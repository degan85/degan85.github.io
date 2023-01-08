의
---
layout: post
title:  "JPA Value 타입 맵핑"
author: Degan
categories: spring 
comments: true
tags: spring
---

## JPA Value 타입 맵핑

- entity 타입에 종속적으로 존재

- Value 타입 종류
	- 기본 타입 (String, Date, Boolean)
	- Composite Value 타입
	- Collection Value 타입
		- 기본 타입의 콜렉션
		- Composite 타입의 콜렉션

- composite 

```java
import javax.persistence.Embeddable;

@Embeddable
public class Address {
    
    private String street;
    private String city;
    private String state;
    private String zipCode;
}
```

```java
@Entity
public class Account {

	// ...

    @Embedded
    private Address address;

    // 컬럼 변경해서 사용
    // @Embedded
    // @AttributeOverrides({
    //     @AttributeOverride(name = "street", column = @Column(name="home_street"))
    // })
    // private Address address;
```

---
## Reference

[[스프링 데이터 JPA 강의]]