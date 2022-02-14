# Spring Framework(스프링 프레임워크)

## 개념
>&nbsp;경량 프레임워크(light-weight)로서 예전 프레임워크는 다양한 경우를 처리할 수 있는 다양한 기능을 가지도록 만들다
보니 하나의 기능을 위해서 너무 많은 구조가 필요했다. 기술이 너무나 복잡하고 방대했기 때문에 전체를 이해하고 개발하기에는 어려움이 많았다. 그래서 스프링 프레임워크가 등장했고, 특정 기능을 위주로 간단한 JAR파일 등을 이용해서
모든 개발이 가능하도록 구성되어 있다.

<br>

## 스프링 프레임워크의 장점
  - 복잡함에 반기를 들어서 만들어진 프레임워크
  - 프로젝트 전체 구조를 설계할 때 유용한 프레임워크
  - 다른 프레임워크들의 포용(여러 프레임워크를 혼용해서 사용 가능 - 접착성)
  - 개발 생산성과 개발도구의 지원

<br>

## 스프링 프레임워크의 특징
  - 제어의 역전(Inversion Of Control)
  - POJO 기반의 구성
  - 의존성 주입(DI)을 통한 객체 간의 관계 구성
  - AOP(Aspect-Oriented-Programing) 지원
  - 편리한 MVC 구조
  - WAS 종속적이지 않은 개발 환경

<br>

### POJO(Plain Old Java Object)
>&nbsp;오래된 방식의 간단한 자바 객체 라는 의미이며, JAVA 코드에서 일반적으로 객체를 구성하는 방식을 스프링 프레임워크에서 그대로 사용할 수 있다는 말이다.

<br>

### 의존성 주입(DI - Dependency Injection)
>&nbsp;의존성(Dependency)이란 하나의 객체가 다른 객체 없이 제대로 된 역할을 할 수 없다는 것을 의미한다. 
>예를 들어 a 객체가 b 객체 없이 동작이 불가능한 상황을 'a가 b에 의존적이다' 라고 표현한다.<br>
>
>&nbsp;주입(Injection)은 말 그대로 외부에서 밀어 넣는것을 의미한다. 예를 들어 a 객체가 필요로 하는 b 객체를 외부에서 밀어 넣는것을 의미한다. 
>주입을 받는 입장에서는 어떤 객체인지 신경 쓸 필요가 없고 어떤 객체에 의존하든 자신의 역할은 변하지 않게 된다.

```
 의존

a ---------------> b
a객체에서 b객체를 직접 생성

 의존성 주입

a----->???<------> b
a객체는 b가 필요하다는 신호만 보내고, b객체를 주입하는 것은 외부에서 이루어짐
```

>&nbsp;의존성 주입방식을 사용하기 위해서는 ???라는 존재가 필요하게 된다. 스프링 프레임워크에서는 ApplicationContext가 ???라는 존재이며, 필요한 객체들을
생성하고 주입까지 해주는 역할을 한다. 따라서 개발자들은 기존의 프로그래밍과 달리 객체와 객체를 분리해서 생성하고, 이러한 객체들을 엮는(wiring) 작업의
형태로 개발하게 된다. ApplicationContext가 관리하는 객체들을 빈(bean, root_context.xml에서 <context:component-scan 설정)이라 부르고, 빈과 빈 사이의 의존 관계를 처리하는 방식으로 XML 설정, 어노테이션 설정, JAVA설정 방식을 이용할 수 있으며 미리 설정해야 한다.

<br>

### AOP(Aspect Oriented Programing)
>&nbsp;관점 지향 프로그래으로써 애플리케이션의 핵심적인 기능과 부가적인 기능을 분리해 Aspect이라는 모듈로 만들어 설계하고 개발하는 방법이다. 좋은 개발 환경에서는 개발자가 비즈니스 로직(핵심 관심사)만 집중할 수 있게 한다. 스프링 프레임워크는 반복적인 코드를 제거해줌으로써 핵심 비즈니스 로직에만 집중할 수 있는 방법을 제공한다. 보안이나 로그, 트랜잭션과 같이 비즈니스 로직은 아니지만 반드시 처리가 필요한 부분을 횡단 관심사(cross-concern)라고 한다.이러한 횡단 관심사를 분리해서 제작하는 것이 가능하고 단 관심사를 모듈로 분리하는 프로그래밍을 AOP라고 하며 비즈니스 로직(핵심 관심)이 정의된 각 클래스에서 횡단 관심사 코드를 심어 사용하는 방식을 위빙(Weaving)이라고 한다.
>
>1) 핵심 비즈니스 로직에만 집중하여 코드 개발
>2) 각 프로젝트마다 다른 관심사 적용시 코드 수정 최소화
>3) 원하는 관심사의 유지보수가 수월한 코드 구성이 가능

