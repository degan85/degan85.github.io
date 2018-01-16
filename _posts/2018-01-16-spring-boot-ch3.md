---
layout: post
title: "[실전 스프링 부트 워크북] 스프링 부트 자동 구성과 주요 기능 "
author: Degan
categories: springboot
tags:	spring boot configuration properties
comments: true
---

### 자동 구성
- 클래스패스, 애너테이션, 기타 자바 구성 클래스를 보고
- 적합한 앱으로 맞춤하는 자동 구성

### 특정 자동 구성 끄기
- @EnableAutoConfiguration의 **exclude**
	- @EnableAutoConfiguration(exclude={ActiveMQAutoConfiguration.class, DataSourceAutoConfiguration.class})

### @EnableAutoConfiguration
- @Enable<기술명> 애너테이션
- 앱에서 필요한 빈을 유추해서 구성하는 클래스
- 자동 구성 클래스는 클래스패스 및 앱에 정의한 빈에 따라 적용 여부가 결정됨

```java
// 코드 3-5 org.springframework.boot.autoconfigure.EnableAutoConfiguration 크래스 중에서

// ...
// 다른 선언 생략
// ..
@Import(EnableAutoConfigurationImportSelector.class)
public @interface EnableAutoConfiguration {
  
  Class<?>[] exclude() default {};

  String[] excludeName() default {};

}
```

- 구성할 클래스를 모두 찾아주는 org.springframework.boot.autoconfigure.EnableAutoConfigurationImplortSelector 클래스 덕분에 기능이 막강함
	- 실제로 자동 구성을 유발하는 getCandidateConfiguration 메서드가 가장 중요
	- SpringFactoriesLoader.loadFactoryNames 메서드는 spring-boot-autoconfigure JAR에 있는 META-INF/spring.factories라는 파일에 프로퍼티 형식으로 열거된 **구성 클래스**를 읽어 들여 **List<String> 타입으로 반환**
	- 
```java
// 코드 3-6 EnableAutoConfigurationImportSelector 클래스 중에서

//...
protected List<String> getCandidateConfiguration(AnnotationMetadata metadata,
						 AnnotationAttributes attributes) {

  return SpringFactoriesLoader.loadFactoryNames(
						getSpringFactoriesLoaderFactoryClass(), getBeanClassLoader());
}
//...
```

```java
# 코드 3-7 spring-boot-autocinfigure-<버전>.jar#META-INF/spring.factories 중에서

# Initializers
org.springframework.context.ApplicationContextInitializer=\
org.springframework.boot.autoconfigure.logging.AutoConfigurationReportLoggingInitializer
...

# Application Listeners
org.springframework.context.Application=\
org.springframework.boot.autoconfigure.BackgroundPreinitializer

# Auto Configure
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
org.springframework.boot.autoconfigure.admin.SpringApplicationAdminJmxAutoConfiguration,\
org.springframework.boot.autoconfigure.aop.AopAutoConfiguration,\
org.springframework.boot.autoconfigure.amqp.RabbitAutoConfiguration,\
org.springframework.boot.autoconfigure.MessageSourceAutoConfiguration,\
org.springframework.boot.autoconfigure.PropertyPlaceholderAutoConfiguration,\
org.springframework.boot.autoconfigure.batch.BatchAutoConfiguration,\
org.springframework.boot.autoconfigure.cache.CacheAutoConfiguration,\
org.springframework.boot.autoconfigure.cassandra.CassandraAutoConfiguration,\
org.springframework.boot.autoconfigure.cloud.CloudAutoConfiguration,\
...
...
```
- 결국 스프링 부트가 앱 타입을 유추할 때 사용할 자동 구성 클래스를 모두 **spring.factories**파일에 몰아 넣음

---

### 자동 구성 클래스 중 CloudAutoConfiguration 클래스


