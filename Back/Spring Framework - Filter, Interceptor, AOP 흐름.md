# Filter, Interceptor, AOP 흐름

![image](https://user-images.githubusercontent.com/74396651/227753543-36ec281b-4073-43d1-ad85-c4a69cd50bdd.png)

- Interceptor와 Filter는 Servlet 단위에서 실행된다. <> 반면 AOP는 메소드 앞에 Proxy패턴의 형태로 실행된다.
- 실행순서를 보면 Filter가 가장 밖에 있고 그안에 Interceptor, 그안에 AOP가 있는 형태이다.
- 따라서 요청이 들어오면 Filter → Interceptor → AOP → Interceptor → Filter 순으로 거치게 된다.

### 순서
```
1. 서버를 실행시켜 서블릿이 올라오는 동안에 필터의 init이 실행되고, 그 후 doFilter가 실행된다. 

2. 컨트롤러에 동작 전 인터셉터의 preHandler가 실행된다

3. 컨트롤러 동작 후 인터셉터의 postHandler, after Completion, doFilter 순으로 진행이 된다.

4. 서블릿 종료 시 destroy가 실행된다.
```
