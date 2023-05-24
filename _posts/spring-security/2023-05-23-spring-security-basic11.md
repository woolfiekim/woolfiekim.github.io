---
layout: single
title:  "[spring security basic] 11. Authentication"
categories: 
    - spring-security  #카테고리
tag: [Spring security] #태그
toc: true  #오른쪽에 있는 목차
toc_sticky: true #목차 고정
author_profile: true  #왼쪽에 자기 소개란 프로필을 이 페이지에 들어갈때 끄는 기능
sidebar:
    nav: "sidebar-category" #navigation.yml에 있는 docs를 뜻한다.
search: true  #이 페이지는 검색에 나옴.
---

## Authentication

### 1. Authentication 설명

Authentication 은 인증이나 인증주체를 뜻한다.

한 마디로, "당신이 누구인지 증명" 하는 것이다.

사용자의 인증 정보를 저장하는 토큰 개념이다.

이는 전역적으로 참조가 가능하다.

### 2. 구조

1. principal : ID or User 객체 (type : Object)
2. credentials : PWD
3. authorities : 권한 목록
4. details : 인증 부가 정보
5. Authenticated : 인증 여부 (인증 o : true / 인증 x : false)


### 3. 인증 성공 시 실제로 보이는 Authentication

![](/assets/images/2023/05/24/filter4.png)