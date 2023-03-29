---
layout: single
title:  "[스프링으로 migration 툴 만들기] 5. Sequence 설정"
categories: 
    - spring  #카테고리
tag: [Spring, migration, setting, package, sequence] #태그
toc: true  #오른쪽에 있는 목차
toc_sticky: true #목차 고정
author_profile: true  #왼쪽에 자기 소개란 프로필을 이 페이지에 들어갈때 끄는 기능
sidebar:
    nav: "sidebar-category" #navigation.yml에 있는 docs를 뜻한다.
search: true  #이 페이지는 검색에 나옴.
---

이번 게시글은 Springboot JPA를 이용해 sequence를 처리하는 방법에 대해 적겠다. 

방법은 내가 알고있는 한에서는 2가지가 있다.

## @SequenceGenerator

JPA에서는 @SequenceGenerator 어노테이션을 사용하여 엔티티의 식별자 값으로 sequence를 지정할 수 있다.

@SequenceGenerator 어노테이션은 다음과 같은 속성을 가진다.

- name: JPA에서 사용할 시퀀스 생성기의 이름이다. @GeneratedValue 어노테이션의 generator 속성에 이 이름을 지정해야 한다.
- sequenceName: 데이터베이스에서 사용할 시퀀스의 이름이다. 이미 데이터베이스에 존재하는 시퀀스를 사용하려면 이 속성을 명시해야 한다.
- allocationSize: 시퀀스 값을 증가시키는 단위위다. 기본값은 50이다. 이 값은 데이터베이스의 시퀀스의 INCREMENT BY 값과 일치해야 한다.
  - 추가로, 이 사이즈는 데이터를 추가할 때 마다 저 사이즈만큼 미리 할당을 해서 가져온다. 왜냐하면, 트래픽이 많을 때 시퀀스가 겹치는 것을 방지하기 위함이다.
- initialValue: 시퀀스의 초기값이다. 기본값은 1이다.

예를 들어, 다음과 같은 엔티티 클래스가 있다고 가정해본다.

```java
@Entity
public class Member {

@Id
@GeneratedValue (strategy = GenerationType.SEQUENCE, generator = "member_seq")
@SequenceGenerator (name = "member_seq", sequenceName = "MEMBER_SEQ", allocationSize = 1)
private Long id;

private String name;

//constructor, getter, setter, toString 생략
}
```

Member 엔티티를 저장할 때, 데이터베이스의 MEMBER_SEQ라는 이름의 시퀀스를 호출하여 id 값을 얻게 된다.

데이터베이스에 이미 시퀀스가 존재하지 않는다면, JPA는 SchemaGeneration 기능을 통해 자동으로 시퀀스를 생성해준다. 하지만, SchemaGeneration 기능을 사용하지 않는다면, 직접 SQL문을 통해 시퀀스를 생성해야 한다.

예를 들어, Oracle 데이터베이스에서는 다음과 같은 SQL문을 사용하여 시퀀스를 생성할 수 있다.

```sql
CREATE SEQUENCE MEMBER_SEQ START WITH 1 INCREMENT BY 1;
```

## IdentifierGenerator를 이용해 sequence 재정의하는 방법

DB에 시퀀스가 있지만, 그 시퀀스 그대로 쓰는 게 아니라 좀 더 다채롭게 바꾸고 싶을 때 재정의 하는 방법이다.
예를 들어, 시퀀스 앞에 년월일을 붙여서 seq = 년월일+seq 이렇게 만들 수도 있다. 

```java
public class CustomIdentifierGenerator implements IdentifierGenerator {
    @Override
    public Serializable generate(SharedSessionContractImplementor session, Object object) throws HibernateException {
        String prefix = "EMP";
        Connection connection = session.getJdbcConnectionAccess().obtainConnection();
        try {
            PreparedStatement ps = connection.prepareStatement("SELECT nextval ('employee_seq')");
            ResultSet rs = ps.executeQuery();
            if (rs.next()) {
                int id = rs.getInt(1);
                String code = prefix + id;
                return code;
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
        return null;
    }
}
```

그런 다음 엔티티 클래스에서 다음과 같이 이 사용자 정의 식별자 생성기를 사용할 수 있다.

```java
@Entity
@Table(name = "employee")
public class Employee {
    @Id
    @GeneratedValue(generator = "emp_seq")
    @GenericGenerator(name = "emp_seq", strategy = "com.example.demo.CustomIdentifierGenerator")
    private String id;
    ...
}
```

이 예제에서는 직원 엔티티에 대한 고유 식별자를 생성하는 사용자 정의 식별자 생성기를 만들었다. 식별자는 `employee_seq`라는 시퀀스를 쿼리하여 `EMP` 접두사와 결합하여 생성된다. 이 사용자 정의 식별자 생성기는 `@GenericGenerator` 어노테이션의 `strategy` 속성에 대한 값으로 지정하여 `Employee` 엔티티 클래스에서 사용된다.

