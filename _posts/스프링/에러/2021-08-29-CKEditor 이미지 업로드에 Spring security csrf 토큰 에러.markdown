---
title: CKEditor 이미지 업로드에 Spring Security CSRF 토큰 에러
categories: springError
---

Spring Security CSRF 토큰을 적용할 경우 CKEditor의 CKFinder 이미지 업로드 403 에러

### 해결법
CKFinder의 uploadUrl에 Spring Security CSRF 토큰 값을 쿼리스트링 값으로 넣어서 해결

**example**
```script
<script th:inline="javascript">
      ClassicEditor
          .create( document.querySelector( '#content' ), {
          ckfinder: {
                  uploadUrl: '/post/upload.do?' + [[${_csrf.parameterName}]] + '=' + [[${_csrf.token}]]
              }
```
