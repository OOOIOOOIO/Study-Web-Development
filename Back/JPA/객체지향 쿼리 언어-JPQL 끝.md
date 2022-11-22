# 객체지향 쿼리 언어 - JPQL(Java Persistence Query Language)

## 배경
- JPA를 사용하면 엔티티 객체를 중심으로 개발한다.
- 단순한 쿼리는 괜찮지만 문제는 검색 쿼리이다.
- 검색할 때도 테이블이 아닌 엔티티 객체를 대상으로 검색한다.
- 모든 DB 데이터를 객체로 변환해서 검색하는 것은 불가능하다.
- 필요한 데이터를 DB에서 불러오려면 결국 검색 조건이 포함된 SQL이 필요하다.

<br>

## JPQL(Java Persistence Query Language)
- JPA는 SQL을 추상화한 JPQL이라는 객체 지향 쿼리 언어를 제공한다.
- SQL과 문법이 유사하며 SELECT, FROM, WHERE, GROUP BY, HAVING, JOIN 등을 지원한다.
- JPQL은 엔티티 객체를 대상으로 쿼리를 수행한다.(SQL은 DB 테이블을 대상으로 쿼리를 수행한다.)
- SQL을 추상화하기 때문에 특정 데이터베이스 SQL에 의존하지 않는다.
- JPQL은 결국 SQL로 변환된다.

