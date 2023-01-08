---
layout: post
title:  "JPA Query Method"
author: Degan
categories: spring boot 
comments: true
tags: spring boot
---

## JPA Query Method


- create method names

```java
interface PersonRepository extends Repository<Person, Long> {

  List<Person> findByEmailAddressAndLastname(EmailAddress emailAddress, String lastname);

  // Enables the distinct flag for the query
  List<Person> findDistinctPeopleByLastnameOrFirstname(String lastname, String firstname);
  List<Person> findPeopleDistinctByLastnameOrFirstname(String lastname, String firstname);

  // Enabling ignoring case for an individual property
  List<Person> findByLastnameIgnoreCase(String lastname);
  // Enabling ignoring case for all suitable properties
  List<Person> findByLastnameAndFirstnameAllIgnoreCase(String lastname, String firstname);

  // Enabling static ORDER BY for a query
  List<Person> findByLastnameOrderByFirstnameAsc(String lastname);
  List<Person> findByLastnameOrderByFirstnameDesc(String lastname);
}
```

- embedded property 
	- Pserson 안에 Address
	- Addres 안에 zipCode

```java
List<Person> findByAddressZipCode(ZipCode zipCode);
```

- Special parameter handling

```java
Page<User> findByLastname(String lastname, Pageable pageable);

Slice<User> findByLastname(String lastname, Pageable pageable);

List<User> findByLastname(String lastname, Sort sort);

// page info 없이 return
List<User> findByLastname(String lastname, Pageable pageable);
```

- Paging and Sorting

```java
// defining sort
Sort sort = Sort.by("firstname").ascending()
  .and(Sort.by("lastname").descending());


// type-safe
TypedSort<Person> person = Sort.sort(Person.class);
Sort sort = person.by(Person::getFirstname).ascending()
  .and(person.by(Person::getLastname).descending());


```

- Limiting Query Results

```java
User findFirstByOrderByLastnameAsc();

User findTopByOrderByAgeDesc();

Page<User> queryFirst10ByLastname(String lastname, Pageable pageable);

Slice<User> findTop3ByLastname(String lastname, Pageable pageable);

List<User> findFirst10ByLastname(String lastname, Sort sort);

List<User> findTop10ByLastname(String lastname, Pageable pageable);
```

- stream으로 return 받으면 try with resource로 받아서 사용
- spring 비동기 처리 가능(@Async, Future)

---
## Reference

- [Spring Data JPA - Reference Documentation](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/)


