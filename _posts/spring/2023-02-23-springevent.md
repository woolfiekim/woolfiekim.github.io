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

KeyFlow 에 사용자 추가하면 AD 에 사용자도 추가해주는데 그 역할을 해주는 것이 ADAgent이다. 

MAM장비에 서비스를 올릴 때 KeyFlow Server, KeyFlow Asset Agent, KeyFlow ADAgent 이렇게 3개의 앱을 올린다. 그 중에 KeyFlow ADAgent 가 저 윈도우 서버에 AD 에 사용자 추가해주는 역할을 하고 있는데 저 KeyFlow ADAgent 가 톰캣처럼 웹서버를 구동하고 있다. 

그래서 KeyFlow 에서 사용자 추가하면, WAS 가 저 KeyFlow ADAgent 의 웹서버를 호출해서 AD 에 사용자를 추가해주고 있다. 

그런데 저 ADAgent 의 웹서버를 WAS 가 호출했는데 응답이 없었던 것이다. 

- AD(Active Directory) 라는 서비스이고 Windows Server 에서 제공해준다.
- SSO(Single Sign On) : 하나의 아이디로 어디서든지 로그인 가능하게 해주는 서비스

그래서 2가지를 수행해줘서 해결했다.

일단 DB에서 커넥션풀을 다 쓰고 있었다. 왜냐하면, restTemplate에서 timeout을 안걸어서 트랙잭션이 끝나질 않았다. 그래서 그 테이블에 락이 걸린거다. 

oracle 은 트랜잭션이 시행된 지 일정시간이 지나면 알아서 커넥션을 끊어주는데 tibero는 그런 기능따위 없다.

회사에서 쓰는 스프링은 톰캣을 쓰는데 톰캣은 쓰레드 기반이다. 요청하나당 쓰레드 하나를 사용 > 트랜잭션을 하나 쓴다. commit이 안되는 트랜잭션이 계속 쌓인다.

그래서 

1. 무한으로 계속 돌고있던 트랜잭션의 세션을 강제로 지워야한다. 

2. ADAgent를 호출하고 응답을 받는 부분을 동기가 아닌 비동기로 바꾸면 되었다.

## 2. api를 동기에서 비동기로 전환

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

### (2) Spring Event 란?

Spring Framework에서 이벤트(Event)는 객체 간의 느슨한 결합(loose coupling)을 가능하게 하는 중요한 기능 중 하나이다. 이벤트는 객체가 다른 객체에게 상태 변화를 알리는 방법으로 사용된다.

Spring Framework에서 이벤트는 **`ApplicationEvent`** 클래스를 상속하는 클래스로 표현된다. **`ApplicationEvent`**는 추상 클래스이며, 이벤트의 구체적인 유형은 이를 상속하는 클래스에서 정의된다.

Spring에서 이벤트를 발생시키려면, **`ApplicationEventPublisher`** 인터페이스를 구현한 클래스를 사용해야한다. Spring은 이 인터페이스를 구현한 **`ApplicationEventPublisherAware`** 인터페이스도 제공한다. 이 인터페이스를 구현하면, 객체 생성 시 Spring이 자동으로 **`ApplicationEventPublisher`**를 주입해 준다.

이벤트를 처리하려면, **`ApplicationListener`** 인터페이스를 구현하는 클래스를 작성해야 한다. 이 인터페이스는 **`onApplicationEvent()`** 메서드를 정의하며, 이벤트가 발생했을 때 호출된다. 이벤트 핸들러를 등록하려면, **`ApplicationEventPublisher`**를 사용하여 등록한다.

Spring에서는 이벤트의 처리 과정에서 다양한 기능을 제공한다. 예를 들어, 이벤트가 발생한 후 이벤트 처리가 완료될 때까지 기다리는 동기식 방식과 이벤트 발생 후 콜백을 받아 처리하는 비동기식 방식이 제공된다. 또한, 이벤트 처리 중 에러가 발생하면 해당 이벤트를 재전송하는 기능도 제공된다.

이벤트는 Spring에서 다양한 용도로 사용된다. 예를 들어, **`ContextRefreshedEvent`** 이벤트는 Spring 애플리케이션이 초기화되었을 때 발생하며, 이벤트 핸들러에서 초기화 작업을 수행할 수 있다. 또 다른 예로는, **`ApplicationEventMulticaster`** 클래스를 사용하여 다른 Spring 애플리케이션 간에 이벤트를 전달할 수도 있다.

### (3) Spring event 사용하는 방법

총 3가지 단계가 있다.

Spring event 단계 : 이벤트 정의(`ApplicationEvent`) > 이벤트 발행(**`ApplicationEventPublisher`**) > 이벤트 처리(**`ApplicationListener`)**

1.  `ApplicationEvent`에서는 event를 생성해준다.
2. **`ApplicationEventPublisher`** 는 이벤트 큐에 event를 넣어준다.
3. **`ApplicationListener`** 는 이벤트 큐에서 하나씩 뽑아서 실행해준다.

여기서 Spring 4.0 이상부터 event 코드 방식이 바뀌었다. 

일단은 바뀌기 전을 먼저 알려주겠다.

### (3) - 1 Spring 4.0 이전

1. 이벤트 정의 

