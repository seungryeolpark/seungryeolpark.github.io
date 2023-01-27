---
title: Failed to determine a suitable driver class 에러
categories: springError
---

SpringBoot 프로젝트를 막 설정하고나서 기동을 하는데 다음과 같은 에러가 나는 경우가 있다.

![에러1](https://user-images.githubusercontent.com/48073115/127774533-1a7047c4-5447-40af-801a-dea73bbdb1ef.png)

SpringBoot는 어플리케이션이 시작될 때 필요한 기본 설정들을 자동으로 설정하게 되어있는데, 그중에 DataSource 설정이 자동 구성될 때 필요한 데이터베이스 정보가 설정되지 않아 발생하는 문제다.  

프로젝트가 생성될 때 application.properties 파일이 자동 생성되었으나 빈파일로 되어있을 것인데, 사용자가 원하는 DB 설정을 넣고, 맞는 드라이버와 라이브러리 설치, JDBC 설정을 해야한다는 의미다.  

만약 당장 JDBC 설정이 필요없고 어떤 DB를 사용할지 결정하지 않았다면 다음의 소스를 참조하면 된다.  
스프링 메인 클래스에 어노테이션을 추가한다.

```java
@SpringBootApplication(exclude={DataSourceAutoConfiguration.class})
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

다시 실행하면 정상적으로 실행된다.

출처 - [[SpringBoot]Failed to determine a suitable driver class 에러](https://lemontia.tistory.com/586)

