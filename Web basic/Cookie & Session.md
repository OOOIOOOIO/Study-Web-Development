# Cookie & Session

- &nbsp;HTTP(Hypertext Transfer Protocol)는 인터넷상에서 데이터를 주고 받기 위해 클라이언트-서버 모델을 따르는 통신규약을 말한다. 이 HTTP는 비연결성(Connection)과 비상태성(Stateless)이라는 특징이 있다. 이는 서버의 자원을 절약하기 위해 모든 사용자의 요청마다 연결과 해제의 과정을 거치기 때문에 연결 상태가 유지되지 않고, 연결 해제 후에 상태 정보가 저장되지 않는다는 것이다. 하지만 쿠키와 세션을 이용하면 이런 비연결성과 비상태성을 보완할 수 있다.
<br>

# Cookie
- ##개념
   - &nbsp;웹 브라우저가 보관하고 있는 데이터로, 웹 서버에 요청을 보낼 때 쿠키를 헤더에 담아서 전송한다. 또한 브라우저마다 저장되는 쿠키가 다르기 때문에 크롬으로 남긴 쿠키와 인터넷 익스플로어에서 남긴 쿠키는 서로 다르므로 호환되지 않고 서버에서는 브라우저가 다르면 다른 사용자로 인식한다.
   - text형식으로 저장한다.
 
<br>

- ## Stateless 중요!
   - 기본적으로 HTTP는 무상태(Stateless) 프로토콜이다.
   - 클라이언트와 서버가 요청과 응답을 주고 받으면 연결이 끊어진다.
   - 클라이언트가 다시 요청하면 서버는 이전 요청을 기억하지 못한다.
   - 클라이언트와 서버는 서로 상태를 유지하지 않는다!

