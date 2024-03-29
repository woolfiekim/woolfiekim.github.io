---
layout: single
title:  "[Spring] project setting"
categories: 
    - project  #카테고리
tag: [java, java17, jpa, queryDSL, spring security, lombok, build.gradle] #태그
toc: true  #오른쪽에 있는 목차
toc_sticky: true #목차 고정
author_profile: true  #왼쪽에 자기 소개란 프로필을 이 페이지에 들어갈때 끄는 기능
sidebar:
    nav: "sidebar-category" #navigation.yml에 있는 docs를 뜻한다.
search: true  #이 페이지는 검색에 나옴.
---

시작하는 프로젝트의 세팅을 보여주겠다.

## spring project setting

< 내가 한 세팅 >
1. 우선 java 버전부터 11에서 17로 바꾸었다.
2. jpa
3. spring boot security
4. lombok
5. mariaDB
6. p6spy
7. Apache commons
8. queryDSL
9. oAuth2
10. war
11. restDocs

위 11개 중에 1, 5, 9, 10 번은 한번도 써보지 못한 세팅이다.

그리고 11번은 딱 한번 써보았으며 3, 8번은 여러 번 써보았지만 공부가 부족하다.

## build.gradle

```plaintext
plugins {
    id 'java'
    id 'war'
    id 'org.springframework.boot' version '3.0.0'
    id 'io.spring.dependency-management' version '1.1.0'
    id 'org.asciidoctor.convert' version '1.5.8'
}

group = 'com.sub'
version = '0.0.1-SNAPSHOT'
sourceCompatibility = '17'

configurations {
    compileOnly {
        extendsFrom annotationProcessor
    }
}

repositories {
    mavenCentral()
}

ext {
    queryDslVersion = "5.0.0"
}

dependencies {
    //jpa
    implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
    //spring security
    implementation 'org.springframework.boot:spring-boot-starter-security'
    implementation 'org.springframework.boot:spring-boot-starter-web'
    //lombok
    compileOnly 'org.projectlombok:lombok'
    developmentOnly 'org.springframework.boot:spring-boot-devtools'
    runtimeOnly 'org.mariadb.jdbc:mariadb-java-client'
    annotationProcessor 'org.springframework.boot:spring-boot-configuration-processor'
    annotationProcessor 'org.projectlombok:lombok'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
    //restDocs
    testImplementation 'org.springframework.restdocs:spring-restdocs-mockmvc'
    testImplementation 'org.springframework.security:spring-security-test'

    //p6spy
    implementation 'com.github.gavlyukovskiy:p6spy-spring-boot-starter:1.8.1'

    //apache commons
    implementation 'org.apache.commons:commons-lang3:3.12.0'

    //queryDSL
    implementation("com.querydsl:querydsl-jpa:${queryDslVersion}")
    annotationProcessor("com.querydsl:querydsl-apt:${queryDslVersion}:jakarta")
    testAnnotationProcessor("com.querydsl:querydsl-apt:${queryDslVersion}:jakarta")

    //oAuth2
    implementation 'org.springframework.boot:spring-boot-starter-oauth2-client:3.0.0'
    annotationProcessor 'jakarta.persistence:jakarta.persistence-api'
    annotationProcessor 'jakarta.annotation:jakarta.annotation-api'

}

test {
    useJUnitPlatform()
}
```