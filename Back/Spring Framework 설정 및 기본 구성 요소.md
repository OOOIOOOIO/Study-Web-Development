# Spring Framework 설정 및 기본 구성 요소

<br>

## STS(Spring Tool Suite) 설치

```
	<STS(Spring Tool Suite) 설치법>
	
	eclipse 플러그인으로 사용

	이클립스 마켓플레이스 --> sts 검색 --> 맨위에 있는 애 설치
	--> 아무것도 따지거나 묻지 않고 동의/next/confirm 등등 누르기
	--> 이클립스 재시작 여부 묻는 애기창 나오면 재시작 클릭
```

<br>

## 프로젝트 생성(Maven 사용)
```
	<프로젝트 생성법>
	
	Alt + Shift + N --> Spring Legacy Project 클릭 --> Project name 입력하고 Spring MVC Project 선택 후 next 클릭
	--> top-level pakage를 작성해야하는데 작성 형식은 "com.~~~.~~~~" 이다. 작성을 완료했으면 finish 를 클릭한다.
```

>&nbsp;Maven은 프로젝트 관리 도구의 일종이다. Maven은 필요한 라이브러리를 특정 문서(pom.xml)에 정의해 놓으면 내가 사용할 라이브러리 뿐만 아니라 해당 라이브러리가 작동하는데에 필요한 다른 라이브러리들 까지 관리하여 네트워크를 통해서 자동으로 다운받아 준다.<br>
> 
>&nbsp;pom.xml : 필요한 라이브러리들을 설치, 등록하는 곳. Building이 되면서 다운된다. (이클립스 빌드패스에서 라이브러리 추가하는것 처럼 여기서 추가하는 것이다.)

<br>

## Lombok 라이브러리 설치

>&nbsp;이클립스와 스프링 플러그인 만으로도 스프링 개발이 가능하지만, Lombok(롬복)을 이용하면 Java 개발시 getter/setter, toString(), 생성자 등을 자동으로 생성해주므로 설치해서 사용하면 편리하다.<br>
>&nbsp;pom.xml에 등록도 해야하고 따로 응용프로그램을 설치해야한다.

```
	<Lombok 라이브러리 설치>
	
	https://projectlombok.org/download에서 1.18.xx 다운로드 --> 다운받은 jar파일이
		1.java 아이콘 모양 : 더블클릭
		2.알집모양 : cmd 켜서 명령어로 : java -jar 파일경로\lombok.jar
	--> 새로 뜨는 창에서 specify ~~ 클릭 후 eclipse 경로 설정
	--> install/update 클릭 > 설치 완료된 후 eclipse 폴더 내에 lombok.jar 포함되어 있는지 확인 -> 이클립스 재시작
```

<br>

## &nbsp;프로젝트 기본 구성 요소

- src/main/java : 작성되는 코드들의 경로
- src/main/resource : 실행할 때 참고하는 기본 경로(mapper.xml, log4jdbc~, ...설정 파일들)
- src/test/java : 테스트 코드를 넣는 경로
- src/test/resource : 테스트 관련 설정 파일 보관 경로

<br>

## Tomcat의 web.xml 파일 : web.xml
- #### 경로
   - src/main/webapp/web.xml

- #### 정의
   - web.xml은 DD(Deployment Descriptor : 배포 설명자)라고 불리며 Web Application의 설정파일이다. DD는 Web Application실행 시 메모리에 로드된다.
   - web.xml에서는 크게 DispatcherServlet, ContextLoderListen, Filter 설정을 한다.
   - DispatcherServlet 
      - Client의 요청을 처리하는 객체이다.
   - ContextLoaderListener 
      - Web Application context 단위의 설정을 로드하는 객체, Client의 요청이 들어오면 제일 먼저 listener로 등록된 곳이 작동한다.
      - Application Context는 Web Application의 Context이며, 모든 곳에서 공통으로 사용하는 객체이다.
<br>

- #### web.xml 파일

