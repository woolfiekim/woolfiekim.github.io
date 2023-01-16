---
layout: single
title:  "[Spring] project setting"
categories: Project  #카테고리
tag: [docker, mariaDB, intelliJ] #태그
toc: true  #오른쪽에 있는 목차
toc_sticky: true #목차 고정
author_profile: true  #왼쪽에 자기 소개란 프로필을 이 페이지에 들어갈때 끄는 기능
sidebar:
    nav: "docs" #navigation.yml에 있는 docs를 뜻한다.
search: true  #이 페이지는 검색에 나옴.
---

이번에는 DB를 직접 설치하고 사용하는 것이 아니라 Docker를 이용해 DB를 사용할 것이다.

우선 도커를 사용하는 이유부터 짚고 넘어가야겠다.

## 도커 사용 이유

### 도커란?

도커 : 데이터 또는 프로그램을 격리시키는 기능을 제공하는 소프트웨어이다

이 기능은 주로 서버에 사용된다.

예를 들어 서버에서는 아파치, MySQL 등 여러 프로그램(소프트웨어)이 함께 동작한다. 

도커는 위와 같이 다양한 프로그램과 데이터를 각각 독립된 환경에 격리하는 기능을 제공한다.

### 데이터나 프로그램을 독립된 환경에 격리해야 하는 이유

대부분의 프로그램은 프로그램 단독으로 동작하지 않고, 어떤 실행 환경이나 라이브러리, 다른 프로그램을 이용해 동작한다.

이 때문에 프로그램 하나를 업데이트하면 다른 프로그램에도 영향을 미치게 된다.

설계할 때는 문제가 없었던 프로그램끼리도 실제로 설치해보면 오류를 일으키는 경우도 있다. 이러한 문제의 원인은 대부분 프로그램 간 공유에 있다.

### 프로그램 격리

일반적인 환경에서는 한 대의 서버 혹은 컴퓨터에서 한 벌만 설치할 수 있는 소프트웨어가 대부분이다.

도커를 사용하게 된다면 같은 소프트웨어를 여러 벌 설치가 가능하다.(각 컨테이너에 하나씩 넣으면 된다.)

```plaintext
즉, 도커를 사용하면
1. 프로그램을 완전히 격리시켜 독립적인 환경에서 안전하게 운영할 수 있다.
( 무엇을 업데이트하더라도 서로 영향을 미치지 않는다.)
2. 컨테이너 기술을 활용하면 여러 개의 웹 서버를 올릴 수 있다.
(물리 서버의 수를 줄일 수 있다.)
3. 운영체제가 달라도 컨테이너를 옮길 수 있다.
(물리적 환경의 차이, 서버 구성의 차이를 무시)
```

이러한 이유 때문에 DB를 직접 설치해서 사용하지 않고 도커를 이용해 사용하기로 결정을 내렸다.

도커를 사용하기 위해 명령어를 사용하지 않고 docker-compose.yml 파일을 사용하였다.
```plaintext
[명령어]

docker run \
    --name mariadb \
    -d \
    -p 3306:3306 \
    --restart=always \
    -e MYSQL_ROOT_PASSWORD=root \
    mariadb
```

```plaintext
[docker-compose.yml]

version: "3.8"

services:
  mariadb:
    container_name: mariadb
    image: mariadb
    ports:
        - "13306:3306"
    environment:
      MYSQL_ROOT_PASSWORD: 12345
```