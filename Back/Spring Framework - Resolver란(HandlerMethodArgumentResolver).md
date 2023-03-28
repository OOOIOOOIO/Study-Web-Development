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


## Resolver 구현
- HandlerMethodArgumentResolver를 상속받는다
- supperotsParameter, resolverArgument를 구현한다

<img width="1141" alt="image" src="https://user-images.githubusercontent.com/74396651/227773289-b628d9b4-bddf-4482-a520-718584d9bf2b.png">


## WebConfig 구현

<img width="846" alt="image" src="https://user-images.githubusercontent.com/74396651/227773280-de82034d-9be4-4dd3-a173-2529257a5779.png">


## Annotation 구현
- @Target(ElementType.PARAMETER) : 해당 Annotation이 생성될 위치 지정, PARAMETER로 지정 시 Method의 Parameter에서만 사용 가능
- @Retention(RetentionPolicy.RUNTIME) : Annotation 유지 정책을 설정
  - RUNTIME은 Byte Code File까지 Annotation 정보를 유지
  - reflection을 이용 Runtime시에 해당 Annotation 정보를 획득
  - reflection : 구체적인 Class Type을 알지 못해도, 그 Class의 method, type, field들에 접근할 수 있도록 해주는 Java API

<img width="428" alt="image" src="https://user-images.githubusercontent.com/74396651/227772386-351c7f6b-4b40-428c-9def-81aec6c7de9d.png">
