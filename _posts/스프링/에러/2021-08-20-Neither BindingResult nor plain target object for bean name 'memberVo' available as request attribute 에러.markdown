---
title: Neither BindingResult nor plain target object for bean name 'memberVo' available as request attribute 에러
categories: springError
---

스프링 유효성을 검사하다가 생긴 에러

### 해결 방법

+ 객체 생성 확인 - 키 값이 "memberVo" 인 경우
+ 해당 URL의 GET 파트에 ModelAttribute를 "memberVo"를 생성해서 전달 했는지 확인

```java
public String memberSign(Model model) {
  model.addAttribute("memberVo", new MemberVo());
```

+ 해당 URL의 POST 파트에 @Valid 하는 객체(VO)에 @ModelAttribute("MemberVo")를 선언 했는지 확인

```java
public String memberInsert(Model model, @ModelAttribute("memberVo"),
                           @Valid MemberVo memberVo, BindingResult bindingResult,
                           RedirectAttributes rttr) {
```

+ 해당 URL의 view 파트에 <form> 태그에서 modelAttribute가 "memberVo"인지 확인
  
```html
<form action="/signup" method="post" th:action th:object="${memberVo}" 
      accept-charset="utf-8" class="form" role="form">
```
  
출처 - [미니 블로그 : 메모하는 습관](https://otrodevym.tistory.com/entry/Neither-BindingResult-nor-plain-target-object-for-bean-name-memberVo-available-as-request-attribute)
