[//]: # (---)

[//]: # (title: "validation")

[//]: # (categories: project_community)

[//]: # (---)

[//]: # ()
[//]: # ([1. @Validate를 이용한 검증 구현]&#40;#validate를-이용한-검증-구현&#41;  )

[//]: # ([2. SweetAlert2를 이용한 검증 구현]&#40;#sweetalert2를-이용한-검증-구현&#41;)

[//]: # ()
[//]: # (## @Validate를 이용한 검증 구현)

[//]: # (### 코드)

[//]: # (![1]&#40;https://user-images.githubusercontent.com/48073115/149625165-478dc31e-3c60-4000-ac85-c459fc1c3e9d.png&#41;)

[//]: # ()
[//]: # (+ 검증 로직을 편하게 사용하기 위해 회원가입 필드 값을 가져올 DTO에 **Bean Validation** 을 적용했다.)

[//]: # ()
[//]: # (![1]&#40;https://user-images.githubusercontent.com/48073115/149625511-3aea651d-50c2-458b-8cad-a0b588a38416.png&#41;)

[//]: # ()
[//]: # (+ errors.properties 에 사용할 검증 에러메시지들을 적는다.)

[//]: # ()
[//]: # (![1]&#40;https://user-images.githubusercontent.com/48073115/149625631-ddf095c2-f545-41f8-b6bc-de338cb6e6ca.png&#41;)

[//]: # (![1]&#40;https://user-images.githubusercontent.com/48073115/149625672-db621a58-528e-49f6-9dda-a181de9381e2.png&#41;)

[//]: # ()
[//]: # (+ 인터페이스 **Validator** 를 이용해 검증 로직을 분리하여 구현했다.)

[//]: # ()
[//]: # (## SweetAlert2를 이용한 검증 구현)

[//]: # (### 코드)

[//]: # (![1]&#40;https://user-images.githubusercontent.com/48073115/149627575-100e33ff-c909-4ce7-8d39-08df3ff454b9.png&#41;)

[//]: # (![1]&#40;https://user-images.githubusercontent.com/48073115/149627419-9e163c7d-dcff-4896-85c8-4245c2285324.png&#41;)

[//]: # (![1]&#40;https://user-images.githubusercontent.com/48073115/149627474-1bf7f99c-b695-4854-83b7-0fff66bc2ed5.png&#41;)

[//]: # ()
[//]: # (+ HashMap errors 의 키값에 field 값을 넣고 values 값에 오류 메시지를 넣었습니다.)

[//]: # (+ 모든 field 값을 검사하여 **Bean Validation** 에 걸려 필드 오류가 생길 경우 리다이렉트로 파라미터를 넘길 수 있는 **addFlashAttribute** 메서드로 오류 메시지와 DTO 파라미터를 리다이렉트로 넘긴다.)

[//]: # ()
[//]: # (![1]&#40;https://user-images.githubusercontent.com/48073115/149627956-5073cdba-eda4-4fb8-b503-ec071ca73862.png&#41;)

[//]: # (![1]&#40;https://user-images.githubusercontent.com/48073115/149628039-73267803-5196-4dd7-9038-0f55160db068.png&#41;)

[//]: # ()
[//]: # (+ 리다이렉트한 DTO가 있을 경우 그것을 받고 없으면 새로 만든다.&#40;SweetAlert2로 오류 메시지가 떠도 수정하기 위한 내용을 저장하기 위함&#41;)

[//]: # (+ 리다이렉트한 오류 메시지가 있을 경우 SweetAlert2 를 실행하고 없으면 실행하지 않는다.)
