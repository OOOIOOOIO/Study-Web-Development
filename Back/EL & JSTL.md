# EL & JSTP
> &nbsp;라이브러리로 자바 구문을 만들어 놓고 필요할 때마다 꺼내 쓰면 되기 때문에 유지보수에 편하다. JSP 페이지 내에서 자바코드와 HTML코드(태그형태)가 섞여 있으면 가독성이 떨어진다. EL문과 JSTL문을 사용하면 HTML과 태그형태로만 구성된 소스코드를 볼 수 있다.
<br>

## EL(Expression Language) 
- &nbsp;값을 간결하고 간편하게 출력할 수 있도록 해주는 스크립트 언어. JSP 문법을 보완하는 역할을 한다.
<br>

### 기능
- JSP의 4가지 기본 객체가 제공하는 영역의 속성 사용
   - page, requst, session, application의 Attribute, Parameter 속성 사용
   
   - Attribute는 작은 Scope에서 큰 Scope 순으로 값을 찾아온다.(page -> requset -> session -> application)
   
   - Attribute란 메소드를 통해 저장되고 관리되는 데이터를 말한다.<br>
    &nbsp;PageContext, Requst에서 사용될 때<br>
    
   
- 집합 객체에 대한 접근 방법 제공(배열이나 List를 호출해서 쓸 수 있다)하고 JavaBean의 프로퍼티에서도 사용된다.

- 수치연산, 관계연산, 논리연산자 제공
   - EL의 문법 안에서 사용 가능
   ```
   연산자
		/		div		${10/3}, ${10 div 3}
		%		mod
		&&		and
		||		or
		!		not
		>		gt
		<		lt
		>=		ge
		<=		le
		==		eq
		!=		ne

		empty		뒤에 올 값이 비어있으면 true / 아니라면 false
				${empty data} : data의 값이 없으면 true

      예시 ) 
		if(session.getAttribute("loginUser") != null){

		}
      
      (위 아래가 같다)
      
		${not empty loginUser}
   ```

- 자바 클래스 메소드 호출 기능 제공
 
- EL만의 기본 객체 제공
   - param : Parameter의 정보를 가지고 있는 객체이다. (key : value )
   - paramValues : Parameter의 정보를 가지고 있는 객체이고 리턴값은 배열이다. (key : value1, value2, ...)
   - initParam : 초기 파라미터 조회한다.
   - header : Header의 정보를 가지고 있는 객체이다. (key : value)
   - headerValues : Header의 정보를 가지고 있는 객체이고 리턴값은 배열이다. (key : value1, value2, ...)
   - cookie : ${cookie.key,value}로 쿠키값을 조회한다.
   - Scope는 각각의 JSP 기본객체의 pageContext, request, session, application이 setAttribute를 통해 저장한 정보를 가지고 있는 객체이다.
   - pageContext 
   - pageScope 
   - requestScope
   - sessionScope
   - applicationScope
<br>
   
