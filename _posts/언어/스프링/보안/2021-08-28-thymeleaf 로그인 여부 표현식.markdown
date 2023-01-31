---
title: Thymeleaf Spring Security 표현식
categories: springSecurity
---

### gradle

```gradle
implementation group: 'org.thymeleaf.extras', name: 'thymeleaf-extras-springsecurity5', version: '3.0.4.RELEASE'
```

### 권한별 표현식

**example**
```thymeleaf
<div sec:authorize="hasRole('ROLE_ADMIN')">
  This content is only shown to administrators.
</div>
<div sec:authorize="hasRole('ROLE_USER')">
  This content is only shown to users.
</div>
```

### Spring Security login 여부 표현식

**example**
```thymeleaf
<th:block th:if="${#authorization.expr('isAuthenticated()')}">
  <span th:text="${#authentication.getPrincipal().getNickname()}"></span>
</th:block>
```

+ ${#authorization.expr('isAuthenticated()')} : 로그인 여부
+ ${#authentication.getPrincipal().getNickname()} : UserDetails 에 상속된 객체 nickname 값

일반적으로 위와 같이 사용하면 아무 문제가 없다.
하지만 security가 적용되지 않은 요청 범위의 페이지(예를 들면 /error/...와 같은 에러 페이지)를 호출할 경우
Caused by: java.lang.IllegalArgumentException: Authentication object cannot be null
에러가 발생한다.

#### 해결법
spring security 를 통해 authorization 이 생성되지 않은 상태에서 expression을 호출해서 생긴 에러이다.

**example**
```thymeleaf
<th:block th:if="${#authorization.getAuthentication() != null && #authorization.expr('isAuthenticated()')}">
...
</th:block>
```

위와 같이 선언하면 해결된다.

출처 - [thymeleaf 에서 spring security login 여부를 변수로 사용하기](http://bluesky-devstudy.blogspot.com/2016/09/thymeleaf-spring-security-login.html)
