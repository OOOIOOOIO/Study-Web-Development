## 서블릿(Servlet)


-	JAVA 클래스이다. JAVA 코드 안에서 HTML 문서를 작성할 수 있는 JAVA 프로그램이다. 
	Java Resources의 src 파일에 java 만드는 것처럼 패키지 만들고 클래스만 servlet으로 만들어준다.

- 작동 순서
    - 사용자가 URL 요청 --><br> 
  --> web.xml에 맵핑해놓은 서블릿을 찾는다. --><br>
	--> 해당하는 서블릿의 클래스로 요청을 전송 --> <br>
	--> Thread에 의해 service() 호출 --><br>
	-->요청 방식에 따라 doGet() 혹은 doPost()를 호출한 후 처리<br>
  
- WAS는 Response 객체를 HttpResponse 형태(정적 형태)로 바꿔서 웹 서버에 전달하고 생성된 Thread를 종료한다. 그리고 HttpServletRequest, HttpServletResponse 객체를 제거한다.

- 서블릿은 사용하기 불편해서 JSP 언어로 작성한다. JSP로 작성한 것은 다시 서블릿으로 바뀌어서 처리되기 때문에 상관없다.
