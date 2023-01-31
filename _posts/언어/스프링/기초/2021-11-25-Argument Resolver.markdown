---
title: Argument Resolver
categories: springBase
---

# Argument Resolver
+ 사용자가 컨트롤러의 메서드 인자값으로 임의의 값을 전달하려할 때 사용된다.
+ ex) 세션에 저장되어 있는 값 중, 특정 이름의 값을 메서드 인자로 전달한다.

## Argument Resolver 작성 방법
1. org.springframwork.web.method.support.HandlerMethodArgumentResolver 를 구현한 클래스 작성
2. supportsParameter 메서드를 오버라이딩 한 후, 원하는 타입의 인자가 있는지 검사하여 있으면 true 리턴
3. resolveArgument 메서드를 오버라이딩한 후, 메서드의 인자로 전달할 값을 리턴한다.
4. 스프링에서 인식될 수 있도록 Argument Resolver 를 WebMvcConfigurer 에 추가한다.

### LoginUserArgumentResolver.java
```java
@RequiredArgsConstructor
@Component
public class LoginUserArgumentResolver implements HandlerMethodArgumentResolver {

   private final HttpSession httpSession;

   @Override
   public boolean supportsParameter(MethodParameter parameter) {
       boolean isLoginUserAnnotation = parameter.getParameterAnnotation(LoginUser.class) != null;
       boolean isUserClass = SessionUser.class.equals(parameter.getParameterType());

       return isLoginUserAnnotation && isUserClass;
   }

   @Override
   public Object resolveArgument(MethodParameter parameter, ModelAndViewContainer mavContainer, NativeWebRequest webRequest, WebDataBinderFactory binderFactory) throws Exception {
       return httpSession.getAttribute("user");
   }
}
```
+ supportsParameter
  + 컨트롤러 메서드의 특정 파라미터를 지원하는지 판단합니다.
  + 지원하면 true 안하면 false
+ resolveArgument
  + 파라미터에 전달할 객체를 생성

### WebMvcConfigurer.java
```java
@RequiredArgsConstructor
@Configuration
public class WebConfig implements WebMvcConfigurer {
   private final LoginUserArgumentResolver loginUserArgumentResolver;

   @Override
   public void addArgumentResolvers(List<HandlerMethodArgumentResolver> argumentResolvers) {
       argumentResolvers.add(loginUserArgumentResolver);
   }
}
```