## 쿠키 미 사용 시
![image](https://user-images.githubusercontent.com/74396651/178660923-2f088205-2d58-479f-9a31-c8c12a33ccfb.png)
- 모든 요청에 사용자 정보가 포함되도록 개발 해야된다.

<br>

## 쿠키 사용 시

![image](https://user-images.githubusercontent.com/74396651/178661022-8cce5766-c601-4cfc-bac9-edd2ed0e924f.png)

<br>

![image](https://user-images.githubusercontent.com/74396651/178661074-5edc2934-49e1-4ef6-97a3-6d8e4ebbc891.png)

<br>

![image](https://user-images.githubusercontent.com/74396651/178661087-a423eb0e-7ab9-4dd9-b32c-53269b69f17e.png)

<br>


<br>

- ## 쿠키의 장단점
   - &nbsp;클라이언트(사용자)의 일정 폴더에 정보를 저장하기 때문에 서버의 부하를 줄일 수 있다. 하지만 데이터가 사용자 컴퓨터에 저장되기 때문에 보안의 위협을 받을 수 있다. 또한 데이터 저장 용량에 한계가 있으며 일반 사용자가 브라우저 내의 "쿠키차단" 기능을 사용하면 무용지물이 되어버린다. 또한 쿠키 정보는 항상 서버에 전송되기 때문에 네트워크 트래픽 추가를 유발할 위험성이 있기에 최소한의 정보만 사용해야한다(세션 id, 인증 토큰). 만약 서버에 전송하지 않고 웹 브라우저 내부에 데이터를 저장하고 싶다면 웹 스토리지(localStorage, sessionStorage)를 사용한다.

<br>

- ## 쿠키의 사용목적
   - 세션 관리(Session Management) : 로그인, 사용자 닉네임, 접속 시간, 장바구니 등의 서버가 알아야할 정보들을 저장한다.
   - 개인화(Personalization) : 사용자마다 각각 다르게 적절한 페이지를 보여줄 수 있다.
   - 트래킹(Tracking) : 사용자의 행동과 패턴을 분석하고 기록한다.

<br>

- ##사용 예시
   - ID 저장, 로그인 상태 유지
   - 일주일간 다시 보지 않기 같은 기능
   - 최근 검색한 상품들을 광고에서 추천
   - 쇼핑몰 장바구니 기능

<br>

## 쿠키 생명주기
![image](https://user-images.githubusercontent.com/74396651/178661890-40f2de1a-c7a4-416d-bfba-fcb047d6fda1.png)

<br>

## 쿠키 도메인
![image](https://user-images.githubusercontent.com/74396651/178661950-2fba3721-09d5-43c0-ba5c-77d661200d40.png)

<br>

## 쿠키 경로
![image](https://user-images.githubusercontent.com/74396651/178661999-f302608d-cc15-4abe-bc6d-20b3e5d1a8c7.png)

<br>

## 쿠키 보안
![image](https://user-images.githubusercontent.com/74396651/178662041-ab000abc-9bd6-4ddc-863d-c4e83cb398ae.png)

<br>

```
쿠키 생성
	Cookie 객체명 = new Cookie("Key", "Value");


쿠키 저장
	사용자 컴퓨터에 저장해야 하므로 응답을 통해서 생성한 쿠키를 보내주어야 한다.
	response.addCookie(쿠키객체);


쿠키 사용
	사용자가 요청 때 함께 보내주는 헤더에서 쿠키를 꺼내 사용한다.
	request.getHeader("Cookie") : 요청에 있는 Header 중에 Cookie 라는 이름의 헤더가 있는지 확인
				null이라면 전송된 쿠키가 없다는 뜻

	request.getCookies() : 전송된 쿠키 객체들의 배열
	
	쿠키객체.getName() : 쿠키이름(key)
	쿠키객체.getValue() : 쿠키값(value)

쿠키 삭제
	쿠키의 유효기간을 설정해주는 방식으로 삭제할 수 있다.
	쿠키객체.setMaxAge(n) : n초 만큼 유지하다가 쿠키 삭제하도록 설정

쿠키를 수정하거나 삭제시 설정된 쿠키 객체를 다시 response를 통해 사용자 컴퓨터에 추가해주어야 한다.(덮어씌우기)
```

### 쿠키가 있기 때문에 여러 페이지를 이동할 때마다 로그인을 하지 않고 사용자의 정보를 유지할 수 있는 것이다. 만약 쿠키가 없다면 다음 페이지로 정보를 파라미터를 통해 넘겨주어야 한다.
<br>


## Session
- 개념
   - JSP 내장객체로서 브라우저마다 한 개씩만 존재하고 브라우저 종료시 소멸된다. 예를 들어 로그인한 사용자에 대해서만 세션을 생성하는 것이 아니라, 로그아웃을 할 경우도 새로운 사용자로 인식해서 새로운 세션이 생성되는 것이다. 
   - Object 형식으로 저장한다.

- 세션의 장단점
   -  JSP에서만 접근할 수 있기 때문에 보안성이 좋고, 저장 용량의 한계가 거의 없다. 하지만 서버에 데이터를 저장하므로 부하에 걸릴 수 있다.(HTML로 접근할 경우 세션이 없기 때문에 세션을 사용할 수 없다.) 

```
세션 생성
	session.setAttribute("Key", "Value");



세션 사용
   session.getAttribute의 return type이 Object이므로 알맞게 형변환해서 사용해야한다.
   String userId = (String)session.getAttribute("Key");

	

세션 삭제
   세션의 특정 속성 삭제하기
	session.removeAttribute("Key");
   
   세션의 모든 속성 삭제
   session.invalidate();
   
```
<br>

## 쿠키와 세션의 비교
> &nbsp; 쿠키는 자동완성이나, 팝업, 장바구닝 등 클라이언트의 편의에 중점을 두고 삭제 혹은 조작되더라도 큰 지장이 없는 수준의 정보를 저장하는 데 사용된다.<br>
&nbsp; 세션은 다른 누군가에게 노출되면 안되는 중요한 정보들을 서버 안에 저장하여 다룬다.<br>
&nbsp; 쿠키의 리소스는 클라이언트의 메모리이고 세션의 리소스는 서버의 메모리이다. 그리고 쿠키는 클라이언트 모르게 접속되는 사이트에 의하여 설정될 수 있는 쿠키가 있을 수도 있기 때문에 한 도메인 당 20개, 하나의 쿠키 당 4kb로 제한하며 세션은 개수나 용량에 제한이 없지만 서버에 부하를 줄 수 있기 때문에 가급적 적게 사용해야 한다.

<br>
<br>
<br>
<br>

참고 인프런 이영한님 강의
