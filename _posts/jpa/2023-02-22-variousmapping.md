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


> 본 포스팅은 김영한 선생님의 강의를 보고 정리한 글입니다.