---
title: "validation"
categories: project_community
---

[1. @Validate를 이용한 검증 구현](#Validate를-이용한-검증-구현)

## @Validate를 이용한 검증 구현
![1](https://user-images.githubusercontent.com/48073115/149625022-9aad099a-cdf9-4273-97f4-fbd1bfa8365a.png)

### 코드
![1](https://user-images.githubusercontent.com/48073115/149625165-478dc31e-3c60-4000-ac85-c459fc1c3e9d.png)

+ 검증 로직을 편하게 사용하기 위해 회원가입 필드 값을 가져올 DTO에 **Bean Validation** 을 적용했다.

![1](https://user-images.githubusercontent.com/48073115/149625511-3aea651d-50c2-458b-8cad-a0b588a38416.png)

+ errors.properties 에 사용할 검증 에러메시지들을 적는다.

![1](https://user-images.githubusercontent.com/48073115/149625631-ddf095c2-f545-41f8-b6bc-de338cb6e6ca.png)
![1](https://user-images.githubusercontent.com/48073115/149625672-db621a58-528e-49f6-9dda-a181de9381e2.png)

+ 인터페이스 **Validator** 를 이용해 검증 로직을 분리하여 구현했다.
