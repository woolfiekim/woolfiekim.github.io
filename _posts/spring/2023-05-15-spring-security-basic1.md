---
layout: single
title:  "[spring security basic] 1. login & logout"
categories: 
    - spring  #카테고리
tag: [Spring, Spring security] #태그
toc: true  #오른쪽에 있는 목차
toc_sticky: true #목차 고정
author_profile: true  #왼쪽에 자기 소개란 프로필을 이 페이지에 들어갈때 끄는 기능
sidebar:
    nav: "sidebar-category" #navigation.yml에 있는 docs를 뜻한다.
search: true  #이 페이지는 검색에 나옴.
---

## 스프링 시큐리티 세팅

### 1. Spring Boot 기반으로 개발하는 Spring Security (정수원 선생님) 인강 - 세팅

인강에서는 jdk가 1.8이상을 기준으로 만드는데, 내가 쓰는 spring boot는 3.0으로 java 17이상 무조건 써야하는 상황이다.

그렇다고 해서 spring boot를 예전 버전으로 돌리고 싶지 않기 때문에 3.0을 그대로 java 17을 써볼 것이다.

### 2. SecurityConfig 설정

인강에서는

```java
extends WebSecurityConfigurerAdapter
```

를 쓴다. WebSecurityConfigurerAdapter 는 java 17에서는 deprecated가 되어있어서 쓰지 못한다.

WebSecurityConfigurerAdapter를 상속받고 configure 메소드를 오버라이딩 하는 방식을 쓰지 않도록 바뀌었다.

대신, @Bean으로 커스텀한 설정을 등록하도록 만들면 된다.


<< 인강 - 원본 >>

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception { 
        http
            .authorizeRequests()			
            .anyRequest().authenticated()		
        .and()
            .formLogin(); 			
}
```

<< java 17 버전 >>

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain fileterChain(HttpSecurity http) throws Exception {
        http
            .authorizeHttpRequests(auth -> auth
                .anyRequest().authenticated()
            );
        http.formLogin();
        return http.build();
    }
}
```

>> 바뀐 버전에서 `http.formLogin();`부분에서 따로 떨어뜨리지 않고 원본처럼 `.and()` 를 써도 된다. 가독성을 위해서 Login과 authorize를 나눠서 작성 한 것 뿐이다.


보면 바뀐 것들이 좀 있다. 

앞에 말했 듯이, 클래스 부분에서 `WebSecurityConfigurerAdapter`를 상속받지 않도록 바뀌었고 오버라이딩을 하지 않는 대신 Bean으로 등록하게 바뀌었다.

그리고 Bean을 보면 `SecurityFilterChain` 을 써서 설정을 커스텀한다. 그리고 spring security 6 (spring boot 3.0)부터 람다식을 쓸 수 있다. 그래서 람다식으로 써보았다.


