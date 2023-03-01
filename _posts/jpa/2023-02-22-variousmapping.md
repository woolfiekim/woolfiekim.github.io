---
layout: single
title:  "[JPA] 연관관계 매핑"
categories: 
    - jpa  #카테고리
tag: [jpa, entity] #태그
toc: true  #오른쪽에 있는 목차
toc_sticky: true #목차 고정
author_profile: true  #왼쪽에 자기 소개란 프로필을 이 페이지에 들어갈때 끄는 기능
sidebar:
    nav: "sidebar-category" #navigation.yml에 있는 docs를 뜻한다.
search: true  #이 페이지는 검색에 나옴.
---

## 1. 다대일 (N:1)

- 가장 많이 사용하는 연관관계이다.
- '다'가 연관관계 주인

## 2. 일대다 (1:N)
- '일'이 연관관계 주인
- 테이블 상에서는 '다' 쪽에 외래키가 있다.
- 이렇게 객체와 테이블의 차이가 있을 경우엔 꼭 `@JoinColumn` 을 꼭 사용해야 한다.
- 아니면, 중간에 테이블(조인 테이블)을 하나 추가해야한다.
- **일대다는 되도록 쓰지 말자**

## 3. 일대일 (1:1)
- 외래키는 아무거나 선택 (DB 유니크 제약조건 추가)

## 4. 다대다 (M:N)
- 실무에서는 안 쓰는 것을 추천


> JPA(Java Persistence API)에서의 프록시(Proxy)는 영속성(Persistence) 컨텍스트(Persistence Context)에서 엔티티(Entity)를 지연 로딩(Lazy Loading)하는 기술이다. JPA에서의 프록시는 지연 로딩을 위해 사용되는 객체로, 필요한 시점에 데이터를 가져오는 것을 통해 성능을 개선할 수 있다.


> 본 포스팅은 김영한 선생님의 강의를 보고 정리한 글입니다.