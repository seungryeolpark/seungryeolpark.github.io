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

### WebSecurityConfig
**WebSecurityConfig.java**

![보안1](https://user-images.githubusercontent.com/48073115/130959681-8134f4d3-9573-424f-b127-ede882f6503d.png)

+ @EnableWebSecurity : Spring Security 를 활성화한다는 의미의 어노테이션
+ WebSecurityConfigurerAdapter : Spring Security의 설정파일로서의 역할을 하기 위해 상속해야 하는 클래스
+ web.ignoring().antMatchers() : 인증을 무시할 경로들을 설정
+ http.authorizeRequests() : 접근에 대한 인증 설정이 가능하다.
  + anyMatchers() : 경로 설정과 권한 설정이 가능하다.
  + anyRequest() : anyMatchers 에서 설정하지 않은 나머지 경로를 의미한다.
    + permitAll() : 누구나 접근이 가능
    + hasRole() : 특정 권한이 있는 사람만 접근 가능
    + authenticated() : 권한의 종류에 상관이 없이 권한이 있으면 접근 가능
+ http.formLogin() : 로그인에 관한 설정이 가능하다.
  + loginPage() : 로그인 페이지 링크 설정
  + defaultSuccessUrl() : 로그인 성공 후 리다이렉트할 주소
+ http.logout() : 로그아웃에 관한 설정이 가능하다.
  + logoutSuccessUrl() : 로그아웃 성공 후 리다이렉트할 주소
  + invalidateHttpSession() : 로그아웃 이후 세션 전체 삭제 여부
+ auth.userDetailsService() : 로그인할 때 필요한 정보를 가져오는 곳
  + passwordEncoder() : 패스워드 인코더 설정
  
### User Entity
+ User Entity 는 UserDetails 를 상속받아서 구현합니다.
+ UserDetails에서 필수로 구현해야 하는 메소드는 아래와 같다.

| 메소드 이름 | 설명 |
|---|---|
| getAuthorities() | 사용자의 권한을 콜렉션 형태로 반환<br> (클래스 자료형은 GrantedAuthority를 구현해야함) |
| getUsername() | 사용자의 id를 반환<br> (id는 unique한 값이여야함) |
| getPassword() | 사용자의 password를 반환 |
| isAccountNonExpired() | 계정 만료 여부 반환<br> (true = 만료되지 않음을 의미) |
| isAccountNonLocked() | 계정 잠금 여부 반환<br> (true = 잠금되지 않음을 의미) |
| isCredentialsNonExpired() | 패스워드 만료 여부 반환<br> (true = 만료되지 않음을 의미) |
| isEnabled() | 계정 사용 가능 여부 반환<br> (true = 사용 가능을 의미) |

출처 - [Spring Security로 로그인/회원가입 프로젝트](https://shinsunyoung.tistory.com/78)
