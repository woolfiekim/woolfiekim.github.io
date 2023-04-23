---
layout: single
title:  "[JPA] JPQL"
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

## JPQL 이란?

- JPA는 SQL을 추상화한 객체 지향 쿼리 언어(JPQL)를 제공한다.
- 검색을 할 때 엔티티 객체를 대상으로 검색한다.


### TypedQuery, Query

- TypedQuery : 반환 타입이 명확할 때 사용
- Query : 반환 타입이 명확하지 않을 때 사용


## QueryDSL

- JPQL 빌더 역할
- 컴파일 시점에서 문법 오류를 찾을 수 있다.
- 동적쿼리를 사용하기 좋다. 
- **실무 사용 권장**


> 본 포스팅은 김영한 선생님의 강의를 보고 정리한 글입니다.   