---
layout: post
title: "[실전 스프링 부트 워크북] REST형 API 만들기 "
author: Degan
categories: springboot
tags:	spring boot rest api jap 
comments: true
---

## REST형 API

### gradle dependencies
```
dependencies {
	compile("org.springframework.boot:spring-boot-starter-web")
	compile("org.springframework.boot:spring-boot-starter-data-jpa")
	compile("org.springframework.boot:spring-boot-starter-data-rest")
	compile("org.springframework.boot:spring-boot-starter-thymeleaf")
	compile group: 'mysql', name: 'mysql-connector-java', version: '5.1.13'
	compile group: 'org.springframework.data', name: 'spring-data-rest-hal-browser', version: '2.4.0.RELEASE'
	testCompile('org.springframework.boot:spring-boot-starter-test')
	compile group: 'com.jayway.jsonpath', name: 'json-path', version: '2.0.0'
	
}
```

---

### mysql 계정 생성

```
mysql> create database journal default character set utf8 default collate utf8_general_ci;

mysql> use journal
Database changed

mysql> CREATE USER 'springboot'@'%' IDENTIFIED BY 'springboot' PASSWORD EXPIRE NEVER;

mysql> GRANT ALL PRIVILEGES ON * . * TO 'springboot'@'%';
Query OK, 0 rows affected (0.00 sec)
```

---

### application.properties

```
#Spring DataSource
spring.datasource.url = jdbc:mysql://localhost:3306/journal
spring.datasource.connectionProperties=useUnicode=true;characterEncoding=utf-8
spring.datasource.username = springboot
spring.datasource.password = springboot
spring.datasource.testWhileIdle = true
spring.datasource.validationQuery = SELECT 1

#JPA-Hibernate
spring.jpa.show-sql = true
spring.jpa.hibernate.ddl-auto = create-drop
spring.jpa.hibernate.naming-strategy = org.hibernate.cfg.ImprovedNamingStrategy
spring.jpa.properties.hibernate.dialect = org.hibernate.dialect.MySQL5Dialect

spring.data.rest.base-path=/api
```

- validationQuery : DB 서버가 오랫동안 유휴 상태라도 접속을 유지하기 위함
- show-sql : SQL 로깅 여부 표시
- hibernate.naming-strategy : DB 테이블과 필드의 명명 전략
- org.hibernate.dialect.MySQL5Dialect : MySql DB 엔진에 최적화한 SQL 생성
- base-path=/api : HAL + JSON 접속

---

### JournalEntry class

```java
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.Table;
import javax.persistence.Transient;

import com.apress.spring.utils.JsonDateSerializer;
import com.fasterxml.jackson.annotation.JsonIgnore;
import com.fasterxml.jackson.databind.annotation.JsonSerialize;

@Entity
@Table(name="entry")
public class JournalEntry {

  @Id
  @GeneratedValue(strategy=GenerationType.AUTO)
  private Long id;
  private String title;
  private Date created;
  private String summary;

  @Transient
  private final SimpleDateFormat format = new SimpleDateFormat("yyyy-MM-dd");

  public JournalEntry(String title, String summary, String date) throws ParseException {
    this.title = title;
    this.summary = summary;
    this.created = format.parse(date);
  }

  JournalEntry() {}

  public Long getId() {
    return id;
  }

  public void setId(Long id) {
    this.id = id;
  }

  public String getTitle() {
    return title;
  }

  public void setTitle(String title) {
    this.title = title;
  }

  @JsonSerialize(using=JsonDateSerializer.class)
  public Date getCreated() {
    return created;
  }

  public void setCreated(Date created) {
    this.created = created;
  }

  public String getSummary() {
    return summary;
  }

  public void setSummary(String summary) {
    this.summary = summary;
  }

  @JsonIgnore
  public String getCreatedAsShort() {
    return format.format(created);
  }

  public String toString() {
    StringBuilder value = new StringBuilder("* JournalEntry(");
    value.append("Id: ");
    value.append(id);
    value.append(",제목: ");
    value.append(title);
    value.append(",요약: ");
    value.append(summary);
    value.append(",작성일자: ");
    value.append(format.format(created));
    value.append(")");

    return value.toString();
  }
}
```

