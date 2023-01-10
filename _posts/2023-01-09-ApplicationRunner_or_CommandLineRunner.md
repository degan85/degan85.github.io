---
layout: post
title:  "ApplicationRunner or CommandLineRunner"
author: Degan
categories: springboot
comments: true
tags: spring
---

## ApplicationRunner or CommandLineRunner

- 스프링 부트 구동 시점에 특정 코드 실행

### CommandLineRunner
- 구동 시점에 실행되는 코드가 자바 문자열 argument 배열에 접근할 필요가 있는 경우
- CommandLineRunner interface 
- @Component 어노테이션 선언

```java
import org.springframework.boot.CommandLineRunner;
import org.springframework.stereotype.Component;

import java.util.Arrays;

@Component
public class DemoCommandLineRunner implements CommandLineRunner {

    @Override
    public void run(String... args) throws Exception {
        System.out.println("CommandLineRunner Args: " + Arrays.toString(args));
    }

```

- test

```bash
java -jar target/demo.jar abc 123
# CommandLineRunner Args: [abc, 123]
```

### ApplicationRunner
- ApplicationArguments 객체가 argument로 넘어옴
- ApplicationRunner interface
- @Component 어노테이션 선언

```java
```java
import org.springframework.boot.ApplicationArguments;
import org.springframework.boot.ApplicationRunner;
import org.springframework.stereotype.Component;

import java.util.Arrays;

@Component
public class DemoApplicationRunner implements ApplicationRunner {

    @Override
    public void run(ApplicationArguments args) throws Exception {
        System.out.println("ApplicationRunner Args: " + Arrays.toString(args.getSourceArgs()));
    }
}
```

- test
```bash
java -jar target/demo.jar abc 123
# ApplicationRunner Args: [abc, 123]
```

### 실행 순서
- @Order 어노테이션으로 순서 명시


---
## Reference

[스프링 부트 구동 시점에 특정 코드 실행 시키기 ](https://www.daleseo.com/spring-boot-runners/)



