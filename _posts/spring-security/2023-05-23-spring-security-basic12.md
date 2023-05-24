---
layout: single
title:  "[spring security basic] 12. 인증 저장소 - SecurityContextHolder, SecurityContext"
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

## SecurityContextHolder, SecurityContext

### 1. SecurityContextHolder, SecurityContext 코드

1. SecurityConfig code

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {
    @Bean
    public SecurityFilterChain adminFilterChain(HttpSecurity http) throws Exception {


        http
            .authorizeHttpRequests(
                requests ->
                    requests
                        .anyRequest().authenticated()
            );

        http
            .formLogin();

        //이러면 자식 스레드와 공유하기 때문에 값이 있다. 
        SecurityContextHolder.setStrategyName(SecurityContextHolder.MODE_INHERITABLETHREADLOCAL);

        return http.build();
    }

}
```

2. RestController code

```java
@RestController
public class SecurityController {

    @GetMapping("/")
    public String index(HttpSession session){

        //인증객체1 (SecurityContextHolder 사용)
        Authentication authentication = SecurityContextHolder.getContext().getAuthentication();

        //인증객체2 (Session 사용)
        SecurityContext context = (SecurityContext)session.getAttribute(
            HttpSessionSecurityContextRepository.SPRING_SECURITY_CONTEXT_KEY);
        Authentication authentication1 = context.getAuthentication();

        /*
         * 인증객체1과 2는 동일한 객체이다.
         */

        return "home";
    }

    @GetMapping("/thread")
    public String thread(){
        //자식 스레드이기 때문에 값이 null이다.
        new Thread(
            () -> {
                Authentication authentication = SecurityContextHolder.getContext().getAuthentication();
            }
        ).start();

        return "thread";
    }
}
```

### 2. 설명

1. SecurityContext (설명할 때 S.C 로 축약해서 부르겠다.)
   1. Authentication 객체가 저장되는 보관소로 언제든지 인증객체를 꺼내서 쓸 수 있다. 
   2. ThreadLocal에 저장되어서 아무 곳에서나 참조가 가능하다.
   3. 인증 완료 후 HttpSession에 인증객체가 저장되어서 전역적인 참조가 가능하다.
2. SecurityContextHolder
   1. S.C 객체 저장
   2. 저장 방식은 총 3가지가 있다.
      1. MODE_THREADLOCAL : (default) 스레드 당 S.C 객체를 할당, 보통 메인 스레드 로컬에 S.C 를 저장한다. 그래서 자식 스레드에서는 S.C 가 null 로 나온다
      2. MODE_INHERITABLETHREADLOCAL : 메인 스레드와 자식 스레드가 동일한 S.C 를 유지한다.
      3. MODE_GLOBAL : Static 변수에다가 S.C 를 저장한다. 

Authentication 객체를 `Authentication authentication = SecurityContextHolder.getContext().getAuthentication();` 로 꺼내서 쓸 수 있다.

![](/assets/images/2023/05/24/filter4.png)