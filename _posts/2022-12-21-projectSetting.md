---
layout: single
title:  "[JAVA] jakarta 와  javax"
categories: project  #카테고리
tag: [java, java17, jpa, queryDSL, jakarta, javax] #태그
toc: true  #오른쪽에 있는 목차
toc_sticky: true #목차 고정
author_profile: false  #왼쪽에 자기 소개란 프로필을 이 페이지에 들어갈때 끄는 기능
sidebar:
    nav: "docs" #navigation.yml에 있는 docs를 뜻한다.
search: true  #이 페이지는 검색에 나옴.
---

프로젝트를 하나 시작하게 되었다.

이번에는 배움을 위해서 프로젝트를 하는 것이기 때문에 최대한 최신버전을 사용하고, 요즘 많이 쓰는 툴들도 사용하려고 한다.

## spring project setting

< 내가 한 세팅 >
1. 우선 java 버전부터 11에서 17로 바꾸었다.
2. jpa
3. spring boot security
4. lombok
5. mysql
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