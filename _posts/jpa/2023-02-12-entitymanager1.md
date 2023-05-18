---
layout: single
title:  "[JPA] 영속성 컨텍스트"
categories: 
    - jpa  #카테고리
tag: [jpa, entitymanager] #태그
toc: true  #오른쪽에 있는 목차
toc_sticky: true #목차 고정
author_profile: true  #왼쪽에 자기 소개란 프로필을 이 페이지에 들어갈때 끄는 기능
sidebar:
    nav: "sidebar-category" #navigation.yml에 있는 docs를 뜻한다.
search: true  #이 페이지는 검색에 나옴.
---

## 1. 영속성 컨텍스트(PersistenceContext)

뜻 : 엔티티를 영구 저장하는 환경

### 1. 엔티티의 생명주기

- 비영속 (new/transient)
    - 영속성 컨텍스트와 전혀 관계가 없는 **새로운** 상태
- 영속 (managed)
    - 영속성 컨텍스트에 **관리**되는 상태
- 준영속 (detached)
    - 영속성 컨텍스트에 저장되었다가 **분리**된 상태
- 삭제 (removed)
    - **삭제**된 상태

![entityLifeCycle.png](/assets/images/2023/02/12/entityLifeCycle.png)

```java
EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");
EntityManager em = emf.createEntityManager();
EntityTransaction tx = em.getTransaction();
tx.begin();

//1. 객체를 생성한 상태(비영속)
Member member = new Member();
member.setId("member1");

//2. 객체를 저장한 상태(영속)
EntityManager em = emf.createEntityManager();
em.getTransaction().begin();
em.persist(member); // << 이부분이 객체를 저장한 상태
//이 때, db에 데이터가 저장되지 않는다. 
tx.commit(); // << 여기서 db로 저장이 된다.

//3. 회원 엔티티를 영속성 컨텍스트에서 분리, 준영속 상태
em.detach(member);

//4. 객체를 삭제한 상태(삭제)
em.remove(member);
```

### 2. 영속성 컨텍스트의 이점
- 1차 캐시
- 동일성 보장
- 트랜잭션을 지원하는 쓰기 지연
- 변경 감지
- 지연 로딩

1. 1차 캐시
- 한 번 조회한 값을 1차 캐시에 저장해서 똑같은 값을 조회 시 캐시에서 조회한다.

2. 동일성 보장
- 1차 캐시저 저장된 값을 읽어 올 때는 그 둘이 같다는 동일성을 보장한다.

3. 트랜잭션을 지원하는 쓰기 지연
```java
EntityManager em = emf.createEntityManager();
EntityTransaction transaction = em.getTransaction();
//엔티티 매니저는 데이터 변경시 트랜잭션을 시작해야 한다.

transaction.begin(); // [트랜잭션] 시작

em.persist(memberA);
em.persist(memberB);
//여기까지 INSERT SQL을 데이터베이스에 보내지 않는다.

transaction.commit(); //트랜잭션 커밋
```

4. 변경 감지
```java
EntityManager em = emf.createEntityManager();
EntityTransaction transaction = em.getTransaction();
transaction.begin(); // [트랜잭션] 시작

// 영속 엔티티 조회
Member memberA = em.find(Member.class, "memberA");

// 영속 엔티티 데이터 수정
memberA.setUsername("hi");
memberA.setAge(10);

transaction.commit(); // [트랜잭션] 커밋
```
조회한 데이터에서 값이 변경됨을 감지해서 그 부분을 업데이트 해준다. 

```java
Member memberA = em.find(Member.class, "memberA");
em.remove(memberA); //엔티티 삭제
```



> 본 포스팅은 김영한 선생님의 강의를 보고 정리한 글입니다.