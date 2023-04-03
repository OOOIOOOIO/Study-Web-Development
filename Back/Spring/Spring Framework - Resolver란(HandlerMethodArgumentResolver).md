# HandlerMethodArgumentResolver
> 후...중복코드를 제거해보자...<br>
> HandlerMethodArgumentREsolver는 interface로써, Controller의 Argument(Pararmeter)에 지정된 변수들을 Annotation이나 객체의 Type에 따라 Resolver를 먼저 거쳐 실제 Data를 Controller에 넘겨주는 역할을 수행한다.<br>
> Controller에 들어오는 Argument(Parameter)를 가공(암호화 > 복호화)하거나 Argument(Parameter)를 추가하거나 수정해야 하느 경우에 사용한다.


## 사용하는 경우
- 매개변수로 사용되는 이자에 대해 공통적으로 처리해야할 로직등이 있을 경우
- 중복 코드를 줄이고 공통 기능으로 추출하여 사용할 수 있다.


## Spring에서의 Resolver 동작 방식
1. Client가 Request 요청
2. Filter 처리
3. Dispatcher Servlet에서 해당 요청 처리
4. Client의 Request를 HandlerMapping이 할당<br>
  4-1. RequestMapping에 대한 매칭 실행(RequestMappingHandlerAdapter 수행)<br>
  4-2. Interceptor 처리<br>
  4-3. Argument Resolver 처리 <-- ArgumentResolver 실행 시점<br>
  4-4. Message Converter 처리<br>
5. Controller의 Method로 invoke 됨

<br>

## Resolver 구현
- HandlerMethodArgumentResolver를 상속받는다
- supperotsParameter, resolverArgument를 구현한다

<img width="1514" alt="image" src="https://user-images.githubusercontent.com/74396651/228201911-eeac5b3e-d7a0-46e7-a97b-ffa4ac2737db.png">
- ### 원하는 형태로 return해주면 된다.

<hr>

## WebConfig 구현

<img width="846" alt="image" src="https://user-images.githubusercontent.com/74396651/228201969-43e29d6a-522f-43f5-94dc-24c15989ff36.png">
- ### resolver 등록

<hr>

## Annotation 구현
- @Target(ElementType.PARAMETER) : 해당 Annotation이 생성될 위치 지정, PARAMETER로 지정 시 Method의 Parameter에서만 사용 가능
- @Retention(RetentionPolicy.RUNTIME) : Annotation 유지 정책을 설정
  - RUNTIME은 Byte Code File까지 Annotation 정보를 유지
  - reflection을 이용 Runtime시에 해당 Annotation 정보를 획득
  - reflection : 구체적인 Class Type을 알지 못해도, 그 Class의 method, type, field들에 접근할 수 있도록 해주는 Java API

<img width="428" alt="image" src="https://user-images.githubusercontent.com/74396651/227772386-351c7f6b-4b40-428c-9def-81aec6c7de9d.png">

<hr>

## Controller 구현

<img width="1501" alt="image" src="https://user-images.githubusercontent.com/74396651/228202070-107e0f47-5c12-4617-b7e2-ee9acc9f9313.png">

<br>
<br>

# 끝!
