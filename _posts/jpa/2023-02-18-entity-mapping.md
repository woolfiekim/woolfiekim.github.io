---
layout: single
title:  "[JPA] 엔티티매핑"
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

## 엔티티 매핑 어노테이션
- 객체와 테이블 매핑 : @Entity, @Table
- 필드와 컬럼 매핑 : @Column
- 기본 키 매핑 : @Id
- 연관관계 매핑 : @ManyToOne, @JoinColumn

## 객체와 테이블 매핑

### @Entity
- JPA가 관리하는 엔티티이다.(테이블과 매핑하는 클래스) 
- 기본 생성자 필수
- final, enum, interface, inner 클래스 사용 불가
- 저장할 필드에 final 사용 불가

### @Table
- 엔티티와 매핑할 테이블 지정
- (name = "") : 매핑할 테이블 이름
- (schema) : DB 스키마 매핑
- (uniqueConstraints) : DDL 생성 시에 유니크 제약 조건 생성

> DDL : "Data Definition Language"의 약어로, 데이터베이스에서 데이터 구조를 정의하거나 조작하는 데 사용되는 SQL(Structured Query Language)의 하위 집합이다. CREATE, ALTER, DROP 등의 SQL 명령어로 구성한다.

- DB 스키마 자동 생성의 속성
  persistence.xml에서 
  ```xml
    <property name="hibernate.hbm2ddl.auto" value="validate" />
  ```
    이렇게, 쓸 수 있다.
    - create : 기존 테이블 삭제 후 다시 생성(drop, create)
    - create-drop : 종료시점에 테이블 drop
    - update : 변경분만 반영
    - validate : 엔티티와 테이블이 정상 매핑되었는지만 확인
    - none: 사용하지 않음

**운영장비에는 create, create-drop, update 절대 사용 불가!!**
- create or update : 개발 초기단계
- update or validate : 테스트 서버
- validate or none : 스테이징과 운영서버

> 스키마 : 데이터베이스는 정보를 구조적으로 저장하기 때문에, 그 구조를 먼저 정해야 한다. 이것이 스키마이다. 스키마는 데이터베이스의 뼈대를 정의하고, 그 안에서 각 데이터가 어떻게 구성되는지를 정의한다. 예를 들어, 옷의 정보를 저장하는 데이터베이스에서 스키마는 어떤 항목(예: 브랜드, 색상, 사이즈 등)을 저장할지를 정의하고, 각 항목의 데이터 타입을 지정한다. 이렇게 스키마를 정의하면 데이터베이스에서 정보를 구조적으로 저장하고, 검색할 때 필요한 정보만 추출하여 사용할 수 있다.

- 매핑 어노테이션
    - @Column : 컬럼 매핑
      - ex) @Column(name="name)
      - insertable, updatable : 등록, 변경 가능 여부 (기본값 : TRUE)
      - name : 필드와 매핑할 테이블의 컬럼 이름
      - nullable : null 값 허용여부설정
      - unique : 유니크 제약조건 걸기
      - columnDefinition : DB 컬럼 정도 직접 제공 > varchar(10) default 'YES' > 방언에 맞게
      - length : 길이 제약조건 (String에만 사용, 기본값 : 255)
    - @Temporal : 날짜 타입 매핑
      - ex) @Temporal(TemporalType.TIME) > 타입은 3가지가 있다.
      - DATE : 날짜 / TIME : 시간 / TIMESTAMP : 날짜와 시간
      - 최신 하이버네이트에서는 이 어노테이션을 쓸 필요가 없다. 
      - `private LocalDate localdate;`(년월일) 나 `private LocalDateTime localdatetime;`(년월일시분초) 이렇게 쓰면 알아서 시간으로 매핑해주기 때문에 @Temporal을 쓸 필요가 없다. 
    - @Enumerated : enum 타입 매핑
      - ex) @Enumerated(EnumType.STRING) > enum 이름을 DB에 저장
      - 기본값 : EnumType.ORDINAL(enum 순서를 DB에 저장)
      - ***ORDINAL 로 사용하지 말자! 왜냐하면 enum 순서를 중간에 바꿔버리면 바꾸기 전 후의 값이 바뀌어 버린다. 그러기 때문에 순서 말고 이름을 저장하는 STRING 을 쓰자.***
    - @Lob : BLOB, CLOB 매핑
    - @Transient : 특정 필드를 컬럼에 매핑하지 않음(매핑 무시)

## 기본 키 매핑

- @Id 
- @GeneratedValue : 값을 자동으로 할당(시퀀스)
  - (strategy = GenerationType.AUTO) : 방언에 따라 자동 지정
  - (strategy = GenerationType.IDENTITY) : DB에 위임. insert sql 실행 후 DB에서 식별자 조회
  - (strategy = GenerationType.SEQUENCE) : DB 시퀀스 오브젝트를 사용

```java
@Entity
@Table(name = "tb_MATERIAL")
@SequenceGenerator(name = "tbMaterialSeq", sequenceName = "TB_MATERIAL_SEQ", allocationSize = 1)
public class TbMaterial {

    @Id
    @GeneratedValue(strategy = GenerationType.SEQUENCE, generator = "tbMaterialSeq")
    private Long id;

    @Column(name = "CLIP_YN", length = 1, columnDefinition = "CHAR")
    private String clipYn;
}
```

- ***결론 : Long 형 + 대체키(seq, uuid) + 키 생성전략 사용***

----




> 본 포스팅은 김영한 선생님의 강의를 보고 정리한 글입니다.