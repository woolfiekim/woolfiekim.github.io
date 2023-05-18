---
layout: single
title:  "[spring security basic] 4. Remember Me 인증"
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

## Remember Me 설정

### 1. Remember Me Configuration 설정 (구버전)

```java
@Configuration
@EnableWebSecurity
@NoArgsConstructor
public class SecurityConfig {

    @Bean
    public SecurityFilterChain fileterChain(HttpSecurity http) throws Exception {

        private UserDetailsService userDetailsService;

        http
            .rememberMe()
            .rememberMeParameter("remember") // 파라미터명 설정
            .tokenValiditySeconds(3600) // 리멤버미 토큰의 유효기간 설정 >> defalut 값 : 14days
            .userDetailsService(userDetailsService)
        ;
        
        return http.build();
    }
}

```

### 2. Remember Me Configuration 설정 (신버전)

```java
@Configuration
@EnableWebSecurity
@NoArgsConstructor
public class SecurityConfig {

    @Bean
    public SecurityFilterChain fileterChain(HttpSecurity http) throws Exception {

        private UserDetailsService userDetailsService;

        http
            .rememberMe(form ->
                form
                    .rememberMeParameter("remember")
                    .tokenValiditySeconds(3600)
                    .userDetailsService(userDetailsService)
            );
        
        return http.build();
    }
}

```

### 3. 설정 후 출력결과

![](/assets/images/2023-05/17/rememberme.png)

