# MyBatis

## MyBatis란
> &nbsp;객체 지향 언어인 자바의 관계형 데이터베이스 프로그래밍을 좀 더 쉽게 할 수 있게 도와주는 개발 프레임 워크로서 
JDBC를 통해 데이터베이스에 엑세스 하는 작업을 갭슐화하고 일반 SQL 쿼리 저장, 프로시저 및 고급 맵핑을 지원하며
모든 JDBC 코드 및 매개 변수의 중복작업을 제거한다. MyBatis에서는 프로그램에 있는 SQL쿼리들을 한 구성파일에 구성하여
프로그램 코드와 SQL을 분리할 수 있는 장점을 가지고 있다.
<br>

## MyBatis 특징
- MyBatis는 복잡한 쿼리나 다이나믹한 쿼리에 강하지만 비슷한 구조를 가진 쿼리를 남발할 수 있다는 단점이 있다. 
- 프로그램 코드와 SQL 쿼리문의 분리로 코드의 간결성 및 유지보수성 향상된다.
- resultType, resultClass 등 VO를 사용하지 않고 조회 결과를 사용자 정의 DTO, MAP 등으로 맵핑하여 사용할 수 있다.
- 빠른 개발이 가능하여 생산성이 향상된다.
- 다양한 프로그램잉 언어로 구현이 가능하다. ex)Java, C#, .NET, Ruby
<br>

## MyBatis 구조
<br>

![MyBatis Database Access 구조](https://user-images.githubusercontent.com/74396651/147461154-ccf0bd86-607d-49e2-a469-d98b4c3e2f28.PNG)
<br>

---------------------
관계형 데이터베이스 프로그래밍
프로시저
도매인객체, VO DTO DAO CRUD
