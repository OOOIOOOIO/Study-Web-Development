# Request Mapping
- #### @RequestMapping()을 사용해 기본 요청

<br>

- #### @GetMapping, PostMapping, DeleteMapping 등을 이용해 특정 HTTP Method 요청

<br>

- #### @GetMapping("/mapping/{userId}")<br>&nbsp;public String mappingPath(@PathVariable("userId") String data){ //TODO }을 사용해 파라미터 가져오기

<br>

- #### @GetMapping(value = "/mapping-header", headers = "mode=debug")을 사용해 특정 헤더 조건 맵핑

<br>

- #### @PostMapping(value = "/mapping-consume", consumes = "application/json")을 사용해 특정 미디어 타입 조건 맵핑(HTTP 요청의 Content-Type 헤더 기반!, 사용)

<br>

- #### @PostMapping(value = "/mapping-produce", produces = "text/html")을 사용해 특정 미디어 타입 조건 맵핑(HTTP 요청의 Accept 헤더 기반!, 제공)


<br>
<hr>
<br>

# Request Parameter 받기
- #### public void requestParamV1(HttpServletRequest request, HttpServletResponse response) throws IOException { //TODO }를 이용하여 request.getParameter로 받기

<br>

- #### public String requestParamV2(@RequestParam("username") String memberName, @RequestParam("age") int memberAge) { //TODO }를 이용
   - HTTP 파라미터 이름이 변수와 같다면 (name)을 생략할 수 있다.
   - 만약 String, int 등 단순 타입이라면 @RequestParam 자체도 생략할 수 있다.
   - @RequestParam(required = true), @RequestParam(required = false)를 이용해 파라미터 유무를 설정할 수도 있다. 만약 false인 곳에 값을 넣지 않는다면 null이 들어가기 때문에 Integer, Long 같은 랩퍼 클래스를 사용해야 한다.
   - @RequestParam(required = false, defaultValue = "guest")를 사용해 값이 들어가지 않는 경우 default로 값이 들어가게 할 수 있다. 이 때 주의할 점은 빈 문자열도 default 값이 들어간다는 점이다. 또한 defaultValue가 들어간 순간 required는 필요 없어진다(true든 false든 값이 없다면 어차피 default가 들어감)!
   - @RequestParam Map<String, Object> paramMap) or MultiValueMap<String, Object>를 사용해 값을 한번에 받아올 수 있다. 만약 value가 1개라면 map을 사용해도 무방하지만 여러개일 가능성이 있다면 Multi를 사용해야한다.

<br>

- #### public String modelAttributeV1(@ModelAttribute HelloData helloData) { //TODO }
   - getter, setter를 알아서 사용해 값을 넣어준다.
   - 만약 rgument resolver로 지정해둔 타입이 아니라면 생략할 수도 있다.

<br>
<hr>
<br>

