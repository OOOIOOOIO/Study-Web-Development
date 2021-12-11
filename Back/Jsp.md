# JSP

## JSP(Java Server Page)
- &nbsp;Jsp란 HTML 중심으로 Java와 같이 연동하여 사용하는 웹 언어이다.
HTML코드 안에 Java 코드를 작성할 수 있도록 도와주는 언어이다.
지금은 잘 안 쓰고 타임리프와 같은 라이브러리를 사용한다.

## Java에서 웹버전으로 프로젝트와 파일 생성 방법 및 서버 연결
- HTML, CSS 만들 때와 같다

-----------------------------------------
## Script tag
#### 개념 : Script tag를 이용해 HTML 내부에 자바 코드를 넣어 프로그래밍이 가능하도록 만들 수 있다.
- 선언문(declaration) : 자바의 변수나 메소드를 정의하는데 사용되는 태그
  - <%! java code %>

- 스크립틀릿(scriptlet) : 자바의 변수 선언 및 자바 로직 코드를 작성하는데 사용되는 태그
  - <% java code %>

- 표현문(expretion) : 변수, 계산식, 메소드 호출의 리턴값 등을 표현해주는 태그. HTML 문서의 안에 작성한 값이 그대로 표현된다. 타입은 문자열이다.
  - <%= java code %>



## 실습코드

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
