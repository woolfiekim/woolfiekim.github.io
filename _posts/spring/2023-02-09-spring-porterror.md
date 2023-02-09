---
layout: single
title:  "[Spring 에러]'Web server failed to start. Port 8080 was already in use.' 실행시 포트에러"
categories: 
    - spring  #카테고리
tag: [Spring] #태그
toc: true  #오른쪽에 있는 목차
toc_sticky: true #목차 고정
author_profile: true  #왼쪽에 자기 소개란 프로필을 이 페이지에 들어갈때 끄는 기능
sidebar:
    nav: "sidebar-category" #navigation.yml에 있는 docs를 뜻한다.
search: true  #이 페이지는 검색에 나옴.
---

```plaintext
Web server failed to start. Port 8080 was already in use.
```

이런 에러가 떴었다.

이미 8080 포트를 사용하고 있기 때문에 그 포트로 실행할 수가 없는 것이다.

해결방법은 두 가지가 있다.

## 1. application.yml (application.properties) 파일에 코드 한 줄을 추가하거나 수정한다.

```java
server.port=8090
```
이 한 줄을 추가하면 된다.
여기서 포트번호는 임의로 아무거나 하면 된다.

## 2. 터미널에서 사용하는 포트 없애기

```shell
lsof -i tcp:8080
```
이 커멘드는 실행중인 8080포트를 확인 하는 것이다. 
있으면 리스트가 뜰 것이다.
리스트에서 PID 넘버(예시 : PID넘버가 25156)만 보면 된다.

```shell
kill -9 25156
```

이제 다시 프로그램을 실행해보자. 될 것이다. 