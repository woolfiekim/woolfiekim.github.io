---
layout: single
title:  "[spring security basic] 9. CSRF"
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

## CSRF, CsrfFilter

### 1. CSRF(사이트간 요청 위조) 코드

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain fileterChain(HttpSecurity http) throws Exception {

        http
            .csrf(); // (default) 활성화 

        http
            .csrf().disable(); // 비활성화
        
        return http.build();
    }
}

```

### 2. CSRF filter 설명

- 모든 api 요청에 랜덤하게 생성된 토큰을 http 파라미터로 요구한다.
- 요청시 전될되는 토큰 값과 서버에 저장된 실제 값과 비교 후 일치하지 않으면 요청은 실패한다.(활성화 상태)

- Client
```html
<input type="hidden" name="${_csrf.parameterName}" value="${_csrf.token}" />
```