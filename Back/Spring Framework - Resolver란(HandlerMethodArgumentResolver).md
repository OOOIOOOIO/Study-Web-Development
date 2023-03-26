# HandlerMethodArgumentResolver
> HandlerMethodArgumentREsolver는 interface로써, Controller의 Argument(Pararmeter)에 지정된 변수들을 Annotation이나 객체의 Type에 따라 Resolver를 먼저 거쳐 실제 Data를 Controller에 넘겨주는 역할을 수행한다.<br>
> Controller에 들어오는 Argument(Parameter)를 가공(암호화 > 복호화)하거나 Argument(Parameter)를 추가하거나 수정해야 하느 경우에 사용한다.


## 사용하는 경우
- 매개변수로 사용되는 이자에 대해 공통적으로 처리해야할 로직등이 있을 경우
- 중복 코드를 줄이고 공통 기능으로 추출하여 사용할 수 있다.


## Spring에서의 Resolver 동작 방식
- Client가 Request 요청
- Filter 처리
- Dispatcher Servlet에서 해당 요청 처리
- Client의 Request를 HandlerMapping이 할당
  - RequestMapping에 대한 매칭 실행(RequestMappingHandlerAdapter 수행)
  - Interceptor 처리
  - Argument Resolver 처리 <-- ArgumentResolver 실행 시점
  - Message Converter 처리
- Controller의 Method로 invoke
