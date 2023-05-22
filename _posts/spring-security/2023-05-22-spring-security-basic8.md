---
layout: single
title:  "[spring security basic] 8. 인증/인가 API"
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

## 인증/인가 API

### 1. 인증/인가 API 코드

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain fileterChain(HttpSecurity http) throws Exception {

       http
            .exceptionHandling(auth ->
                auth
                    .authenticationEntryPoint((request, response, authException) -> {
                        response.sendRedirect("/login ");
                    })
                    .accessDeniedHandler((request, response, accessDeniedException) -> {
                        response.sendRedirect("/denied" );
                    })
            );


        http
            .formLogin()
            .successHandler((request, response, authentication) -> {
                RequestCache requestCache = new HttpSessionRequestCache();
                SavedRequest savedRequest = requestCache.getRequest(request, response);
                String redirectUrl = savedRequest.getRedirectUrl(); //사용자가 원래 가고 싶었던 페이지 경로
                response.sendRedirect(redirectUrl); //인증에 성공하고 나서 바로 그 이전의 정보를 가져와서 바로 이동하도록 처리
            });
        
        return http.build();
    }
}

```



