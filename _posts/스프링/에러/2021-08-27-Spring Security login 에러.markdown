---
title: Spring Security login 동작안하는 에러
categories: springError
---

Spring Security의 기본적인 login check해서 있으면 인증해주는 processing이 동작 안하는 에러

### 원인
기본적으로 login 동작하는 processingUrl 이 post method : /login 인데, controller의 RequestMapping에 Http method 상관 없이 /login 경로가 있을 경우, 
혼동하여 Spring Security 의 processing 이 동작을 안함

### 해결법
class WebSecurityConfig extends WebSecurityConfigurerAdapter 를 만들고 Override 메소드 configure(HttpSecurity httpSecurity) 를 만들어 
httpSecurity.formLogin().loginProcessingUrl("경로") 에서 controller의 RequestMapping에 안걸리는 경로로 지정하면 된다.

**example**
```java
@EnableWebSecurity
@Configuration
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {
@Override
    public void configure(HttpSecurity httpSecurity) throws Exception {
        httpSecurity.formLogin()
                        .loginPage("/login/login")
                        .loginProcessingUrl("/auth")
                        .defaultSuccessUrl("/")
    }
}
```
