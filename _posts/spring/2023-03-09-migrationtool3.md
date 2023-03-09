---
layout: single
title:  "[스프링으로 migration 툴 만들기] 3. 프로젝트 github에 첫 commit시 생기는 일"
categories: 
    - spring  #카테고리
tag: [Spring, migration, setting, error, github, commit] #태그
toc: true  #오른쪽에 있는 목차
toc_sticky: true #목차 고정
author_profile: true  #왼쪽에 자기 소개란 프로필을 이 페이지에 들어갈때 끄는 기능
sidebar:
    nav: "sidebar-category" #navigation.yml에 있는 docs를 뜻한다.
search: true  #이 페이지는 검색에 나옴.
---

## 1. github 레파지토리와 인텔리제이 스프링 프로젝트 연결

갓 만든 프로젝트를 github에 올릴 때

먼저 프로젝트를 따로 만들고, github에 레파지토리를 생성할 때 그 프로젝트와 레파지토리를 이어주는 방법은

그 프로젝트를 열은 인텔리제이의 terminal(프로젝트경로로 처음부터 잡아줘서 사용)에 아래 명령어를 입력한다.

```shell
git init
git remote add origin ~~~~
```

여기서 물결표시에는

![](/assets/images/2023-03/09/github.png)

여기서 복사한 url을 넣어주면 된다!

## 2. 파일들이 `Unversioned Files` 란에 빨갛게 파일들이 떠있다.

갓 만든 프로젝트를 이제 깃헙에 commit하려고 한다. 

![](/assets/images/2023-03/09/commit2.png)

커밋창에 가보니 `Unversioned Files` 란에 빨갛게 파일들이 떠있다.

원래 커밋할 파일들이 저 란에 있는 게 아닐텐데...

일단 인텔리제이의 terminal 창에 깃 상태를 봐보자.

```shell
git status
```

위 사진과 같이 나올 것이다. 그리고 괄호치고 "git add <file>..." 이라고 나오는데 

이를 아주 쉬운 단축키로 이걸 처리할 수 있다.

MacOs 기준으로 설명하겠다.

commit창이 아니라 project 창에 가서 

` cmd + option + a ` 이렇게 단축키를 누르면 

![](/assets/images/2023-03/09/commit3.png)

짜쟈쟌~~ 초록색으로 `Changes` 란에 파일들이 초록색으로 변해있다. 

이제 이대로 커밋하면 된당!

![](/assets/images/2023-03/09/good.jpg)