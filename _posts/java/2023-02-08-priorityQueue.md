---
layout: single
title:  "[JAVA] List<Map<String, Object>> 정렬"
categories: 
  - java  #카테고리
tag: [java, blog, github, List, Map, sort, 정렬] #태그
toc: true  #오른쪽에 있는 목차
toc_sticky: true #목차 고정
author_profile: true  #왼쪽에 자기 소개란 프로필을 이 페이지에 들어갈때 끄는 기능
sidebar:
    nav: "sidebar-category" #navigation.yml에 있는 docs를 뜻한다.
search: true  #이 페이지는 검색에 나옴.
---

이번 포스팅은 `List<Map<String, Object>>` 를 정렬 하는 방법이다. 

## 1. Comparator를 사용

```java
//정렬할 리스트
List<Map<String, Object>> list = = new ArrayList<>(); 

list..sort(
    Comparator.comparing(
        (Map<String, Object> map) -> (String)map.get("CREATEDATE")
    ).reversed() //reversed는 내림차순이다. reversed를 지우면 오름차순이다. 
); 

//여러 컬럼을 조건으로 정렬
list.sort(
    Comparator.comparing(
        (Map<String, Object> map) -> (String)map.get("CREATEDATE")
    ).thenComparing(
        (Map<String, Object> map) -> (String)map.get("ID")
    ).reversed()
);
```

## 2. PriorityQueue를 사용

```java
//정렬할 리스트
List<Map<String, Object>> list = = new ArrayList<>();

PriorityQueue<Map<String, Object>> queue =
    new PriorityQueue<>((o1, o2) ->
        StringUtils.compare(String.valueOf(o1.get("CREATEDATE")), String.valueOf(o2.get("CREATEDATE")))
            * -1); //여기서 -1은 내림차순, -1을 없애면 오름차순

queue.addAll(list);

//생성일이 가장 최신인 데이터
Map<String, Object> result = queue.poll(); //poll은 데이터를 지우고 꺼내고 queue.peek() 로 바꾸면 데이터를 지우지 않고 꺼낸다.
```