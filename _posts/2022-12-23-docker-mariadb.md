---
layout: single
title:  "[Spring] project setting"
categories: project  #카테고리
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

도커 : 데이터 또는 프로그램을 격리시키는 기능을 제공하는 소프트웨어이다

이 기능은 주로 서버에 사용된다.

예를 들어 서버에서는 아파치, MySQL 등 여러 프로그램(소프트웨어)이 함께 동작한다. 

도커는 위와 같이 다양한 프로그램과 데이터를 각각 독립된 환경에 격리하는 기능을 제공한다.