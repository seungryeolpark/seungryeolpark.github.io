---
title: spring security
categories: springSecurity
---

### Spring Security란?
+ 보안 솔루션을 제공해주는 Spring 기반의 스프링 하위 프레임워크
+ Spring Security 에서 제공해주는 보안 솔루션을 사용하면 개발자가 보안 관련 로직을 짤 필요가 없어 굉장히 간편하다.
+ Spring Security 를 이해하기 위해서는 인증과 권한에 대해 알아야한다.

### 인증과 권한
+ 인증은 자신이 누구라고 주장하는 사람을 정말 누구가 맞는지 확인하는 작업
+ 권한은 특정 부분에 접근할 수 있는지에 대한 여부를 확인하는 작업
+ 인증과 권한을 확인하기 위해 직접 로직을 짠다면 상당한 시간과 노력이 필요하다.
+ Spring Security 에서 제공하는 인증 메커니즘과 권한 부여 기능을 사용한다면 인증/인가 처리를 쉽게 처리할 수 있다.

### Spring Security 기능
+ 모든 URL에 대한 인증을 요구
+ 로그인 폼을 생성, 로그아웃 처리
+ CSRF 공격을 방어
+ Session Fixation 공격을 방어
+ 요청 헤더 보안
  + HTTP Strict Transport Security (HSTS)
  + X-Content-Type-Options
  + 캐시 컨트롤 (정적 리소스는 캐싱)
  + X-XSS-Protection
  + 클릭 재킹 방지를 위한 X-Frame-Options
+ 서블릿 API 메소드와 통합
  + HttpServletRequest#getRemoteUser()
  + HttpServletRequest.html#GetUserPrincipal()
  + HttpServletRequest.html#isUserInRole(java.lang.String)
  + HttpServletRequest.html#login(java.lang.String, java.lang.String)
  + HttpServletRequest.html#logout()
  
  