Spring에서 이벤트는 **`ApplicationEvent`** 클래스를 상속하는 클래스로 표현된다. 예를 들어, 다음은 **`MyCustomEvent`**
 클래스를 정의하는 예제이다.

```java
public class MyCustomEvent extends ApplicationEvent {
    public MyCustomEvent(Object source) {
        super(source);
    }

    // 이벤트 관련 메서드
    public void doSomething() {
        // 이벤트 처리 로직
    }
}
```

1. 이벤트 발행

이벤트를 발행하려면, **`ApplicationEventPublisher`**인터페이스를 구현한 클래스를 사용한다. Spring은 이 인터페이스를 구현한**`ApplicationEventPublisherAware`** 인터페이스도 제공한다. 이 예제에서는 **`MyCustomEventPublisher`**클래스가 이를 구현하도록 하겠다.

```java
@Component
public class MyCustomEventPublisher implements ApplicationEventPublisherAware {
    private ApplicationEventPublisher eventPublisher;

    @Override
    public void setApplicationEventPublisher(ApplicationEventPublisher eventPublisher) {
        this.eventPublisher = eventPublisher;
    }

    public void doSomethingAndPublishEvent() {
        // 이벤트 발행
        MyCustomEvent event = new MyCustomEvent(this);
        eventPublisher.publishEvent(event);
    }
}
```

위 예제에서 **`setApplicationEventPublisher()`** 메서드를 통해 **`ApplicationEventPublisher`**를 주입받는다. **`doSomethingAndPublishEvent()`** 메서드를 호출하면, **`MyCustomEvent`** 객체를 생성하여 **`publishEvent()`** 메서드를 호출하여 이벤트를 발행한다.

이렇게 해도 되는데, 굳이 **`ApplicationEventPublisherAware`** 인터페이스를 구현할 필요가 없다. 왜냐하면, 이미 스프링 컨테이너가 구동될 때 **`ApplicationEventPublisher`** 가 bean으로 등록되기 때문에 추가 로직이 필요없다면 위 코드대로 하지 않고 (3) - 2 / 3번 처럼 하면 된다. 추가 로직이 필요하면 위 코드에 추가 로직을 넣어서 쓰면 된다.

1. 이벤트 처리

이벤트를 처리하려면, **`ApplicationListener`** 인터페이스를 구현한 클래스를 작성한다. 이 예제에서는 **`MyCustomEventHandler`** 클래스가 이를 구현하도록 하겠다.

```java
javaCopy code
@Component
public class MyCustomEventHandler implements ApplicationListener<MyCustomEvent> {
    @Override
    public void onApplicationEvent(MyCustomEvent event) {
        // 이벤트 처리
        event.doSomething();
    }
}
```

위 예제에서 **`onApplicationEvent()`** 메서드를 구현하여 이벤트를 처리한다. 이벤트 핸들러를 등록하려면, **`@Component`** 어노테이션을 사용하여 Spring이 이 클래스를 자동으로 스캔하도록 한다.

1. 이벤트 테스트

이벤트를 테스트하려면, **`MyCustomEventPublisher`** 클래스를 사용하여 이벤트를 발행하고, **`MyCustomEventHandler`** 클래스에서 이벤트 처리가 제대로 되는지 확인한다.

```java
javaCopy code
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes = { MyCustomEventPublisher.class, MyCustomEventHandler.class })
public class MyCustomEventTest {
    @Autowired
    private MyCustomEventPublisher publisher;

    @Test
    public void testCustomEvent() {
        // 이벤트 발행
        publisher.doSomethingAndPublishEvent();
    }
}

```

### (3) - 2 Spring 4.0 이상

1. 이벤트 정의

```java
@ToString
@Getter
public class MyCustomEvent {

    private final MyJob myJob;
    private final Map<String, Object> item;

    @Builder
    private MyCustomEvent(MyJob myJob, Map<String, Object> item) {
        this.myJob = myJob;
        this.item = defaultIfNull(item, Collections.emptyMap());
    }

}
```

Spring 4.0 이 전에는 `extends ApplicationEvent` 이부분이 있었는데 상속을 받지 않는 것을 볼 수가 있다.

`ApplicationEvent` 를 상속받지않고 POJO 형식으로 이벤트를 정의할 수 있게 되었다. 그래서 굳이 `ApplicationEvent` 을 상속받지 않고 `EventPublisher` 를 통해서 발생시킬 수 있다.

1. 이벤트 발행

이 부분은 없어도 된다. 왜냐하면 이벤트 처리에서 할 것이기 때문이다.

1. 이벤트 처리

```java
import com.commons.listener.myjob.event.MyCustomEvent;
import org.springframework.context.ApplicationEventPublisher;

@Slf4j
@RequiredArgsConstructor
@Service
@Transactional(readOnly = true)
public class CustomService {

    private final ApplicationEventPublisher publisher;
		
		@Transactional
		public void custom(Map<String, Object> item) throws Exception {
        publisher.publishEvent(
            MyCustomEvent.builder()
                .myJob(MyJob.ADD)
                .item(item)
                .build());
    }
	
}
```

2번의 발행 부분의 publisher를 여기서 사용하게 된다. 

훨씬 간단해진 것을 볼 수가 있다. 그리고 너무 Spring에만 의존하지 않게 된다.