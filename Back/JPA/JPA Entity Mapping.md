# Entity 

# @Entity
- JPA가 관리하는 클래스로 Entity라고 한다.
- JPA를 사용해서 테이블과 맵핑할 클래스에 달아준다.

## 주의할 점
- <code>기본 생성자</code>가 필수이다.
   - 파라미터가 없는 public or protected 생성자를 사용해야 한다.
   - JPA spec으로, JPA를 구현해 사용하는 라이브러리들은 다양한 기술(ex) Reflection, ..)을 사용하기 때문에 객체를 프록시할 때 필요하다.
- final 클래스, enum, interface, inner 클래스는 Entity로 사용할 수 없다.
- DB에 저장하고 싶은 컬럼은 final을 붙이면 안된다.(final을 사용하면 안된다.)


## 속성 @Entity(name = "~")
- name
   - JPA에서 사용할 엔티티 이름을 지정한다.
   - default : 클래스 이름 사용

<hr>

# @Table
- 엔티티와 맵핑할 테이블을 지정하는 것이다.

## 속성 @Table(name = "~", )
- name
   - 맵핑할 테이블 이름을 지정한다.
   - default : 엔티티 이름 사용
- catalog
   - DB catalog 맵핑
- schema
   - DB schema 맵핑 
- uniqueConstraints(DDL)
   - DDL 생성 시에 유니크 제약 조건 생성

<hr>

# @Column
- 컬럼 맵핑

## 속성 @Column(...)
- name
   - 객체명과 DB 컬럼명을 다르게 하고 싶은 경우, DB 컬럼명으로 설정할 이름을 속성에 기입한다.
- insertable
   - 
- updatable
   - 컬럼을 수정했을 때 DB에 추가를 할 것인지 여부
   - update 문이 나갈 때 해당 컬럼을 반영할 것인지 여부
- nullable
   - true일 시 NOT NULL 제약 조건이 된다.
- unique
   - 잘 사용하지 않는다. --> 설정시 constraint 지맘대로이름만듬 unique(column) 처럼 마음대로 만든다.
   - 이름에 대한 설정을 직접하려면 아래의 방법을 사용해야 한다.
   - <code>@Table(uniqueConstraints = {@UniqueConstraint(name = "NAME_AGE_UNIQUE", columnNames = {"NAME", "AGE"})})</code>
- columnDefinition
   - 직접 컬럼 정보를 저장할 수 있다
   - <code>@Column(columnDefinition = "varchar(100) default 'EMPTY'")</code>
- length
   - 문자 길이 제약 조건
   - String 타입에만 사용한다.
- precision
   - 숫자가 엄청 큰 BigDecimal과 같은 경우(private BigDecimal age)

<hr>

# @Enumerated
- Enum Type 맵핑
   - Enum 객체 사용 시 해당 annotation 사용
   - DB에는 Enum Type이 없기 때문에 사용한다.
- EnumType.ORDINAL
   - enum에 저장된 변수 순서대로 DB에 저장(default) : 0, 1, 2, ..가 저장된다.
- EnumType.STRING
   - enum에 저장된 변수를 DB에 저장 : 변수 이름이 저장된다.

## 주의할 점
- ORDINAl은 사용하지 않는다.
- 이후 수정사항에 따라 새로운 TYPE이 추가될 수도 있을 뿐더러 순서가 변경되면 큰일나기 때문이다.
- 필수로 EnumType.STRING을 사용한다.

<hr>

# @Temporal
- 날짜 Type 맵핑
- DATE, TIME, TIMESTAMP 3가지 타입이 있다.
- 최신 하이버네이트는 JAVA에서 LocalDate, LocatDateTime을 사용할 경우 어노테이션이 없어도 지원해준다.

<hr>

# @Lob
- DB에 VARCHAR보다 더 큰 내용을 넣고 싶은 경우 사용
- @LOB에는 지정할 수 있는 속성이 없다.
- 맵핑하는 필드의 타입에 따라 DB의 BLOB, CLOB과 맵핑된다.
   - CLOB : 필드 타입이 문자열일 경우
     - String, char[], java.sql.CLOB
   - BLOB : 그 외 나머지ㅣ
     - byte[], java.sql.BLOB

