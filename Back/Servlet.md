## 서블릿(Servlet)


- 서블릿이란 JAVA 클래스이다. JAVA 코드 안에서 HTML 문서를 작성할 수 있는 JAVA 프로그램이다. 

- Java Resources의 src 파일에 java 만드는 것처럼 패키지 만들고 클래스만 servlet으로 만들어준다.

- 서블릿 작동 순서
    - 사용자가 URL 요청 --><br> 
  --> web.xml에 맵핑해놓은 서블릿을 찾는다. --><br>
  --> 해당하는 서블릿의 클래스로 요청을 전송 --> <br>
  --> Thread에 의해 service() 호출 --><br>
  -->요청 방식에 따라 doGet() 혹은 doPost()를 호출한 후 처리<br>
  
- WAS는 Response 객체를 HttpResponse 형태(정적 형태)로 바꿔서 웹 서버에 전달하고 생성된 Thread를 종료한다. 그리고 HttpServletRequest, HttpServletResponse 객체를 제거한다.


## 응답 처리 방법
- doGet()이나 doPost() 메서드 안에서 처리

- HttpServletResponse 객체 이용

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

			// HTML 파일을 써서 응답해주어야 하기 때문 response 객체가 가지고 있는 writer를 받아와야 한다. 
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




## web.xml 설정
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

- 서블릿은 사용하기 불편해서 JSP 언어로 작성한다. JSP로 작성한 것은 다시 서블릿으로 바뀌어서 처리되기 때문에 상관없다.
