---
layout: single
title:  "[Spring event] 스프링 비동기"
categories: 
    - spring  #카테고리
tag: [Spring, spring event] #태그
toc: true  #오른쪽에 있는 목차
toc_sticky: true #목차 고정
author_profile: true  #왼쪽에 자기 소개란 프로필을 이 페이지에 들어갈때 끄는 기능
sidebar:
    nav: "sidebar-category" #navigation.yml에 있는 docs를 뜻한다.
search: true  #이 페이지는 검색에 나옴.
---

# Spring 비동기

회사에서 배포한 서버가 time-out이 났다. 

그 이유가 다른 서버를 불러서 답이 오는 것을 기다리는데, 그 서버가 타 회사의 것이여서 어떻게 된 지는 모르겠지만 서버가 응답을 하지 않아서 time-out이 난 것이였다.

그래서 해결 방법은 일단 DB에서 트랜잭션 걸려잇는거 커넥션풀을 다 쓰고있었다. 왜냐하면, restTemplate에서 timeout을 안걸어서 트랙잭션이 끝나질 않았다. 그래서 그 테이블에 락이 걸린거다. 

oracle 은 일정시간이 지나면 알아서 커넥션을 끊어주는데 tibero는 그런 기능따위 없다.

회사에서 쓰는 스프링은 톰캣을 쓰는데 톰캣은 쓰레드를 기반이다. 요청하나당 쓰레드 하나를 사용 > 트랜잭션을 하나 쓴다. commit이 안되는 트랜잭션이 계속 쌓인다.

트랜잭션의 세션을 지웠다. 

하고 타 회사의 api를 호출하고 응답을 받는 부분을 동기가 아닌 비동기로 바꾸면 되었다.

## 1. api를 동기에서 비동기로 전환

### (1) timeout 설정

일단, 비동기로 바꿔도 계속 응답이 오는 걸 무작정 기다릴 수는 없다. 그러니 timeout을 설정하자

```java
@Bean
    public RestTemplate getRestTemplate() {

        HttpComponentsClientHttpRequestFactory requestFactory = new HttpComponentsClientHttpRequestFactory();

        requestFactory.setConnectTimeout(30_000); //30초
        requestFactory.setReadTimeout(30_000);

        return new RestTemplate(requestFactory);
    }
```

- 위 코드에서 **`setConnectTimeout`**은 서버와의 연결을 맺는데 소요되는 시간을 설정
- **`setReadTimeout`**은 서버로부터 응답을 받는 데 소요되는 시간을 설정

- 여기서 숫자에 `_`를 중간에 삽입하여도 아무런 지장이 없다. 이는 숫자가 클 경우 몇 인지 가독성을 높이기 위해 붙이는데 쓰인다.

예를 들어

```java
Long number = 123_456_789L;
```

이런 식으로 쓰인다. 

### (2) Spring Event 를 사용해 동기를 비동기로 전환

Spring Framework에서 이벤트(Event)는 객체 간의 느슨한 결합(loose coupling)을 가능하게 하는 중요한 기능 중 하나이다. 이벤트는 객체가 다른 객체에게 상태 변화를 알리는 방법으로 사용된다.

Spring Framework에서 이벤트는 **`ApplicationEvent`** 클래스를 상속하는 클래스로 표현된다. **`ApplicationEvent`**는 추상 클래스이며, 이벤트의 구체적인 유형은 이를 상속하는 클래스에서 정의된다.

Spring에서 이벤트를 발생시키려면, **`ApplicationEventPublisher`** 인터페이스를 구현한 클래스를 사용해야한다. Spring은 이 인터페이스를 구현한 **`ApplicationEventPublisherAware`** 인터페이스도 제공한다. 이 인터페이스를 구현하면, 객체 생성 시 Spring이 자동으로 **`ApplicationEventPublisher`**를 주입해 준다.

이벤트를 처리하려면, **`ApplicationListener`** 인터페이스를 구현하는 클래스를 작성해야 한다. 이 인터페이스는 **`onApplicationEvent()`** 메서드를 정의하며, 이벤트가 발생했을 때 호출된다. 이벤트 핸들러를 등록하려면, **`ApplicationEventPublisher`**를 사용하여 등록한다.

Spring에서는 이벤트의 처리 과정에서 다양한 기능을 제공한다. 예를 들어, 이벤트가 발생한 후 이벤트 처리가 완료될 때까지 기다리는 동기식 방식과 이벤트 발생 후 콜백을 받아 처리하는 비동기식 방식이 제공된다. 또한, 이벤트 처리 중 에러가 발생하면 해당 이벤트를 재전송하는 기능도 제공된다.

이벤트는 Spring에서 다양한 용도로 사용된다. 예를 들어, **`ContextRefreshedEvent`** 이벤트는 Spring 애플리케이션이 초기화되었을 때 발생하며, 이벤트 핸들러에서 초기화 작업을 수행할 수 있다. 또 다른 예로는, **`ApplicationEventMulticaster`** 클래스를 사용하여 다른 Spring 애플리케이션 간에 이벤트를 전달할 수도 있다.