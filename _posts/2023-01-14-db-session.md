---
layout: single
title:  "[Tibero] DB 세션 초과 이슈"
categories: DB  #카테고리
tag: [db, tibero, oracle] #태그
last_modified_at : 2023-01-17
toc: true  #오른쪽에 있는 목차
toc_sticky: true #목차 고정
author_profile: true  #왼쪽에 자기 소개란 프로필을 이 페이지에 들어갈때 끄는 기능
sidebar:
    nav: "docs" #navigation.yml에 있는 docs를 뜻한다.
search: true  #이 페이지는 검색에 나옴.
---
test
회사에서 일을 하는 데 갑자기 DB 연결이 끊겼다. 

당황하지 않고 나는 선임을 불렀다.

이런 적이 몇 번 있었는데 그때마다 선임을 불렀었다.

이제는 나도 한 회사의 직원으로 이런 자잘한 오류도 내가 해결하고 싶어져서 이번 기회에 이 이슈를 해결하는 방법을 적어두겠다.

## spring tibero DB 연결 끊김 에러 문구

일단 에러 문구는 이러하다.

```shell
~~~ Unable to open a session.
```

## 끊긴 이유

회사에서 사용하는 DB는 tibero이다.

tibero(oracle)는 갖고 있는 세션이 기본 100개이다. 

이 세션이 초과되면 DB 연결이 불가하다. 그래서 끊기게 된 것이다.

(다만, oracle은 tibero와 비슷하지만, 이 에러가 뜨지 않는 이유는 쓰지 않는 세션은 제거해주기 때문에 이 이슈가 나오지 않는다.)

## 해결 방안

1. 세션을 조회해서 안 쓰는 세션 끊기
2. 인스턴스 내렸다 올리기
3. 접속할 수 있는 최대 세션 크기 늘리기

이 중에 2번이 가장 간단한 방법이다.

(회사) Termius 접속

```Bash
docker ps ## 컨테이너 확인 
docker stop tibero ## tibero container 내리기

## tibero instance 파일 제거
cd /home/tibero/instance/tibero 
ls -al ## 모든 파일 조회
rm -rf .proc.list lsnr.out.xx tbsvr.out.xx ## 인스턴스 파일 제거

## 컨테이너 다시 시작
docker start tibero
```

이렇게 하니 다시 DB접속이 가능해졌다.

여기서, 세션과 인스턴스에 대해서 정리를 한 번 해야할 것 같다.


### 세션이란?

- 오라클 데이터베이스 세션은 DB 접속을 시작으로, 여러 작업을 수행한 후 접속 종료까지의 전체 기간을 의미한다.
- 사용자는 스프링부트, H2 console 과 같은 WAS, DB 접근 툴을 이용해 DB에 접근 할 수 있다. 사용자는 DB 서버에 연결요청을 하고 WAS는 이를 대신해서 DB와 커넥션을 연결 해준다. WAS의 커넥션 - DB 커넥션이 연결되면 DB는 세션을 하나 만든다. 이 세션이 생성이 되면 DB로 오는 모든 요청을 DB세션이 처리해준다. 
- 이 세션을 통해 트랜잭션을 시작하고, 종료할 수 잇다. 

### 인스턴스란? 

- 데이터를 입출력할 수 있도록 해주는 하나의 소프트웨어이다.
- 사용자가 DB에 접근해 트랜잭션을 처리 할 수 있는 프로세스와 메모리 영역이다.
- 실제 사용자가 DB에 접근하면 1개의 인스턴스가 할당된다. 프로그램의 쓰레드와 같은 개념이다.
- 인스턴스는 사용자와 물리 DB간의 다리역할을 해준다. 