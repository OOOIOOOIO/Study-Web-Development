# JPA(Java Persistenc API)
> &nbsp;자바 진영에서 ORM(Object-Relational Mapping) 기술 표준으로 사용되는 인터페이스의 모음이다.
> JPA를 구현한 대표적인 오픈소스(구현체)로 Hibernate, EclipseLink 등이 있다.<br>
> &nbsp;JPA는 특정 데이터베이스에 종속되지 않고 다양한 데이터베이스 방언을 설정할 수 있다.

<br>

## JPA 사용 이유
> &nbsp;JPA는 반복적인 CRUD SQL을 처리해준다. JPA는 매핑된 관계를 이용해서 SQL을 생성하고 실행하는데, 개발자는 어떤 SQL이 실행될지 생각만하면 되고, 예측도 쉽게 할 수 있다. 
> 추가적으로 JPA는 네이티브 SQL이란 기능을 제공해주는데 관계 매핑이 어렵거나 성능에 대한 이슈가 우려되는 경우 SQL을 직접 작성하여 사용할 수 있다.
> JPA를 사용하여 얻을 수 있는 가장 큰 것은 SQL아닌 객체 중심으로 개발할 수 있다는 것이다. 이에 따라 당연히 생산성이 좋아지고 유지보수도 수월하다. 
> 또한 JPA는 객체와 쿼리문에 대한 패러다임의 불일치도해결하였다. 예를 들면 JAVA에서는 부모클래스와 자식클래스의 관계 
> 즉, 상속관계가 존재하는데 데이터베이스에서는 이러한 객체의 상속관계를 지원하지 않는다.(상속 기능을 지원하는 DB도 있지만 객체 상속과는 다름). 

<br>

## JAP 구동 방식

![image](https://user-images.githubusercontent.com/74396651/197495190-79560de4-50f3-41ca-844c-e726468840b4.png)


<br>
<hr>
<br>

## ORM(Object-Relational Mapping)
> &nbsp;자바 애플리케이션의 Class와 RDB(Relational DataBase)의 테이블을 맵핑(연결)해주는 역할을 하며 기술적으로는 
> 애플리케이션의 객체를 RDB 테이블에 자동으로 영속화 해주는 것이라고보면 된다.<br>

<br>

### ORM 장점
- SQL문이 아닌 Method를 통해 DB를 조작할 수 있어, 개발자는 객체 모델을 이용하여 비즈니스 로직을 구성하는데만 집중할 수 있다.
   - (내부적으로는 쿼리를 생성하여 DB를 조작함. 하지만 개발자가 이를 신경 쓰지 않아도됨)
- Query와 같이 필요한 선언문, 할당 등의 부수적인 코드가 줄어들어, 각종 객체에 대한 코드를 별도로 작성하여 코드의 가독성을 높다.
- 객체지향적인 코드 작성이 가능하다. 오직 객체지향적 접근만 고려하면 되기때문에 생산성 증가한다.
- 매핑하는 정보가 Class로 명시 되었기 때문에 ERD를 보는 의존도를 낮출 수 있고 유지보수 및 리팩토링에 유리하다.
- 예를들어 기존 방식에서 MySQL 데이터베이스를 사용하다가 PostgreSQL로 변환한다고 가정해보면, 새로 쿼리를 짜야하는 경우가 생김. 이런 경우에 ORM을 사용한다면 쿼리를 수정할 필요가 없다.

<br>

## ORM 단점
- 프로젝트의 규모가 크고 복잡하여 설계가 잘못된 경우, 속도 저하 및 일관성을 무너뜨리는 문제점이 생길 수 있다.
- 복잡하고 무거운 Query는 속도를 위해 별도의 튜닝이 필요하기 때문에 결국 SQL문을 써야할 수도 있다.
- 학습비용이 비싸다.

<br>

## 스키마 자동 생성
- JPA는 DB 스키마를 자동으로 생성하는 기능을 지원한다. 클래스의 매핑 정보를 분석하여 어떤 테이블이 어떤 컬럼을 사용하는지 알 수 있고 방언(dialect)에 따라 해당 DB에 맞는 스키마를 생성할 수 있다.
- 방언
    - H2: org.hibernate.dialect.H2Dialect
    - oracle 10g: org.hibernate.dialect.Oracle10gDialect
    - MySQL: org.hibernate.dialect.MySQL5InnoDBDialect
- 하지만 맹목적으로 믿으면 안되고 수정을 거쳐 목적에 맞게 스키마를 작성해야 한다.

<br>

### 설정하기
- application.properties
```java
spring.jpa.hibernate.ddl-auto=create
```

- application.yml
```java
spring:
  jpa:
    hibernate:
      ddl-auto: create
    # show-sql: true # DDL 출력
```
    - spring.jpa.hibernate.ddl-auto 속성
    - create: 기존 테이블을 삭제하고 새로 생성 (DROP - CREATE)
    - create-drop: create와 동일하나 어플리케이션 종료시 테이블 삭제 (DROP - CREATE - DROP)
    - update: 데이터베이스 테이블 - 엔터티 매핑정보를 비교해서 변경사항만 수정
    - validate: 데이터베이스 테이블 - 엔터티 매핑정보를 비교해서 차이가있으면 어플리케이션을 실행하지 않음
    - none: 자동 생성 기능을 사용하지 않음
    
### local에서 test 및 개발 단계에서는 create, update를 사용할 수 있지만, 실제 서비스, 베포 단계에서는 validate, none을 사용해야 한다. none을 권장한다.