# Request Mapping!
```java
package hello.springmvc.basic.requestmapping;

import org.springframework.http.MediaType;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RestController;

import lombok.extern.slf4j.Slf4j;

@RestController
@Slf4j
public class MappingController {

	/*
	 
	 @RequestMapping("")  기본 요청
	 둘다 허용 /hello-basic, /hello-basic/
	 HTTP 메서드 모두 허용 GET, HEAD, POST, PUT, PATCH, DELETE
	 
	 @RequestMapping(value="hello-basic", method = RequestMethod.GET)
	 method 특정 HTTP 메서드 요청만 허용
 	 GET, HEAD, POST, PUT, PATCH, DELETE
 	 
	*/
	
//	@RequestMapping({"hello-basic", "hello"}) // 여러개도 가능
	@RequestMapping(value="hello-basic", method = RequestMethod.GET)
	public String helloBasic() {
		log.info("hello basic");
		return "ok";
	}
	
	
	/*
	 * 편리한 축약 애노테이션 (코드보기)
	 * @GetMapping
	 * @PostMapping
	 * @PutMapping
	 * @DeleteMapping
	 * @PatchMapping
	 */
	
	@GetMapping(value = "/mapping-get-v2")
	public String mappingGetV2() {
	 log.info("mapping-get-v2");
	 return "okk";
	}
	
	
	/*
	 * @PathVariable 사용
	 * 변수명이 같으면 생략 가능(아래 메서드)
	 * @PathVariable("userId") String userId -> @PathVariable userId
	 */
	@GetMapping("/mapping/{userID}")
	public String mappingPath(@PathVariable("userID") String data) {
		log.info("mappingPath userId={}", data);
		return "okkk";
	}
	
	
	/*
	 * @PathVariable 사용 다중
	 * @PathVariable의 이름과 파라미터 이름이 같으면 ("이름")을 생략할 수 있다.
	 */
	@GetMapping("/mapping/users/{userId}/orders/{orderId}")
	public String mappingPath(@PathVariable String userId, @PathVariable Long orderId) {
		log.info("mappingPath userId={}, orderId={}", userId, orderId);
		return "okkkk";
	}
	
	
	/*
	 * 파라미터로 추가 매핑
	 * params="mode",
	 * params="!mode"
	 * params="mode=debug"
	 * params="mode!=debug" (! = )
	 * params = {"mode=debug","data=good"}
	 * 
	 * params가 있어야 맵핑이 된다! 특정 파라미터 정보가 있어야 호출이 된다!
	 * 잘 사용하지는 않는다.
	 */
	@GetMapping(value = "/mapping-param", params = "mode=debug")
	public String mappingParam() {
		log.info("mappingParam");
		return "okkkkk";
	}
	
	
	/*
	 * 특정 헤더로 추가 매핑
	 * headers="mode",
	 * headers="!mode"
	 * headers="mode=debug"
	 * headers="mode!=debug" (! = )
	 * 
	 * params와 맥락이 같다. 특정 header 정보가 있어야 호출이 된다!
	 */
	@GetMapping(value = "/mapping-header", headers = "mode=debug")
	public String mappingHeader() {
		log.info("mappingHeader");
		return "okkkkkk";
	}
	
	
	/*
	 * Content-Type 요청 헤더 기반 추가 매핑 Media Type
	 * consumes="application/json"
	 * consumes="!application/json"
	 * consumes="application/*"
	 * consumes="*\/*"
	 * 
	 * MediaType.APPLICATION_JSON_VALUE
	 * HTTP 요청의 Content-Type 헤더를 기반으로 미디어 타입으로 매핑한다.
	 * 
	 * 만약 맞지 않으면 HTTP 415 상태코드(Unsupported Media Type)을 반환한다 
	 * consumes : 소비할 자원의 타입!!! 그러니까 요청된 자원을 뜻한다!
	 */
	@PostMapping(value = "/mapping-consume", consumes = MediaType.APPLICATION_JSON_VALUE) // "application/json"
	public String mappingConsumes() {
	 log.info("mappingConsumes");
	 return "okkkkkkk";
	}
	
	/*
	 * Accept 요청 헤더 기반 Media Type
	 * produces = "text/html"
	 * produces = "!text/html"
	 * produces = "text/*"
	 * produces = "*\/*"
	 * 
	 * HTTP 요청의 Accept 헤더를 기반으로 미디어 타입으로 매핑한다.
	 * 만약 맞지 않으면 HTTP 406 상태코드(Not Acceptable)을 반환한다.
	 * 
	 * produces : 요청을 처리하고 제공할 자원!! 그러니까 응답할 자원을 뜻한다!
	 */
	@PostMapping(value = "/mapping-produce", produces = MediaType.TEXT_HTML_VALUE) // "text/html"
	public String mappingProduces() {
		log.info("mappingProduces");
		return "okkkkkkk";
	}

	
}

```

<br>
<hr>
<br>

# Request Mapping API(Rest방식)
```java
package hello.springmvc.basic.requestmapping;

import org.springframework.web.bind.annotation.DeleteMapping;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PatchMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

/**
 * 회원 관리 API
 * 회원 목록 조회: GET /users
 * 회원 등록: POST /users
 * 회원 조회: GET /users/{userId}
 * 회원 수정: PATCH /users/{userId}
 * 회원 삭제: DELETE /users/{userId}
 */

@RestController
@RequestMapping("/mapping/users")
public class MappingClassController {
	

	
	@GetMapping
	public String user() {
	
		return "get users";
	}
	
	@PostMapping
	public String addUser() {
		return "post user";
	}
	
	@GetMapping("/{userId}")
	public String findUser(@PathVariable String userId) {
		return "get userId=" + userId;
	}

	@PatchMapping("/{userId}")
	public String updateUser(@PathVariable String userId) {
		 return "update userId=" + userId;
	}
	 
	@DeleteMapping("/{userId}")
	public String deleteUser(@PathVariable String userId) {
		 return "delete userId=" + userId;
	}

	
	
}

```

