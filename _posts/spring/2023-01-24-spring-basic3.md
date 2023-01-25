---
layout: single
title:  "[Spring] Spring의 핵심 원리 기초3 "
categories: 
    - spring  #카테고리
tag: [Spring] #태그
toc: true  #오른쪽에 있는 목차
toc_sticky: true #목차 고정
author_profile: true  #왼쪽에 자기 소개란 프로필을 이 페이지에 들어갈때 끄는 기능
sidebar:
    nav: "sidebar-category" #navigation.yml에 있는 docs를 뜻한다.
search: true  #이 페이지는 검색에 나옴.
---

## 6. 싱글톤 컨테이너

### 1. 웹 애플리케이션과 싱글톤

- 싱글톤이 만들어지게 된 계기
    - AppConfig를 요청할 때마다 객체를 새로 생성한다. 고객 트래픽이 초당 100이 나오면 초당 100개 객체가 생성되고 소멸된다. → 메모리 낭비가 심하다.
    - 그래서 해결방안은 해당 객체가 딱 1개가 생성되고, 공유하도록 설계하면 된다. → 싱글톤 패턴
- 클래스의 인스턴스가 딱 1개만 생성되는 것을 보장하는 디자인 패턴
- 객체 인스턴스를 2개 이상 생성하지 못하도록 막아야 한다.
    - private 생성자를 사용해 외부에서 new 키워드로 객체 생성을 못하게 막는다.
    
    [싱글톤 예시](https://github.com/woolfiekim/spring-basic-study/blob/master/src/test/java/hello/core/singleton/SingletonService.java)
    
- 이러한, 싱글톤 패턴은 문제점이 많다. 이를 스프링 컨테이너가 해결해준다.

### 2. 싱글톤 컨테이너

스프링 컨테이너가 싱글톤 패턴의 문제점을 해결하고, 객체 인스턴스를 싱글톤으로 관리한다.(전에 만든 그 스프링 빈이 바로 싱글톤으로 관리되는 빈이다.)

- 싱글톤 레지스트리 : (스프링 컨테이너는 싱글톤 컨테이너 역할을 한다.) 이렇게 싱글톤 객체를 생성하고 관리하는 기능을 말한다.
  
[싱글톤 레지스트리 예시](https://github.com/woolfiekim/spring-basic-study/blob/master/src/test/java/hello/core/singleton/SingletonTest.java)

- 참고 : 스프링의 기본 빈 등록 방식은 싱글톤이지만, 싱글톤 방식만 지원하는 것은 아니다. 요청할 때 마다 새로운 객체를 생성해서 반환하는 기능도 제공한다.

### 3. 싱글톤 방식의 주의점

- 싱글톤 방식은 여러 클라이언트가 하나의 같은 객체 인스턴스를 공유하기 때문에 싱글톤 객체는 상태를 유지(stateful)하게 설계하면 안된다.
- **무상태(stateless)로 설계해야 한다.**
    - 특정 클라이언트에 의존적인 필드가 있으면 안된다.
    - 값을 변경할 수 있는 필드가 있으면 안된다.
    - 가급적 읽기만 가능해야한다.
    - 필드 대신에 자바에서 공유되지 않는 지역변수, 파라미터, ThreadLocal 등을 사용해야 한다.
- 스프링 빈의 필드에 공유 값을 설정하면 정말 큰 장애가 발생할 수 있다!!!

[싱글톤 방식의 주의점 예시](https://github.com/woolfiekim/spring-basic-study/blob/master/src/test/java/hello/core/singleton/StatefulService.java)

### 4. @Configuration과 싱글톤

각자 다른 빈에서 같은 메서드를 호출하면서 2개의 객체가 생성되어 싱글톤이 깨지는 것처럼 보인다.


본 포스팅은 김영한 선생님의 강의를 보고 정리한 글입니다.