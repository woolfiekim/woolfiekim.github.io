---
layout: single
title:  "[Google] google search console - 현재 색인이 생성되지 않음"
categories: 
  - blog  #카테고리
tag: [blog, gitBlog, google, search console] #태그
toc: true  #오른쪽에 있는 목차
toc_sticky: true #목차 고정
author_profile: true  #왼쪽에 자기 소개란 프로필을 이 페이지에 들어갈때 끄는 기능
sidebar:
    nav: "sidebar-category" #navigation.yml에 있는 docs를 뜻한다.
search: true  #이 페이지는 검색에 나옴.
---

오늘은 내 블로그 글들이 구글 서치가 잘 되는지 확인을 하는 날이다.

[구글 서치 콘솔 페이지](https://search.google.com/search-console)에서 확인이 가능하다.

![페이지](/assets/images/2023-02-21/console.png) 

아래 페이지 버튼을 누르면 된다.

잘 되고 있지 않을까? 하고 들어갔는 데 이게 무슨 일인가!!

![index](/assets/images/2023-02-21/index.png)

딸랑 색인이 5개만 되어있다..ㅠㅠㅠㅠㅠㅠ

엉엉엉 이게 무슨일인가!!!

이유를 찾아보자. 

![페이지](/assets/images/2023-02-21/reason.png)

페이지를 내려가보면 이유가 적혀있다. 

저 사유를 클릭하면 저 사유로 색인이 생성되지 않는 페이지들을 볼 수 있다.

![error](/assets/images/2023-02-21/errorpage.png)

여기서 더 페이지를 내리면
![error](/assets/images/2023-02-21/finds.png)

이 리스트가 뜬다. 리스트에 마우스 오버하면 돋보기아이콘이 뜨고 그 아이콘을 누르면

![error](/assets/images/2023-02-21/test.png)

이렇게 페이지가 뜬다. 여기서 URL테스트를 누르고 (테스트는 좀 오래 걸릴 수 있다.)

URL을 Google에 등록할 수 있음이라는 문구가 뜨면 색인생성을 요청한다. 
![error](/assets/images/2023-02-21/indexrequest.png)

이렇게 마지막 문구가 뜨면 완료이다.
![error](/assets/images/2023-02-21/final.png)

나머지도 다 해보자.

색인 생성 요청은 하루 할당량이 있기 때문에 오늘 하루 만에 38개를 할 수는 없다. 그러기 때문에 며칠에 걸쳐서 해야할 것 같다. 

