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

- api(get제외)나 요청 보낼 때 랜덤하게 생성된 토큰을 http 파라미터로 요구한다.
  - get을 제외한 patch, post, put, delete는 데이터 변경이 있기 때문에 안전장치가 필요하다.
- 요청시 전될되는 토큰 값과 서버에 저장된 실제 값과 비교 후 일치하지 않으면 요청은 실패한다.(활성화 상태)

- Client
```html
<input type="hidden" name="${_csrf.parameterName}" value="${_csrf.token}" />
```
위 클라이언트의 태그가 필요한 경우는 thymeleaf + spring security 가 한 프로젝트에 같이 있을 때이다.

기본적으로 spring security 를 쓰면 페이지 요청 시 Model Attribute 에 csrf 정보를 자동으로 넣어준다.

보통은 한 프로젝트에서 클라이언트와 서버를 작업하지 않고 분리를 한다.

그럴 때에는 csrf 커스텀을 반드시 해야한다. 그리고 csrf 정보를 request Param 으로 보내줘야한다.

그러나, 중간에 토큰을 가로채기가 쉽기 때문에 위와 같은 방법들을 쓰지 않는다. 

그래서 보통은 csrf 기능을 끈다.

```java
http
    .csrf().disable();
```
