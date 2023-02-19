---
layout: single
title:  "[JPA] 연관관계 매핑"
categories: 
    - jps  #카테고리
tag: [jpa, entity] #태그
toc: true  #오른쪽에 있는 목차
toc_sticky: true #목차 고정
author_profile: true  #왼쪽에 자기 소개란 프로필을 이 페이지에 들어갈때 끄는 기능
sidebar:
    nav: "sidebar-category" #navigation.yml에 있는 docs를 뜻한다.
search: true  #이 페이지는 검색에 나옴.
---

## 1. 단방향 연관관계

Member : Team = N : 1

여기서 외래키가 있는 곳은 Member이다. 

외래키가 있는 곳을 주인으로 정하기 때문에 

아래 예시에서 Member.team이 연관관계의 주인이다.


```java
@Entity
public class Member {

    @Id
    @GeneratedValue
    private Long id;

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "TEAM_ID")
    private Team team;

}

@Entity
public class Team {

    @Id
    @GeneratedValue
    @Column(name = "TEAM_ID")
    private Long id;

}
```
```java
//팀 저장
Team team = new Team();
team.setName("TeamA"); 
em.persist(team);

//회원 저장
Member member = new Member(); 
member.setName("member1");
member.setTeam(team); 
em.persist(member);

//조회
Member findMember = em.find(Member.class, member.getId());
//참조를 사용해서 연관관계 조회
Team findTeam = findMember.getTeam();

// 새로운 팀B
Team teamB = new Team(); 
teamB.setName("TeamB"); 
em.persist(teamB);
//회원1의 팀B로 수정
member.setTeam(teamB);
```

## 2. 양방향 연관관계 / 연관관계 주인

Member : Team = N : 1

테이블에서는 FK만 있으면 서로 조인하면서 불러올 수 있다.(양방향, 연관관계 1개)

하지만 객체에서는 그런게 없기 때문에 Team entity에도 무언가를 주어야한다.

서로 다른 단방향이 2개, 연관관계가 2개이다.


```java
@Entity
public class Team {

    @Id
    @GeneratedValue
    @Column(name = "TEAM_ID")
    private Long id;

    @OneToMany(mappedBy = "team") //mappedBy는 무엇과 연결 되어있는지 알려주는 것이다.
    private List<Member> members = new ArrayList<>();

}
```
```java
Member findMember = em.find(Member.class, member.getId());
List<Member> members = findMember.getTeam().getMembers(); //역방향 조회
```

연관관계의 주인만 외래키를 관리한다. (등록, 수정)

**= 외래키가 있는 곳이 주인!!**

mappedBy를 쓰는 쪽은 주인이 아니며, 읽기만 가능하다.

```java
team.getMembers().add(member); 
//이 코드는 실행이 되지 않는다.
//연관관계의 주인이 아니기 때문에 등록이 불가하다.
member.setTeam(team);
//이렇게 연관관계의 주인인 member쪽에서 등록이 가능하다.
```


> 본 포스팅은 김영한 선생님의 강의를 보고 정리한 글입니다.


