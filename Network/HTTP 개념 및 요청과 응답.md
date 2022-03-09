# HTTP(HyperText Transfer Protocol)

<br>

<img src="https://mdn.mozillademos.org/files/13673/HTTP%20&%20layers.png" style="width:700px; "/>

<br>

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


### HTTP 메서드(Method)

![](https://user-images.githubusercontent.com/4013025/48322141-cf7af680-e604-11e8-8a76-ae4d92a83793.png)

![](https://static.packt-cdn.com/products/9781838983994/graphics/image/C15309_01_02.jpg)

<br>


#### 일반적으로 웹에서 REST API를 작성할 때 HTTP Methods를 이용해 CRUD를 표현한다. REST API, REST 방식은 따로 정리하겠다.
[REST 정리](https://github.com/OOOIOOOIO/Study_Web_development/blob/master/Network/REST.md)

<br>

#### 종류

- GET
   - 주로 데이터를 읽거나(Read) 검색(Retrieve)할 때에 사용되는 메소드이다. 만약  GET 요청이 성공적으로 이루어진다면 XML이나 JSON과 함께
   200 HTTP 응답 코드를 리턴한다. 에러가 발생하면 주로 404(Not found) error 혹은 400(Bad request) 에러가 발생한다.
   - HTTP 명서에 의하면 GET 요청은 오로지 데이터를 읽을 때에만 사용된다. 수정, 삭제와 같은 데이터를 조작하는 연산에 사용하면 안된다
   - GET Method는 idempotent하다
```
ex) url

GET /polite/1998

```

- POST
   - 주로 새로운 리소스를 생성(Create)할 때 사용된다. 조금 더 구체적으로 POST는 하위 리소스들을 생성하는데 사용된다. 성공적으로 Creation을 
   완료하면 201(Created) HTTP 응답을 반환한다.
   - Post Method는 idempotent하지 않다.
```
ex) json 형식

POST /polite
body : {name : "SeongHo Kim"}
Content-Type : "application/json"

```

- PUT
   - 변경 가능한 리소스를 업데이트하는 데 사용되며 항상 리소스의 식별 정보를 포함해야 한다.
   - Put Method는 idempotent하다.
```
ex) json 형식

PUT /polite
body : {name : "SEONGHO KIM"}
Content-Type : "application/json" // 없어도 됨

```


- DELETE
   - 특정 리소스를 제거하는 데 사용한다.(일반적으로 Request body가 아닌 URI경로에 제거하려는 리소스의 ID를 전달한다.)
   - 데이터를 삭제하는 것이기 때문에 요청 시 Body와 Content-Type이 비워져 있다.
```
ex) url

DELETE /polite/1998

```

- PATCH
   -  변경 가능한 리소스의 부분 업데이트에 사용되며 항상 리소스 식별 정보를 포함해야 한다.(잘 사용하지 않는다. PUT을 사용해 전체 객체를 업데이트하는 것이 관례라고 한다.)

- HEAD
   - 클라이언트가 본문 없이 리소스에 대한 헤더만 검색하는 경우 사용한다.(일반적으로 클라이언트가 서버에 리소스가 있는지 확인하거나 메타 데이터를 읽으려는 때만 GET 대신 사용한다.)

- OPTIONS
   - 클라이언트가 서버의 리소스에 대해 수행 가능한 동작을 알아보기 위해 사용한다.(일반적으로 서버는 이 리소스에 대해 사용할 수 있는 HTTP 요청 메서드를 포함하는 Allow 헤더를 반환한다. CORS에 사용한다고 한다.)

- RFC

- Idempotent : 멱등성
   - 여러번 수행핻 같은 결과를 도출한다는 뜻. 호출로 인해 데이터의 변형이 없다는 뜻이다. 

<br>

## HTTP 응답(Response)

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FCmjnf%2FbtqWTYTN3X1%2F34p8xLsQtEIk0xMzyjIw8k%2Fimg.png">

### 1XX : Information responses
>&nbsp;상태코드가 "1"로 시작하는 경우는 서버가 요청을 받았으며, 서버에 연결된 클라이언트는 작업을 계속 진행하라는 의미이다. 해당 코드는 HTTP 1.0에서 지원되지 않는다.

- 100 : continue
   - 작업이 진행 중임을 나타내는 응답코드이다. 현재까지의 진행상태에 문제가 없으며, 클라이언트가 계속해서 요청을 하거나 이미 요청을 완료한 경우 무시하라는 것을 의미한다.

- 101 : Switching Protocol
   - 클라이언트에 의해 보내진 업그레이드 요청 헤더에 대한 응답으로 보내진다. 서버에서 프로토콜을 변경할 것임을 알려줌 해당 코드는 Websocket 프로토콜 전환 시에 사용된다.

- 102 : Processing(WebDAV)
   - 서버가 요청을 수신하였으며 이를 처리하고 있지만, 아직 제대로 된 응답을 알려줄 수 없음을 나타낸다.
   
### 2XX : Successful responses
- 200 : OK
   - 요청이 성공적으로 되었으며 요청에 따른 응답을 해준다.

- 201 : Created
   - 요청이 성공적으로 되었으며, 그 결과로 새로운 리소스가 생성되었다는 뜻이다. 일반적으로 POST or PUT 메소드 이후에 따라온다.

- 203 : Non-Authoritative Information
- 204 : No Content
- 205 : Reset Content
- 206 : Partial Content
- 207 : Multi-Status
- 208 : Already Reported
- 226 : IM Used

### 3XX : Redirection messages

### 4XX : Client error responses

### 5XX : Server error responses


>&nbsp;마찬가지로 응답에도 본문(body)이 존재한다. 요청에 대한 데이터가 담겨있으며 브라우저는 응답 메세지에 담겨있는 HTML을 받아 화면에 렌더링을 한다.






