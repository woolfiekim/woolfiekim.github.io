---
layout: single
title:  "[tibero] session 끊기"
categories: 
    - db  #카테고리
tag: [db, tibero, session, transaction] #태그
toc: true  #오른쪽에 있는 목차
toc_sticky: true #목차 고정
author_profile: true  #왼쪽에 자기 소개란 프로필을 이 페이지에 들어갈때 끄는 기능
sidebar:
    nav: "sidebar-category" #navigation.yml에 있는 docs를 뜻한다.
search: true  #이 페이지는 검색에 나옴.
---

# Spring 비동기

회사에서 배포한 서버가 time-out이 났다. 

회사 앱에 사용자 추가하면 AD 에 사용자도 추가해주는데 그 역할을 해주는 것이 ADAgent이다. 

회사장비에 서비스를 올릴 때 ****Server, **** Asset Agent, **** ADAgent 이렇게 3개의 앱을 올린다. 그 중에 **** ADAgent 가 저 윈도우 서버에 AD 에 사용자 추가해주는 역할을 하고 있는데 저 **** ADAgent 가 톰캣처럼 웹서버를 구동하고 있다. 

그래서 회사앱 에서 사용자 추가하면, WAS 가 저 **** ADAgent 의 웹서버를 호출해서 AD 에 사용자를 추가해주고 있다. 

그런데 저 ADAgent 의 웹서버를 WAS 가 호출했는데 응답이 없었던 것이다. 

- AD(Active Directory) 라는 서비스이고 Windows Server 에서 제공해준다.
- SSO(Single Sign On) : 하나의 아이디로 어디서든지 로그인 가능하게 해주는 서비스

그래서 2가지를 수행해줘서 해결했다.

일단 DB에서 커넥션풀을 다 쓰고 있었다. 왜냐하면, restTemplate에서 timeout을 안걸어서 트랙잭션이 끝나질 않았다. 그래서 그 테이블에 락이 걸린거다. 

oracle 은 트랜잭션이 시행된 지 일정시간이 지나면 알아서 커넥션을 끊어주는데 tibero는 그런 기능따위 없다.

회사에서 쓰는 스프링은 톰캣을 쓰는데 톰캣은 쓰레드 기반이다. 요청하나당 쓰레드 하나를 사용 > 트랜잭션을 하나 쓴다. commit이 안되는 트랜잭션이 계속 쌓인다.

그래서 

1. 무한으로 계속 돌고있던 트랜잭션의 세션을 강제로 지워야한다.

2. [ADAgent를 호출하고 응답을 받는 부분을 동기가 아닌 비동기로 바꾸면 되었다.](https://woolfiekim.github.io/spring/springevent/)


## 1. 트랜잭션 세션 삭세

세션 삭제하는 방법은 크게 2가지가 있다. 

GUI툴을 사용하거나 명령어를 쓰는 방법이다. 둘 다 권한이 있는 관리자계정으로 들어가야 가능하다.

### (1) GUI툴을 이용해서 세션삭제

![tiberoGUI](/assets/images/2023/02/24/tiberoGUI.png)

Tibero studio라고 tibero를 관리하는 GUI툴이다.

![sessionManager](/assets/images/2023/02/24/sessionManager.png)

Session Manager에 들어간다.

![Untitled](/assets/images/2023/02/24/list.png)

여기서 빨간 부분으로 표시한 Running 부분을 Session Close하면 된다.

![Untitled](/assets/images/2023/02/24/sessionClose.png)

지금 사진에서는 Running이 많지 않다. 세션들을 다 쓰고 있는 상태라면 Running이 아주 많을 것이다. 그러면 해당 세션들을 Close하면 된다.

### (2) 명령어로 세션 조회 후 삭제

Tibero에서 현재 사용 중인 세션을 조회하기 위해서는 다음과 같은 SQL 문을 사용할 수 있다.

```sql
SELECT * FROM v$session WHERE status = 'ACTIVE';
```

이 쿼리는 현재 활성화된 세션들만 보여준다. 결과에는 각 세션의 ID, 사용자명, 호스트명, 프로세스 ID, 현재 실행 중인 SQL 문 등이 포함된다.

Tibero에서 세션을 닫는 SQL 문은 다음과 같다.

```sql
ALTER SYSTEM KILL SESSION '<sid>,<serial#>';
```

여기서 `<sid>`는 세션 ID이고, `<serial#>`는 직렬 번호(serial number)이다. 이 정보는 `v$session`
 뷰에서 조회할 수 있다.

예를 들어, 세션 ID가 123, 직렬 번호가 456인 세션을 종료하려면 다음과 같이 SQL 문을 실행한다.

```sql
ALTER SYSTEM KILL SESSION '123,456';
```

세션을 종료하면 해당 세션에서 실행 중인 모든 트랜잭션이 롤백된다. 따라서 이 기능을 사용할 때는 주의해야 한다.