<br>
<hr>
<br>

# HTTP 요청 기본 및 헤더 조회하기!
```java
package hello.springmvc.basic.request;

import java.util.Locale;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.springframework.http.HttpMethod;
import org.springframework.util.MultiValueMap;
import org.springframework.web.bind.annotation.CookieValue;
import org.springframework.web.bind.annotation.RequestHeader;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import lombok.extern.slf4j.Slf4j;

@Slf4j
@RestController
public class RequestHeaderController {
	
	@RequestMapping("/headers")
	public String headers(HttpServletRequest request,
						HttpServletResponse response,
						HttpMethod httpMethod,
						Locale locale,
						@RequestHeader MultiValueMap<String, String> headerMap, // MultiValueMap : 하나의 키에 여러 값을 받을 수 있다. HTTP header, HTTP 쿼리 파라미터와 같이 하나의 키에 여러 값을 받을 때 사용한다. 
						@RequestHeader("host") String host,
						@CookieValue(value = "myCookie", required = false) String cookie) {
		log.info("request={}", request);
		log.info("response={}", response);
		log.info("httpMethod={}", httpMethod);
		log.info("locale={}", locale); // 언어 정보
		log.info("headerMap={}", headerMap); //헤더 정보
		log.info("header host={}", host);
		log.info("myCookie={}", cookie);
		return "ok";
	 }
/*
2022-07-19 12:10:14.063  INFO 25868 --- [nio-9090-exec-1] h.s.b.request.RequestHeaderController    : request=org.apache.catalina.connector.RequestFacade@48446834
2022-07-19 12:10:14.064  INFO 25868 --- [nio-9090-exec-1] h.s.b.request.RequestHeaderController    : response=org.apache.catalina.connector.ResponseFacade@5e94ed59
2022-07-19 12:10:14.065  INFO 25868 --- [nio-9090-exec-1] h.s.b.request.RequestHeaderController    : httpMethod=POST
2022-07-19 12:10:14.065  INFO 25868 --- [nio-9090-exec-1] h.s.b.request.RequestHeaderController    : locale=ko_KR
2022-07-19 12:10:14.065  INFO 25868 --- [nio-9090-exec-1] h.s.b.request.RequestHeaderController    : headerMap={accept=[text/html], user-agent=[PostmanRuntime/7.29.0], postman-token=[475e0d84-bbbd-4830-b2ad-0690367f240b], host=[localhost:9090], accept-encoding=[gzip, deflate, br], connection=[keep-alive], content-length=[0]}
2022-07-19 12:10:14.065  INFO 25868 --- [nio-9090-exec-1] h.s.b.request.RequestHeaderController    : header host=localhost:9090
2022-07-19 12:10:14.065  INFO 25868 --- [nio-9090-exec-1] h.s.b.request.RequestHeaderController    : myCookie=null
*/
}

```

<br>
<hr>
<br>

