---
layout: single
title:  "[스프링으로 migration 툴 만들기] 1. spring setting"
categories: 
    - spring  #카테고리
tag: [Spring, migration, setting] #태그
toc: true  #오른쪽에 있는 목차
toc_sticky: true #목차 고정
author_profile: true  #왼쪽에 자기 소개란 프로필을 이 페이지에 들어갈때 끄는 기능
sidebar:
    nav: "sidebar-category" #navigation.yml에 있는 docs를 뜻한다.
search: true  #이 페이지는 검색에 나옴.
---

오늘은 회사에서 새 업무를 받았다.

마이그레이션 툴을 만들라는 것이다.

회사 일이기 때문에 자세히는 설명은 못하지만, 3줄 요약으로 간단히 설명하겠다.

## 1. 프로젝트 설명

1. 특정 상황이면, 여러 테이블을 조합해서 경로를 만들어 그 경로에 맞게 실제로 파일이 있는지 확인
2. 없으면 작업을 만들어주는 테이블에 데이터를 만들어 넣는다.
3. 엄청 많은 양의 데이터를 처리해야한다.

처음 만들어보는 툴이고 일주일이라는 시간 안에 제작과 테스트를 다 해야하는 위기에 봉착했다. 

이번 주는 개인 공부를 못하겠지만, 이 툴 하나를 만듦으로써 많은 것을 알게 되는 귀중한 시간이 될 거라고 내 직감이 말하고 있다.

일단, 이 프로젝트를 만들기 위해서 생각해야할 것이 있다.

## 2. 프로젝트 빌드업

1. 많은 양의 데이터를 한 번에 처리를 해야한다.
   1. db에서 데이터를 export(csv파일)한다.
   2. csv를 읽어서 다른 테이블에 넣을 데이터들을 만들어 추가한다.
2. 어떠한 os여도 사용할 수 있게 한다.
3. 간단한 클라이언트를 만들어야한다.(만약, 시간이 남는다면 리액트로 해보고싶다.)

후... 1, 2번부터 처음 해보는 것들이라 불안하고 무섭다... 

## 3. spring project setting

이제, spring setting을 해보자.

setting도 생각없이 했던 것을 조금 더 생각해보기로 했다.

intellij에서 새 프로젝트를 만들었다. 그리고 dependencies 부분이다.

![](/assets/images/2023-03/09/setting1.png)

여기서 `Web` 카테고리에 `Spring Web` 과 `Spring Reactive Web` 이 있다. 

둘의 차이점을 이번에 알게 되었다.

1. Spring Web
   1. 톰캣같은 웹서버를 띄어주는 것이다. 
   2. 쓰레드방식을 사용한다.(Jpa, Security)
2. Spring Reactive Web
   1. 이벤트방식(jpa를 못쓴다.)
   2. reactive용 jdbc를 써야한다. 
   3. 로컬 쓰레드를 쓰지 못하지만, 빠르다.(사용자가 많은 서비스에서 사용하기 용이)
   4. 요청이 들어오면 큐에 쌓는다.

![](/assets/images/2023-03/09/aha.jpeg)

아하! 이런 차이가 있었구나! 나는 계속 쓸 툴이 아니라 쓰고 버리는 일회성 툴이고 그 툴은 나만 쓰기 때문에 1번을 사용해야하는 것을 명백하게 알 수 있다!

+ 추가로 `Web` 카테고리에 `Spring Web Services` 부분은 soap이나 WDSL같은 옛날 통신용이라고 한다. 현재는 거의 쓰지 않는다.


![](/assets/images/2023-03/09/setting2.png)

세팅은 위와 같이 했다.

세팅 후 생긴 에러들은 다음 게시글에 쓰겠다.