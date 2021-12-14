# HTML Form and Servlet

<hr/>

## Web Service의 기본적인 동작 과정

![Web service](https://user-images.githubusercontent.com/74396651/146043207-3e87bf78-979d-44d0-9a1a-f81bdc187241.PNG)<br>
<ol>
	<li>사용자가 웹 페이지 form(HTML Form)을 통해 정보를 입력한다.(input)</li>	
	<li>Servlet의 doGet 혹은 doPost 메서드는 입력한 form data에 맞게 DB 또는 다른 소스에서 관련된 정보를 검색한다.</li>
	<li>이 정보를 이용하여 사용자의 요청에 맞는 적절한 동적 컨텐츠(HTML page)를 만들어서 제공한다.(Oupput)</il>
</ol>

## HTML Form

#### input elements(텍스트 상자)가 포함된 웹 페이지의 한 부분(section)
- 사용자가 입력한 정보(form contents를 웹 서버로 전송하기 위한 submit element(버튼)가 존재한다.
- action에는 form을 처리하는 서버 쪽 URL을 기입한다.

#### 클라이언트(browser)가 요청하는 URL 정보
- 요청을 보낼 서버의 IP주소 : Port 번호 / App 이름(프로젝트명) / action으로 보낼 html
- 요청을 보낼 서버의 IP주소 : Port 번호 / App 이름(프로젝트명)?param=hahaha&param2=hohoho&.....(get방식)
   - Ex) localhost9090:ServletTest/join_db.jsp
- IP 주소
   - 요청을 보낼 서버의 "위치"를 의미한다.
   - browser와 WAS가 같은 PC에 있으면 IP 주소 == localhost 이다.
- Port 번호
   - 컴퓨터 안에서 작동하는 애플리케이션을 식벽하기 위해 사욛하는숫자이다.

### jsp를 이용한 form 태그 
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>joinview</title>
<style>
	input{
		border-radius: 5px;
	}
</style>
</head>
<body>
	<!-- 아이디, 비밀번호, 이름, 핸드폰번호 입력받아서 "가입하기" 버튼 클릭
		-> join_db.jsp로 이동
	 -->
	<form name="loginForm" action="join_db.jsp" method="post">
		<p>
			<label>아이디 <input type="text" name="userid"></label>
		</p>
		<p>
			<label>비밀번호 <input type="password" name="userpw"></label>
		</p>
		<p>
			<label>이름 <input type="text" name="username"></label>
		</p>
		<p>
			<label>핸드폰번호 <input type="tel" name="userphone"></label>
		</p>
		<input type="submit" value="가입하기">
	</form>
</body>
</html>
```

#### Form Tag 속성
- name : form을 식별하기 위한 이름 지정
- action : form을 전송할 서버 쪽 스크립트 파일 지정
- method : form을 서버에 전송하는 방식으로, HTTP 메서드 지정(GET or POST)
- target : action에서 지정한 스크립트 파일을 현재 창이 아닌 다른 위치에서 열리도록 지정
- accept-charset : form 전송에 사용할 문자 인코딩을 지정
- enctype : 넘기는 Content의 Type 지정(주로 파일을 넘길 때 사용)

<hr/>

## 서블릿(Servlet)

- 서블릿이란 JAVA 클래스이다. JAVA 코드 안에서 HTML 문서를 작성할 수 있는 JAVA 프로그램이다. 
- Java Resources의 src 파일에 java 만드는 것처럼 패키지 만들고 클래스만 servlet으로 만들어준다.
- 
![ervlet](https://user-images.githubusercontent.com/74396651/146051879-a06952ce-d46c-40b8-af9f-c1c0b780863f.png)



#### 서블릿 작동 순서
1. 사용자가 URL 요청
2. web.xml에 맵핑해놓은 서블릿을 찾는다.
3. 해당하는 서블릿의 클래스로 요청을 전송 
4. Thread에 의해 service() 호출 
5. 요청 방식에 따라 doGet() 혹은 doPost()를 호출한 후 처리
6. 이후 WAS는 Response 객체를 HttpResponse 형태(정적 형태)로 바꿔서 웹 서버에 전달하고 생성된 Thread를 종료한다. 그리고 HttpServletRequest, HttpServletResponse 객체를 제거한다.


#### 응답 처리 방법
- doGet()이나 doPost() 메서드 안에서 처리
- 
- HttpServletRequest request 객체를 이용해 클라이언트의 요청으로부터 들어온 파라미터에 대한 값을 request.getParameter(""); 얻어온다.
- 
- HttpServletResponse response 객체와 PrintWriter out 객체를 이용하여 PrintWriter out = response.getWriter(); 형식으로 응답으로 내보낼 출력 스트림을 얻어낸 후 out.println("");으로 스트림에 텍스트를 기록하게 된다.

- setContentType()을 이용해 클라이언트에 전송할 데이터 종류(MIME-TYPE) 지정

- 클라이언트와 서블릿의 통신은 자바 I/O 스트림을 이용함

 
## MIME-TYPE
- 서버에서 클라이언트에게 데이터를 전송할 때는 어떤 종류의 데이터를 전송하는지 알려줘야함(클라이언트가 더 빠르게 처리할 수 있도록)

- 톰캣 컨테이너에서 제공하는 여러 가지 전송 데이터 종류 중 하나를 지정해서 전송함

       (톰캣 컨테이너에서 미리 설정해 놓은 데이터 종류 -> MIME-TYPE)

- HTML로 전송시 : text/html (웹 브라우저는 보통 HTML만 인식하므로 이 종류를 사용함)

- 일반 텍스트로 전송시 : text/plain

- XML 데이터로 전송시 : application/xml

- 새로운 종류의 데이터를 지정하고 싶으면 CATALINA_HOME\conf\web.xml 수정


## 서블릿 
```java
package com.it.servlet; //com.it.servlet은 com폴더 안에 it 폴더 안에 servlet 폴더라는 뜻

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class MyServlet
 */

// 만약 사용자가 브라우저에 localhost:9090/day01/MyServlet로 URL 요청을 했다면 이곳으로 와라! 를 @어노테이션으로 표현을 한다.(주소/프로젝트명/서블렛클래스이름)
// @어노테이션 방법은 Servlet 버전 3.0 이상부터 가능하다.
//@WebServlet("/MyServlet") web.xml에서 서블릿을 등록해주었기 때문에 @어노테이션은 사용하면 안된다(중복)
public class MyServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;

	// 요청이 get 방식이면 호출되는 메소드 : 주소창에 정보가 다 보인다. 주소창의 "?" 앞까지가 실제 요청, 뒤로는 사용자가 보내는 데이터(파라미터)
	// request : 사용자의 요청 자체를 답음 객체
	// response : 사용자에게 응답해야될 때 그 응답 자체를 나타내는 객체 
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.setContentType("text/html; charset=UTF-8");

		// HTML 파일을 써서 응답(출력)해주어야 하기 때문 response 객체가 가지고 있는 writer를 받아와야 한다. 
		PrintWriter out = response.getWriter();
		
		String userid = request.getParameter("userid");
		String username = request.getParameter("username");

		// 브라우저에 출력 
		out.println("<html><body><p>안녕하세요 응답한 페이지 입니다.<br>");
		out.println("아이디 : " + userid + "<br>");
		out.println("이름 : " + username + "<br>");

		out.println("</p></body><html>");


	}

	// 요청이 post 방식이면 호출되는 메소드
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		doGet(request, response);
	}

}