### EL 기본
```jsp
<!-- el_test1.jsp  -->

<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	\${10+20} : ${10+20}<br> <!-- \붙여주면 html처럼 출력된다.  -->
	\${10>3} : ${10>3}<br>
	<%
		//String data = "hello"
		pageContext.setAttribute("data", "hello");
		request.setAttribute("data", "bye");
		session.setAttribute("data", "good night");
	%>
	<!--
		 data는 변수명을 의미하는 것이 아니라 setAttribute()할 때의 key값을 찾는다.
		찾는  우선순위는 pageContext -> request -> session -> application
	 -->
	\${data } : ${data }<br>
	\${requestScope.data } : ${requestScope.data }<br>
	\${sessionScope.data } : ${sessionScope.data }
	
</body>
</html>   

<!-- -------------------------------------------------------------------------------------------  -->
   
   <!-- el_test2.jsp  -->
   
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<!--  -->
	<%
		Cookie cookie = new Cookie("xmas", "12-25");
		response.addCookie(cookie);
	%>
	<form action="el_test3.jsp" method="post">
		아이디 <input type="text" name="id"><br>
		비밀번호<input type="password" name="pw"><br>
		취미<br>
		게임<input type="checkbox" name="hobby" value="게임"><br>
		영화<input type="checkbox" name="hobby" value="영화"><br>
		<input type="submit">
	</form>
</body>
</html>
   
   
<!-- -----------------------------------------------------------------------------------------  -->
   
   <!-- el_test3.jsp  -->
   
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%request.setCharacterEncoding("UTF-8");%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	아이디 ${param.id}<br> <!-- parameter로 날아왔기 때문에 param.파라미터명 으로 꺼내기-->
	비밀번호${param.pw}<br>
	취미${paramValues.hobby[0]}, ${paramValues.hobby[1]}<br> <!-- 같은 id로 여러개 날아오면 배열 -->
	<%-- 취미${paramValues["hobby"][0]}, ${paramValues["hobby"][1]}<br> --%> <!-- 같은 의미 -->
	이름${param.name != null?param.name:"이름없음"}<br> <!-- 삼항연산자 자주 씀 -->
	쿠키값${cookie.xmas.value} <!--쿠키꺼내오기  -->
	
	
</body>
</html>
```
<br>

## JSTL(JSP Standard Tag Library) 
- &nbsp;연산이나 조건문, 반복문을 편하게 처리할 수 있으며, JSP 페이지 내에서 자바코드를 사용하지 않고 로직을 구현할 수 있도록 효과적인 방법을 제공한다. JSP에서 JAVA 코드를 사용할 수 있는 스크립틀릿(<% >%)같은 구문을 쓰면 HTML 태그와 섞여 가독성도 떨어지고 코드가 복잡해질 수 있다. 이를 해결하기 위해 JSTL이라는 태그가 나오게 되었다.
<br>

### 사용하는 법
- JSTL 라이브러리 다운로드
   - archive.apache.org/dist/jakarta/taglibs/standard/binaries/ 들어가서
   - akarta-taglibs-standard-1.1.2.zip 다운로드하기
   - Build Path에 다운받은 .jar파일 등록 및 WEB_INF -> lib 에도 넣기
   - 이후 jsp파일 맨 위에 <%@ taglib uri="http://java.sun.com/jsp/jstl/?" prefix="?"%> 작성하기.
<br>

### Core 태그( prefix="c")
- 태그 종류
   - set : JSP에서 사용될 변수 선언
   - remove : 선언한 변수 제거
   - if : if, else if를 사용해 조건문 처리
   - choose(when, otherwise) : 다중조건을 사용할 때 사용
   - forEach : JAVA의 for문과 동일
   - forTokens : JAVA의 StringTokenizer 클래스의 역할과 동일
   - import : URL을 사용하여 다른 자원의 결과를 삽입
   - redirect : 지정한 경로로 이동
   - url : URL을 작성
   - catch : 예외처리
   - out : JspWriter 클래스의 역할과 동일
<br>

