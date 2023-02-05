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

## 8. 의존관계 자동 주입

### 1. 의존관계 주입 방법

- 생성자 주입
- 수정자 주입(setter 주입)
- 필드 주입
- 일반 메서드 주입

1. 생성자 주입
- 생성자를 통해서 의존 관계를 주입 받는 방법이다.
- 생성자 호출 시점에 딱 1번만 호출된다. → **불변, 필수** 의존 관계에 사용

2. 수정자 주입(setter 주입)
- setter라 불리는 필드의 값을 변경하는 수정자 메서드를 통해서 의존관계를 주입하는 방법이다.
- **선택, 변경** 가능성이 있는 의존관계에 사용

3. 필드 주입
- 코드가 간결해서 좋지만, 외부에서 변경이 불가해 테스트가 힘들다.
- 사용하지 말자!

4. 일반 메서드 주입
- 일반 메서드를 통해서 주입 받을 수 있다.
- 한 번에 여러 필드를 받을 수 있다.


### 2. 옵션 처리

주입할 스프링 빈이 없어도 동작하기 위해서 자동 주입 대상을 옵션으로 처리하는 방법

- `@Autowired(required=false)` : 자동 주입할 대상이 없으면 수정자 메서드 자체가 호출 안됨
- `org.springframework.lang.@Nullable` : 자동 주입할 대상이 없으면 null이 입력된다.
- `Optional<>` : 자동 주입할 대상이 없으면 `Optional.empty`가 입력된다.

[예시 코드](https://github.com/woolfiekim/spring-basic-study/blob/master/src/test/java/hello/core/autowired/AutowiredTest.java)

### 3. 생성자 주입을 선택해라!

[예시 코드와 설명](https://github.com/woolfiekim/spring-basic-study/blob/master/src/test/java/hello/core/order/OrderServiceImplTest.java)

### 4. 롬복과 최신 트랜드

- @Getter, @Setter, @ToString 등등이 있다.
- `@RequiredArgsConstructor` : final 이 붙은 필드를 모아서 생성자를 자동으로 만들어준다. 그러면 `@Autowired를` 생략할 수 있다.

### 5. @Autowired 필드명, @Qualifier, @Primary

조회 대상 빈이 2개 이상일 때 해결 방법

- `@Autowired` 필드 명 매칭
- `@Qualifie` → `@Qualifier` 끼리 매칭 → 빈 이름 매칭
- `@Primary` 사용


> 본 포스팅은 김영한 선생님의 강의를 보고 정리한 글입니다.