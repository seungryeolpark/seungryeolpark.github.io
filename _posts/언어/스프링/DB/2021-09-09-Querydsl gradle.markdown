---
title: Querydsl gradle 설정
categories: springDB
---

### Gradle 설정
```gradle
plugins {
  //querydsl 추가
  id 'com.ewerk.gradle.plugins.querydsl' version '1.0.10'
}

dependencies {
  //querydsl 추가
 implementation 'com.querydsl:querydsl-jpa'
}

//querydsl 추가
def querydslDir = "$buildDir/generated/querydsl"
querydsl {
 jpa = true
 querydslSourcesDir = querydslDir
}
sourceSets {
 main.java.srcDir querydslDir
}
configurations {
 querydsl.extendsFrom compileClasspath
}
compileQuerydsl {
 options.annotationProcessorPath = configurations.querydsl
}
```

### Querydsl 환결설정 검증
+ **Gradle Intellij 사용법**
  + Gradle -> Tasks -> build -> clean
  + Gradle -> Tasks -> other -> compileQuerydsl
+ **Gradle 콘솔 사용법**
  + ./gradlew clean compileQuerydsl
+ **Q 타입 생성 확인**
  + project에서 build -> generated -> querydsl
    + entity java 파일이 생성되어 있는지 확인

### 참고
+ Q타입은 컴파일 시점에 자동 생성되므로 버전관리(GIT)에 포함하지 않는 것이 좋다.
+ 앞서 설정에서 **생성 위치를 gradle build 폴더 아래 생성**되도록 했기 때문에 이 부분도 자연스럽게 해결된다.
  + ex) def querydslDir = "$buildDir/generated/querydsl"
+ 대부분 gradle build 폴더를 git에 포함하지 않는다.

출처 - [실전! Querydsl](https://www.inflearn.com/course/Querydsl-%EC%8B%A4%EC%A0%84/dashboard)
