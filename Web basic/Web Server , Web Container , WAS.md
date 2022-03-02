# Web Server와 WAS 그리고 Web Container<br><br>

## Web Server<br>
- &nbsp;Web Server 개념<br>
   - &nbsp;Web Server는 하드웨어(Web Sever가 설치되어 있는 컴퓨터)
   - &nbsp;소프트웨어(웹 브라우저의 클라이언트 등으로부터 HTTP 요청을 받아 정적인 컨텐츠를 제공하는 컴퓨터 프로그램)로 구분된다.
   - &nbsp;사용자가 요청하는 정적 컨텐츠(이미지 파일 등)를 HTTP를 통해 제공해주는 서버이다. 

- &nbsp; Web Server 기능<br>
   - HTTP를 기반으로 클라이언트의 요청을 서비스 하는 기능을 담닫한다.
   - 정적인 요청이 클리어언트로부터 요청될 경우 해당 요청은 바로 클라이언트에게 응답한다.
   - 동적인 요청이 클라이언트로부터 요청될 경우 해당 요청은 웹 서버에서 처리할 수 없기 때문에 컨테이너(Container)로 보내진다. 이후 컨테이너에서 처리한 결과를 다시 클라이언트에게 응답한다.


- ex) Apache, Nginx, IIS ...

<br>

## WAS(Web Application Server)<br>
- &nbsp;WAS 개념<br>
   - &nbsp;Web Server + Web Container. 웹 서버로부터 오는 동적인 요청을 처리하는 서버이다.
   - &nbsp;HTTP를 통해 컴퓨터나 장치에 애플리케이션을 수행해주는 MiddleWare(미들웨어:소프트웨어 엔진)이다.

- &nbsp;WAS 목적, 역할 : Web Sever 기능들을 구조적으로 분리하여 처리하고자하는 목적으로 제시되었다.<br>
   - &nbsp;분산 트랜잭션(논적인 작업 단위), 보안, 메시징, 쓰레드 처리 등의 기능을 처리하는 분산 환경에서 사용된다.
   - &nbsp;주로 DB와 같이 수행된다.
   - &nbsp;현재 WAS 내에 있는 Web Server는 기존의 WebServer와 정적인 컨텐츠를 처리하는 데 있어서 성능상 큰 차이가 없다.

- &nbsp;WAS 주요 기능<br>
   - &nbsp;프로그램 실행 환경과 DB 접속 기능 제공
   - &nbsp;여러 개의 트랙잭션 관리 가능
   - &nbsp;업무를 처리하는 비즈니스 로직을 수행
   
- ex) Tomcat, WebSphere, JBoss, JEUS, ...           


## Web Container<br>
- Web Container<br>
   - &nbsp;동적인 데이터 요청(DB 접근 연산 등)이 들어왔을 때 서버(Server)가 연산을 요청하는 곳.
서버(Server)에서 들어온 동적인 요청의 처리가 끝나면 정제된(Static한) 데이터로 서버(Server)에 돌려준다.
- ex) JSP, Servlet

