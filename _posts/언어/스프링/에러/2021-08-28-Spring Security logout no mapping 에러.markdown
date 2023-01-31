---
title: Spring Security logout no mapping 에러
categories: springError
---

Spring Securiy logout 실행하는 url에 경로를 넣어놨지만 찾지 못하는 에러

### 원인
로그인할 때 csrf 를 적용하였을 경우 로그아웃할 때도 csrf 를 적용해야 하는데, 하지 않았을 경우 
no mapping 에러가 뜬다.

### 해결법
로그인할 때 csrf 를 적용하였으면 로그아웃할 때 csrf와 함께 post 를 수행해야 한다.

**example**
```html
<a class=nav-link href="#" onclick="document.getElementById('logout-form').submit();">
    <img th:src="@{/img/icon/logout.png}" width="20" height="20" />
    <span th:text="#{main.nav.logout}" />
</a>
<form id="logout-form" th:action="@{/logout}" method="post">
    <input type="hidden" th:name="${_csrf.parameterName}" th:value="${_csrf.token}">
</form>
```
