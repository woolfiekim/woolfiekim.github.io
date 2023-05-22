---
layout: single
title:  "[spring security basic] 7. 권한 설정 및 표현식"
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

## 권한 설정 및 표현식

### 1. 권한 설정 및 표현식 코드

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain fileterChain(HttpSecurity http) throws Exception {

        http
            .authorizeHttpRequests(auth ->
                auth
                    .requestMatchers("/login").permitAll() // 모두 허용
                    .requestMatchers("/user").hasRole("USER")
                    .requestMatchers("/admin/pay").hasRole("ADMIN")
                    .requestMatchers("/admin/**").hasAnyRole("ADMIN", "SYS") // 여러 권한들을 넣을 수 있다.
                    .anyRequest().authenticated() // 인증된 사용자면 어떠한 요청에서도 허용
            );
        
        return http.build();
    }

    @Bean
    public void configure(AuthenticationManagerBuilder auth) throws Exception{
        auth.inMemoryAuthentication().withUser("user").password("{noop}1111").roles("USER");
        auth.inMemoryAuthentication().withUser("sys").password("{noop}1111").roles("SYS", "USER");
        auth.inMemoryAuthentication().withUser("admin").password("{noop}1111").roles("ADMIN", "SYS", "USER");
        //{noop} : 어떠한 암호화를 했는 지 알려주는 것을 prefix로 붙여주는 것이다.
        //{noop} 이 뭔지 설정을 안하면 "DelegatingPasswordEncoder" 클래스에서 default 값이 따로 있어서 자동으로 설정이 된다.
    }
}

```

### 2. 권한 설정의 자세한 설명

`authorizeHttpRequests` 에 설정한 auth 는 위부터 순서대로 적용이 된다.

그러기 때문에 설정 시 **구체적인 경로가 먼저오고 큰 범위의 경로가 뒤에 오도록** 해야 한다.


```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {

    http
        .authorizeHttpRequests(auth ->
            auth
                .requestMatchers("/").authenticated() //인증된 사용자의 접근을 허용
                .requestMatchers("/").fullyAuthenticated() //인증된 사용자(ID, PWD로 로그인한 유저)의 접근을 허용, rememberMe 인증 제외
                .requestMatchers("/").permitAll() //무조건 접근을 허용
                .requestMatchers("/").denyAll() //무조건 접근을 허용하지 않음
                .requestMatchers("/").anonymous() //익명사용자("ROLE_Anonymous" 권한을 가진 사용자만)의 접근을 허용
                .requestMatchers("/").rememberMe() //리멤버미를 통해 인증된 사용자의 접근을 허용
                .requestMatchers("/").hasRole("USER") //사용자가 주어진 역할이 있다면 접근을 허용 (prefix인 "ROLE_" 이 붙지않은)
                .requestMatchers("/").hasAuthority("ROLE_USER") //사용자가 주어진 권한이 있다면(prefix인 "ROLE_" 이 붙은)
                .requestMatchers("/").hasAnyRole("ADMIN", "SYS") //사용자가 주어진 권한이 있다면 접근을 허용(권한 여러 개 가능)
                .requestMatchers("/").hasAnyAuthority("ROLE_ADMIN", "ROLE_SYS") //사용자가 주어진 권한이 있다면 접근을 허용(권한 여러 개 가능)
        );
}

```

