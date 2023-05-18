---
layout: single
title:  "[spring security basic] 5. 동시 세션 제어"
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

## 동시 세션 설정

### 1. 동시 세션 설정

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain fileterChain(HttpSecurity http) throws Exception {

        http
            .sessionManagement(form ->
                form
                    .maximumSessions(1) // 최대 허용 가능 세션 수 ( -1 : 무제한 로그인 세션 허용 )
                    .maxSessionsPreventsLogin(false) // 동시 로그인 차단
                    // true            : 현재 사용자 인증 실패 (이전 사용자는 계속 사용 가능)
                    // false (default) : 이전 사용자 세션 만료 (현재 사용자만 계속 사용 가능)
                    .invalidSessionUrl("invalid") // 세션이 유효하지 않을 때 이동할 페이지
                    .expiredUrl("/expired") // 세션이 만료된 경우 이동할 페이지 
                    // expiredUrl 과 invalidSessionUrl 을 같이 적을 경우 invalidSessionUrl 을 우선시 한다.
            );
        
        return http.build();
    }
}

```

### 2. 설정 후 출력결과


**.maxSessionsPreventsLogin(false) 일 경우**

![](/assets/images/2023/05/18/false.png)

**.maxSessionsPreventsLogin(true) 일 경우**

![](/assets/images/2023/05/18/true.png)

