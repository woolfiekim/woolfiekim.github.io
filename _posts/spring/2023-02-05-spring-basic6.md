---
layout: single
title:  "[Spring] Spring의 핵심 원리 기초4 "
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

## 9. 빈 생명주기 콜백
### 1. 빈 생명주기 콜백 시작

- 스프링 빈의 라이프 사이클 : 객체 생성 → 의존관계 주입

**스프링은 의존 관계 주입이 완료되면 스프링 빈에게 콜백 메서드를 통해 초기화 시점을 알려주는 다양한 기능을 제공, 스프링 컨테이너가 종료되기 직전에 소멸 콜백을 준다.** 

- 스프링 빈의 이벤트 라이프 사이클
    - 스프링 컨테이너 생성 → 스프링 빈 생성 → 의존관계 주입 → 초기화 콜백 → 사용 → 소멸전 콜백 → 스프링 종료
    - 초기화 콜백 : 빈이 생성되고, 빈의 의존관계 주입이 완료된 후 호출
    - 소멸전 콜백 : 빈이 소멸되기 직전에 호출

스프링은 크게 3가지 방법으로 빈 생명주기 콜백을 지원한다.

1. 인터페이스
2. 설정 정보에 초기화 메서드, 종료 메서드 지정
3. @PostConstruct, @PreDestroy 어노테이션 지원

### 2. 인터페이스 (IntializingBean, DisposableBean)

- `InitializingBean`은 `afterPropertiesSet()` 메서드로 초기화를 지원하다.
- `DisposableBeand`은 `destroy()` 메서드로 소멸을 지원한다.

이 인터페이스는 스프링 전용 인터페이스에 의존하고, 이름을 변경할 수 없다. 초창기에 나온 방법이기에 지금은 거의 사용하지 않는다.

### 3. 빈 등록 초기화, 소멸 메서드 지정

`@Bean(initMethod = "init", destroyMethod = "close")`

메서드 이름을 자유롭게 줄 수 있다. 스프링 빈이 스프링 코드에 의존하지 않는다. 코드가 아니라 설정 정보를 사용하기 때문에 코드를 고칠 수 없는 외부 라이브러리에도 초기화, 종료 메서드를 적용할 수 있다.

### 4. 애노테이션 (**@PostConstruct, @PreDestroy)**

최신 스프링에서 가장 권장하는 방법이다.

> 본 포스팅은 김영한 선생님의 강의를 보고 정리한 글입니다.