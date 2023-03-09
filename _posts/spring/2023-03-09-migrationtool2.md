---
layout: single
title:  "[스프링으로 migration 툴 만들기] 2. Could not resolve org. springframework. boot: spring-boot-gradle-plugin:3.0.4."
categories: 
    - spring  #카테고리
tag: [Spring, migration, setting, error] #태그
toc: true  #오른쪽에 있는 목차
toc_sticky: true #목차 고정
author_profile: true  #왼쪽에 자기 소개란 프로필을 이 페이지에 들어갈때 끄는 기능
sidebar:
    nav: "sidebar-category" #navigation.yml에 있는 docs를 뜻한다.
search: true  #이 페이지는 검색에 나옴.
---


```plaintext
A problem occurred configuring root project
• No matching variant of org.springframework.boot:spring-boot-
> Could not resolve all files for configuration ':classpath'.
> Could not resolve org. springframework. boot: spring-boot-gradle-plugin:3.0.4.
Required by:
project : > org.springframework.boot:org.springframework.boot.gradle.plugin:3.0.4
> No matching variant of org.springframework. boot: spring-boot-gradle-plugin:3.0.4 was found. The consumer wE
- Variant 'apiElements' capability org.springframework.boot: spring-boot-gradle-plugin:3.0.4 declares a 1
- Incompatible because this component declares an API of a component compatible with Java 17 and the
- Other compatible attribute:
- Doesn't say anything about org. gradle.plugin.api-version (required '7.6.1')
- Variant 'javadocElements' capability org.springframework. boot: spring-boot-gradle-plugin:3.0.4 declares
Incompatible because this component declares documentation and the consumer needed a library
- Other compatible attributes:
```

세팅을 끝내고 create버튼을 눌렀더니 위와 같은 에러가 떴다. 

![](/assets/images/2023-03/09/error1.png)

사실 이 에러는 처음 프로젝트를 만들면 보여주는 Getting Started 를 자세히 들여다보면 답이 나와있다.

`~~~~'1.8' to '17'` 라고 써있다. 

이 프로젝트는 자바 버전이 낮은 것으로 써야하기 때문에 17이 아니라 1.8로 바꾸었다.

여기서 내 실수가 드러난 것이다.

자바 버전에 맞춰서 spring boot의 버전도 맞춰줬어야하는데 아뿔싸, 바꾸지 않은 것이다!

![](/assets/images/2023-03/09/error2.png)

위 사진처럼 build.gradle 파일로 들어가

```gradle
plugins{
    id 'org.springframework.boot' version '2.7.7'
}

sourceCompatibility = '1.8'
```

이렇게 두 가지의 숫자를 바꾸어주었다.

그랬더니 빌드 성공!!

![](/assets/images/2023-03/09/good.jpg)