![image](https://user-images.githubusercontent.com/74396651/200510079-047ca048-a154-4c3b-b4cf-16105739703d.png)

![image](https://user-images.githubusercontent.com/74396651/200511302-45e2d792-2602-4deb-891e-a36853c9a12e.png)

<br>

## 문법
- 엔티티와 속성은 대소문자를 구분한다.
- JPQL 키워드는 대소문자를 구분하지 않는다.
   - select m from Member as m where m.age > 18 
- 테이블 이름이 아닌 엔티티 이름을 사용한다.(별도로 엔티티 이름을 지정하지 않았다면 클래스 이름이 테이블 이름이다.)
- 별칭(alias)은 필수이다.(as는 생략 가능하지만 권장)

- 구조
![image](https://user-images.githubusercontent.com/74396651/200513945-bdc37b86-820c-468c-a64f-4f8e773e4e51.png)

<br>

## 집합과 정렬
- count, sum, avg, max, min 
- GROUP BY, HAVING
- ORDER BY
- 등 다 지원한다.
![image](https://user-images.githubusercontent.com/74396651/200512526-99fcafca-e2fb-42e0-bbf0-62b9b76bb879.png)

<br>

## TypeQuery, Query
- TypeQuerty : 반환 타입이 명확할 때 사용한다.
- Querty : 반환 타입이 명확하지 않을 때 사용한다.

![image](https://user-images.githubusercontent.com/74396651/200512963-4c3bcc9b-bd0d-421f-ba58-629dcf3e35b0.png)

## 결과 조회 API
- query.getResultList() : 결과가 하나 이상일 때 사용하며 리스트로 반환
   - 결과가 없다면 빈 리스트로 반환한다. 
- quert.getSingResult() : 결과가 정확히 하나일 때 사용하며 단일 객체로 반환
   - 결과가 없으면, javax.persistence.NoResultException
   - 둘 이이면, javax.persistence.NonUniqueResultException 을 던진다.

<br>

## 파라미터 바인딩
- 이름 기준
![image](https://user-images.githubusercontent.com/74396651/200513709-a62e5ea2-5d26-48a7-88c7-e8c6c06e0971.png)

<br>

- 위치 기준
![image](https://user-images.githubusercontent.com/74396651/200513741-1d1a20ea-02bd-49b6-b35c-44ddcda99d3a.png)

<br>

## 프로젝션
- SELECT 절에 조회할 대상을 지정하는 것
- DISTINCT로 중복을 제거할 수 있다.
- 프로젝션 대상
   - 엔티티
   - 임베디드 타입
   - 스칼라 타입(숫자, 문자 등 기본 데이터 타입)
- 엔티티 프로젝션
   - select m from Memer m 
   - select m.team from Member m 
- 임베디드 타입 프로젝션
   - select m.address from Member m
- 스칼라 타입 프로젝션
   - select m.name, m.age from Member m

<br>

## 프로젝션 - 여러 값 조회
- Query 타입으로 조회
- Object[] 타입(배열)으로 조회
- new 명령어로 조회
    - 단순 값을 DTO로 바로 조회할 때 사용한다.
    - 패키지 명을 포함해 전체 클래스 명을 입력해야 한다.
    - 순서와 타입이 일치하는 생성자가 필요하다.
    - ex) select new jpabook.jpql.UserDTO(m.username, m.age) from Member b

<br>

## 페이징 API
- JPA는 페이징을 두 API로 추상화하여 제공한다.
    - setFirstResult(int startPosition) : 조회 시작 위치(0부터 시작)
    - setMaxResult(int maxResult) : 조회할 데이터 수 

```java
// 페이징 쿼리 0 ~ 19(20) 조회
String jpql = "select m from Member m order by m.age desc";

List<Member> resultList = em.createQuery(jpql, Member.class)
                        .setFirstResult(0)
                        .setMaxResults(19)
                        .getResultList();
```

<br>

## Join(조인)
- 내부 조인
    - select m from Member m join m.team t
    - select t from Team t where t.team_id = m.id 쿼리가 나간다!!!(N+1문제)
- 외부 조인
    - select m from Member m left join m.team t
- 세타 조인
    - select count(m) from Member m, Team t where m.username = t.name

<br>

## Join - ON 절
- JPA2.1부터 ON절 지원
- 조인 대상 필터링(조건)
```java
ex) 회원과 팀을 조인하면서, 팀 이름이 A인 팀만 조인
// JPQL
select m, t from Member m left join m.team t on t.name='A'

// SQL
select m.*, t.* 
from Member m 
left join Team t on m.TEAM_ID = t.id and t.name='A'
```
<br>
- 연관관계 없는 엔티티 외부 조인(하이버네이트 5.1부터)
```java
ex) 회원의 이름과 팀의 이름이 같은 대상 외부 조인
// JPQL
select m, t from Member m left join Team t on m.username = t.name

// SQL
SELECT m.*, t.* 
FROM Member m
LEFT JOIN Team t 
ON m.username = t.name
```

<br>

## 서브 쿼리
- JPA의 WHERE, HAVING 절에서만 사용 가능
- SELECT 절도 가능(하이버네트에서 지원)
- FROM 절의 서브쿼리는 불가능 --> JOIN으로 풀 수 있으면 풀어서 해결
- 지원 함수
    - NOT EXITS / EXISTS( subquerty ) : 서브 쿼리에 결과가 존재하면 참
    - ALL, ANY, SOM ( subquery)
       - ALL : 모두 만족하면 참
       - ANY, SOME : 조건을 하나라도 만족하면 참
    - NOT IN / IN ( subquery ) : 서브쿼리의 결과 중하나라도 같은 것이 있으면 참

```java
// JPQL 예시
// ex) 팀 A 소속인 회원
select m from Member m
where exists (select t from Team t where t.name='팀A')

// ex) 전체 상품 각각의 재고보다 주문량이 많은 재품들
select o from Orders o
where o.orderAmount > ALL (select p.stockAmount from Product p)

// ex) 어떤 팀이든 팀에 소속된 회원
select m from Member m
where m.team = ANY (select t from Team t)

```

<br>

## JPQL 타입 표현 및 기타 표현
- 문자
    - 'hello', 'she"s'
- 숫자
    - 10L, 10D, 10F
- Boolean
    - TRUE, FALSE
- ENUMM
    - jpabook.MemberType.Admin(패키지명 포함)
- 엔티티 타입
    - TYPE(m) = Member(상속 관계에서 사용)
- SQL과 문법이 같은 식 지원
    - EXISTS, IN
    - AND, OR, NOT
    - =, <=, >=, <, >
    - BETWEEN, LIKE, IS NULL

<br>

## 조건식 - CASE
![image](https://user-images.githubusercontent.com/74396651/200992133-0c6d4846-d350-4ae3-9d8d-7a87f2b8d362.png)

<br>

- COALESCE : 하나씩 조회해 null이 아니면 반환
- NULLIF : 두 값이 같으면 NULL 반환, 다르면 첫번째 값 반환

![image](https://user-images.githubusercontent.com/74396651/200992246-a8202f91-c6ef-4305-a6ad-b57e045a7e5e.png)

<br>

## JPQL 기본 함수
- CONCAT
- SUBSTRING
- TRIM
- LOWER, UPPER
- LENGTH
- LOCATE
- ABS, SQRT, MOD
- SIZE, INDEX(JPA 용도) 
- #### 사용자 정의 함수
    - 하이버네이트는 사용 전 방언에 추가해야 한다.
    - 사용하는 DB 방언을 상속받고 사용자 정의 함수를 등록한다.
    - ex) select function('group_concat', i.name) from Item i

<br>

## JPQL - 경로 표현식
- 경로 표현식
    -  . 을 찍어 객체 그래프를 탐색하는 것

![image](https://user-images.githubusercontent.com/74396651/200994489-c685bafa-e5e0-4df0-b01f-9f2a16a9939a.png)

- #### 용어 정리 
- 상태 필드(state field)
    - 단순히 값을 저장하기 위한 필드(그냥 컬럼)
    - ex) m.name
- 연관 필드(association field)
    - 연관관계를 위한 필드(엔티티, 컬렉션 등)
    - 단일 값 연관 필드 
       - @ManyToOne, @OneToOne
       - ex) m.team
    - 컬렉션 값 연관 필드 
       - @OneToMany, @ManyToMany
       - ex1) m.orders

<br>
   
- #### 특징
- 상태 필드(state field) 
    - 경로 탐색의 끝, 더이상 탐색하지 않는다.
- 단일 값 연관 경로
    - 묵시적으로 내부 조인(inner join)이 발생한다. 탐색 가능
- 컬렉션 값 연관 경로
    - 묵시적으로 내부 조인(inner join)이 발생한다. 더이상 탐색하지 않는다.
    - FROM 절에서 명시적 조인을 통해 별칭을 얻으면 별칭을 통해 탐색이 가능하다.

<br>

### 상태 필드 경로 탐색
```java
// JPQL
select m.name, m.age from Member m

// SQL
SELECT m.name, m.age FROM Member m
```

<br>

### 단일 값 연관 경로 탐색
```java
// JPQL
select m.team from member

// SQL
SELECT m.*
FROM Member m
INNER JOIN Member m ON m.team_id=t.id
```

<br>

## 명시적 조인, 묵시적 조인
- 명시적 조인(권장!!!!!!!!!!!)
    - join 키워드를 직접 사용
    - jpql ex) select m from Member m join m.team t
- 묵시적 조인
    - 경로 표현식에 의해 묵시적으로 SQL 조인이 발생한다.(inner join만 가능)
    - jpql ex) select m.team from Member m 

### 경로 탐색을 사용한묵시적 조인 시 주의사항
- 항상 내부 조인이 일어난다.
- 컬렉션은 경로 탐색의 끝이다. 사용하기 위해선 명시적 조인을 통해 별칭을 얻어 사용한다.
- 경로 탐색은 주로 SELECT, WHERE 절에서 사용하지만 묵시적 조인으로 인해 SQL의 FROM(JOIN)절에 영향을 준다.
- 가급적이면 명시적 조인을 사용하라!!!
- JOIN은 SQL 튜닝에서 중요한 포인트이다.
- 묵시적 조인은 조인이 일어나는 상황을 한눈에 파악하기 어렵다.

<br>

## JOIN - fetch join(페치 조인)
- SQL 조인의 종류가 아니다!
- JPQL에서 성능 최적화를 위해 제공하는 기능
- 연관된 엔티티나 컬렉션을 SQL 한 번에 함께 조회하는 기능이다.(Eager와 비슷)
- join fetch 명령어를 사용한다.
- "LEFT / INNER JOIN FETCH 조인 경로" 를 통해 사용
- 다대일 관계에서 사용하낭? 헷갈리낟..

<br>

## 엔티티 fetch join
- 회원을 조회하면서 연관된 팀도 함께 조회(SQL 한 번에 조회)
```java
// JPQL
select m from Member m join fetch m.team

// SQL
SELECT m.*, t.* 
FROM MEMBER M
INNER JOIN TEAM t ON m.TEAM_ID=T.ID
```
![image](https://user-images.githubusercontent.com/74396651/203377439-a5cf7944-ffd4-42db-9025-1761dc59f508.png)

![image](https://user-images.githubusercontent.com/74396651/203379238-6da59266-37e7-4f01-abbf-30326b4974a2.png)


<br>

## 컬렉션 fetch join
- 일대다 관계
```java
// JPQL
select t from Team t join fetch t.members where t.name = 'A'

// SQL
SELECT t.* m.*
FROM TEAM T
INNNER JOIN MEMBER m ON t.ID = m.TEAM_ID
WHERE t.NAME = 'A'
```
![image](https://user-images.githubusercontent.com/74396651/203378437-6272e4d9-5db0-4105-9b99-9352f668652a.png)

![image](https://user-images.githubusercontent.com/74396651/203379319-d3453d76-ee47-472e-b0df-bcbb1bd5c066.png)

<br>

## fetch join과 DISTINCT
- SQL의 DISTINCT는 중복된 결과를 제거하는 명령
- JPQL의 DISTINCT 2가지 기능 제공
    - SQL에 DISTINCT를 추가
    - application에서 entity 중복 제거! 

![image](https://user-images.githubusercontent.com/74396651/203379863-23eb0dd8-bf22-4aed-a8a5-b07cd50d9af0.png)

![image](https://user-images.githubusercontent.com/74396651/203380062-b13f0b9c-01a0-4bdd-b66b-c8c4221fa7a0.png)

<br>

## fetch join과 일반 join의 차이
- 일반 join 실행시 연관된 엔티티를 함께 조회하지 않는다.
- JPQL은 결과를 반환할 때 연관관계를 고려하지 않는다.
- 단지 SELECT 절에 지정한 엔티티만 조회할 뿐이다.
- 여기서는 팀 엔티티만 조회하고 회원 엔티티는 조회하지 않는다.
```java
// 일반 join

// JPQL
select t 
from Team t
inner join t.members m
where t.nave = 'A'

// SQL
SELET t.*
FROM TEAM t
INNER JOIN MEMBER m ON t.ID = m.TEAM_ID
WHERE t.NAME = 'A'

```

- fetch join을 사용할 때만 연관된 엔티티도 함께 조회한다.
- fetch join은 객체 그래프를 SQL 한번에 조회하는 개념이다!
```java
// fetch join
// JPQL
select t
from Team t join fetch t.members
where t.name = 'A'

// SQL
SELECT t.* m.*
FROM TEAM t
INNER JOIn MEMBER m ON t.ID = m.TEAM_ID
WHERE t.NAME = 'A'
```

<br>

## fetch join의 특징과 한계
- fetch join 대상에는 별칭을 줄 수 없다.
    - 하이버네이트는 가능하지만, 가급적 사용하지 않는다.
- 둘 이상의 컬렉션은 fetch join을 할 수 없다.
- 컬렉션을 fetch join 할 경우 페이징 API(setFirestResult, setMaxResults)를 사용할 수 없다.
    - 일대일, 다대일 같은 단일 값 연관 필드들은 fetch join해도 페이징 가능하다!!!
    - 하이버네이트는 경고 로그를 남기고 메모리에서 페이징한다.(매우 위험!!!)
- 연관된 엔티티들을 SQL 한 번으로 조회가 가능하다! -> 성능 최적화!!
- 엔티티에 직접 적용하는 글로벌 로딩 전략보다 우선한다.
    - 글로벌 로딩 전략 : @OneToMany(fetch = FetchType.LAZY)
    - 간단히 말해 default로 안전하게 LAZY로 하고 필요할 때 EAGER로 가져온다는 뜻. 
- 실무에서 글로벌 로딩은 모두 LAZY
- 최적화가 필요한 곳에서 fetch join을 적용하자!

<br>

> &nbsp;여러 테이블을 조인해서 엔티티가 가진 모양이 아닌 전혀 다른 결과를 내야할 경우 fetch join보다 일반 join을 사용학 필요한 데이터들만 조회하여 DTO로 반환하는 것이 효과적이다.<br><br>
> &nbsp;모든 것을 fetch join으로 해결할 수 없다! 페치 조인은 객체 그래프를 유지할 때 사용하면 효과적이다!

<br>

# JPQL 다형성

![image](https://user-images.githubusercontent.com/74396651/203382547-e1527b5c-0f22-4f76-b9a9-f66d4b774370.png)

## TYPE
- 조회 대상을 "특정 자식"으로 한정
    - ex) Item 중 Book, Movie를 조회하라
```java
// JPQL
select i from Item i where type(i) IN (Book, Movie)

// SQL
SELECT i 
FROM ITEM i
WHERE i.DTYPE IN ('B', 'M')
```

<br>

## TREAT(JPA 2.1)
- 자바의 타입 캐스팅과 유사하다.
- 상속 구조에서 부모 타입을 특정 자식 타입으로 다룰 때 사용한다.
- FROM, WHERE, SELECT(하이버네이트에서 지원)을 사용한다.
- ex) 부모 Item, 자식 Book
```java
// JPQL
select i from Item i where treat(i as Book).authorr = 'kim'

// SQL
SELECT i.*
FROM ITEM i
WHERE i.DTYPE = 'B' and i.author = 'kim'
```

<br>

## 엔티티 직접 사용
- JPQL에서 엔티티를 직접 사용하면 SLQ에서 해당 엔티티의 기본 키 값을 사용한다.
```java
// JPQL
select count(m.id) from Member m // 엔티티 id 사용
select count(m) from Member m // 엔티티 직접 사용

// SQL
SELECT COUNT(m.id) AS cnt
FROM MEMBER m
```

![image](https://user-images.githubusercontent.com/74396651/203383876-e7e984d2-2439-4e12-935e-419391ff8f50.png)

![image](https://user-images.githubusercontent.com/74396651/203383928-20886d06-b76b-474c-9daa-9d497b072c65.png)

<br>

## Named Query, 정적 쿼리
- 미리 정의해서 이름을 부여해두고 사용하는 JPQL
- 정적 쿼리이다.
- 어노테이션, XML에 정의하여 사용한다.
- application 로딩 시점에 초기화 후 재사용하는 방식이다.
- application 로딩 시점에 쿼리를 검증한다. -> 컴파일 에러로 잡을 수 있다!!!

![image](https://user-images.githubusercontent.com/74396651/203384221-135f4c78-2533-480c-aefd-3b44c0a53b5e.png)

![image](https://user-images.githubusercontent.com/74396651/203384284-1f9d6ba4-879e-4d88-8bf1-8ecdd7c35c24.png)

![image](https://user-images.githubusercontent.com/74396651/203384332-9066a875-9395-48ce-99f7-8f6eafb76fe1.png)


<br>

# JPQL - 벌크 연산
- 예륻 들어 100개의 상품 가격을 모두 1000원씩 올릴 때 dirty checking으로 실행할 경우 너무 많은 SQL이 실행된다. 이를 해결하기 위해 벌크 연산이 사용된다!

<br>

## 벌크 연산 예제
- excuteUpdate()를 통해 쿼리 한 번으로 여러 테이블 로우(엔티티)를 변경할 수 있다.
- excuteUpdate()의 결과는 영향을 받은 엔티티의 수(로우 수)를 반환한다.

![image](https://user-images.githubusercontent.com/74396651/203384963-3f5304ae-b345-42ba-9518-e65324340f56.png)

<br>

## 벌크 연산 주의점
- 벌크 연산은 영속성 컨텍스트를 "무시"하고 DB에 직접 쿼리를 날린다!
- 벌크 연산을 먼저 실행하고 이후 영속성 컨텍스트를 초기화 한다!!! 

<br>

### 생각 정리

SELECT 쿼리를 가져올 시 엔티티 안에 외래키, 즉 객체가 있을 시 프록시 객체로 가져오게 된다(LAZY). 이때 프록시 객체를 실제 데이터로 불러올 때마다 SELECT 쿼리가 발생하는 문제가 발생한다, 이것이 바로 N+1 문제이다. 즉시 로딩, 지연 로딩이든 모두 N+1 문제가 발생하기 때문에 이를 해결하기 위해선 fetch join을 사용해야 한다.



<br>
<br>
<br>
<br>
<br>
<br>

참조 : 인프런 김영한님 강의
