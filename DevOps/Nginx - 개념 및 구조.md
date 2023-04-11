# Nginx란
Nginx란 경량 웹 서버이다. 클라이언트로부터 요청을 받았을 때 요청에 맞는 정적 파일을 응답해주는 HTTP Web Server로 활용되기도 하고, Reverse Proxy Server로 활용하여 WAS 서버의 부하를 줄일 수 있는 로드 밸런서로 활용되기도 한다.

## Reverse Proxy란

![image](https://user-images.githubusercontent.com/74396651/231026766-ca554c09-4a11-411b-8b39-be4ca905130f.png)
클라이언트는 가짜 서버에 요청(request)하면, 프록시 서버가 배후 서버(reverse server)로부터 데이터를 가져오는 역할을 한다. 여기서 프록시 서버가 Nginx, 리버스 서버가 응용프로그램 서버를 의미한다.
웹 응용프로그램 서버에 리버스 프록시(Nginx)를 두는 이유는 요청(request)에 대한 버퍼링이 있기 때문이다. 클라이언트가 직접 App 서버에 직접 요청하는 경우, 프로세스 1개가 응답 대기 상태가 되어야만 한다. 따라서 프록시 서버를 둠으로써 요청을 배분하는 역할을 한다.

<br>

# Nginx의 흐름

```
client -> nginx -> was(server) -> db 흐름
```
Nginx는 Event-Driven 구조로 동작하기 때문에 한 개 또는 고정된 프로세스만 생성하여 사용하고, 비동기 방식으로 요청들을 Concurrency 하게 처리할 수 있다.<br>
위의 그림에서 보이듯이 Nginx는 새로운 요청이 들어오더라도 새로운 프로세스와 쓰레드를 생성하지 않기 때문에 프로세스와 쓰레드 생성 비용이 존재하지 않고,적은 자원으로도 효율적인 운용이 가능하다.<br>
이러한 Nginx의 장점 덕분에 단일 서버에서도 동시에 많은 연결을 처리할 수 있다.

## Event-Driven 

![image](https://user-images.githubusercontent.com/74396651/231026862-38961afd-0dec-4539-951b-5c1f0e3a0453.png)

Nginx는 비동기 처리 방식(Event-Drive) 방식을 채택하고 있다.

- 동기(Synchronous) : A가 B에게 데이터를 요청했을 때, 이 요청에 따른 응답을 주어야만 A가 다시 작업 처리가 가능 (하나의 요청, 하나의 작업에 충실)

- 비동기(Asynchronous) : A의 요청을 B가 즉시 주지 않아도, A의 유휴시간으로 또 다른 작업 처리가 가능한 방식

<br>

# Nginx의 구조

![image](https://user-images.githubusercontent.com/74396651/231024656-a0cc9109-f8a3-4955-9994-d361a9ca7ae3.png)

Nginx는 하나의 Master Process와 다수의 Worker Process로 구성되어 실행됩니다. Master Process는 설정 파일을 읽고, 유효성을 검사 및 Worker Process를 관리한다.<br>
모든 요청은 Worker Process에서 처리한다. Nginx는 이벤트 기반 모델을 사용하고, Worker Process 사이에 요청을 효율적으로 분배하기 위해 OS에 의존적인 메커니즘을 사용한다.<br>
Worker Process의 개수는 설정 파일에서 정의되며, 정의된 프로세스 개수와 사용 가능한 CPU 코어 숫자에 맞게 자동으로 조정된다.

<br>

[정리 짱](https://ssdragon.tistory.com/60)
