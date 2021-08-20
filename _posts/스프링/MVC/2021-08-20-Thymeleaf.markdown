---
title: Thymeleaf
categories: springMVC
---

## 입력 폼 처리
+ th:object
  + 커맨드 객체를 지정한다.
+ *{...}
  + 선택 변수 식으로 th:object 에서 선택한 객체에 접근한다.
+ th:field
  + HTML 태그의 id, name, value 속성을 자동으로 처리해준다.
  + id 는 th:field 에서 지정한 변수 이름과 같다.
  + name 은 th:field 에서 지정한 변수 이름과 같다.
  + value 는 th:field 에서 지정한 변수의 값을 사용한다.

**예제**
```html
<form action="/signup" method="post" th:action th:object="${signupMember}" accept-charset="utf-8" class="form" role="form">   <legend>회원가입</legend>
          <div>
            <label for="loginId">아이디</label>
            <input type="text" id="loginId" th:field="*{loginId}" class="form-control" placeholder="아이디를 입력하세요">
          </div>
          <div>
            <label for="nickname">닉네임</label>
            <input type="text" id="nickname" th:field="*{nickname}" class="form-control" placeholder="닉네임을 입력하세요">
          </div>
  ...
```

+ #{...}
  + 스프링의 메시지를 편리하게 조회한다.

+ #fields
  + #field 로 BindingResult 가 제공하는 검증 오류에 접근할 수 있다.
+ th:errors
  + 해당 필드에 오류가 있는 경우에 태그를 출력한다.(th:if 의 편의 버전이다.)
+ th:errorclass
  + th:field 에서 지정한 필드에 오류가 있으면 class 정보를 추가한다.
