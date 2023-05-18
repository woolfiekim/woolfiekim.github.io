---
layout: single
title:  "[스프링으로 migration 툴 만들기] 4. 프로젝트 package 구성"
categories: 
    - spring  #카테고리
tag: [Spring, migration, setting, package] #태그
toc: true  #오른쪽에 있는 목차
toc_sticky: true #목차 고정
author_profile: true  #왼쪽에 자기 소개란 프로필을 이 페이지에 들어갈때 끄는 기능
sidebar:
    nav: "sidebar-category" #navigation.yml에 있는 docs를 뜻한다.
search: true  #이 페이지는 검색에 나옴.
---

## package 구성도

이번에는 package 구성을 다르게 하였다. 

원래는 controller, service, model, entity 이런 식으로 나눠서 구성을 하였다.

그런데, 요즘 추세는 그렇게 나누지 않고 entity 별(?)로 나눈다고 한다.


일단 전체 구성도는 이렇다. 

![](/assets/images/2023/03/28/tool1.png)

이번 게시물에서는 commons, configure, domain을 어떻게 구성했는지 간단하게 적을 것이다.

## commons package

우선, commons package부터 보자.

![](/assets/images/2023/03/28/tool2.png)

보면 3개의 package 가 있다.

1. generator : 다시 무언가를 재 정립할 때 쓰이는 package 이다. 예를 들어, sequence를 내 입맛에 맞춰서 바꾸고 싶을 때 `CustomSequenceGenerator` 이렇게 클래스를 만들어서 쓴다.
2. model : controller에서 클라이언트랑 호출 응답을 할 때, 담을 공통 Request, Response 클래스를 만들어서 넣는다. 
3. type : entity가 아닌 enum, converter나 abstract class 같은 어떤 entity나 어떤 클래스에서 쓸 수 있는 것들을 넣는다.


## configure package

![](/assets/images/2023/03/28/tool3.png)

여기서는 `@Configuration` 을 쓰는 클래스를 넣는 곳이다.


## domain package

![](/assets/images/2023/03/28/tool4.png)

이곳에서 entity별로 package를 만들어서 repository, service, controller 를 만들어서 넣으면 된다.

예시로 asset entity 만 보여주겠다.


## asset entity 안 쪽 package, class 구조

### 1. 전체 구조도

![](/assets/images/2023/03/28/tool5.png)

보면 이렇게 모든게 모여있다. 

asset entity 말고도 다른 entity도 사용에 맞게 package를 구성하면 된다. 어떤 것은 entity package만 있을 수도 있다.

### 2. controller, entity, model

![](/assets/images/2023/03/28/tool6.png)

entity package 에는 entity 뿐만 아니라 이 entity에서만 쓰는 enum 이나 converter 도 넣을 수 있다.

model package 에는 response 같이 응답을 담을 것들을 넣으면 된다.

### 3. repository 

![](/assets/images/2023/03/28/tool7.png)

respository package에는 queryDSL, JpaRepository 를 넣으면 된다.

### 4. service 

![](/assets/images/2023/03/28/tool8.png)

service 는 이렇게 구성하면 된다. 

