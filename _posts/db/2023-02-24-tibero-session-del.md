---
layout: single
title:  "[] Mac 에 H2 설치하기"
categories: 
    - db  #카테고리
tag: [db, h2, mac] #태그
toc: true  #오른쪽에 있는 목차
toc_sticky: true #목차 고정
author_profile: true  #왼쪽에 자기 소개란 프로필을 이 페이지에 들어갈때 끄는 기능
sidebar:
    nav: "sidebar-category" #navigation.yml에 있는 docs를 뜻한다.
search: true  #이 페이지는 검색에 나옴.
---

# Spring 비동기

회사에서 배포한 서버가 time-out이 났다. 

KeyFlow 에 사용자 추가하면 AD 에 사용자도 추가해주는데 그 역할을 해주는 것이 ADAgent이다. 

MAM장비에 서비스를 올릴 때 KeyFlow Server, KeyFlow Asset Agent, KeyFlow ADAgent 이렇게 3개의 앱을 올린다. 그 중에 KeyFlow ADAgent 가 저 윈도우 서버에 AD 에 사용자 추가해주는 역할을 하고 있는데 저 KeyFlow ADAgent 가 톰캣처럼 웹서버를 구동하고 있다. 

그래서 KeyFlow 에서 사용자 추가하면, WAS 가 저 KeyFlow ADAgent 의 웹서버를 호출해서 AD 에 사용자를 추가해주고 있다. 

그런데 저 ADAgent 의 웹서버를 WAS 가 호출했는데 응답이 없었던 것이다. 

- AD(Active Directory) 라는 서비스이고 Windows Server 에서 제공해준다.
- SSO(Single Sign On) : 하나의 아이디로 어디서든지 로그인 가능하게 해주는 서비스

그래서 2가지를 수행해줘서 해결했다.

일단 DB에서 커넥션풀을 다 쓰고 있었다. 왜냐하면, restTemplate에서 timeout을 안걸어서 트랙잭션이 끝나질 않았다. 그래서 그 테이블에 락이 걸린거다. 

oracle 은 트랜잭션이 시행된 지 일정시간이 지나면 알아서 커넥션을 끊어주는데 tibero는 그런 기능따위 없다.

회사에서 쓰는 스프링은 톰캣을 쓰는데 톰캣은 쓰레드 기반이다. 요청하나당 쓰레드 하나를 사용 > 트랜잭션을 하나 쓴다. commit이 안되는 트랜잭션이 계속 쌓인다.

그래서 

1. 무한으로 계속 돌고있던 트랜잭션의 세션을 강제로 지워야한다. 

2. ADAgent를 호출하고 응답을 받는 부분을 동기가 아닌 비동기로 바꾸면 되었다.