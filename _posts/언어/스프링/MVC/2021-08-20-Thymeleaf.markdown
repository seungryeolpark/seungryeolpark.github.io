---
title: Thymeleaf
categories: springMVC
---
**목차**
+ [기본 표현식](#기본-표현식)
+ [입력 폼 처리](#입력-폼-처리)
+ [반복문 처리](#반복문-처리)


## 기본 표현식

### 간단한 표현
+ 변수 표현식 : ${...}
+ 선택 변수 표현식 : *{...}
+ 메시지 표현식 : #{...}
+ 링크 URL 표현식 : @{...}
+ 조각 표현식 : ~{...}

### 리터럴
+ 텍스트 : 'one text', 'two text', ...
+ 숫자 : 0, 34, 3.0, 12.3, ...
+ 불린 : true, false
+ 널 : null
+ 리터럴 토큰 : one, sometext, main, ...

### 문자 연산
+ 문자 합치기 : +
+ 리터럴 대체 : |The name is ${name}|

### 산술 연산
+ Binary Operators : +, -, *, /, %
+ Minus sign (unary operator) : -

### 불린 연산 :
+ Binary Operators : and, or
+ Boolean negation (unary operator) : !, not

### 비교와 동등
+ 비교 : >, <, >=, <= (gl, lt, ge, le)
+ 동등 연산 : ==, != (eq, ne)

### 조건 연산
+ If-then : (if) ? (then)
+ If-then-else : (if) ? (then) : (else)
+ Default : (value) ?: (defaultvalue)
+ th:if="조건"
  + if 문으로 조건에 true하면 출력한다.
+ th:unless="조건"
  + not if 문으로 조건에 false하면 출력한다.
### 특별한 토큰
+ No-Operation : _

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
+ th:field="*{...}"
  + thymeleaf의 th:field 는 매우 똑똑하게 동작하는데, 정상 상황에서는 모델 객체의 값을 사용하지만, 오류가 발생하면 FieldError에 보관한 값을 사용해서 값을 출력한다.
  + 타입 오류로 바인딩에 실패하면 스프링은 FieldError 를 생성하면서 사용자가 입력한 값을 넣어둔다. 그리고 해당 오류를 BindingResult 에 담아서 컨트롤러를 호출한다. 따라서 타입 오류 같은 바인딩 실패시에도 사용자의 오류 메시지를 정상 출력할 수 있다.

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

## 반복문 처리
### th:each
```java
@Data
public class BoardListDto {
    private Long boardId;
    private String title;
    private String nickname;
    private LocalDateTime createTime;
    ...
```
```java
public String board(Model model) {
    List<BoardListDto> boardListDtoList = boardService.getBoardList();
    
    model.addAttribute("boardDtoList", boardListDtoList);
    ...
```
```thymeleaf
<tr th:each="boardDto : ${boardDtoList}">
    <td th:text="${boardDto.boardId}"></td>
    <td th:text="${boardDto.title}"></td>
    <td th:text="${boardDto.nickname}"></td>
    <td th:text="${boardDto.createTime}"></td>
</tr>
```

## 참고
```thymeleaf
<tr th:each="member : ${members}">
    <td th:text="${member.id}"></td>
    <td th:text="${member.name}"></td>
    <td th:text="${member.address?.city}"></td>
    <td th:text="${member.address?.street}"></td>
    <td th:text="${member.address?.zipcode}"></td>
</tr>
```
+ 타임리프에서 ? 를 사용하면 null 을 무시한다.

출처 - [스프링 MVC 2편 - 백엔드 웹 개발 활용 기술](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-2/dashboard)