![](https://user-images.githubusercontent.com/74396651/158010670-d94d7cf3-f504-4636-b761-dabad54f8c7d.png)

<br>

- 7 ~ 16 Line
```xml
	<context-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>/WEB-INF/spring/root-context.xml</param-value>
	</context-param>
	
	<listener>
		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
	</listener>
	
1. <context-param>을 통해 공통으로 사용할 의존성 설정 파일을 지정한다. 
   <param-name>을 통해 contextConfigLocation 이라는 name을 설정하고
   <param-value>를 통해 설정 파일의 경로를 설정한다.
	 
2. <listener>를 통해 listner를 설정하며
   <listener-class>를 통해 listner를 등록한다.
   등록된 ContextLoaderListener는 context로 등록된 contextConfigLocation를 찾고 root-context.xml을 읽는다.
```

- 18 ~ 31 Line
```xml
	<servlet>
		<servlet-name>appServlet</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		<init-param>
			<param-name>contextConfigLocation</param-name>
			<param-value>/WEB-INF/spring/appServlet/servlet-context.xml</param-value>
		</init-param>
		<load-on-startup>1</load-on-startup>
	</servlet>
		
	<servlet-mapping>
		<servlet-name>appServlet</servlet-name>
		<url-pattern>/</url-pattern>
	</servlet-mapping>

1. <servlet>을 통해 servlet을 등록한다.
   <servlet-name>을 통해 servlet의 name을 설정하고
   <servlet-class>를 통해 servlet의 경로를 설정한다.	
   <init-param>은 servlet이 실행되기 전 참조할 빈(bean)을 설정한다.
   <param-name>은 servlet이 참조할 bean의 name을 설정하고
   <param-value>를 통해 servlet이 참조할 경로를 설정한다.
	   
2. <servlet-mapping>을 통해 "1"에서 설정한 servlet에 맵핑(URL연결)을 해준다.
   <servlet-name>을 통해 연결할 servlet을 설정하고
   <url-pattern>을 통해 URL을 통해 들어올 형식을 지정해준다.("/"를 입력하면, 모든 url요청이 servlet으로 들어온다.)
```

#### web.xml의 동작을 정리해보자면 우선 ContextLoaderListener을 통해 contextConfigLocation가 읽힐 것이다. 그럼 root-context.xml을 읽어 올 것이다. 다음으로 DispatcherServlet인 appServlet가 읽히며 이때 servlet-context.xml이 참조될 것이다. 그리고 appServlet을 "/"으로 맵핑을 하여 모든 url이 servlet으로 들어올 수 있게 된다.

<br>


## bean 관리용 스프링 설정 파일 : root-context.xml
- 경로
   - src/main/webapp/WEB-INF/spring/root-context.xml

- #### 정의
   -  root-context에 등록되는 bean들은 모든 context에서 사용할 수 있다.(공유)
   -  service나 DAO를 포함한 웹 환경에 독립적인 bean들을 담아둔다.
   -  서로 다른 servlet-context에서 공유해야 하는 bean들을 등록해놓고 사용할 수 있다.
   -  servlet-context에 있는 bean들은 사용하지 못한다.
<br>

![](https://user-images.githubusercontent.com/74396651/158013356-5a06e9e1-041c-443f-9f79-303e921bc9aa.png)

<br>

## 웹과 관련된 스프링 설정 파일 : servlet-context.xml
- #### 경로 
   - src/main/webapp/WEB-INF/spring/appServlet/servlet-context.xml

- #### 정의
   - servlet-context에 등록되는 bean들은 해당 context에서만 사용할 수 있다.
   - DispatcherServlet이 직접 사용하는 controller를 포함한 웹 관련 bean을 등록하는데 사용한다.
   - 독자적인 context들을 가지며 root-context 내에 있는 bean들 또한 사용할 수 있다.

<br>

![](https://user-images.githubusercontent.com/74396651/158012780-1d5db128-a0b1-42ae-8ae0-5f0cf8eb83e3.png)

<br>

```xml
	<annotation-driven />

	<resources mapping="/resources/**" location="/resources/" />

	<beans:bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<beans:property name="prefix" value="/WEB-INF/views/" />
		<beans:property name="suffix" value=".jsp" />
	</beans:bean>
	
	<context:component-scan base-package="com.sh.controller" />

1. <annotation-driven />는 Annotation을 활용할 때 기본적은 Default 방식을 설정해주는 것이다.

2. 
	<beans:bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<beans:property name="prefix" value="/WEB-INF/views/" />
		<beans:property name="suffix" value=".jsp" />
	</beans:bean>
	
Controller에서는 이동하고자 하는 곳을 return을 통해 명시한다. return을 homepage라고 했을 경우 
/WEB-INF/views/homepage.jsp 가 완성된다.

3. <context:component-scan base-package="com.sh.controller" /> 해당 패키지를 스캔해서 Annotaion을 명시한 class를 bean으로 등록해준다.
```

<br>

## 템플릿 프로젝트의 jsp 파일 경로(Front)

- src/main/webapp/WEB-INF/viws

<br>

## Maven이 사용하는 pom.xml
- 프로젝트명/pom.xml
- 이전 eclipse에서 buildpath에 설정하는 것들을 여기선 Maven을 이용한 pom.xml에 등록하여 다운로드 받는다.

``` 
	라이브러리 오류 발생시

	C 드라이브 -> 사용자 -> 계정폴더 -> .m2폴더 안의 내용 다 지우기 -> 이클립스 재시작 
	-> 프로젝트 우클릭 -> Maven -> Update Project
```

<br>

# 스프링 프레임워크 동작
>&nbsp;스프링 프레임워크가 시작되면서 먼저 스프링이 사용하는 메모리 영역을 만든다. 스프링 내부적으로 ApplicationContext라는 이름의 객체가 만들어진다.<br>
>&nbsp;스프링은 자신이 생성하고 관리해야 하는 객체들에 대한 설정을 알아야 하고, 이 설정파일은 root-context.xml 이라는 파일로 만들어져 있다.<br>
>&nbsp;root-context.xml에 설정되어 있는 <context:component-scan> 태그의 내용을 통해서 컴포넌트가 존재하는 패키지를 스캔하기 시작한다.<br>
>&nbsp;해당 패키지에 있는 클래스들 중에서 스프링이 사용하는 @Component라는 어노테이션이 존재하는 클래스의 인스턴스를 생성한다.




