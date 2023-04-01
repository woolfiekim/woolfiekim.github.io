---
layout: single
title:  "[spring http] Network 기본 지식2"
categories: 
    - spring  #카테고리
tag: [Spring, http] #태그
toc: true  #오른쪽에 있는 목차
toc_sticky: true #목차 고정
author_profile: true  #왼쪽에 자기 소개란 프로필을 이 페이지에 들어갈때 끄는 기능
sidebar:
    nav: "sidebar-category" #navigation.yml에 있는 docs를 뜻한다.
search: true  #이 페이지는 검색에 나옴.
---

API URI 설계를 할 때, 가장 중요한 것은 ***리소스 식별*** 이다.

## 리소스란?

리소스란, 등록하고 수정하고 조회하는 ***어떠한 것***이 리소스이다.

ex. 회원 조회, 회원 등록, 회원 수정, 회원 삭제 >> 리소스 : 회원

리소스인 회원을 URI에 매핑한다.

```plaintext
회원 조회 : /members/{id}
회원 등록 : /members/{id}
회원 수정 : /members/{id}
회원 삭제 : /members/{id}
```

* 참고사항 : 계층 구조상 상위를 컬렉션으로 보고 복수단어를 사용하기 권장한다.
  * member -> members

## 리소스와 행위 분리

이제 조회, 등록, 수정, 삭제를 할 때 위에 있는 예시의 api가 같다.

이를 어떻게 구분해야할까?

행위는 조회, 등록, 수정, 삭제 와 같은 것을 말한다. 

행위란 곧 `메서드`이다.

메서드는 get, post, put, delete, patch가 있다.

## 클라이언트에서 서버로 데이터 전송하는 방법

1. 쿼리 파라미터 (get - 주로 검색어)
2. 바디 (post, put, patch)


## 상태코드
- 1XX : 요청이 수신되어 처리중 > 거의 사용하지 않는다.
- 2XX : 요청 정상 처리
- 3XX : 요청을 완료하려면 추가 행동이 필요
- 4XX : 클라이언트 오류, 잘못된 문법 등으로 서버가 요청을 수행할 수 없음
- 5XX : 서버 오류, 서버가 정상 요청을 처리하지 못함