![Web Application Server](https://gmlwjd9405.github.io/images/web/webserver-vs-was1.png)<br>

<br>

## 왜 Web Server와 WAS를 구분하는가<br>
- &nbsp;그 이유는 클라이언트에 의해 콘텐츠가 서버에 요청 될 때를 생각해보면 된다. 클라이언트는 정적, 동적 2가지 컨텐츠를 요청할 수 있다. 만약 정적인 경우라면 WAS(Web Container)까지 가지 않고 Web Server 단에서 요청을 처리할 수 있을 것이다. 만약 Web Sever에서 동적처리까지 할당한다면 사용자가 원하는 요청에 대한 결과값을 모두 미리 만들어 놓고 제공을 해야하는데 이는 자원도 절대적으로 부족하고 비효율적이기에 동적 컨텐츠일 경우 WAS를 통해 데이터를 DB에서 가져와 사용자의 요청에 맞게 적절한 컨텐츠를 제공한다. <br>&nbsp;따라서 Web Server에서는 정적인 컨텐츠만 처리하도록, WAS에서는 동적인 컨텐츠를 처리하도록 하여 기능을 분배해 전체 서버의 부담을 줄일 수 있다.

- &nbsp;그렇다면 WAS에 있는 서버에서 정적인 컨텐츠까지 처리하면 되지 않을까 생각할 수도 있다. 서버 하나로 모든 걸 처리할 수 있다면 오히려 편리할 수 있다고 생각할 수도 있지만 앞서 말했듯이 WAS와 Web Server를 분리한다면 서버의 부담을 줄이는 것을 포함해 여러 이점이 있다.<br>

<br>

&nbsp;우선 기능을 분리하는 경우를 살펴보자.<br>

   - &nbsp;WAS는 DB 조회나 다양한 로직을 처리하느라 바쁘기 때문에 단순 정적 컨텐츠는 WebServer에서 빠르게 클라이언트에게 응답하는 것이 효율적이다.

   - &nbsp;또한 WAS는 기본적으로 동적 컨텐츠를 제공하기 위한 목적으로 존재한다. 만약 정적 컨텐츠의 요청까지 WAS에서 처리한다면 서버의 부하가 커지고 동적 컨텐츠에 대한 처리 속도가 떨어지게 될 것이다. 이는 목적성에 맞지 않는다. 

<br>

&nbsp;다음으로 보안 강화의 경우가 있다.<br>
   - &nbsp;SSL에 대한 암-복호화 처리에 Web Server가 사용된다.

<br>

&nbsp;마지막으로 자원 이용의 효율성 및 장애 극복, 베포와 유지보수의 편의성의 측면이 있다.<br>

   - &nbsp;WAS는 여러개가 연결 가능하기 때문에 Load Balancing을 위해 Web Server를 사용한다.
   
   - &nbsp;fail over(장애 극복), fail back 처리에 유리하다. 특히 대용량 웹 어플리케이션 같은 경우 여러개의 서버를 사용하기 때문에 Web Server와 WAS를 분리하여 무중단 배포(운영)에 대한 장애 극복에 쉽게 대응할 수 있다. 예를 들어 앞 단의 WebServer에서 오류가 발생한 WAS를 이용하지 못하도록 한 후 WAS를 재시작함으로써 사용자는 오류를 느끼지 못하고 이용할 수 있다.
   
   - &nbsp;여러 웹 애플리케이션 서비스가 가능한데, 하나의 서버에서 PHP Application과 Java Application을 함께 사용하는 경우이다. 또한 접근 허용 IP를 관리하고 2대 이상의 서버에서 세션 관리 등도 Web Sever에서 처리하면 효율적이기에 Web Server와 WAS를 분리한다.
   
<br>

## Web Service Architecture<br>
1. Client --> Web Server --> DB
2. Client --> WAS --> DB
3. Client --> Web Server --> WAS --> DB
![Client --> Web Server --> WAS --> DB](https://gmlwjd9405.github.io/images/web/web-service-architecture.png)<br>


- &nbsp;3번의 동작 과정<br>
   - &nbsp;Web Server는 웹 브라우저 클라이언트로부터 HTTP 요청을 받는다.
   - &nbsp;Web Server는 클라이언트의 요청(Request)을 WAS에 보낸다.
   - &nbsp;WAS는 관련된 Servlet을 메모리에 올린다.
   - &nbsp;WAS는 web.xml을 참조하여 해당 Servlet에 대한 Thread를 생성한다. (Thread Pool 이용)
   - &nbsp;HttpServletRequest와 HttpServletResponse 객체를 생성하여 Servlet에 전달한다.
      - &nbsp;5-1. Thread는 Servlet의 service() 메서드를 호출한다.
      - &nbsp;5-2. service() 메서드는 요청에 맞게 doGet() 또는 doPost() 메서드를 호출한다.
   - &nbsp;doGet() 또는 doPost() 메서드는 인자에 맞게 생성된 적절한 동적 페이지를 Response 객체에 담아 WAS에 전달한다.
   - &nbsp;WAS는 Response 객체를 HttpResponse 형태로 바꾸어 Web Server에 전달한다.
   - &nbsp;생성된 Thread를 종료하고, HttpServletRequest와 HttpServletResponse 객체를 제거한다.

<br>

## 결론
- &nbsp;Tomcat같은 WAS 앞 단에 Apache, IIS 같은 Web Server를 연동한다. 이는 위에 말했듯이 기능을 분리하면서 얻는 성능 향상, 보안상의 이유 그리고 효율성 및 장애극복, 유지보수 편의성 등이 있기 때문이다.<br>
&nbsp;따라서 우리는 Web Server와 WAS의 목적에 맞게, 기능에 맞게 사용해야할 것이다.

<br>

## 용어 정리<br>
- &nbsp;정적 컨텐츠(Static) : DB에서 정보를 가져오거나 별도로 서버에서의 처리가 없어도 사용자들에게 보여줄 수 있는 컨텐츠. 어떠한 사용자가 오던 간에 동일한 컨텐츠를 보여준다.<br>     
ex) HTML, CSS, JS, Image, ...

- &nbsp;동적 컨텐츠(Dynamic) : 서버처럼 DB에서 정보를 가져오는 것 같이 어떠한 요청에 의해 서버가 일을 수행하고 그에 대한 결과를 보여주는 컨텐츠. 사용자들마다 각기 다른 페이지를 보여줄 수 있다.<br>     
![정적, 동적](https://gmlwjd9405.github.io/images/web/static-vs-dynamic.png)<br>

- &nbsp;MiddleWare(미들웨어) : 비즈니스 로직을 Client와 DBMS 사이의 Middleware Server에서 동작하게 함으로써 Client의 입력에 맞는 출력을 보여준다. 흔히 Web Server는 Apache, WAS(MiddleWare Server)는 Tomcat이라 의미한다.<br>
   - &nbsp;Client <--> MiddleWare Server <--> DB server(DMBS)
   1. &nbsp;Client는 단순히 요청만 중앙에 있는 MiddleWare
   2. &nbsp;MiddleWare Sever에 대부분의 로직이 수행된다.
   3. &nbsp;이때, 데이터를 조작할 일이 있으면 DBMS에 부탁한다.
   4. &nbsp;로직의 결과를 Client에게 전송한다.
   5. &nbsp;Client는 그 결과를 화면에 보여준다.<br> 
![미들웨어](https://mblogthumb-phinf.pstatic.net/20160707_205/kbh3983_1467881125343z0GAF_PNG/aa.PNG?type=w2)<br>

- &nbsp;Load Balancing : 하나의 인터넷 서비스가 발생하는 트래픽이 많을 때 여러 대의 서버가 분산처리하여 서버의 로드율, 부하량이 증가하고 속도가 저하되기 때문에 이를 고려하여 적절히 분산처리해 해결해주는 서비스이다.<br>

- &nbsp;무중단 배포(운영) : 서비스 장애와 배포의 부담을 최소화하기 위해 운영 중인 서비스를 중단하지 않고 신규 소프트웨어를 배포하는 기술이다.