<br>

- ## 트랜잭션의 지원
>&nbsp;DB작업시 트랜잭션 관리를 매번 상황에 맞게 코드로 작성하지 않고,
어노테이션이나 XML로 트랜잭션 관리를 설정할 수 있다.

<br>

## MVC 구조
![Spring MVC 프로세스 모델](https://t1.daumcdn.net/cfile/tistory/2219E449553CF49A29)
> ㄴ
> ㄻ
> ㄻ
> ㄻㄴ
> ㄻ
> ㄴㄹㄴ
> ㄻㄴ
> ㄻㄴ
> ㄹㄴ
> ㄻㄴㅇㄹ
> ㄴㅇㄹ
> ㅁㄴㄹ
> ㄴㄹ
> ㄴㅁㄹ

<br>

# Spring 환경설정
>##&nbsp;eclipse를 사용해 Spring Framework 사용

- STS(Spring Tool Suite) 설치

```
eclipse 플러그인으로 사용

이클립스 마켓플레이스 -> sts 검색 -> 맨위에 있는 애 설치
-> 아무것도 따지거나 묻지 않고 동의/next/confirm 등등 누르기
-> 이클립스 재시작 여부 묻는 애기창 나오면 재시작 클릭
```

<br>

- 프로젝트 생성(Maven)

>Maven은 프로젝트 관리 도구의 일종이다. Maven은 필요한 라이브러리를 특정 문서(pom.xml)에 정의해 놓으면 내가 사용할 라이브러리 뿐만 아니라 해당 라이브러리가 작동하는데에 필요한 다른 라이브러리들 까지 관리하여 네트워크를 통해서 자동으로 다운받아 준다.<br>
> 
>&nbsp;pom.xml : 필요한 라이브러리들을 설치, 등록하는 곳. Building이 되면서 다운된다. (이클립스 빌드패스에서 라이브러리 추가하는것 처럼 여기서 추가하는 것이다.)

<br>

- Lombok 라이브러리 설치

>&nbsp;이클립스와 스프링 플러그인 만으로도 스프링 개발이 가능하지만, Lombok(롬복)을 이용하면 Java 개발시 getter/setter, toString(), 생성자 등을 자동으로 생성해주므로 설치해서 사용하면 편리하다.
pom.xml에 등록도 해야하고 따로 응용프로그램을 설치해야한다.

```
https://projectlombok.org/download에서 1.18.xx 다운로드 -> 다운받은 jar파일이
	1.java 아이콘 모양 : 더블클릭
	2.알집모양 : cmd 켜서 명령어로 : java -jar 파일경로\lombok.jar
-> 새로 뜨는 창에서 specify ~~ 클릭 후 eclipse 경로 설정
-> install/update 클릭 > 설치 완료된 후 eclipse 폴더 내에 lombok.jar 포함되어 있는지 확인 -> 이클립스 재시작
```

<br>

## &nbsp;프로젝트 기본 구성 요소

- src/main/java : 작성되는 코드들의 경로
- src/main/resource : 실행할 때 참고하는 기본 경로(mapper.xml, log4jdbc~, ...설정 파일들)
- src/test/java : 테스트 코드를 넣는 경로
- src/test/resource : 테스트 관련 설정 파일 보관 경로

<br>

## 웹과 관련된 스프링 설정 파일

- src/main/webapp/WEB-INF/spring/appServlet/servlet-context.xml

<br>

## bean 관리용 스프링 설정 파일

- src/main/webapp/WEB-INF/spring/root-context.xml

<br>

## Tomcat의 web.xml 파일

- src/main/webapp/web.xml

<br>

## 템플릿 프로젝트의 jsp 파일 경로

- src/main/webapp/WEB-INF/viws

<br>

## Maven이 사용하는 pom.xml
- 프로젝트명/pom.xml

``` 
라이브러리 오류 발생시

C 드라이브 -> 사용자 -> 계정폴더 -> .m2폴더 안의 내용 다 지우기 -> 이클립스 재시작 
-> 프로젝트 우클릭 -> Maven -> Update Project
```

<br>

# 스프링 프레임워크 테스트 환경(JUnit)

> &nbsp;자바 프로그래밍 언어용 유닛 테스트 프레임워크이며 가장 많이 사용되는 테스트 환경이다. 테스트 성공시 JUnit GUI 창에 녹색으로 표시 / 실패시 적색으로 표시된다. 하나하나의 케이스별로(단위로 나누어서) 테스트를 하는 단위 테스트 도구이다.

- 테스트 환경 구축하기
   - 테스트 클래스 위쪽에 어노테이션 추가 <br>
   &nbsp;@RunWith(SpringJUnit4ClassRunner.class) : JUnit 실행 시 <br>
   &nbsp;@ContextConfiguration("file:src/main/webapp/WEB-INF/spring/root-context.xml") : 받아올 component가 있을 시<br>
   &nbsp;@Log4j : 로그로 출력할 시
   - 클래스 내부에 테스트할 메소드 선언
   - 테스트할 메소드 위에 어노테이션 추가 <br>
   &nbsp;@Test : Test할 메소드다~

<br>

# 스프링 프레임워크 동작
>&nbsp;스프링 프레임워크가 시작되면서 먼저 스프링이 사용하는 메모리 영역을 만든다. 스프링 내부적으로 ApplicationContext라는 이름의 객체가 만들어진다. 스프링은 자신이 생성하고 관리해야 하는 객체들에 대한 설정을 알아야 하고, 이 설정파일은 root-context.xml 이라는 파일로 만들어져 있다. root-context.xml에 설정되어 있는 <context:component-scan> 태그의 내용을 통해서 컴포넌트가 존재하는 패키지를 스캔하기 시작한다.
해당 패키지에 있는 클래스들 중에서 스프링이 사용하는 @Component라는 어노테이션이 존재하는 클래스의 인스턴스를 생성한다.


<br>

# DataBase 연결

## Oracle과 연동하기

```
Build Path 추가 -> 왼쪽 메뉴에 바로 위에있는 Deployment Assembly 선택
-> Add -> Java Build Path ~~ 선택 -> ojdbc6.jar 선택 -> Apply
```

<br>

## Spring - Mybatis

>&nbsp;SQL이 짧고 간결한 경우에는 어노테이션을 이용해서 쿼리문을 작성해준다. SQL이 복잡하거나 길어지는 경우에는 어노테이션보다 XML을 이용하는 것이 좋다. Mybatis-Spring의 경우 Mapper 인터페이스와 XML을 연동해서 동시에 이용할 수 있다. 인터페이스객체.메소드()를 사용하는 순간 해당하는 인터페이스의 경로를 namespace로 가지고 있는 xml 파일로 찾아가서 메소드명과 동일한 id의 쿼리문을 수행하여 결과로 돌려준다. <br>
&nbsp;Mybatis는 내부적으로 JDBC의 PreparedStatement를 이용해서 SQL을 처리한다. 따라서 SQL에 전달되는 파라미터는 JDBC에서와 같이 ? 로 치환되어서 처리된다. 복잡한 SQL의 경우 ?로 나오는 값이 제대로 전달 되었는지 확인하기가 쉽지 않고 실행한 SQL의 내용을 정확히 확인하기 어렵기 때문에 log4jdbc-log4j2 라이브러리를 사용하여 어떤 값인지를 확인할 수 있다. <br>
&nbsp;테스트 코드 실행시 많은 양의 로그가 출력되기 때문에 불편할 수 있다. 이럴 때에는 로그의 레벨을 이용해서 수정해 준다. resources/log4j.xml 파일에 있는 level 태그를 수정한다.








