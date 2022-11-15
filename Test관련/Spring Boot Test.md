# Spring Boot Test
> &nbsp; 스프링 혹은 스프링에서 테스트를 돌리려고 하면 항상 에러가 발생하여 이번에 제대로 정리하려고 한다. 테스트 코드를 작성하다 보면 @SpringBootTest, @ExtendWith, @Runwith, @Mock, MockBean 등 다양한 어노테이션이 있다. Junit4, Junit5에 따라서 사용방법도 달라진다.

<br>

## @SpringBootTest

- ### 역할
   - @SpringBootApplication을 찾아 테스트를 위한 Bean을 생성한다.
   - @MockBean으로 정의된 Bean을 찾아 대체시킨다.
   - @RunWith(SpringRunner.class)와 같이 정의해야 동작한다.(Junit5에서는 생략, @ExtendWith(SpringExtension.class)가 내장되어 있다.)

<br>

- ### 옵션
   - properties
     - application.properties or .yml에 정의된 properties를 재정의한다.(Test 디렉토리에 resource 디렉토리를 만들어 Test용 application.properties 파일은 설정하자!)
   - classes
     - @Configuration이 정의된 Class를 세팅하여 Bean생성 범위를 직접 정의한다.
     - 지정되지 않는 경우 AutoConfiguration을 통해 모든 Bean이 생성된다.
   - webEnviroment
     - 테스트 환경을 선택하는 옵션
     - MOCK(default)
       - WebApplicationContext를 로드하고 서블릿컨테이너 대신 MockServlet을 제공
       - @AutoConfigurationMockMvc와 함께 지정하여 MockMvc를 사용한 테스트 수행 시 사용
     - RANDOM_PORT
       - EmbeddedWebApplicationContext를 로드하여 실제 서블릿 환경 구성, 임의 포트 listen
     - DEFINE_PORT
       - RANDOM_PORT와 동일하지만 실제 application.yml에서 지정한 포트 사용
     - NONE
       - 일반적인 ApplicationContext를 로드하여 서블릿 환경을 구성하지 않는다.

<br>

## RunWith
- ### 역할
   - JUnit의 테스트 실행방법을 정의 하는 어노테이션이다.
     - org.junit.runnner를 상속받은 객체를 정의한다.
     - 실제 테스트 클래스를 호출하는 책임이 있는 클래스이다.
   - @Autowired, @MockBean 등을 로딩하는 주체
     - @SpringBootTest는 ApplicationContext를 전부 구성하고 Spring 관련 Configuration을 수행한다.
     - 반면 @RunWith(SpringRunner.class)는 테스트 파일에 정의된 @Autowired, @MockBean 등만 ApplicationContext로 로딩하는 역할을 수행한다.
     - 즉, Mock과 Autowired 등을 사용할 수 있게 해주는 브릿지 역할이다.
   - Junit5에서는 @ExtendWith(SpringExtension.class)으로 변경되었으며, @SpringBootTest 내부에 내장되어 있기 때문에 생략해도 무방하다.

<br>

## @WebMvcTest
- ### 역할
   - Controller의 동작만 확인하는 경우 사용된다.
   - 아래와 같이 Controller와 관련된 어노테이션 및 클래스만 로딩한다.
```java
@Controller, @RestController, @ControllerAdvice, @JsonComponent,

Converter, GenericConverter, Filter, HandlerInterceptor, WebMvcConfigurer,  HandlerMethodArgumentResolver
```
   - @MockMvc와 함께 사용되며 @ExtendWith(SpringExtension.class)이 내장되어 있다.
   - 그렇기 때문에 컨트롤러에서 호출하는 Service 객체등은 Mock으로 정의해야 한다!
   - 하지만 잘 사용하지는 않는다! RequestParams, PathVariable 검증 혹은 에러케이스의 응답처리 확인용으로 사용할 것 같다.

<br>

## @AutoConfigurationMockMvc
- ### 역할
   - @WebMvcTest와 사용방법은 유사하지만
   - @Service, @Repository 등과 같은 Bean도 같이 로딩한다.
   - @SpringBootTest와 함께 사용된다.
     - webEnvironment 옵션을 통해 실제 WAS를 띄울지 MOCK 서버를 띄울지 결정할 수 있다.(Mock이 default)