# Request Parameter 받기!
```java
package hello.springmvc.basic.request;

import java.io.IOException;
import java.util.Map;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.ResponseBody;

import hello.springmvc.basic.HelloData;
import lombok.extern.slf4j.Slf4j;

@Controller
@Slf4j
public class RequestParamController {

	/*
	 * 반환 타입이 없으면서 이렇게 응답에 값을 직접 집어넣으면, view 조회X
	 */
	
	@RequestMapping("/request-param-v1")
	public void requestParamV1(HttpServletRequest request, HttpServletResponse response) throws IOException {
		String username = request.getParameter("username");
		int age = Integer.parseInt(request.getParameter("age"));
		
		log.info("username={}, age={}", username, age);
		
		response.getWriter().write("ok");
	}
	
	
	/*
	 * @RequestParam 사용
	 * - 파라미터 이름으로 바인딩
	 * @ResponseBody 추가
	 * - return을 할 때 View 조회를 무시하고, HTTP message body에 직접 해당 내용 입력한다!
	 */
	@RequestMapping("/request-param-v2")
	@ResponseBody
	public String requestParamV2(@RequestParam("username") String memberName, @RequestParam("age") int memberAge) {
		log.info("username={}, age={}", memberName, memberAge);
		return "ok";
	}
	
	
	/*
	 * @RequestParam 사용
	 * HTTP 파라미터 이름이 변수 이름과 같으면 @RequestParam(name="xx") 생략 가능
	 */
	@RequestMapping("/request-param-v3")
	@ResponseBody
	public String requestParamV3(@RequestParam String username, @RequestParam int age) {
		log.info("username={}, age={}", username, age);
		return "ok";
	}
	
	
	/*
	 * @RequestParam 알아서 사용
	 * String, int 등의 단순 타입이면 @RequestParam도 생략 가능!
	 * 파라미터랑 변수 이름이 같으면 생략 가능하다!
	 */
	@RequestMapping("/request-param-v4")
	@ResponseBody
	public String requestParamV4(String username, int age) {
		log.info("username={}, age={}", username, age);
		return "ok";
	}
	
	
	/*
	 * @RequestParam.required : default true
	 * /request-param-required -> username이 없으므로 예외(400 예외)
	 *
	 * 주의!!!
	 * /request-param-required?username= -> 빈문자로 통과된다.
	 *
	 * 주의!!!
	 * /request-param-required
	 * int age -> null을 int에 입력하는 것은 불가능(500 예외), 따라서 Integer 변경해야 함(또는 다음에 나오는 defaultValue 사용)
	 * 
	 * true : 무조건 있어야 한다. 없으면 예외 발생
	 * false : 없어도 잘 호출된다. 만약 false로 넣지 않는 경우도 있다면 랩퍼 클래스처럼 Integer, Long 등을 사용해야한다.
	 */
	@ResponseBody
	@RequestMapping("/request-param-required")
	public String requestParamRequired(@RequestParam(required = true) String username, @RequestParam(required = false) Integer age) {
		log.info("username={}, age={}", username, age);
		return "ok";
	}
	
	
	/*
	 * @RequestParam
	 * - defaultValue 사용
	 *
	 * 참고: defaultValue는 빈 문자의 경우에도 적용된다.
	 * /request-param-default?username= --> username=guest가 된다!
	 * 
	 * 사실 defaultValue가 들어가면 required는 무의미하다.
	 * 만약 값이 없다면 defaultValue에 있는 값이 들어갈 것이기 때문이다!!!
	 * 
	 */
	@RequestMapping("/request-param-default")
	@ResponseBody
	public String requestParamDefault(@RequestParam(required = true, defaultValue = "guest") String username, @RequestParam(required = false, defaultValue = "-1") int age) {
		log.info("username={}, age={}", username, age);
		return "ok";
	}
	
	
	/*
	 * @RequestParam Map, MultiValueMap
	 * Map(key=value)
	 * MultiValueMap(key=[value1, value2, ...]) ex) (key=userIds, value=[id1, id2])
	 * 
	 * 파라미터의 값이 1개가 확실하다면 Map을 사용해도 되지만, 그렇지 않다면 MultiValueMap을 사용하자~~!
	 */
	@RequestMapping("/request-param-map")
	@ResponseBody
	public String requestParamMap(@RequestParam Map<String, Object> paramMap) {
		log.info("username={}, age={}", paramMap.get("username"), paramMap.get("age"));
		return "ok";
	}
	
	/*
	 * @ModelAttribute 사용
	 * 참고: model.addAttribute(helloData) 코드도 함께 자동 적용됨, 뒤에 model을 설명할 때
	자세히 설명
	 */
	@RequestMapping("/model-attribute-v1")
	@ResponseBody
	public String modelAttributeV1(@ModelAttribute HelloData helloData) {
		log.info("username={}, age={}", helloData.getUsername(), helloData.getAge());
		return "ok";
	}

	/*
	 * @ModelAttribute도 생략 가능하다!
	 * String, int 같은 단순 타입 = @RequestParam을 사용하고
	 * argument resolver로 지정해둔 타입 외 = @ModelAttribute를 자동으로 만들어서 넣어준다.(argument resolver : HTTPRequest, Response, ...같은 것들)
	 */
	@RequestMapping("/model-attribute-v2")
	@ResponseBody
	public String modelAttributeV2(HelloData helloData) {
		log.info("username={}, age={}", helloData.getUsername(), helloData.getAge());
		return "ok";
	}
	
}
















```













