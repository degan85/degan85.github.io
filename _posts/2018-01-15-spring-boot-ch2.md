---
layout: post
title: "[실전 스프링 부트 워크북] 간단한 스프링 부트 웹 어플리케이션 "
author: Degan
categories: springboot
tags:	spring boot cli groovy
comments: true
---

### groovy

```groovy
// app.groovy

@RestController
class WebApp {
	@RequestMapping("/")
	String greeting() {
		"<h1>스프링 부트 시작</h1>"
	}
}
```
- `$ spring run app.groovy` 실행
	- 스프링 부트 CLI(Command Line Interface) 있어야 함
- http:localhost:8080/ 접속

---

### java

- main 메서드에서 싱글턴 클래스 SpringApplication으로 전체 앱 실행
- run 메서드는 애플리케이션 파라미터(arg)를 인자로 받음

```java
// SimpleWebApp.java

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class SimpleWebApp {

  public static void main(String[] args) {
    SpringApplication.run(SimpleWebApp.class, args);
  }
}
```

- @SpringBootApplication
	- @configuration
	- @EnableAutoConfiguration
		- 스프링 부트 자동 구성
	- @ComponentScan
	- 이 뭉친 에너테이션
- spring-boot-starter-web의 의존체 중 하나인 톰캣으로 앱을 띄움

```java
// SimpleWebController.java

import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class SimpleWebController {

  @RequestMapping("/")
  public String greetings() {
    return "<h1>자바에서도 스프링 부트 시작!</h1>";
  }
}
```

- 실행
	- `$ spring run *.java`
	- `$ mvn spring-boot:run`
	- `$ mvnw spring-boot:run`
	- `$ gradle bootRun`
- 단독형 앱 실행
	- `$ mvn package` or `$ gradle build`
	- `$ java -jar target/파일.jar
- http://localhost:8080/ 접속

---

## Reference

[실전 스프링 부트 워크북](http://www.hanbit.co.kr/store/books/look.php?p_code=B2433442478)

