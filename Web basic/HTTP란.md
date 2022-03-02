# HTTP

<img src="https://mdn.mozillademos.org/files/13673/HTTP%20&%20layers.png" style="width:700px; "/>

## HTTP(HyperText Transfer Protocol)
- &nbsp;초기에는 HTML과 같은 하이퍼미디어 문서를 주로 전송했지만, 최근에는 Plain text, JSON, XML 등 다양한 형태의 정보도 전송하는 애플리케이션 레이어 프로토콜이다.

- &nbsp;일반적으로 안정적인 TCP/IP 레이어를 기반으로 사용하는 응용 프로토콜이다.

- &nbsp;HTTP는 클라이언트가 요청을 생성하기 위한 연결을 연 후 응답을 받을 때까지 대기하는 전통적인 클라이언트-서버 모델을 따른다.

- &nbsp;HTTP는 무상태 프로토콜이며, 이는 서버가 자원을 절약하기 위해 모든 사용자의 요청마다 연결과 해제의 과정을 거치기 때문에 어떠한 상태나 데이터를 유지하지 않음을 의미한다.

<br>

## HTTP 작동 방식

#### &nbsp;클라이언트가 어떠한 요청(서비스)를 URI를 통해 서버에 요청(Request)을 하게 되면 서버에서는 해당 요청에 대한 결과를 응답(Response)하는 형태로 작동한다.

- 클라이언트
   - 서버에게 요청을 보내는 리소스 사용자 
   - ex) 웹 브라우저, 모바일 애플리케이션, IOT, ...

- 서버
   - 클라이언트가 보낸 요청에 대한 응답을 제공하는 리소스 관리자

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdcUnst%2FbtqWXaFbg2p%2F86WwcX7OsOvnkoO71T0RbK%2Fimg.png" width=800px height=600px>

<br>

## HTTP 요청(Request)

<br>

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fc7mI3U%2FbtqWX45M76d%2FgGoVLK6rcUJhekrxMcq6a1%2Fimg.png">

- header에서 한 줄 띄고 "본문"이라는 것이 적혀 있는데, 본문엔 요청을 할 떄 같이 보낼 데이터가 담겨져있다.

<br>

### 메서드(Method)

![](https://user-images.githubusercontent.com/4013025/48322141-cf7af680-e604-11e8-8a76-ae4d92a83793.png)

![](https://static.packt-cdn.com/products/9781838983994/graphics/image/C15309_01_02.jpg)
- GET
   - 특정 리소스를 받기 위한 요청이다. 따라서 리소스의 생성, 수정 및 삭제 등에 사용해서는 안된다. 

- POST
   - 리소스를 생성하거나 컨트롤러를 실행하는 데 사용한다.

- PUT
   - 변경 가능한 리소스를 업데이트하는 데 사용되며 항상 리소스의 식별 정보를 포함해야 한다.

- PATCH
   -  변경 가능한 리소스의 부분 업데이트에 사용되며 항상 리소스 식별 정보를 포함해야 한다.(잘 사용하지 않는다. PUT을 사용해 전체 객체를 업데이트하는 것이 관례라고 한다.)

- DELETE
   - 특정 리소스를 제거하는 데 사용한다.(일반적으로 Request body가 아닌 URI경로에 제거하려는 리소스의 ID를 전달한다.)

- HEAD
   - 클라이언트가 본문 없이 리소스에 대한 헤더만 검색하는 경우 사용한다.(일반적으로 클라이언트가 서버에 리소스가 있는지 확인하거나 메타 데이터를 읽으려는 때만 GET 대신 사용한다.)

- OPTIONS
   - 클라이언트가 서버의 리소스에 대해 수행 가능한 동작을 알아보기 위해 사용한다.(일반적으로 서버는 이 리소스에 대해 사용할 수 있는 HTTP 요청 메서드를 포함하는 Allow 헤더를 반환한다. CORS에 사용한다고 한다.)

<br>

## HTTP 응답(Response)

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FCmjnf%2FbtqWTYTN3X1%2F34p8xLsQtEIk0xMzyjIw8k%2Fimg.png">

- 200과 201은 성공적으로 요청과 응답되었다는 뜻이다.

- 마찬가지로 응답에도 본문이 존재한다. 요청에 대한 데이터가 담겨있으며 브라우저는 응답 메세지에 담겨있는 HTML을 받아 화면에 렌더링을 한다.






