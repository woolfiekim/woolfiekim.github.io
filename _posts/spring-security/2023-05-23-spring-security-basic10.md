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

### 1. 필터 초기화와 다중 보안 설정 코드

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
                // .requestMatchers("/admin/**").permitAll()
                // .requestMatchers("/actuator/**").permitAll()
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