---
layout: post
title:  "JPA setting postgresql"
author: Degan
categories: spring boot 
comments: true
tags: spring boot
---

## JPA setting postgresql

```
spring.datasource.url=jdbc:postgresql://localhost:5432/springdata
spring.datasource.username=degan
spring.datasource.password=pass

# 운영시에는 validate로
spring.jpa.hibernate.ddl-auto=create
spring.jpa.properties.hibernate.jdbc.lob.non_contextual_creation=true

spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=true

# sql parameter 
logging.level.org.hibernate.type.descriptor.sql=trace
```

---
## Reference


