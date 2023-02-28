---
layout: single
title:  "[Spring Security] google OAuth2 설정"
categories: 
    - spring  #카테고리
tag: [Spring, security, oauth2, google] #태그
toc: true  #오른쪽에 있는 목차
toc_sticky: true #목차 고정
author_profile: true  #왼쪽에 자기 소개란 프로필을 이 페이지에 들어갈때 끄는 기능
sidebar:
    nav: "sidebar-category" #navigation.yml에 있는 docs를 뜻한다.
search: true  #이 페이지는 검색에 나옴.
---

개인프로젝트를 하면서 처음으로 구글 로그인을 해보기로 했다. 

문제는 내가 아직 spring security에 대해서 제대로 모른다는 것이다...

spring security 인강으로 공부하려면 아직 모르는게 너무 많은 초짜배기라서 들어야할 인강이 산더미같이 많다...!

spring 인강을 차례차례 듣고 있는 데 이러다가는 개인프로젝트를 죽었다 깨어나도 올해는 안될 거 같아서 그냥 맨땅에 헤딩.

그니까, 김영한님의 인강에서 나온 말을 인용하자면, "야생형"이 되버려고 한다.

그리고 나한테는 아주 좋은 선임이 있다. 선임의 도움도 받아가면서 한번 해보았다.

## Google OAuth 서비스 등록

[구글클라우드콘솔](https://console.cloud.google.com/)에 들어가면 된다. 

![](/assets/images/2023-02-28/google1.png)

웹 콘솔에 들어가서 저 빨간 박스를 클릭한다. (제가 이미 한번 테스트겸 만든 이름이 들어있지만 무시하시면 된다.)

![](/assets/images/2023-02-28/google2.png)

새 프로젝트를 누른다.

![](/assets/images/2023-02-28/google3.png)

필수 사항인 프로젝트 이름을 적는다. 

![](/assets/images/2023-02-28/google4.png)

위에 보면 파란색 박스처럼 생성한 프로젝트의 이름이 그대로 뜰 것이다. 
그러면, OAuth 동의화면에 들어가서 "외부"를 클릭하고 만들기 버튼을 클릭한다.

![](/assets/images/2023-02-28/google5.png)

여기서 필수사항은 빨간 박스들이다. 앱 이름, 이메일, 개발자 연락처 정보를 적는다.

![](/assets/images/2023-02-28/error.png)

여기서 나는 계속 "앱을 저장하는 중에 오류가 발생했습니다." 라고 에러가 뜨면서 저장이 되질 않았다. 

이유를 확인해보니 프로젝트 이름에 "google"이 들어가서였다. google을 지우고 저장 후 계속을 눌렀더니 성공했다!

![](/assets/images/2023-02-28/google6.png)

그리고 범위를 추가해준다.

![](/assets/images/2023-02-28/google7.png)

이 부분은 자신이 원하는 부분을 선택하면 된다. 나는 위 빨간색 박스 3개만 골랐다.

고른 뒤 업데이트를 해준다.

![](/assets/images/2023-02-28/google8.png)

그러면 파란색 박스처럼 내가 선택한 범위들이 뜰 것이다. 저장 후 계속을 누르자.

![](/assets/images/2023-02-28/google9.png)

테스트 사용자를 여기서 추가할 수 있다. 하지만, 나는 나 혼자 만들기 때문에 추가없이 하겠다. 

![](/assets/images/2023-02-28/google10.png)

이 부분은 요약이기 때문에 내가 지금까지 설정한 값들이 모여있다. 한번 보고 잘못 설정한 게 있는지 확인해보자.

![](/assets/images/2023-02-28/google11.png)

`사용자 인증 정보`에서 `사용자 인증 정보 만들기` 를 선택한다. 

![](/assets/images/2023-02-28/google12.png)

OAuth 클라이언트 ID를 선택한다.

![](/assets/images/2023-02-28/google13.png)

애플리케이션 유형을 선택해주면 되는데 일단 웹 애플리케이션을 선택했다. 

![](/assets/images/2023-02-28/google14.png)

그러면 위 사진과 같은 구성으로 나올 것이다. 유형에 따라서 구성은 다르게 나올 것이다. 

나는 웹 애플리케이션으로 알려주겠다. 

여기서 이름, URI를 필수적으로 넣어준다.

test용이기 때문에 이름은 그냥 구글에서 넣어준 디폴트 값으로 그대로 할 것이고, URI는 정말 중요하다!!!

```plaintext
http://localhost:8080/login/oauth2/code/google
```

이렇게 적어주어야한다. 

내 URI가 좀 다른 이유는 8000번 포트를 사용하고 API라는 경로도 추가로 설정을 다르게 했다.

설정을 나처럼 다르게 하지않았다면 위에 적은 그대로 바꾸지 않고 쓰면 된다. 


![](/assets/images/2023-02-28/google15.png)

만들기 버튼을 누르면 위 사진과 같은 창이 하나 뜰 것이다.

이 ID와 비밀번호는 정말 중요하니 카피해서 어디다가 저장해둬야한다.

![](/assets/images/2023-02-28/google16.png)

확인 버튼을 누르면 이렇게 설정한 정보가 뜰 것이다. 

이렇게 구글 콘솔에서 할 설정은 끝났다.

이제 Spring 으로 넘어가보자.