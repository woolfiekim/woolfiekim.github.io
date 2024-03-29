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

## 10. 빈 스코프

### 1. 빈 스코프란?

: 빈이 존재할 수 있는 범위

스코프 종류

- 싱글톤 : 기본 스코프로 스프링 컨테이너의 시작과 종료까지 유지되는 가장 넓은 범위의 스코프이다.
- 프로토타입 : 프로토타입 빈의 생성과 의존관계 주입까지만 관여하는 매우 짧은 범위의 스코프이다.
- 웹 관련 스코프
    - request : 웹 요청이 들어오고 나갈 때까지 유지되는 스코프
    - sessiong : 웹 세션이 생성되고 종료 될 때까지 유지되는 스코프
    - application : 웹의 서블릿 컨텍스트와 같은 범위로 유지되는 스코프

### 2. 프로토타입 스코프

1. 프로토타입 스코프의 빈을 스프링 컨테이너에 요청한다.

2. 스프링 컨테이너는 이 시점에 프로토타입 빈을 생성하고, 필요한 의존관계를 주입한다.

3. 스프링 컨테이너는 생성한 프로토타입 빈을 클라이언트에 반환한다.

4. 이후에 스프링 컨테이너에 같은 요청이 오면 항상 새로운 프로토타입 빈을 생성해서 반환한다.

**그래서 `@PreDestroy` 같은 종료 메서드가 호출되지 않는다.**


### 3. 웹 스코프

- 웹 스코프의 특징
    - 웹 스코프는 웹 환경에서만 동작
    - 프로토타입과 다르게 스프링이 해당 스코프의 종료시점까지 관리한다. 따라서 종료 메서드가 호출된다.
- 웹 스코프 종류
    - **request** : HTTP 요청 하나가 들어오고 나갈 때 까지 유지되는 스코프, 각각의 HTTP 요청마다 별도의 빈 인스턴스가 생성되고, 관리된다.
    - **session :** HTTP Session과 동일한 생명주기를 가지는 스코프
    - **application:** 서블릿 컨텍스트( ServletContext )와 동일한 생명주기를 가지는 스코프
    - **websocket:** 웹 소켓과 동일한 생명주기를 가지는 스코프