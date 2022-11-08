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
