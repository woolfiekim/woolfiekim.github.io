---
layout: single
title:  "[DATA STRUCTURE] 자료구조 - 해쉬 테이블"
categories: 
  - data-structure  #카테고리
tag: [data structure, algorithm, 알고리즘, 자료구조, 해쉬테이블] #태그
toc: true  #오른쪽에 있는 목차
toc_sticky: true #목차 고정
author_profile: true  #왼쪽에 자기 소개란 프로필을 이 페이지에 들어갈때 끄는 기능
sidebar:
    nav: "sidebar-category" #navigation.yml에 있는 docs를 뜻한다.
search: true  #이 페이지는 검색에 나옴.
---
## 1. 해쉬 테이블
: Key, Value로 매핑하는 데이터 구조

## 2. 장단점
- 장점 : 검색 속도가 빠르다.
- 단점 : value를 저장할 때 key값이 같은 것이 이미 있을 때를 따로 처리해야할 로직이 필요하다.

## 3. HashMap

## 4. 시간복잡도
- 일반적인 경우엔 O(1)
- 최악(충돌발생) : O(n)

검색 시 해쉬 테이블 사용
- 배열 > 데이터 저장 및 검색 : O(n)
- 해쉬 테이블 > 검색 : O(1)