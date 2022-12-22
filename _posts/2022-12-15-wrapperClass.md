---
layout: single
title:  "[JAVA] 래퍼클래스(Wrapper Class)"
categories: coding  #카테고리
tag: [java, blog, github, type, Wrapper Class, boxing, unboxing] #태그
toc: true  #오른쪽에 있는 목차
toc_sticky: true #목차 고정
author_profile: true  #왼쪽에 자기 소개란 프로필을 이 페이지에 들어갈때 끄는 기능
sidebar:
    nav: "docs" #navigation.yml에 있는 docs를 뜻한다.
search: true  #이 페이지는 검색에 나옴.
---

이번에는 다음으로 가장 많이 쓰였던 래퍼 클래스에 대해서 정리하는 시간을 가질 것이다.

코드를 짜다보면 기본 타입의 데이터를 객체로 표현해야하는 경우가 많이 있다.

이 때, 기본 자료 타입을 객체로 다루기 위해 사용하는 클래스들을 래퍼 클래스라고 한다.
- ex) 

```java
String val = "value";
MyEntity me = new MyEntity(val);
```

|기본타입(primitive type)|래퍼클래스(wrapper class)|
|---|---|
|byte|Byte|
|char|Character|
|int|Integer|
|float|Float|
|double|Double|
|boolean|Boolean|
|long|Long|
|short|Short|

## 박싱(Boxing)과 언박싱(UnBoxing)

- 박싱은 기본타입 > 래퍼클래스로 바꾸는 과정
- 언박싱은 그 반대로 래퍼클래스 > 기본타입으로 바꾸는 과정
  