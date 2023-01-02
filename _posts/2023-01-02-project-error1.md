---
layout: single
title:  "[JAVA] JPA missing table error"
categories: PROJECT  #카테고리
tag: [JAVA, JPA, eeror] #태그
toc: true  #오른쪽에 있는 목차
toc_sticky: true #목차 고정
author_profile: true  #왼쪽에 자기 소개란 프로필을 이 페이지에 들어갈때 끄는 기능
sidebar:
    nav: "docs" #navigation.yml에 있는 docs를 뜻한다.
search: true  #이 페이지는 검색에 나옴.
---

```plaintext
org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'entityManagerFactory' defined in class path resource [org/springframework/boot/autoconfigure/orm/jpa/HibernateJpaConfiguration.class]: Invocation of init method failed; nested exception is jakarta.persistence.PersistenceException: [PersistenceUnit: default] Unable to build Hibernate SessionFactory; nested exception is org.hibernate.tool.schema.spi.SchemaManagementException: Schema-validation: missing table [xxx]
```

프로젝트 설정을 하고 디버깅을 돌렸더니 이런 에러가 떴다.

해결 방법을 알아보고 여러가지를 바꾸고 해결할 수 있게 되었다.

## 에러가 생긴 이유
일단, 이렇게 에러가 된 이유는 

***mysql을 쓰려고 했던 설정을 mariDB로 바꾸면서 그 설정을 바꾸지 않아서 생긴 일이다.***

## 에러 해결 방법
1. 일단 entity 파일에서 @Table(name="TB_USER") 부분에서 name을 소문자로 바꾸었다. "tb_user"
2. application.yml 파일에서 
   
   ```plaintext
   spring:
    jpa:
      properties:
        hibernate:
            dialect: org.hibernate.dialect.MariaDB103Dialect
   ```
   이 방언 부분에서 mysql로 적혀있었다. 그래서 되지 않았던 것이다.
   mariaDB용으로 바꾸었다. 


그랬더니 디버깅이 잘 되었다! 

해결 완료!!