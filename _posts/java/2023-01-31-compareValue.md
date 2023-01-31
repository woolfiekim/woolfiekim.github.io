---
layout: single
title:  "[JAVA] 값(객체) 비교"
categories: 
  - java  #카테고리
tag: [java, blog, github, type, primitive type, reference type] #태그
toc: true  #오른쪽에 있는 목차
toc_sticky: true #목차 고정
author_profile: true  #왼쪽에 자기 소개란 프로필을 이 페이지에 들어갈때 끄는 기능
sidebar:
    nav: "sidebar-category" #navigation.yml에 있는 docs를 뜻한다.
search: true  #이 페이지는 검색에 나옴.
---

객체(값)을 비교하는 방법은 크게 두 가지가 있다.
1. ==
2. equals()

어떻게 다른 지 설명하겠다. 

## == : 주소값 비교

이는 실제 값이 아니라 두 대상의 주소값을 비교한다.

전에 올린 [자료형 게시글](https://woolfiekim.github.io/java/type/)에서 primitive type에서 보통 쓰인다.

```java
int a = 1;
int b = 1;

return a == b; //true

Str
```

## equals() : 값 비교

이는 실제 값을 비교한다. 

reference type에서도 사용 가능하다.

```java
Integer a = Integer.valueOf(1);
Integer b = Integer.valueOf(1);

return a.equals(b); //true
```

> 결론 : 값을 비교하고 싶을 때는 맘편하게 equals()를 쓰자.