```java
// 코드 3-8 org.springframework.boot.autoconfigure.cloud.CloudAutoConfiguration 클래스

package org.springframework.boot.autoconfigure.cloud;

import org.springframework.boot.autoconfigure.AutoConfigureOrder;
import org.springframework.boot.autoconfigure.EnableAutoConfiguration;
import org.springframework.boot.autoconfigure.condition.ConditionalOnClass;
import org.springframework.boot.autoconfigure.condition.ConditionalOnMissingBean;
import org.springframework.boot.autoconfigure.condition.ConditionalOnProperty;
import org.springframework.cloud.Cloud;
import org.springframework.cloud.app.ApplicationInstanceInfo;
import org.springframework.cloud.config.java.CloudScan;
import org.springframework.cloud.config.java.CloudScanConfiguration;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Import;
import org.springframework.context.annotation.Profile;
import org.springframework.core.Ordered;

@Configuration
@Profile("cloud")
@AutoConfigureOrder(CloudAutoConfiguration.ORDER)
@ConditionalOnClass(CloudAutoConfiguration.class)
@ConditionalOnMissingBean(Cloud.class)
@ConditionalOnProperty(prefix = "spring.cloud", name = "enabled", havingValue = "true", matchIfMissing = true)
@Import(CloudScanConfiguration.class)
public class CloudAutoConfiguration {

  // 클라우드 구성을 (데이터, 몽고보다) 먼저 해야 한다.
  public static final int ORDER = Ordered.HIGHEST_PRECEDENCE + 20;

}
```

- 클라우드 애플리케이션을 구성하는 클래스
- 클라우드 앱인지 @ConditionalOnClass, @ConditionalOnMissingBean 두 애너테이션으로 분간
- @ConditionalOnProperty는 spring.cloud 프로퍼티가 enabled일 경우에만 적용하기 위해 붙인 애너테이션
	- @Profile에 cloud라는 값을 지정했기 때문에 이 자동 구성은 cloud 프로파일을 켰을 때만 실행
- @Import는 앞서 나온 다른 애너테이션 조건이 충족될 경우 CloudScanConfiguration 클래스를 임포트 함

---

### 커스텀 배너

- 앱을 실행하면 ㄹ그 첫 부분에 스프링 부트 배너 등장
- http://patorjk.com에서 텍스트를 아스키 아트로 변환
- `src/main/resources`폴더에 **banner.txt** 파일로 저장
- **applications.properties** 파일에 `spring.main.banner-mode=off` 설정하면 배너 표시 안 됨

---

### 애플리케이션 구성

- URL, IP 크레덴셜, DB 등 갖가지 구성 정보를 잘 보관해야 함
- 구성 정보를 하드 코딩하는 일은 극구 피해야 함
- 안전하면서 배포하기 쉽게 외부화(externalization)하는 편이 좋음
- 스피링에서는
	- XML 파일에 <context:property-placeholder/> 태그 사용
	- @PropertySource 애너테이션 사용
- 스프링 부트에서는
	- application.properties 파일
	- application.yml 파일
	- 환경 변수 사용(클라우드 환경에선 이 방법이 기본)
	- 테미널 창에 인자로 전달

### 프로퍼티 설정 예
- 스프링 부트는 다음 순서대로 구성 프로퍼티를 재정의
	- 실행 명령어와 함께 전달한 인자
	- SPRING_APPLICATION_JSON
	- JNDI (java:comp/env)
	- System.getProperties()
	- OS 환경 변수
	- RandomValuePropertySource (random.*)
	- JAR 패키지 외부의 프로파일 관련 구성(application-{vmfhvkdlfaud}.jar)
	- JAR 패키지 내부의 프로파일 관련 구성(application-{vmfhvkdlfaud}.jar)
	- JAR 패키지 외부의 애플리케이션 프로퍼티(application.properties)
	- JAR 패키지 내부의 애플리케이션 프로퍼티(application.properties)
	- @PropertySource
	- SpringApplication.setDefaultProperties

(앱이 자체 application.properties를 지닌 JAR 라이브러리를 의존체를 사용한다면, 앱쪽 application.properties 파일을 '외부', JAR 라이브러리 안에 선언된 application.properites 파일을 '내부'라 함)

#### 명령행 인자

```java
@SpringBootApplication
public class SpringBootConfigApplication {

  private static Logger log = LoggerFactory.getLogger(SpringBootConfigApplication.class);

  public static void main(String[] args) {
    SpringApplication.run(SpringBootConfigApplication.class, args);
  }

  @Value("${server.ip}")
  String serverIp;

  @Bean
  CommandLineRunner values() {
    return args -> {
      log.info(" > 서버 IP: " + serverIp);
    };
  }
}
```

- `$ ./mvnw spring-boot:run -Dwerver.ip= 192.168.12.1`
- 또는
```console
$ ./mvnw package -DskipTests=true
$ java -jar target/파일.jar --server.ip= 192.168.12.1
```
- 또는 application.properties에 다음과 같이 입력
	- `server.ip=192.168.12.1`
