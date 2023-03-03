---
layout: single
title:  "[JPA] 고급매핑"
categories: 
    - jpa  #카테고리
tag: [jpa, entity] #태그
toc: true  #오른쪽에 있는 목차
toc_sticky: true #목차 고정
author_profile: true  #왼쪽에 자기 소개란 프로필을 이 페이지에 들어갈때 끄는 기능
sidebar:
    nav: "sidebar-category" #navigation.yml에 있는 docs를 뜻한다.
search: true  #이 페이지는 검색에 나옴.
---

## 1. 상속관계 매핑

- 관계형 DB는 상속 관계 못함.
- 실제 물리 모델로 구현하는 방법
  - 조인전략
  - 단일 테이블 전략
  - 구현 클래스마다 테이블 전략 (추천하지않음)

- 주요 어노테이션
1. @Inheritance(strategy = InheritanceType.XXX)
    - JOINED : 조인전략
    - SINGLE_TABLE : 단일 테이블 전략
    - TABLE_PER_CLASS : 구현 클래스마다 테이블 전략

2. @DiscriminatorColumn(name = "dtype")
    - 조인할 때 하위 테이블의 이름들을 dtype이라는 컬럼에 넣는다. 
    - dtype은 다른 컬럼 명으로 바꿀 수 있다.
    - 이 어노테이션을 안 쓸 때 DTYPE 이 default 값이다.

3. @DiscriminatorValue("A")
    - 2번에서 dtype 컬럼 안에 넣어지는 테이블 이름을 바꾸는 것이다.
    - A 말고 다른 걸로 바꿀 수 있다.


```java
@Entity
@Inheritance(strategy = InheritanceType.JOINED)
@DiscriminatorColumn(name = "dtype")
@Getter @Setter
public abstract class Item {
    //abstract : 추상클래스를 적용하면 Item 테이블이 만들어지지 않는다.
    //Item 테이블을 만들고 싶으면 astract을 빼라.

    @Id
    @GeneratedValue
    @Column(name = "item_id")
    private Long id;

    private String name;
    private int price;

}
```

여기서 item 테이블을 실제로 조회하면 컬럼은 4개가 나온다.

id, name, price 그리고 dtype

```java

@Entity
@Getter
@Setter
@DiscriminatorValue("A")
public class Album extends Item {

    private String artist;
    private String etc;
}
```

Album 테이블에 값이 넣어질 때, item 테이블의 dtype컬럼에 Album이라는 값이 들어가는게 아니라 A가 들어간다.

> 본 포스팅은 김영한 선생님의 강의를 보고 정리한 글입니다. 