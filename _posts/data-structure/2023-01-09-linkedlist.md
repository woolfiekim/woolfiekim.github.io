---
layout: single
title:  "[DATA STRUCTURE] 자료구조 - 링크드 리스트(Linked List)"
categories: 
  - data-structure  #카테고리
tag: [data structure, algorithm, 알고리즘, 자료구조, 링크드리스트, linked list, 파이썬, python] #태그
toc: true  #오른쪽에 있는 목차
toc_sticky: true #목차 고정
author_profile: true  #왼쪽에 자기 소개란 프로필을 이 페이지에 들어갈때 끄는 기능
sidebar:
    nav: "sidebar-category" #navigation.yml에 있는 docs를 뜻한다.
search: true  #이 페이지는 검색에 나옴.
---

## 1. 링크드 리스트 (Linked List) 구조
* 연결 리스트라고도 함
* 배열은 순차적으로 연결된 공간에 데이터를 나열하는 데이터 구조
* 링크드 리스트는 떨어진 곳에 존재하는 데이터를 화살표로 연결해서 관리하는 데이터 구조
* 본래 C언어에서는 주요한 데이터 구조이지만, 파이썬은 리스트 타입이 링크드 리스트의 기능을 모두 지원
* 링크드 리스트 기본 구조와 용어
  - 노드(Node): 데이터 저장 단위 (데이터값, 포인터) 로 구성
  - 포인터(pointer): 각 노드 안에서, 다음이나 이전의 노드와의 연결 정보를 가지고 있는 공간
  - 넣어질 데이터와, 다음에 저장할 데이터의 주소값값도 같이 갖고있다.

## 2. **더블 링크드 리스트(Doubly linked list)**

1. 더블 링크드 리스트 기본 구조
    1. 이중 연결 리스트라고도 함
    2. 양방향으로 연결되어 있어서 노드 탐색이 양쪽으로 모두 가능

![doubly linked list](/assets/images/2023-02-10/doublyLinkedList.png)

- 출처 : [위키피디아](https://www.fun-coding.org/00_Images/doublelinkedlist.png)