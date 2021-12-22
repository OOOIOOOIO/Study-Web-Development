# JSP & JavaBeans

## JSP(Java Server Page)
- &nbsp;Jsp란 HTML 중심으로 Java와 같이 연동하여 사용하는 웹 언어이다. HTML코드 안에 Java 코드를 작성할 수 있도록 도와주는 언어이다.
- Servlet의 모든 기능 + 추가적인 기능을 제공한다.
지금은 잘 안 쓰고 타임리프와 같은 라이브러리를 사용한다.

## JSP 내부적인 동작 과정
- JSP 문서는 background에서 Servlet으로 자동 변환된다.
1. JSP가 실행되면 WAS는 내부적으로 JSP 파일을 Java Servlet(.java)로 변환한다.
2. WAS는 변환현 Servlet을 동작하여 필요한 기능을 수행한다.
   - Servlet의 동작 (Servlet 이란 참고)
   - WAS는 사용자 요청에 맞는 적절한 Servlet 파일을 컴파일(.class 파일 생성)한다.
   - .class 파일을 메모리에 올려 Servlet 객체를 만든다.
   - 메모리에 로드될 때 Servlet 객체를 초기화하는 init() 메서드가 실행된다.
   - WAS는 Request가 올 때마다 thread를 생성하여 처리한다.
   - 각 thread는 Servlet의 단일 객체에 대한 service() 메서드를 실행한다.
   - service() 메서드는 요청에 맞는 적절한 메서드(doGet, doPost 등)를 호출한다.
3. 수행 후 생성된 데이터를 웹 페이지와 함께 클라이언트에게 응답한다.

## JSP의 특징
- HTML기반 스크립트 언어이기 때문에 HTML + JAVA 기능을 그대로 사용할 수 있다.
- 사용자 정의 태그(custom tags)를 사용하여 보다 효율적으로 웹 사이트를 구성할 수 있다.
   - JSTL(JSP Standard Tag Library) 사용
- Servlet과 다르게 JSP는 수정된 경우 재배포 할 필요 없이 WAS에서 처리해준다.
- Tomcat(WAS)이 이미 만들어 놓은 객체를 사용한다.

## JSP가 제공하는 기본 객체
- request : javax.servlet.HttpServletRequest / HttpServletRequest Object
   - 한번의 요청을 처리하는데 사용되는 모든 JSP페이지에서 공유될 값을 저장한다. 주로 하나의 요청을 처리하는데 사용되는 JSP페이지 사이에서 정보를 전달하기 위해 사용된다.
- response : javax.servlet.HttpServletResponse / HttpServletResponse Object 
   - 응답 정보를 저장한다
- session : javax.servlet.http.HttpSession / HttpSession Object 
   - 사용자와 관련된 정보를 JSP들이 공유하기 위해 사용된다. 사용자의 로그인 정보 등과 같은 것들을 저장한다.
- out : javax.servlet.jsp.jspWriter / PrintWriter Object 
   - JSP 페이지가 생성하는 결과를 출력할 때 사용되는 출력 스트림
- application : ServletContext Object 
   - 모든 사용자와 관련해서 공유할 정보를 저장합니다. 임시 디렉터리 경로와 같은 웹 어플리케이션의 설정 정보를 주로 저장합니다.
- pageContext : jsp.pageContext (JSP페이지에 대한 정보를 저장합니다.)
   - 하나의 JSP페이지 내에서 공유될 값을 저장합니다. 주로 커스텀 태그에서 새로운 변수를 추가할 때 사용된다.
- config : Javax.servlet.ServletConfig
   - JSP페이지에 대한 설정 정보를 저장합니다.
- page : javax.lang.Object
   - JSP페이지를 구현한 자바 클래스 인스턴스
- exception : javax.lang.Throwable 
   - 에러페이지에서만 사용 가능하다

## 기본 객체의 속성
#### pageContext, request, session, application는 속성을 갖고 있다. 속성은 각각의 기본 객체가 존재하는 동안 사용될 수 있고, JSP 페이지 사이에서 정보를 주고 받거나 공유하기 위한 목적으로 사용된다.

