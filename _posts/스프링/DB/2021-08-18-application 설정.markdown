---
title: 데이터베이스 application.yml 설정 
categories: springDB
---

## MariaDB 설정
**application.yml**
```yml
spring:
  datasource:
    url: jdbc:mariadb://localhost:3306/test?characterEncoding=UTF-8&serverTimezone=UTC
    driverClassName: org.mariadb.jdbc.Driver
    username: username
    password: password
  jpa:
    hibernate:
      ddl-auto: create
    properties:
      show_sql: true
      format_sql: true
    
logging.level:
  org.hibernate.SQL: debug
  org.hibernate.type: trace
```
### spring.jpa.hibernate.ddl-auto
+ create
  + 애플리케이션 실행 시점에 테이블을 drop하고 다시 생성한다.

### spring.jpa.properties
+ show_sql
  + System.out 에 하이버네이트 실행 SQL을 남긴다.

### logging.level
+ org.hibernate.SQL: debug
  + logger를 통해 debug(로깅레벨)이상 하이버네이트 실행 SQL을 남긴다.

출처 - [실전! 스프링 부트와 JPA 활용1](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8-JPA-%ED%99%9C%EC%9A%A9-1/dashboard)