### JSTL 기본
```jsp 
<!-- jstl_test1.jsp -->

<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>    
<!-- jstl 라이브러리 추가하기 ,prefix 정해줘야 그거로 시작하는 거 쓸 수 있음-->
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%> 
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h2>JSTL 변수</h2>
	<!-- set var="key값,변수명" : 변수 선언  == pageContext.setAttribute("userid". "apple")-->
	<c:set var="userid" value="apple" /><br>
	
	<!-- out value="": 출력하기 -->
	회원 아이디 : <c:out value="${userid}"/><br>
	회원 아이디 : ${userid }<br>
	
	<!-- scope으로 어느 타입으로 선언할 지도 정해줄 수 있다. == session.setAttribute("userid". "apple") -->
	<c:set var="userid" scope="session">banana</c:set> <!-- value로 안주고 content로 줘도 똑같이 value로 들어감 -->
	세션의 아이디 : ${sessionScope.userid }
	
</body>
</html>

<!-- -------------------------------------------------------------------------------------------  -->

<!-- jstl_test2.jsp -->

<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"  %>    
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h2>JSTL 제어문(조건문)</h2>
	<c:set var="num" value="100"></c:set>
	
	<p>c:if</p>
	<!-- if test="조건", 조건 el문 사용 -->
	<c:if test="${num>50}">
		이 수는 50보다 큽니다!<br>
	</c:if>
	<c:if test="${num>30}">
		이 수는 30보다 큽니다!<br>
	</c:if>
	
	<hr>
	
	<p>c:choose(if ~ else if ~ else 문도 choose로 표현. )</p>
	<!-- switch case와 비슷하다. choose태그 안에는 when만 있어야 한다. when은 if, otherwise는 else 느낌-->
	<c:choose>
		<c:when test="${num>50 }">
			이 수는 50보다 큽니다!>
		</c:when>
		<c:when test="${num>30 }">
			이 수는 30보다 큽니다!>
		</c:when>
		<c:when test="${num>10 }">
			이 수는 10보다 큽니다!>
		</c:when>
		<c:otherwise>
			이 수는 그 외의 숫자입니다.
		</c:otherwise>
	
	</c:choose>
</body>
</html>

<!-- -------------------------------------------------------------------------------------------  -->

<!-- jstl_test3.jsp -->
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h2>JSTS 제어문(조건문) 예제 : 현재 페이지에서 ㅇ</h2>
	<c:choose>
		<c:when test="${empty param.userid}"> <!-- 파라미터(인풋)가 없을 때는 기본(현재) 페이지로 들어가기 -->
			<form><!-- action을 정해주지 않으면 현재 페이지로 날아가고, 방식은 기본이 get이다. -->
				아이디<input type="text" name="userid"><br>	
				비밀번호<input type="password" name="userpw"><br>	
				<input type="submit"><br> 
			</form>
		</c:when>
		<c:otherwise> <!-- 만약 파라미터가 날아왔다면(인풋에 입력해서 넘어왔다면) 다른 페이지 -->
			<c:choose>
				<c:when test="${param.userid == 'admin'}">
					<span style="color:red;">관리자 페이지</span>
				</c:when>
				<c:when test="${param.userid == 'apple'}">
					<b style="color:red;">김사과님 어서오세요</b>
				</c:when>
				<c:otherwise>
					<script>
						alert("비회원 입니다! 돌아가세요!");
						history.go(-1) /* history.go(-1) : 뒤로가기 */
					
					</script>
				</c:otherwise>
			
			</c:choose>
		</c:otherwise>
	</c:choose>
</body>
</html>


<!-- -------------------------------------------------------------------------------------------  -->

<!-- jstl_test4.jsp -->
<%@page import="java.util.HashMap"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h2>JSTL 제어문(반복문)</h2>
	<!-- forEach == int i=0; i<=9 step은 파이썬의 range(0,10,2)-->
	<c:forEach var="i" begin="0" end="9" step="1">
		<p id="pTag${i}">${i}</p>		
	</c:forEach>
	
	<hr>
	
	<!-- 이런식으로도 가능. jstl과 표현문만. el문에서는 안됨 -->
	<c:set var="arData" value="<%=new int[]{10, 20, 30, 40, 50} %>"/>
	<c:forEach var="i" begin="0" end="4" step="1" >
		${arData[i]}
	</c:forEach><br>
	
	<!-- 빠른 for문  -->
	<c:forEach var="data" items="${arData}">
		${data}
	</c:forEach>
	
	<hr>
	
	<!-- map도 for에 들어감 -->
	<%
		HashMap<String, Integer> map = new HashMap<>();
		map.put("하나",1);
		map.put("둘",2);
		map.put("셋",3);
		pageContext.setAttribute("map", map);
	%>
	<c:forEach var="entry" items="${map}">
		${entry.key } : ${entry.value }<br>
	</c:forEach>
</body>
</html>
```













