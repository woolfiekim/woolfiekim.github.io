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

## 7. 컴포넌트 스캔

### 1. 컴포넌트 스캔과 의존관계 자동 주입하기

- 컴포넌트 스캔은 `@Component` 가 붙은 클래스를 스캔해서 스프링 빈으로 등록해준다.
- (1) @ComponentScan
    - `@ComponentScan` 은 `@Component`가 붙은 모든 클래스를 스프링 빈으로 등록한다.
    - 이때 스프링 빈의 기본이름은 클래스 명을 사용하되 맨 앞 글자만 소문자를 사용한다.
        - ex) MemberServiceImpl.class → memberServiceImpl
        - 빈이름을 직접 지정 : ex) `@Component(”memberService2”)`
- (2) @Autowired 의존관계 자동 주입
    - 생성자에 `@Autowired`를 지정하면, 스프링 컨테이너가 자동으로 해당 스프링 빈을 찾아서 주입해준다.
    - `== getBean(MemberRepository.class)`와 동일하다고 생각하면 된다.

### 2. 탐색 위치와 기본 스캔 대상

1. 탐색할 패키지의 시작 위치 지정

```java
@ComponentScan(
          basePackages = "hello.core",
}
```

- `basePackages` : 탐색할 패키지의 시작 위치를 지정한다. 이 패키지를 포함해 하위 패키지를 모두 탐색한다.
    - `basePackages = {"hello.core", "hello.service"}` : 이렇게 여러 시작 위치를 지정 가능
- `basePackageClasses` : 지정한 클래스의 패키지를 탐색 시작 위치로 지정
- 지정하지 않으면, `@ComponentScan` 이 붙은 설정 정보 클래스의 패키지가 시작 위치가 된다.
- ***권장하는 방법*** : 패키지 위치를 지정하지 않고, 설정 정보 클래스의 위치를 프로젝트 최상단에 두는 것이다.
    - 스프링 부트도 이 방법을 기본으로 제공한다.
    - `@SpringBootApplication` (스프링부트의 대표 시작 정보)을 이 프로젝트의 시작 루트 위치에 두는 것이 관례이다. 이 설정 안에 `@ComponentScan` 이 들어있다.

1. 컴포넌트 스캔 기본 대상

컴포넌트 스캔의 대상

- `@Component` : 컴포넌트 스캔에서 사용
- `@Controlller` : 스프링 MVC 컨트롤러에서 사용
- `@Service` : 스프링 비즈니스 로직에서 사용
- `@Repository` : 스프링 데이터 접근 계층에서 사용
- `@Configuration` : 스프링 설정 정보에서 사용

본 포스팅은 김영한 선생님의 강의를 보고 정리한 글입니다.