<br>

## @Mock vs @MockBean
> &nbsp; @MockBean과 @Mock 모두 <code>stub(더미 객체 느낌)</code> 가능한 Mock 객체를 생성하는 역할을 한다.
> 더미 객체이기 때문에 이름과 형식적 기능만 있을 뿐이다! 실제 소스코드가 수행되지 않고 결과에 대한 리턴만 stub할 수 있다.

### Stub
- #### test stub
    - 스텁은 'canned answer'을 호출한 쪽에 제공(provide)한다는 것이다. 여기서 canned answer는 "미리 준비된 답변은 일반적인 질문에 대한 미리 정해진 답변"이라는 뜻이다. 
    - 즉, stub은 실제코드나 아직 준비되지 못한 코드를 미리 정해진 답변으로 가장하는 매커니즘이다.

- #### 특징
- dummy객체가 실제로 동작하는 것처럼 만들어 놓은 객체
- 실제 코드나 아직 준비되지 못한 코드의 행동으로 가장하는 행위
- 호출자를 실제 구현물로부터 격리시키는 목적으로 사용 가능
- 인터페이스 or 기본 클래스가 최소한으로 구현된 상태
- 테스트에서 호출된 요청에 대해 미리 준비해둔 결과를 제공

- #### 주로 사용되는 경우
- 구현되지 않은 함수나, 라이브러리에세 제공하는 함수를 사용하고자 할 때
- 함수가 반환하는 값을 임의로 생성하고 싶을 때
- 복잡한 논리 흐름을 가지는 경우, 테스트를 단순화 하고 싶을 때
- 의존선을 가지는 유닛의 응답을 모사하여 독립적인 시험 수행을 하고자 할 때

- #### 사용하여 얻을 수 있는 이점
- 의존하는 것에 대하여 독립적인 개발/테스트가 가능하다.
    - interface만 존재하는 것을 stub으로 개발하고 테스트 할 수 있다.
- 촘촘한 테스트가 가능하다.
    - stub으로 다양한 응답결과(canned answer) 케이스를 만들어 테스트 할 수 있다. 

<br>

### @MockBean
- @MockBean은 스프링 하위 패키지에 존재하기에 "스프링 전용 Mock"이라고 보면 된다.
- @MockBean으로 정의한 객체는 ApplicationContext 구성 시 BeanFactory에 실제 Bean 대신 MockBean을 생성하여 로딩한다.

<br>

### @Mock
- @Mock은 ApplicationContext, BeanFactory에 포함되지 않는다.
-  테스트 코드상에서 주입하거나 @InjectMock(spring bean 주입) 등 추가 어노테이션으로 의존성 관리를 직접 해야한다. 

<br>

## @Spy vs @SpyBean
> &nbsp; @Spy와 @SpyBean의 차이는 @Mock과 @MockBean의 차이와 같다. ApplicationContext, BeanFactory에 로딩되어 자동으로 주입되느냐, 명시적으로 @InjectMock을 넣어주느냐의 차이.

<br>

### @Spy
- Mock같은 경우 가상 객체이기 때문에 실제 소스코드가 실행되지 않고 결과에 대한 리턴만 stub할 수 있다.
- 이럴 때, 일부 메소드는 stub하고 다른 메서드는 실제 소스코드가 수행되는 테스트를 구성하고 싶을 때 사용하는 것이 @Spy이다.
- Mock을 원하는 대상 메서드만 stub처리하면 다른 메서드는 실제 소스코드가 수행된다.

<br>

### @SpyBean
- ApplicationContext 로딩 시 BeanFactory에 등록된다.
- 자동으로 의존성 주입이 되며 사용밥법은 @Spy와 동일하다.

<br>
<hr>
<br>

## Junit4 vs Junit5

![image](https://user-images.githubusercontent.com/74396651/201607837-b21c4136-1081-4a7e-9473-95d1e2499d03.png)

- expected가 사라져 Junit5에선 Assertions.assertThrows() or Assertions.assertThatThrownBy()를 사용해야한다.





<br>
<br>
<br>
<br>

참조 : https://velog.io/@leejisoo/SpringBootTest-%EC%A0%95%EB%A6%AC







