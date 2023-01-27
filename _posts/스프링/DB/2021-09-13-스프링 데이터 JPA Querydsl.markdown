---
title: 스프링 데이터 JPA와 Querydsl
categories: springDB
---

**목차**
+ [사용자 정의 리포지토리](#사용자-정의-리포지토리)
+ [스프링 데이터 페이징 활용](#스프링-데이터-페이징-활용)
+ [컨트롤러에서 Pageable 파라미터로 받기](#컨트롤러에서-Pageable-파라미터로-받기)


## 사용자 정의 리포지토리
### 사용자 정의 리포지토리 구성
![사용자정의리포지토리1](https://user-images.githubusercontent.com/48073115/132995620-fd3afef5-ece2-4529-888d-c18057accabb.png)

1. 사용자 정의 인터페이스 작성
2. 사용자 정의 인터페이스 구현
3. 스프링 데이터 리포지토리에 사용자 정의 인터페이스 상속

### 스프링 데이터 페이징 활용
**전체 카운트를 한번에 조회하는 단순한 방법**
```java
QueryResults<MemberTeamDto> results = queryFactory
    ...
    .offset(pageable.getOffset())
    .limit(pageable.getPageSize())
    .fetchResults();

List<MemberTeamDto> content = results.getResults();
long total = results.getTotal();

return new PageImpl<>(content, pageable, total);
```
```java
PageRequest pageRequest = PageRequest.of(0, 3);
```
+ fetchResults() 를 사용하면 내용과 전체 카운트를 한번에 조회할 수 있다.(실제 쿼리는 2번 호출)
+ fetchResults() 는 카운트 쿼리 실행시 필요없는 order by 는 제거한다.
+ Pageable 인터페이스
  + pageable.getOffset() : offset 을 리턴합니다.
  + pageable.getPageSize() : 현재 페이지의 아이템 수량을 리턴합니다.
+ PageRequest.of(int page, int size) : 페이지 번호(0부터 시작), 페이지당 데이터의 수

**데이터 내용과 전체 카운트를 별도로 조회하는 방법**
```java
List<MemberTeamDto> content = queryFactory
    ...
    .offset(pageable.getoffset())
    .limit(pageable.getPageSize())
    .fetch();
    
long total = queryFactory
    ...
    .fetchCount();
    
return new PageImpl<>(content, pageable, total);
```
+ 전체 카운트를 조회 하는 방법을 최적화 할 수 있으면 이렇게 분리하면 된다.
+ 전체 카운트를 조회할 때 조인 쿼리를 줄일 수 있다면 상당한 효과가 있다.

**CountQuery 최적화**
```java
JPAQuery<Member> countQuery = queryFactory
    ...
    
return PageableExecutionUtils
    .getPage(content, pageable, countQuery::fetchCount);
```
+ count 쿼리가 생략 가능한 경우 생략해서 처리
    + 페이지 시작이면서 컨텐츠 사이즈가 페이지 사이즈보다 작을 때
    + 마지막 페이지 일 때 (offset + 컨텐츠 사이즈를 더해서 전체 사이즈 구함)

**참고**
+ PageableExecutionUtils.getPage 는 마지막 인자로 함수를 전달하는데 내부 동작에 의해서 토탈카운트가 페이지 사이즈 보다 적거나, 마지막 페이지일 경우 해당 함수를 실행하지 않는다.

### 컨트롤러에서 Pageable 파라미터로 받기
#### 요청 파라미터
```java
/members?page=0&size=10&sort=id,desc&sort=username,desc
```
#### 설정
**글로벌 설정 : 스프링 부트**
```java
spring.data.web.pageable.default-page-size=20 //기본 페이지 사이즈
spring.data.web.pageable.max-page-size=2000 //최대 페이지 사이즈
```

**개별 설정 : @PageableDefault 어노테이션 사용**
```java
public String list(
        @PageableDefault(size = 12, sort = "username", direction = Sort.Direction.DESC) Pageable pageable) {
                ...
```

**페이지 파라미터가 2개 이상일 경우**
```java
/members?member_page=0&order_page=1
```
```java
public String list(
        @Qualifier("member") Pageable memberPageable,
        @Qualifier("order") Pageable orderPageable, ...) {
                ...
```

출처 - [실전! Querydsl](https://www.inflearn.com/course/Querydsl-%EC%8B%A4%EC%A0%84)

출처 - [스프링데이터 JPA - 컨트롤러에서 Pageable 파라미터로 받아 페이징하기](https://yoonbing9.tistory.com/38)