- setAttribute(String name, Object value) : 이름이 name인 속성의 값을 value로 지정한다. return type : void
- getAttribute(String name) : 이름이 name인 속성의 값을 구한다. 존재하지 않을 경우 null을 반환한다. return type : Object
- removeAttribute(String name) : 이름이 name인 속성을 삭제한다. return type : void
- getAttributeNames() : 속성의 이름 목록을 구한다.(pageContext 제외) : return type : Enumeration


## Script tag
#### 개념 : Script tag를 이용해 HTML 내부(body)에 자바 코드를 넣어 프로그래밍이 가능하도록 만들 수 있다. 끝에 세미콜론(;)을 붙이지 않는다.
- 선언문(declaration) 태그 : 자바의 변수나 메소드를 정의하는데 사용되는 태그
  - <%! java code %>

- 스크립틀릿(scriptlet) 태그 : 자바의 변수 선언 및 자바 로직 코드를 작성하는데 사용되는 태그
  - <% java code %>

- 표현문(expretion) 태그 : 변수, 계산식, 메소드 호출의 리턴값 등을 표현해주는 태그. HTML 문서의 안에 작성한 값이 그대로 표현된다. 타입은 문자열이다.
  - <%= java code %>

- 디렉티브(directive) 태그 : 현재 JSP 페이지에 대한 정보를 설정하는 태그이다. 되도록 페이지의 최상단에 선언하고 import를 제외한 나머지 속성들은 딱 한번씩만 작성할 수 있다.
   -  <%@ page 속성명 = "" %>
   -  language		: 사용할 프로그래밍 언어, 기본값 java
	 - contentType	: 생성할 문서의 콘텐츠 유형, 기본값 text/html
	 - pageEncoding	: 인코딩 설정, 기본값 ISO-8859-1
	 - import		: 사용할 자바 클래스 추가
	 - session		: 세션 사용 여부 설정, 기본값 true(생략되어 있는 경우가 많다)
	 - info		: 페이지에 대한 설명 작성(주석처럼 이용)
	 - errorPage	: 현재 페이지에서 예외 발생시 거기로 이동할 페이지 설정
	 - isErrorPage	: 현재 페이지를 오류페이지로 설정할지에 대한 여부 

- include 디렉티브 태그 : 현재 JSP 페이지의 특정 영역에 외부 파일의 내용을 포함시키는 태그이다. 보통 header와 footer는 대부분의 페이지에서 동일한 내용으로 작성되기 때문에 각 JSP 파일마다 그 코드들을 반복해서 작성하는 것이 아니라 유지보수 및 편의를 위해 외부 파일로 만든 후 include하여 사용한다.
   -  <%@ include file="파일명" %>
  
- 주석(comment) : <%-- comment --%>

## 액션 태그
#### 개념 : 서버나 클라이언트에게 어떤 행동을 하도록 명령하는 태그. 페이지와 페이지 사이를 제어하거나 다른 페이지의 실행 결과 내용을 현재 페이지에 포함시키거나 자바빈즈(객체) 등의 다양한 기능을 제공한다. 액션태그는 XML 형식인 <jsp: />를 이용한다.

-	<jsp:forward /> 	: 다른 페이지로 이동, 페이지의 흐름을 제어하기 위한 역할
-	<jsp:param /> 	: 현재 페이지에서 다른 페이지에 값을 전달하기 위한 역할 
-	<jsp:useBean /> : 해당하는 Bean(자바 객체)이 이미 존재하는지 확인하고 없다면 지정된 객체를 생성한다.(객체는 .java 파일이며 주로 DTO이다.)
-	<jsp:setProperty /> : Bean(자바 객체)의 속성을 설정한다.
-	<jsp:getProperty /> : 주어진 속성값을 가져오며 이를 문자열로 변환하고 동적인 웹페이지를 생성하는데 사용할 수 있다.

#### 용도
- 동적으로 파일을 삽입
- JavaBeans 구성 요소를 재사용
- 사용자를 다른 페이지로 이동(forward)시킬 수 있다.
- 주로 MVC 패턴의 Model로 사용된다.

#### https://www.javatpoint.com/jsp-action-tags-forward-action 참고 
  
