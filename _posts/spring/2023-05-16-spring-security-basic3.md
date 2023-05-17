---
layout: single
title:  "[spring security basic] 3. Form logout 인증"
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

### 1. logout Configuration 설정

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain fileterChain(HttpSecurity http) throws Exception {

        http
            .logout()
            .logoutUrl("/logout") // default 는 "logout"이다. spring security 의 logout 처리는 post 방식으로 처리된다.
            .logoutSuccessUrl("/login") // 로그아웃 성공 시 이동할 페이지만 알려줌
            .addLogoutHandler(new LogoutHandler() {
                @Override
                public void logout(HttpServletRequest request, HttpServletResponse response,
                    Authentication authentication) {
                    HttpSession session = request.getSession();
                    session.invalidate(); //세션 무효화
                }
            })
            .logoutSuccessHandler(new LogoutSuccessHandler() {
                //.logoutSuccessUrl()과 비슷하다. 다만 .logoutSuccessHandler()이 좀 더 다채롭게 구현이 가능하다.
                @Override
                public void onLogoutSuccess(HttpServletRequest request, HttpServletResponse response,
                    Authentication authentication) throws IOException, ServletException {
                    response.sendRedirect("/login");
                }
            })
            .deleteCookies("remember-me") //서버에서 만든 쿠키를 삭제
        ;
        
        return http.build();
    }
}

```

### 2. 설정 후 출력결과

![](/assets/images/2023-05/15/logout.png)

