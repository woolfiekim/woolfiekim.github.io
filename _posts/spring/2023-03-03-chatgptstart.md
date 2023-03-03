---
layout: single
title:  "[Spring chatGPT] 스프링으로 openAI api 클라이언트 사용하기"
categories: 
    - spring  #카테고리
tag: [Spring, chatGPT, openAI, java] #태그
toc: true  #오른쪽에 있는 목차
toc_sticky: true #목차 고정
author_profile: true  #왼쪽에 자기 소개란 프로필을 이 페이지에 들어갈때 끄는 기능
sidebar:
    nav: "sidebar-category" #navigation.yml에 있는 docs를 뜻한다.
search: true  #이 페이지는 검색에 나옴.
---

이번엔 아주 핫한 chatGPT를 한 번 써보려고 한다.

평소에도 chatGPT로 코딩하면서 모르는 기초 지식으로 물어보면서 공부하고 있는 데 아주 유용하다. 

비슷해서 뭐가 다른 것인지 잘 모르는 용어들이 있을 경우 그 용어들을 정의, 비교를 해주어서 이해하기 딱 좋다.

그래서 이렇게 좋은 chatGPT를 내 개인프로젝트에서 사용할 수 있는 방법이 있을 것 같아서 열심히 구글링하거나 chatGPT에게 물어봤다.

chatGPT는 진짜.... 후... 라이브러리 없이 사용하는 방법을 알려줘서 몇 시간동안 따라하다가 포기했다.

좀 더 간단하게 설정할 수 있는 방법을 이 블로그에 적겠다.

## 1. API key 발급받기

[openapi api key발급받는 사이트](https://platform.openai.com/docs/quickstart/build-your-application)에 들어간다.


![](/assets/images/2023-03/03/chat2.png)

사이트에 들어가면 위 사진에 표시한 부분에 들어가서 `Create new secret key` 버튼을 누르면

![](/assets/images/2023-03/03/chat3.png)

위처럼 키를 발급해준다.(참고로 저기 뜬 key는 삭제 했기 때문에 그대로 쓴다고 해도 써지질 않을 것이다. 따로 발급받길 바람.)

키는 복사해서 3번 컨트롤러 생성 부분에 넣어주면 된다.

## 2. dependency 추가

일단, 라이브러리부터 설치해야한다.

build.gradle 의 dependencies에 아래를 넣는다.

```gradle
implementation group: 'com.theokanning.openai-gpt3-java', name: 'client', version: '0.9.0'
```

![](/assets/images/2023-03/03/chat1.png)

꼭 잊지말고 위 코끼리 모양을 누르자!!


## 3. controller 생성

```java
@PostMapping
    public ResponseEntity<?> sendQuestion(@RequestBody String request) {
        
        //1번에 발급받은 API key를 붙여 넣는다.
        OpenAiService service = new OpenAiService("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");
        CompletionRequest completionRequest = CompletionRequest.builder()
            .prompt(request)
            .model("text-davinci-003") // 이 모델로 해줘야 제대로 대화가 된다. 하지만 한국어는 잘 안된다. 다른 모델을 써야할듯...
            .echo(false) // 이 기능은 내가 질문한 걸 똑같이 뱉어주고 나서 그 질문의 답을 그 뒤에 붙여서 보내기 때문에 질문을 반복할 필요가 없기 때문에 false
            .build();
        return ResponseEntity.ok(service.createCompletion(completionRequest).getChoices());
    }
```

이러면 끝이다! 

인섬니아로 한 번 호출해보자!

## 4. 실행 결과

![](/assets/images/2023-03/03/chat4.png)

뭔가 앞에 쓸데없는 사족이 붙은 기분이 들지만 기분 탓일 것이다.

아무튼! 뭔가 대화가 된다. 성공이다!