# EL & JSTP
> &nbsp;라이브러리로 자바 구문을 만들어 놓고 필요할 때마다 꺼내 쓰면 되기 때문에 유지보수에 편하다. JSP 페이지 내에서 자바코드와 HTML코드(태그형태)가 섞여 있으면 가독성이 떨어진다. EL문과 JSTL문을 사용하면 HTML과 태그형태로만 구성된 소스코드를 볼 수 있다.
## EL(Expression Language) 
- &nbsp;값을 간결하고 간편하게 출력할 수 있도록 해주는 스크립트 언어. JSP 문법을 보완하는 역할을 한다.

### 기능
- JSP의 4가지 기본 객체가 제공하는 영역의 속성 사용
   - page, requst, session, application의 Attribute, Parameter 속성 사용
   
   - Attribute는 작은 Scope에서 큰 Scope 순으로 값을 찾아온다.(page -> requset -> session -> application)
   
   - Attribute란 메소드를 통해 저장되고 관리되는 데이터를 말한다.<br>
    &nbsp;PageContext, Requst에서 사용될 때<br>
    
   
- 집합 객체에 대한 접근 방법 제공(배열이나 List를 호출해서 쓸 수 있다)하고 JavaBean의 프로퍼티에서도 사용된다.

- 수치연산, 관계연산, 논리연산자 제공
   - EL의 문법 안에서 사용 가능

- 자바 클래스 메소드 호출 기능 제공
 
- EL만의 기본 객체 제공
   - param : 파라미터값을 얻어온다. (key : value )
   - paramValues : 파라미터값을 여러개 얻어온다. (key : value1, value2, ...)
   - initParam : 초기 파라미터 조회한다.
   - header : 헤더값을 얻어온다. (key : value)
   - headerValues : 헤더값을 여러개 얻어온다. (key : value1, value2, ...)
   - cookie : ${cookie.key,value}로 쿠키값을 조회한다.
   - pageContext : pageContext 객체를 참조할 때 사용한다.
   - pageScope : pageScope에 접근할 때 사용한다.
   - requestScope: requestScope에 접근할 때 사용한다.
   - sessionScope: sessionScope에 접근할 때 사용한다.
   - applicationScope: applicationScope에 접근할 때 사용한다.


## JSTL(JSP Standard Tag Library) 
- &nbsp;연산이나 조건문, 반복문을 편하게 처리할 수 있으며, JSP 페이지 내에서 자바코드를 사용하지 않고 로직을
구현할 수 있도록 효과적인 방법을 제공한다.

### ㅇ