- @Table(name="entry") : 하이버네이트는 entry라는 테이블 생성
- @JsonSerialize(using=JsonDateSerializer.class)
	- 커스텀 클래스로 데이터를 직렬화
	- 여기서는  표준 ISO.DATE 형식의 yyyy-MM-dd 패턴으로 표시
	- @JsonIgnore : JSON 문자열로 나타낼 때 대상에서 제외

```java
import java.io.IOException;
import java.text.SimpleDateFormat;
import java.util.Date;

import org.springframework.stereotype.Component;

import com.fasterxml.jackson.core.JsonGenerator;
import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.JsonSerializer;
import com.fasterxml.jackson.databind.SerializerProvider;

@Component
public class JsonDateSerializer extends JsonSerializer<Date>{

    private static final SimpleDateFormat dateFormat = new SimpleDateFormat("yyyy-MM-dd");
    
    public void serialize(Date date, JsonGenerator gen, SerializerProvider provider)
            throws IOException, JsonProcessingException {
        String formattedDate = dateFormat.format(date);
        gen.writeString(formattedDate);
    }
}
```

- JSON 변환기는 필요한 시점에 JsonDateSerializer 클래스를 불러서 사용
- 스피링 MVC의 HttpMessageConverter<T>클래스 내부에서 알아서 처리
- JsonSerializer를 상속하므로 직렬화할 serialize 메서드를 재정의해야 함 
- 직렬화 로직은 JSON 잭슨 라이브러리를 기반

---

### JournalRepository class

```java
import java.util.Date;
import java.util.List;

import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.data.repository.query.Param;
import org.springframework.data.rest.core.annotation.RepositoryRestResource;
import org.springframework.format.annotation.DateTimeFormat;
import org.springframework.format.annotation.DateTimeFormat.ISO;
import org.springframework.transaction.annotation.Transactional;

import com.apress.spring.domain.JournalEntry;

@Transactional
@RepositoryRestResource(collectionResourceRel = "entry", path = "journal")
public interface JournalRepository extends JpaRepository<JournalEntry, Long> {

  List<JournalEntry> findByCreatedAfter(@Param("after") @DateTimeFormat(iso = ISO.DATE) Date date);
  List<JournalEntry> findByCreatedBetween(@Param("after") @DateTimeFormat(iso = ISO.DATE) Date after, @Param("before") @DateTimeFormat(iso = ISO.DATE) Date before);
  List<JournalEntry> findByTitleContaining(@Param("word") String word);
  List<JournalEntry> findBySummaryContaining(@Param("word") String word);

}
```

- @Transactional 
	- REST 호출에 트랜잭션을 걸음
	- API 동시 호출 시 데이터 정합성에 문제 없도록 보호
- @RepositoryRestResource
	- 결로를 journal로 바꿈
	- 컬렉션 리소스는 복수형 대신 entry로 함

---

### data.sql

```
INSERT INTO ENTRY(title,summary,created) VALUES('간단한 스프링 부트 프로젝트','스프링 부트 프로젝트를 처음 만들어보았다','2016-01-03 00:00:00.00');
INSERT INTO ENTRY(title,summary,created) VALUES('스프링 부트 입문','오늘부터 스프링 부트를 배웠다','2016-01-02 00:00:00.00');
INSERT INTO ENTRY(title,summary,created) VALUES('스프링 부트 해부','스프링 부트를 자세히 살펴보았다','2016-02-02 00:00:00.00');
INSERT INTO ENTRY(title,summary,created) VALUES('스프링 부트 클라우드','클라우드 파운드리를 응용한 스프링 부트를 공부했다','2016-02-05 00:00:00.00');
```

- src/main/resources/data.sql

---

### 확인

- 구글 크롬에서 JSONView 애드온 설치 후 접속
- http://localhost:80880/api/brower
	- HAL 브라우저는 REST형 API를 자세히 보기 편리한 도구
	- **Links** 영역에서 entry 캐션 줄에 주황색 아이콘 클릭하면 신규 데이터 입력 가능
	- Explorer 필드에 /api/journal을 입력하고 **Go** 누름 데이터 조회

---

## Reference

[실전 스프링 부트 워크북](http://www.hanbit.co.kr/store/books/look.php?p_code=B2433442478)

