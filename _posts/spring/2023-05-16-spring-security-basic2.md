---
layout: single
title:  "[spring security basic] 2. Form login 인증"
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

## login 설정

### 1. login Configuration 설정

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain fileterChain(HttpSecurity http) throws Exception {

        http.formLogin()
            .loginPage("/loginPage") // 이걸 안쓰면 spring security에서 제공하는 기본 로그인 페이지로 가게 된다.
            .defaultSuccessUrl("/") // 로그인 성공 후 이동페이지
            .failureUrl("/login") // 로그인 실패 시 이동페이지
            .usernameParameter("userId") // 아이디 파라미터명 변경
            .passwordParameter("passwd") // 비번 파라미터명 변경
            .loginProcessingUrl("/login_proc") // 로그인 폼에서 제출 버튼을 누르면 호출될 URL
            .successHandler(new AuthenticationSuccessHandler() { // 로그인 성공 후 동작 설정
                // 이 successHandler를 쓰면, defaultSuccessUrl은 작동을 하지 않는다.
                @Override
                public void onAuthenticationSuccess(HttpServletRequest request, HttpServletResponse response,
                    Authentication authentication) throws IOException, ServletException {
                    //Authentication : 인증에 성공했을 때 최종적으로 그 인증의 결과를 담은 인증 객체
            
                    System.out.println("authentication : " + authentication.getName()); //인증에 성공한 사용자 이름
            
                    response.sendRedirect("/");
                    //인증에 성공하고 root페이지로 이동
                    // 여기서 successHandler가 정의되면 .defaultSuccessUrl("/") 은 작동을 안한다.
                }
            })
            .failureHandler(new AuthenticationFailureHandler() {
                @Override
                public void onAuthenticationFailure(HttpServletRequest request, HttpServletResponse response,
                    AuthenticationException exception) throws IOException, ServletException {
                    //AuthenticationException : 인증 예외의 객체
            
                    System.out.println("exception : " + exception.getMessage());
            
                    response.sendRedirect("/login");
                }
            })
            .permitAll()
        ;

        return http.build();
    }
}

```

### 2. 설정 후 출력결과

![](/assets/images/2023-05/15/login.png)

