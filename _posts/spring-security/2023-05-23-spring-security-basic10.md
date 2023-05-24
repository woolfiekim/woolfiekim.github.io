---
layout: single
title:  "[spring security basic] 10. 필터 초기화와 다중 보안 설정"
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

## 필터 초기화와 다중 보안 설정

### 1. 필터 초기화와 다중 보안 설정 코드 (인강 - 구버전)

```java
@Configuration
@EnableWebSecurity
@Order(0)
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    protected void configure(HttpSecurity http) throws Exception {

        http
            .antMatcher("/admin/**")
            .authorizeRequests()
            .anyRequest().authenticated()
        .and()
            .httpBasic();
    }

}

@Configuration
@Order(1)
public class SecurityConfig2 extends WebSecurityConfigurerAdapter {

protected void configure(HttpSecurity http) throws Exception {

        http
            .authorizeRequests()
            .anyRequest().permitAll()
        .and()
            .formLogin();
    }
}

```



### 2. 필터 초기화와 다중 보안 설정 코드 (신버전)

```java
@Configuration
@EnableWebSecurity
@Order(0)
public class SecurityConfig {

    @Bean
    public SecurityFilterChain adminFilterChain(HttpSecurity http) throws Exception {

        http.securityMatcher("/admin/**");

        http
            .authorizeHttpRequests(auth -> auth
                .anyRequest().authenticated()
            );

        http
            .httpBasic();

        return http.build();
    }

}

@Configuration
@Order(1)
class SecurityConfig2 {

    @Bean
    public SecurityFilterChain globalFilterChain2(HttpSecurity http) throws Exception {
        http
            .authorizeHttpRequests(auth ->
                auth.anyRequest().permitAll()
            );

        http.formLogin(Customizer.withDefaults());

        return http.build();
    }
}

```


### 3. 신버전으로 코드 설명

1. 일단 구버전과 다르게 Bean 으로 설정을 하기 때문에 이름을 다르게 설정해야한다. 

2. 시큐리티 다중보안 시 경로 넣기

```java
//구버전
http
    .antMatcher("/admin/**")
//신버전
http
    .securityMatcher("/admin/**");
```

antMatcher는 deprecated 가 되어서 사용이 불가하다. 

그러기 때문에 신버전처럼 쓰면 된다.


### 4. 결과

![](/assets/images/2023/05/24/filter1.png)

디버그를 돌려보면 위와 같이 WebSecurity class 에서 빈으로 등록해놓은 필터들이 리스트 형식으로 나온다.

![](/assets/images/2023/05/24/filter2.png)

이렇게 리스트로 보이게 된다.

![](/assets/images/2023/05/24/filter3.png)

그리고 FilterChainProxy 에서 요청에 맞는 필터를 찾아서 쓰게 된다.
