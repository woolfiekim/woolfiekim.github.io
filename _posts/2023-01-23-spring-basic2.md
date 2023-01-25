---
layout: single
title:  "[Spring] Spring의 핵심 원리 기초2 "
categories: 
    - spring  #카테고리
tag: [Spring] #태그
toc: true  #오른쪽에 있는 목차
toc_sticky: true #목차 고정
author_profile: true  #왼쪽에 자기 소개란 프로필을 이 페이지에 들어갈때 끄는 기능
sidebar:
    nav: "docs" #navigation.yml에 있는 docs를 뜻한다.
search: true  #이 페이지는 검색에 나옴.
---

본 포스팅은 김영한 선생님의 강의를 보고 정리한 글입니다.

## 4. IOC, DI 그리고 컨테이너

### 4-1. 제어의 역전 IoC(Inversion of Control) : 프로그램의 흐름을 스스로 제어하는 것이 아니라 외부에서 관리하는 것

- 프레임워크가 내가 작성한 코드를 제어하고, 대신 실행하면 그것은 프레임워크다.(JUnit)
- 반면에 내가 작성한 코드가 직접 제어의 흐름을 담당한다면 그것은 라이브러리다.

### 4-2. 의존관계 주입 DI(Dependency Injection)

- 의존관계는 **정적인 클래스 의존 관계와 실행 시점에 결정되는 동적인 객체(인스턴스) 의존관계** 둘을 분리해서 생각해야한다.
- 정적인 클래스 의존관계
    - 애클리케이션을 실행하지 않아도 분석할 수 있다. (import 만 봐도 알 수 있다)
- 동적인 객체 인스턴스 의존관계
    - 애플리케이션의 실행 시점에 실제 생성된 객체 인스턴스의 참조가 연결된 의존 관계다.

- 애플리케이션 **실행 시점(런타임)**에 외부에서 실제 구현 객체를 생성하고 클라이언트에 전달해서 클라이언트와 서버의 실제 의존관계가 연결되는 것을 의존관계 주입이라 한다.
- 의존관계 주입을 하면 클라이언트 코드, 정적인 클래스의 의존관계를 변경하지 않고, 동적인 객체 인스턴스 의존관계와 클라이언트가 호출하는 대상의 타입 인스턴스를 변경할 수 잇다.
- 만든 예시의 [AppConfig](https://github.com/woolfiekim/sprint-basic-study/blob/master/src/main/java/hello/core/AppConfig.java)처럼 객체를 생성하고 관리하면서 의존관계를 연결해 주는 것을 IoC 컨테이너 or **DI 컨테이너**라 한다.
- 또는 어셈블러, 오브젝트 팩토리 등으로 불리기도 한다.

## 5.  스프링 컨테이너

- ApplicationContext를 스프링 컨테이너라 한다.
- 스프링 컨테이너는 `@Configuration` 이 붙은 `[AppConfig](https://github.com/woolfiekim/sprint-basic-study/blob/master/src/main/java/hello/core/AppConfig.java)` 를 설정 정보로 사용한다.
- `@Bean` 이라 적힌 메서드를 모두 호출해서 반환된 객체를 스프링 컨테이너에 등록한다. 이렇게 등록된 객체를 스프링 빈이라 한다.
- 스프링 빈은 `@Bean` 이 붙은 메서드의 명을 스프링 빈의 이름으로 사용한다.
  - 빈 이름을 직접 부여할 수 있다. @Bean(name="memberService2")
- 그렇게 하면 필요한 객체를 `AppConfig`에서 직접 조회가 아닌 스프링 컨테이너를 통해서 필요한 스프링 빈(객체)를 찾아야 한다. `applicationContext.getBean()` 메서드를 사용해서 찾을 수 있다.

### 5-1 스프링 컨테이너 생성

- `ApplicationContext`를 스프링 컨테이너라 한다. (인터페이스)
- 스프링 컨테이너는 XML을 기반으로 만들 수 있고, 애노테이션 기반의 자바 설정 클래스로 만들 수 있다.
- 자바 설정 클래스를 기반으로 스프링 컨테이너 `ApplicationContext`를 만들어보자.
`new AnnotationConfigApplicationContext(AppConfig.class);` 이 클래스는 `ApplicationContext` 인터페이스의 구현체이다.
- (스프링 컨테이너에 넣은 스프링 빈을 확인하기)

[sprint-basic-study/ApplicationContextInfoTest.java at master · woolfiekim/sprint-basic-study](https://github.com/woolfiekim/sprint-basic-study/blob/master/src/test/java/hello/core/beanfind/ApplicationContextInfoTest.java)

### 5-2 BeanFactory와 ApplicationContext

![](/assets/images/2023-01-24/factory1.png)

- BeanFactory
    - 스프링 컨테이너의 최상위 인터페이스이다.
    - 스프링 빈을 관리하고 조회하는 역할을 담당한다.
    - `getBean()` 을 제공한다.
- ApplicationContext
    - BeanFactory 기능을 모두 상속받아서 제공한다.
    - 그러면, BeanFactory와의 차이점은 애플리케이션을 개발할 때 빈을 관리하고 조회하는 기능 뿐만이 아니라 수 많은 부가기능이 필요하다. 이러한 부가기능을 ApplicationContext가 제공한다.
    - 그래서 BeanFactory보다 부가기능이 포함된 ApplicationContext를 사용한다.
    - BeanFactory, ApplicationContext를 스프링 컨테이너라 한다.

![](/assets/images/2023-01-24/factory2.png)

ex) 메시지 소스를 활용한 국제화 기능(한국-한국어 / 영어권-영어로 출력), 환경변수(로컬, 개발, 운영 등을 구분해서 처리), 애플리케이션 이벤트, 편리한 리소스 조회

### 5-3 스프링 빈 설정 메타 정보 - BeanDefinition

- 빈 설정 메타 정보라 한다.
- `@Bean`, `<bean>` 당 각각 하나씩 메타 정보가 생성된다.
- [<bean>예시](https://github.com/woolfiekim/sprint-basic-study/blob/master/src/main/resources/appConfig.xml)