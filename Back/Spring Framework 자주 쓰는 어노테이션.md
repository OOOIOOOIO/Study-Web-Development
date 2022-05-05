# Spring Framework 자주 쓰는 어노테이션(@)

<br>

## 의존성 주입용도

- @Required
   - Required 어노테이션을 사용해 optional하지 않은, 꼭 필요한 속성들을 정의한다. Controller 클래스의 메서드에서 requestBody로 가져올 만한 것들이나 PathVariable로 가져올만한
   꼭 필요한 값들에 쓰일 수 있다.
   
- @Autowired
   - 속성(field), setter method, constructor(생성자)에서 사용한다. 무조건적인 객체에 대한 의존성을 주입시키며 스프링이 자동적으로 값을 할당한다. 
   Controller 클래스에서 DAO나 Service에 관한 객체들을 주입시킬 때 많이 사용한다.
   
- @Injext
   - Autowired 어노테이션과 비슷한 역할을 한다. 더욱 스탠다드한 어노테이션이다.


## 컨트롤러 관련


- @Controller
   - Spring의 Controller를 의미한다. Spring MVC에세 Controller 클래스에 쓰인다.

- @RestController
   - Spring에서 Controller 중 View로 응답하지 않는 컨트롤러이다. HttpResponse로 바로 응답이 가능하며 @ResponseBody 역할을 자동적으로 해주는 어노테이션이다.

- @RequestMapping
   - Spring의 컨트롤러 혹은 그 메서드의 URI를 정의하는 곳에 쓰인다. 요청형식인 GET, POST, PATCH, PUT, DELETE를 정의하기도 한다. 요청형식을 정의하지 않는다면 GET이다.

- @PathVariable
   - URI에서 "/" 다음으로 넘어오는 값들을 파싱하는 어노테이션이다.

- @RequestParam
   - 쿼리스트링을 파라미터에 파싱해준다.

- @RequestBody
   - POST나 PUT, PATCH로 요청을 받을 때, 요청으로 넘어온 body를 자바 타입으로 파싱해준다.   

- @ResponseBody
   - HttpMessageConverter를 이용하여 JSON, xml로 요청에 응답할 수 있게 해주는 어노테이션이다. 이미 RestController 어노테이션이 붙어 있다면 사용하지 않아도 된다.

## 데이터 접근 관련

- @Service
   - Service Class에서 쓰이며 비즈니스 로직을 수행하는 클래스라는 것을 나타내는 용도이다.

- @Repository
   - DAO Class에서 쓰이며 데이터베이스에 접근하는 메서드를 가지고 있는 클래스에서 쓰인다고 볼 수 있다.