<hr>

# @Transient
- 특정 필드를 컬럼에 맵핑하지 않겠다는 의미
- DB에 관계없이 메모리에서만 사용하고자 하는 객체에 사용

<hr>

# 기본 키 맵핑
- 직접 할당 : @Id만 사용
- 자동 생성 : @GeneratedValue, 기본 키 생성을 데이터베이스에 위임
   - IDENTITY : (MySQL의 auto_increment)
   - SEQUENCE : Oracle의 sequence
     - @SequenceGenerator 필요
   - TABLE : 모든 DB에서 사용
     - @TableGenerator 필요
   - AUTO : 방언에 따라 자동 지정, 기본값.

## IDENTITY 전략
- 기본 키 생성을 DB에 위임
- 주로 MySQL, PostgreSQL, SQL Server, DB에서 사용
- JPA는 보통 트랜잭션 커밋 시적에 INSERT SQL 실행
- AUTO_INCREMENT는 DB에 INSERRT SQL을 실행한 이후에 ID 값을 알 수 있음
- IDENTITY 전략은 em.persist() 시점에 즉시 INSERT SQL을 실행하고 DB에서 실별자를 조회한다.

## SEQUENCE 전략
- 데이터 시퀀스는 유일한 값을 순서대로 생성하는 특별한 DB 오브젝트이다.
- Oracle, PostgreSQL, DB2, H2 DB에서 사용

<hr>

# DB 스키마 자동 생성
```java
<property name="hibernate.hbm2ddl.auto" value="create"/>
```
- 애플리케이션 실행 시점(loading)에 자동으로 DDL을 생성한다.
- 이렇게 생성된 DDL은 개발 로컬 장비에서만 사용한다.
   - 즉, 운영 서버에서는 사용하지 않거나, 적절히 다듬은 후 사용한다.
   - 로컬 PC에서 개발할 때 사용한다.

## 주의할 점
- 운영 장비에는 절대로 create, create-drop, update를 사용하면 안된다.
- 개발 초기 단계(로컬)
   - create or update
- 테스트 서버(여러 명이 같이 사용하는 개발 서버)
   - update or validate
   - 웬만하면 테스트 서버도 validate or none을 사용하는 것을 권장한다.
   - 왜냐하면 update일 시 alter가 발생해 데이터가 많을 경우 락이 걸려 자칫 시스템이 중단될 수도 있기 때문이다.

- 스테이징과 운영 서버
   - validate or none

<hr>

## DDL 생성 기능(제약조건)

- DDL 생성 기능은 DDL을 자동 생성할 때만 사용되고 JPA의 실행 로직에는 영향을 주지 않는다.
   - DB에만 영향을 주는 것이지 애플리케이션에 영향을 주는 것이 아니다.(ex alter table 같은 스크립트)
   - insert, update는 Runtime에 영향을 준다.

<br>
<hr>
<br>

# 예제

```java
package hellojpa;

import javax.persistence.*;

@Entity
@SequenceGenerator( // SEQUENCE 전략
        name = "MEMBER_SEQ_GENERATOR",
        sequenceName = "MEMBER_SEQ", //맵핑할 db 시퀀스 이름
        initialValue = 1, allocationSize = 1)
public class MemberPK {
    
    // @ID : 직접 PK 할당, 내가 직접 id를 세팅할 떄(auto_increment 사용 x)
    // @GeneratedValue : 자동 생성 ex)auto_increment, sequence
    // IDENTITY : MYSQL / SEQUENCE : ORACLE
    @Id
//    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @GeneratedValue(strategy = GenerationType.SEQUENCE,
            generator = "MEMBER_SEQ_GENERATOR")
    private Long id;

    @Column(name = "name", nullable = false)
    private String username;

    public MemberPK(){};

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }
}

```



참조 : 인프런 김영한님 강의