- 이름이 같은 인자를 명령행에서 전달하면 application.properties 파일보다 우선 적용
- JSON 형식
	- `$ ./mvnw spring-boot:run -Dspring.application.json='{"server":{"ip":"192.168.145.78"}}'
- 환경 변수로 
	- `$ SERVER_IP= 192.168.150.46 ./mvnw spring-boot:run`

#### 느슨한 바인딩
- 스프링 부트는 바인딩을 느슨하게 처리함
- message.destinationName : 표준 카멜 표기법
- message.destination-name : 프로퍼티와 TAML 파일 권장 표기법
- MASSAGE_DESTINATION_NAME : OS 환경 변수 권장 표기법

#### 경로와 이름 변경
- application.properties 또는 YAML 파일 찾는 순서
	- 현재 폴더 하위의 /config 폴더
	- 현재 폴더
	- 클래스패스 /config 패키지
	- 클래스패스 루트
- `$ ./mvnw spring-boot:run -Dspring.config.name=mycfg -Dspring.config.location= file:app/`
	- 파일 이름 변경
	- 경로 변경
- 파일 경로 변경하려면 환경 변수 SPRING_CONFIG_LOCATION을 바꿈

#### 프로파일 켜기
- 스프링 프레임워크 3.1 버전부터 프로파일에 따라 커스텀 프로퍼티와 빈을 생성할 수 있음
- 앱을 다시 컴파일 또는 패키징하지 않아도 실행 환경을 분기할 수 있음
- @ActiveProfiles 애너테이션에 액티브 프로파일을 지정
- setActiveProfiles 메서드로 액티브 프로파일 바꿀 수 있음
- SPRING_PROFILES_ACTIVE 환경 변수 또는 spring.profiles.active라는 프로퍼티를 이용해도 됨

- application-qa.properties
	- `server.ip=location`
- application-prod.properties
	- `server.ip=http://my-remote.server.com`
- 실행
	- `$ ./mvnw clean spring-boot:run -Dspring.profiles.active=prod`

#### 커스텀 프로퍼티 접두어
- 자바 클래스에 해당 프로퍼티를 맴버 변수로 선언하고
- @ConfigurationProperties 애너테이션을 붙인 다음 게터와 세터 추가
- STS 사용자는 **spring-boot-configuration-properties** 의존체를 폼 파일에 추가하는 걸 권장
	- 프로퍼티 코드의 자동완성 기능을 지원 
- application.properties
	- 
```console
server.ip=192.168.3.5

myapp.server-ip=192.168.34.56
myapp.name=나의 구성 애플리케이션
myapp.description=그냥 코드입니다
```

```java
@SpringBootApplication
public class SpringBootConfigApplication {

  private static Logger log = LoggerFactory.getLogger(SpringBootConfigApplication.class);

  public static void main(String[] args) {
    SpringApplication.run(SpringBootConfigApplication.class, args);
  }

  @Value("${myapp.server-ip}")
  String serverIp;

  @Autowired
  MyAppProperties props;
  
  @Bean
  CommandLineRunner values() {
    return args -> {
      log.info(" > 서버 IP: " + serverIp);
      log.info(" > 애플리케이션명: " + props.getName());
      log.info(" > 애플리케이션 정보: " + props.getDescription());
    };
  }

  @Component
  @ConfigurationProperties(prefix="myapp")
  public static class MyAppProperties {
    private String name;
    private String description;
    private String serverIp;

    public String getName() {
      return name;
    }
    public void setName(String name) {
      this.name = name;
    }
    public String getDescription() {
      return description;
    }
    public void setDescription(String description) {
      this.description = description;
    }
    public String getServerIp() {
      return serverIp;
    }
    public void setServerIp(String serverIp) {
      this.serverIp = serverIp;
    }
  }
}
```

- @Component는 스프링 빈 클래스임을 표시하는 애너테이션
- @ConfigurationProperties(prefix="myapp")
	- application.properties 파일에 정의된 모든 프로퍼티 앞에
	- 접두어 myapp을 붙였다고 스프링 부트에 알리는 애너테이션

---

## Reference

[실전 스프링 부트 워크북](http://www.hanbit.co.kr/store/books/look.php?p_code=B2433442478)
