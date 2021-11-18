## Web Server
사용자가 요청하는 정적 컨텐츠(이미지 파일 등)를 HTTP를 통해 제공해주는 서버이다. 동적인 요청이 클라이언트로부터 요청될 경우 해당 요청은 웹 서버에서 처리할 수 없기 때문에 컨테이너(Container)로 보내진다.
ex) Apache, Nginx, ...

## Web Container
동적인 데이터 요청(DB 접근 연산 등)이 들어왔을 때 서버(Server)가 연산을 요청하는 곳.
서버(Server)에서 들어온 동적인 요청의 처리가 끝나면 정제된(Static한) 데이터(JSP, 서블릿 등)로 서버(Server)에 돌려준다.

## WAS(Web Application Server)
웹 서버 + 웹 컨테이너. 웹 서버로부터 오는 동적인 요청을 처리하는 서버이다.
ex) Tomcat, tmaxjus, Oracle, ...           
![Web Application Server](https://gmlwjd9405.github.io/images/web/webserver-vs-was1.png)

## 용어 정리

- 정적 컨텐츠(Static) : DB에서 정보를 가져오거나 별도로 서버에서의 처리가 없어도 사용자들에게 보여줄 수 있는 컨텐츠. 어떠한 사용자가 오던 간에 동일한 컨텐츠를 보여준다.     
ex) HTML, CSS, JS, Image, ...

- 동적 컨텐츠(Dynamic) : 서버처럼 DB에서 정보를 가져오는 것 같이 어떠한 요청에 의해 서버가 일을 수행하고 그에 대한 결과를 보여주는 컨텐츠. 사용자들마다 각기 다른 페이지를 보여줄 수 있다.     
![정적, 동적](https://gmlwjd9405.github.io/images/web/static-vs-dynamic.png)
