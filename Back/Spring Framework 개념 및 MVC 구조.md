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
  - 제어의 역전(IOC - Inversion Of Control)
  - POJO 기반의 구성
  - 의존성 주입(DI Dependency Injection)을 통한 객체 간의 관계 구성
  - AOP(Aspect-Oriented-Programing) 지원
  - 편리한 MVC 구조
  - WAS에 종속적이지 않은 개발 환경

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

			 의존

			a ---------------> b
			a객체에서 b객체를 직접 생성

			 의존성 주입

			a----->???<------> b
			a객체는 b가 필요하다는 신호만 보내고, b객체를 주입하는 것은 외부에서 이루어짐

>&nbsp;의존성 주입방식을 사용하기 위해서는 ???라는 존재가 필요하게 된다. 스프링 프레임워크에서는 ApplicationContext가 ???라는 존재이며, 필요한 객체들을 생성하고 주입까지 해주는 역할을 한다. 따라서 개발자들은 기존의 프로그래밍과 달리 객체와 객체를 분리해서 생성하고, 이러한 객체들을 엮는(wiring) 작업의 형태로 개발하게 된다. ApplicationContext가 관리하는 객체들을 빈(bean, root_context.xml에서 <context:component-scan 설정)이라 부르고, 빈과 빈 사이의 의존 관계를 처리하는 방식으로 XML 설정, 어노테이션 설정, JAVA설정 방식을 이용할 수 있으며 미리 설정해야 한다.

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

# Spring Framework 구조

<br>

![Spring Framework 구조](https://blog.kakaocdn.net/dn/Kn1lN/btq5fDsegsV/pMxVSu0gPJqqLTzEYFsuHk/img.png)

<br>

- Spring Core : Spring Core는 일반 JAVA(POJO) 영역이며  Spring Framework의 핵심인 Bean Factory Container이다. Bean Factory는 IOC(제어의 역전)패턴을 적용하여 객체 구성부터 의존성 처리(DI)까지 모든 일을 처리하는 역할을 하고 있다.

- Spring Context : Spring Context는 Spring Framework의 Context 정보들을 제공하는 설정 파일이다. Spring Context에는 JNDI, EJB, Validation, Scheduiling, Internaliztaion 등 엔터프라이즈 서비스들을 포함하고 있다.

- Spring AOP : Spring AOP module은 Spring Framework에서 AOP를 적용 할수 있게 도와주는 Module이다.

- Spring DAO : DAO란 Data Access Object의 약자로 Database Data에 접근하는 객체이다. Spring JDBC DAO는 추상 레이어를 지원함으로써 코딩이나 예외처리 하는 부분을 간편화 시켜 일관된 방법으로 코드를 짤 수 있게 도와준다.

- Spring ORM : ORM이란 Object relational mapping의 약자로 간단하게 객체와의 관계 설정을 하는 것을 말한다. Spring에서는 Ibatis, Hibernate, JDO 등 인기있는 객체 관계형 도구(OR도구)를 사용 할 수 있도록 지원합니다.

- Spring Web : Spirng에서 Web context module은 Application module에 내장되어 있고 Web기반의 응용프로그램에 대한 Context를 제공하여 일반적인 Web Application 개발에 필요한 기본적인 기능 및 Jakarta Structs 와의 통합 지원을 하고 있다.

- Spring MVC : Spring에서는 MVC에서는 Model 2 구조로 Apllication을 만들 수 있도록 지원한다. MVC (Model-View-Controller) 프레임 워크는 웹 응용 프로그램을 작성하기위한 완전한 기능을 갖춘 MVC를 구현합니다. MVC 프레임 워크는 전략 인터페이스를 통해 고급 구성 가능하며 JSP, Velocity, Tiles, iText 및 POI를 포함한 수많은 뷰 기술을 지원하고 있다.

<br>

# 스프링 프레임워크 동작시 생기는 일
>스프링 프레임워크가 시작되면서 먼저 스프링이 사용하는 메모리 영역을 만든다. <br><br>
>내부적으로 ApplicationContext라는 이름의 객체가 만들어진다. <br><br>
>자신이 생성하고 관리해야 하는 객체들에 대한 설정을 알아야 하고, 이 설정파일은 root-context.xml 이라는 파일로 만들어져 있다. <br><br>
>root-context.xml에 설정되어 있는 <context:component-scan> 태그의 내용을 통해서 컴포넌트가 존재하는 패키지를 스캔하기 시작한다.<br><br>
>해당 패키지에 있는 클래스들 중에서 스프링이 사용하는 @Component라는 어노테이션이 존재하는 클래스의 인스턴스를 생성한다.

<br>

# Spring MVC 구조 
>&nbsp;스프링 프레임워크는 하나의 기능을 위해서만 만들어진 프레임워크가 아닌 '코어'라고 할 수 있는 여러 서브 프로젝트를 결합해서 다양한 상황에 대처할 수 있도록 개발되었다. 그 중 하나가 스프링 MVC 구조이다.<br>
>&nbsp;HttpServletRequest, HttpServletResponse를 거의 사용할 필요 없이 기능 구현이 가능하다. 다양한 타입의 파라미터 처리, 다양한 타입의 리턴 타입 사용 및 GET 방식, POST 방식 등 전송 방식에 대한 처리를 어노테이션으로 처리, 상속/인터페이스 방식 대신 어노테이션으로 설정이 가능하다.

<br>

![Spring MVC 프로세스 모델 - Front-Controller 패턴](https://t1.daumcdn.net/cfile/tistory/2219E449553CF49A29)

## Spring MVC 주요 구성요소
- DispatcherServlet : 클라이언트의 요청을 전달 받는 역할. Controller에게 클라이언트의 요청을 전달하고, Controller가 리턴한 결과 값을 ViewResolver에 전달하여 알맞은 응답을 생성하도록하고 응답을 전송한다. (스프링 제공)
- HandlerMapping : Client의 요청 URL을 어떤 Controller가 처리할 지를 결정한다. (스프링 제공)
- Controller : Clent의 요청을 처리한 뒤, 그 결과를 DispatcherServlet에게 알려준다. (실제 로직)
- ViewResolver : Controller의 처리 결과를 생성할 View를 결정한다.
- View : Controller의 처리 결과를 보여준다.

<br>

## Front-Controller 패턴 동작 과정

1. 사용자(client)의 Request는 Front-Controller인 DispatcherServlet을 통해 처리한다.

2. HandlerMapping은 Request의 처리를 담당할 컨트롤러를 찾기 위해 존재한다. HandlerMapping 인터페이스를 구현한 여러 객체 중 @RequestMapping이라는 어노테이션이 적용된 것을 기준으로 controller를 판단하며, 적절한 controller를 찾았다면 HandlerAdapter를 이용해 해당 컨트롤러를 동작시킨다.

3. Controller의 처리가 완료되었다면 "어디로, 어떻게 갈 것인지" 라는 결과가 나오고 그 결과를 ViewResolver가 리턴을 통해 받아, 어떤 View에서 처리하는 것이 좋을지 해석해준다.

4. 해석된 결과를 가지고 실제 응답을 보내야 하는 데이터를 JSP, Velocity, Freemaker 등을 이용해 생성해준다.

5. 생성된 응답(페이지)을 DispatcherServlet을 통해 전송한다.

<br>