## 스크립트 
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>scriptTag1</title>
</head>
<body>
    <h2>script tag 1</h2>

    <!-- 선언문  -->
    <%! 
      // 내부는 전부 자바의 문법, 선언문 안에 선언할 시 전역변수이다.
      // count는 전역변수
      int count = 3;

      String sayHi(String data){
        return "HELLO " + data;
      }
    %>

    <!-- 스크립틀릿 -->
    <%
      for(int i = 0; i < count; i++){
        out.println(i+". JAVA SERVER PAGES<br>");
      }
    %>
    1. JAVA SERVER PAGES<br>
    2. JAVA SERVER PAGES<br>
    3. JAVA SERVER PAGES<br>

    <!-- 표현문  -->
    <!--표현문은 서블릿의 out.println의 매개변수로 들어가는 것이다. 따라서 ; 쓰면 안된다. -->
    <%-- sayHi("JSP")의 결과 : <%out.println(sayHi("JSP")); %> --%>
    sayHi("JSP")의 결과 : <%= sayHi("JSP") %> 

    <!-- HTML 주석은 컴파일이 모두 되고 나서 페이지에서 감춰지는 형태이다. -->
    <%-- JSP 주석은 안에 작성된 모든 코드가 무시되므로 JSP 주석을 권장한다.--%>
    <hr>
    <p>Now : <%=new java.util.Date() %></p>
    <table border="1">
        <%
          for(int i=1;i<=3;i++){
            out.println("<tr>");
            for(int j=1;j<=5;j++){
              out.println("<td>"+i+"행 "+j+"열</td>");
            }
            out.println("</tr>");
          }
        %>
    </table>
    <hr>
    <table border="1"> <!-- 이렇게 해주는게 JSP를 쓰는 것 -->
        <%
        for(int i=1;i<=3;i++){
        %>
          <tr>
          <%
          for(int j=1;j<=5;j++){
          %>
            <td><%=i %>행 <%=j %>열</td>
          <%
          }
          %>
          </tr>
        <%
        }
        %>
    </table>
    <hr>
    <table border="1">
        <% for(int i = 1; i<=5;i++){%>
          <tr class="">
          <%for(int j =1; j<=i; j++){%>
            <td><%=i %>행<%=j%>열</td>
          <%} %>
          </tr>
        <% }%>
        <% for(int i = 1; i<=4;i++){%>
          <tr>
          <%for(int j =1; j<=5-i; j++){%>
            <td><%=i+5 %>행<%=j%>열</td>
          <%} %>
          </tr>
        <% }%>
    </table>

</body>
</html>
```


<hr/>

## JavaBeans
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<form action="beanResult.jsp"> <!-- action : 보내기 -->
		정수 데이터 <input name="intData"><br>
		문자열 데이터<input name="strData"><br>
		실수 데이터<input name="doubleData"><br>
		<input type="submit">
	</form>
</body>
</html>
```

### 위의 코드에 있는 input이 파라미터로 넘어온다.
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>beanResult</title>
</head>
<body>
	<jsp:useBean class="test.TestDTO" id="bean"/> 
  <!-- 
      여는태그와 닫는태그 사이에 아무것도 없다면  / 이거루 퉁친다 
      class= "자바빈즈로 쓸 클래스" id="이름"
      id는 setProperty의 name과 같다.
  -->
	
	<!-- 
		jsp.setProperty는 외부에서 날라온 파라미터 중에 같은 name을 가지고 있는 것이 있다면
		value 속성을 작성하지 않더라도 값이 자동으로 들어간다.
    
    		DTO 객체의 필드명(변수명)들을  jsp 파라미터의 name(input의 name)과 동일하게 맞춰주면 
  		각각 설정하지 않고 property="*" 해주면 끝이기 때문에 사용하기 편하다.
    
		form 태그의 action을 통해 받은 input 값들을 setProperty가 newUser javaBean(자바 객체)의 속성값(변수값)으로 설정해준다. 
	 -->
	<jsp:setProperty property="intData" name="bean"/>
	<jsp:setProperty property="strData" name="bean"/>
	<jsp:setProperty property="doubleData" name="bean"/>
  <!-- 
    
  -->
  
	<%
		out.println(bean.getDoubledata());
		out.println(bean.getIntdata());
		out.println(bean.getStrdata());
	%>
  	<!-- 이것과 같다.
		<%=bean.getDoubledata()%>
		<%=bean.getIntdata()%>
		<%=bean.getStrdata()%>
	 -->
</body>
</html>
```
