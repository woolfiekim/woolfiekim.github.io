---
layout: single
title:  "[spring security basic] 6. 세션 고정 보호"
categories: 
    - spring-security  #카테고리
tag: [Spring security] #태그
toc: true  #오른쪽에 있는 목차
toc_sticky: true #목차 고정
author_profile: true  #왼쪽에 자기 소개란 프로필을 이 페이지에 들어갈때 끄는 기능
sidebar:
    nav: "sidebar-category" #navigation.yml에 있는 docs를 뜻한다.
search: true  #이 페이지는 검색에 나옴.
---

## 세션 고정 보호

### 1. 세션 고정 보호 설정

공격자가 먼저 서버에 접속해서 받은 세션(a)으로 

원래 그 계정의 사용자가 서버에 접속하기 전에 세션을 (a)로 바꾸고 로그인을 할 시, 

공격자가 같은 세션(a)으로 로그인없이 서버에 접속할 수 있다. 



이를, 방지해주는 것이 sessionManagement 이다.

여기서 .sessionManagement().changeSessionId() 를 쓴다면 공격자가 사용자의 세션을 아무리 바꿔도 

사용자가 로그인을 할 시 세션을 그대로 사용하지 않고 바꾸기 때문에 공격자가 같은 세션으로 접속을 해도 접속 할 수가 없다.

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain fileterChain(HttpSecurity http) throws Exception {

        http
            .sessionManagement(form ->
                form
                    .sessionFixation()
                    .changeSessionId() // <<와 .none(), .migrateSession(), .newSession() 이렇게 4개 버전이 있다. changeSessionId가 default이다.
                    // .none() 은 무방비 상태로 공격을 당할 수 있다.
                    // .changeSessionId() 는 공격에서 자유로울 수 있다.
                    .sessionCreationPolicy(SessionCreationPolicy.ALWAYS) // 스프링 시큐리티가 항상 세션 생성
                    .sessionCreationPolicy(SessionCreationPolicy.IF_REQUIRED) // 스프링 시큐리티가 필요시 생성 (default)
                    .sessionCreationPolicy(SessionCreationPolicy.NEVER) // 스프링 시큐리티가 생성하지 않지만 이미 존재하면 사용
                    .sessionCreationPolicy(SessionCreationPolicy.STATELESS) // 스프링 시큐리티가 생성하지 않고 존재해도 사용하지 않음
            );

        
        return http.build();
    }
}

```