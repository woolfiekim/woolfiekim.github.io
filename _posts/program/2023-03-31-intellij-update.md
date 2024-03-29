---
layout: single
title:  "[Intellij] 인텔리제이 업데이트 시 설정"
categories: 
  - program  #카테고리
tag: [Intellij, update, gradle] #태그
toc: true  #오른쪽에 있는 목차
toc_sticky: true #목차 고정
author_profile: true  #왼쪽에 자기 소개란 프로필을 이 페이지에 들어갈때 끄는 기능
sidebar:
    nav: "sidebar-category" #navigation.yml에 있는 docs를 뜻한다.
search: true  #이 페이지는 검색에 나옴.
---

2023-03 이번에 인텔리제이가 업데이트를 하였다.

꽤 대규모인거 같다. 문서를 보니 바뀐게 한 두가지가 아니다.

새로운 건 뭐든 좋은 법!!

바로 업데이트를 해봤다.

그런데, 사수가 **gradle**일 경우는 설정을 건들여야한다고 알려줬다.

설정을 봐보자.

## gralde 의 build and run 설정 

설정은 `cmd + ,` 를 누르면 들어가진다.


![](/assets/images/2023/03/31/gradle1.png)

위 사진과 같은 경로로 들어가면 된다. 

빨간 박스로 표시한 부분이 업데이트 전과 달라졌다는 것을 볼 수가 있다.

원래는 `Build` 만 잇었던 곳이 `Build and run` 이라고 바뀌었다.

빌드하고 실행을 한다는 것이다.

## Gradle 로 그대로 설정을 둘 때의 단점들

### 단점1 : 실행 시 오래걸린다.

이대로 두고 사용을 해도 되지만 `Gradle`로 실행을 한다면 핫스왑이 되지 않아서 실행을 할 때마다 굉장히 오래걸린다.

> 핫스왑 : 바뀐 부분이 있는 부분만 다시 빌드해서 빌드 시 시간이 절약된다.

![](/assets/images/2023/03/31/gradle3.png)

실행 시 사진의 빨간 박스처럼 나오면서 시간이 꽤 걸린다.

### 단점2. : 실행 후 중지 시 에러문구가 뜬다.

그리고 `Gradle`로 실행을 하고 실행을 중지할 때 에러 문구를 볼 수 있다.

![](/assets/images/2023/03/31/gradle4.png)

빌드와 같이 하기 때문에 빌드가 실패했다는 문구인데 무시해도 된다. 하지만, 내 성격상 이런 빨간 줄은 보기가 싫으니 `Intellij IDEA`로 바꾸려는 것이다. 

![](/assets/images/2023/03/31/2.jpeg)

## Intellij IDEA 로 변경해보자.

![](/assets/images/2023/03/31/gradle2.png)

위 사진처럼 선택하고 `Apply` & `OK` 버튼을 꼭 눌러주자!!


## Intellij IDEA 로 바꿀 시 단점과 장점

### 장점 1 : 실행 속도가 빠르다.

Intellij IDEA로 바꾼다면,

![](/assets/images/2023/03/31/gradle5.png)

위 사진과 같이 빨간 부분이 사라져 있는 것을 볼 수가 있다.

핫스왑으로 실행하기 때문에 굉장히 실행 속도가 빠르다.

### 장점 2 : 실행 후 중지 시 에러 문구가 사라져있다. 

![](/assets/images/2023/03/31/gradle6.png)

바꾸면 이렇게 중지를 해도 빨간 에러 문구가 뜨지 않는다. 

![](/assets/images/2023/03/31/1.jpg)

### 단점 : 보지 못한 새 폴더들이 생성된다.


![](/assets/images/2023/03/31/gradle8.png)

이렇게 `generated`라는 폴더(Q파일이 있다)가 원래는 `build` 폴더 안에 있어야하는데 따로 떨어져 나온다.

![](/assets/images/2023/03/31/gradle7.png)

그리고 위 사진을 보면 `out`이라는 폴더가 새로 생성되어있음을 확인 할 수 있다.

이 폴더들은 github에 올릴 때 올리지 않게 gitignore을 해줘야한다.

![](/assets/images/2023/03/31/gradle9.png)

위 사진처럼 추가로 적으면 된다.

`out/` 은 인텔리제이가 자동으로 추가해준다.



## 결론 : 필자는 Intellij IDEA 로 바꿀 것이다.

왜냐하면, 단점도 단점답지 않는 것 같다. 처음에 gitignore에 추가하면 되니까! 

장점이 꽤 내 성격 상 참 잘 맞는거 같다.

그리고 이 설정은 `gradle` 로 프로젝트를 새로 생성할 때마다 건들여줘야한다....

이게 참.... 귀찮을 거 같다...... 

이것도 한번 설정을 건들면 다른 새 프로젝트를 만들 때도, 유지할 수 있도록 Intellij 쪽에서 업데이트 해줬으면 좋겠다!


## 추가 글 : Intellij의 공식문서에서 설정을 바꾸지 말라한다....

사수가 말을 해줬다... 개인 프로젝트를 하는 데 설정을 바꿨더니 몇몇의 파일이 제외되었다구... 설정을 바꾸지 않는게 낫다는 결론이다..ㅠㅠ