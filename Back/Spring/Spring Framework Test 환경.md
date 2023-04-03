# 스프링 프레임워크 테스트 환경(JUnit)

> &nbsp;자바 프로그래밍 언어용 유닛 테스트 프레임워크이며 가장 많이 사용되는 테스트 환경이다. 테스트 성공시 JUnit GUI 창에 녹색으로 표시,  실패시 적색으로 표시된다. 하나하나의 케이스별로(단위로 나누어서) 테스트를 하는 단위 테스트 도구이며 테스트를 하기 위한 코드는 "src/test/java"와 "src/test/resource" 폴더 안에 넣고 사용하면 된다.

<br>

## pom.xml에 라이브러리 추가하기

<br>

![](https://user-images.githubusercontent.com/74396651/154190631-48bc578a-cb37-41b3-a336-ca96f6f83da0.png)

<br>

## JUnit 테스트 지원 어노테이션
<br>

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbs88QT%2FbtqCM9tNG2h%2FhMAf7Waj6vxA09cEElfHCK%2Fimg.png)

<br>

- ### @Test <br> 
&nbsp;테스트할 메소드위에 선언하며 테스트를 수행할 메서드로 지정한다. JUnit에서는 각각의 테스트가 서로 영향을 주지 않고 독립적으로 실행되기 때문에 @Test 단위 마다 필요한 객체를 생성해 지원해준다.<br>

- ### @Ignore <br>
&nbsp;테스트할 메소드 위에 선언하며 테스트를 수행하지 않도록 지정한다. 메소드는 남겨두고 테스트에 포함되지 않도록 하기 위해 사용한다.<br>

- ### @Before / @After <br>
&nbsp;테스트 메소드가 실행되기 전-후로 항상 실행되는 메소드를 지정합니다. 공통적으로 실행되어야 하는 메소드가 있다면 이 어노테이션을 붙여주며 각각의 테스트 메소드에 적용된다.<br>

- ### @BeforeClass / @AfterClass <br>
&nbsp;각각의 메소드가 아닌 해당 클래스에서 딱 한번만 수행되는 메소드이다. <br>

<br>

## Spring-Test 어노테이션
- ### @RunWith(SpringJUnit4ClassRunner.class) <br>
&nbsp;ApplicationContext를 만들고 관리하는 작업을 할 수 있도록 JUnit의 기능을 확장해줍니다. 스프링의 핵심 기능인 컨테이너 객체를 생성해 테스트에 사용할 수 있도록 해준다고 보면 된다. 원래 JUnit에서는 테스트 메소드 별로 객체를 따로 생성해 관리하는 반면, Spring-Test 라이브러리로 확장된 JUnit에서는 컨테이너 기술을 써서 싱글톤으로 관리되는 객체를 이용해 모든 테스트에 사용한다.<br>

- ### @ContextConfiguration("file:src/main/webapp/WEB-INF/spring/root-context.xml" "<--경로") <br>
&nbsp;스프링 빈(Bean) 설정 파일의 위치를 지정하여 굳이 별도로 컨테이너를 추가하지 않고 Bean을 등록해둔 xml 파일을 지정해 컨테이너에서 사용할 수 있도록 한다. @RunWith 어노테이션은 컨테이너를 생성하겠다는 의미인데, 어떤 파일을 참조할지 모르는 상태이기에 @ContextConfiguration("경로") 어노테이션을 함께 써주어야 한다.<br>

- ### @Autowired <br>
&nbsp;스프링과 마찬가지로 자동으로 의존성을 주입해준다.

<br>

## 연습

![](https://user-images.githubusercontent.com/74396651/154209130-f61b5751-e9d0-460b-bed9-7dd078714c90.png)