```

#### HttpServletRequest request 객체
- 사용자가 HTML Form에 입력한 내용(userid, username, ..)을 request 객체에서 받아온다.
   -  즉 HTTP 프로토콜의 Request 정보를 Servlet에 전달
- 헤더 정보, 파라미터, 쿠키, URI, URL, Body의 Stream 등을 읽어 들이는 메서드가 있다.
   - request.getHeader("원하는 헤더 이름") : 원하는 헤더의 데이터를 가져온다.
   - request.getParameter("가져올 파라미터") : parameter 값을 가져온다(URL 혹은 input tag를 통해 파라미터가 넘어온다). return type : String
   - request.getParameterValues("가져올 파라미터") : name이 같은 parameterreturn가 2개 이상일 때  type : String[]

#### HttpServletResponse response 
- response.setContetType : 인자의 내용에 맞게 동적인 HTML 코드를 생성하여 response 객체에 담아 반환한다.
- PrintWriter out = response.getWriter() : 응답으로 내보낼 출력 스트림을 얻어낸 후 out.println("");으로 스트림에 텍스트를 기록하게 된다.
- out 객체를 통해 Web page(view 생성)에 텍스트를 반환한다.


## web.xml 설정
```html
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd" id="WebApp_ID" version="4.0">
  <display-name>day01</display-name>
  <welcome-file-list>
    <welcome-file>index.html</welcome-file>
    <welcome-file>index.htm</welcome-file>
    <welcome-file>index.jsp</welcome-file>
    <welcome-file>default.html</welcome-file>
    <welcome-file>default.htm</welcome-file>
    <welcome-file>default.jsp</welcome-file>
  </welcome-file-list>

  <!-- com.it.servlet.MyServlet 클래스를 MyServlet이라는 이름의 서블릿으로 등록한다는 뜻! 
	@ 어노테이션과 같은 것이라서 둘 중 하나만 써야된다.
  -->
  <servlet>
	<servlet-name>MyServlet</servlet-name>
	<servlet-class>com.it.servlet.MyServlet</servlet-class>
  </servlet>

  <!-- 브라우저에서 /MyServlet 이라는 요청이 들어오면
	MyServlet 이라는 이름의 서블릿을 작동키라는 뜻!
   -->
  <servlet-mapping>
	<servlet-name>MyServlet</servlet-name>
	<url-pattern>/MyServlet</url-pattern>
  </servlet-mapping>
</web-app>
```
## 정리를 마치며
- 서블릿은 사용하기 불편해서 JSP 언어로 작성한다. JSP로 작성한 것은 다시 서블릿으로 바뀌어서 처리되기 때문에 상관없다.
- Java Servlet 컨테이너 / 웹 서버는 일반적으로 멀티 쓰레드 환경이다. 같은 Servlet에 여러 개의 요청이 동시에 실행될 수 있기 때문
에 runtime도 그에 따라 상이하다. Servlet은 메모리에 한 번 올라오지만 멀티 쓰레드 환경에서 여러 쓰레드가 하나의 Servlet을 공유하기 때문에 Concurrency Control(병행성 제어)가 필요하다고 한다.
![서블렛 병행성](https://user-images.githubusercontent.com/74396651/146058538-f06216e2-962c-4ffa-a524-9723563d6da4.png)
