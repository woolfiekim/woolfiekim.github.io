---
layout: single
title:  "[Spring] Spring의 핵심 원리 기초1 "
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

## 1. 스프링의 핵심가치 : 객체 지향 프로그램

## 2. 좋은 객체 지향 설계의 5가지 원칙 (SOLID)

1. SRP(Single responsibility principle) : 단일 책임 원칙
    1. 한 클래스는 하나의 책임(클 수도, 작을 수도 있다)만 가져야한다.
    2. **중요한 기준**은 변경이다. 변경이 있을 때 파급 효과가 적으면 단일 책임 원칙을 잘 따른 것.
        
        ex) UI 변경, 객체의 생성과 사용을 분리
        
2. OCP(Open/closed principle) : 개방-폐쇄 원칙
    1. 소프트웨어 요소는 **확장에는 열려**있고 **변경에는 닫혀**있다.
    2. **다형성**을 활용해보자.
    3. 인터페이스를 구현한 새로운 클래스를 하나 만들어서 새로운 기능을 구현
    **-** 객체를 생성하고, 연관관계를 맺어주는 별도의 조립, 설정자가 필요하다.
3. LSP(Liskov substitution principle) : 리스코프 치환 원칙
    1. 다형성에서 하위 클래스는 **인터페이스 규약을 다 지켜야 한다**는 것
    2. 단순히 컴파일에 성공하는 것을 넘어서는 이야기
4. ISP(Interface segregation principle) : 인터페이스 분리 원칙
    1. 특정 클라이언트를 위한 인터페이스 여러 개가 범용 인터페이스 하나보다 낫다
    2. 인터페이스가 명확해지고, 대체 가능성이 높아진다.
5. DIP(Dependency inversion principle) : 의존관계 역전 원칙
    1. 구현 클래스에 의존하지 말고, 인터페이스에 의존하라
    2. **역할에 의존하게 해야한다.**

## 3. 스프링이 만들어진 이유

**DI(Dependency Injection): 의존관계, 의존성 주입**

**클라이언트 코드의 변경 없이 기능 확장**