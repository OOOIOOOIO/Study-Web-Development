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

<br>
<hr>
<br>

# RequestBodyString!
```java
package hello.springmvc.basic.request;

import java.io.IOException;
import java.io.InputStream;
import java.io.Writer;
import java.nio.charset.StandardCharsets;

import javax.servlet.ServletInputStream;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.springframework.http.HttpEntity;
import org.springframework.http.HttpStatus;
import org.springframework.http.RequestEntity;
import org.springframework.http.ResponseEntity;
import org.springframework.stereotype.Controller;
import org.springframework.util.StreamUtils;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.ResponseBody;

import lombok.extern.slf4j.Slf4j;

@Slf4j

@Controller
public class RequestBodyStringController {
	
	/*
	 * HTTP 메시지 바디의 데이터를 InputStream을 사용해 직접 읽을 수 있다.
	 */
	@PostMapping("/request-body-string-v1")
	public void requestBodyString(HttpServletRequest request, HttpServletResponse response) throws IOException {
		ServletInputStream inputStream = request.getInputStream();
		String messageBody = StreamUtils.copyToString(inputStream, StandardCharsets.UTF_8);

		log.info("messageBody={}", messageBody);
		
		response.getWriter().write("ok");
	}
	
	
	/*
	 * Spring MVC는 다음파라미터를 지원한다.
	 * InputStream(Reader): HTTP 요청 메시지 바디의 내용을 직접 조회할 수 있다.
	 * OutputStream(Writer): HTTP 응답 메시지의 바디에 직접 결과 출력할 수 있다.
	 */
	@PostMapping("/request-body-string-v2")
	public void requestBodyStringV2(InputStream inputStream, Writer responseWriter) throws IOException {
		String messageBody = StreamUtils.copyToString(inputStream, StandardCharsets.UTF_8);
		
		log.info("messageBody={}", messageBody);
		
		responseWriter.write("ok");
	}
	
	
	/*
	 * HttpEntity: HTTP header, body 정보를 편리하게 조회할 수 있다.
	 * - 메시지 바디 정보를 직접 조회(@RequestParam X, @ModelAttribute X)
	 * - HttpMessageConverter 사용 -> StringHttpMessageConverter 적용
	 *
	 * 응답에서도 HttpEntity 사용 가능
	 * - 메시지 바디 정보 직접 반환(view 조회X)
	 * - HttpMessageConverter 사용 -> StringHttpMessageConverter 적용
	 * 
	 * - HttpEntity를 상속받은 RequestEntity, ResponseEntity를 사용할 수도 있다.
	 */
	@PostMapping("/request-body-string-v3")
	public HttpEntity<String> requestBodyStringV3(HttpEntity<String> httpEntity) {
//	public HttpEntity<String> requestBodyStringV3(RequestEntity<String> httpEntity) {
		String messageBody = httpEntity.getBody();
		
		log.info("messageBody={}", messageBody);
		
		return new HttpEntity<>("ok");
//		return new ResponseEntity<String>("ok", HttpStatus.OK);
	}
	
	
	/*
	 * @RequestBody
	 * - 메시지 바디 정보를 직접 조회(@RequestParam X, @ModelAttribute X)
	 * - HttpMessageConverter 사용 -> StringHttpMessageConverter 적용
	 *
	 * @ResponseBody
	 * - 메시지 바디 정보 직접 반환(view 조회X)
	 * - HttpMessageConverter 사용 -> StringHttpMessageConverter 적용
	 */ 
	@PostMapping("/request-body-string-v4")
	@ResponseBody
	public String requestBodyStringV4(@RequestBody String messageBody) {
		log.info("messageBody={}", messageBody);
		return "ok";
	}
	
	
	
	
}



```

<br>
<hr>
<br>

# RequestBodyJson
```
package hello.springmvc.basic.request;

import java.io.IOException;
import java.nio.charset.StandardCharsets;

import javax.servlet.ServletInputStream;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.springframework.http.HttpEntity;
import org.springframework.stereotype.Controller;
import org.springframework.util.StreamUtils;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.ResponseBody;

import com.fasterxml.jackson.databind.ObjectMapper;

import hello.springmvc.basic.HelloData;
import lombok.extern.slf4j.Slf4j;

/**
 * {"username":"hello", "age":20}
 * content-type: application/json
 */

@Slf4j
@Controller
public class RequestBodyJsonController {
	
	
	private ObjectMapper objectMapper = new ObjectMapper();
 
	@PostMapping("/request-body-json-v1")
	public void requestBodyJsonV1(HttpServletRequest request, HttpServletResponse response) throws IOException {
		ServletInputStream inputStream = request.getInputStream();
		String messageBody = StreamUtils.copyToString(inputStream, StandardCharsets.UTF_8);
		
		log.info("messageBody={}", messageBody);
		
		HelloData data = objectMapper.readValue(messageBody, HelloData.class);
		
		log.info("username={}, age={}", data.getUsername(), data.getAge());
		
		response.getWriter().write("ok");
	}
	
	
	/*
	 * @RequestBody
	 * HttpMessageConverter 사용 -> StringHttpMessageConverter 적용
	 *
	 * @ResponseBody
	 * - 모든 메서드에 @ResponseBody 적용
	 * - 메시지 바디 정보 직접 반환(view 조회X)
	 * - HttpMessageConverter 사용 -> StringHttpMessageConverter 적용
	 */
	@ResponseBody
	@PostMapping("/request-body-json-v2")
	public String requestBodyJsonV2(@RequestBody String messageBody) throws	IOException {
		HelloData data = objectMapper.readValue(messageBody, HelloData.class);
	 
		log.info("username={}, age={}", data.getUsername(), data.getAge());
		
		return "ok";
	}
	
	
	/*
	 * @RequestBody 생략 불가능(@ModelAttribute 가 적용되어 버림(body가 아닌 parameter를 처리하게 된다!!!)
	 * - 객체를 바로 받을 수 있다.
	 * HttpMessageConverter 사용 -> MappingJackson2HttpMessageConverter (contenttype: application/json)
	 *
	 */
	@ResponseBody
	@PostMapping("/request-body-json-v3")
	public String requestBodyJsonV3(@RequestBody HelloData data) {
		log.info("username={}, age={}", data.getUsername(), data.getAge());
		
		return "ok";
	}
	
	/*
	 * @HttpEntiity로 body 꺼내기
	 */
	
	@ResponseBody
	@PostMapping("/request-body-json-v4")
	public String requestBodyJsonV4(HttpEntity<HelloData> httpEntity) {
		HelloData data = httpEntity.getBody();
		
		log.info("username={}, age={}", data.getUsername(), data.getAge());
		
		return "ok";
	}
	
	
	/*
	 * @RequestBody 생략 불가능(@ModelAttribute 가 적용되어 버림)
	 * HttpMessageConverter 사용 -> MappingJackson2HttpMessageConverter (contenttype: application/json)
	 *
	 * @ResponseBody 적용
	 * - 메시지 바디 정보 직접 반환(view 조회X)
	 * - HttpMessageConverter 사용 -> MappingJackson2HttpMessageConverter 적용(Accept: application/json)
	 * 흐름 : Json 요청 -> RequestBody -> 객체 - -> ResponseBody > Json 응답
	 */
	@ResponseBody
	@PostMapping("/request-body-json-v5")
	public HelloData requestBodyJsonV5(@RequestBody HelloData data) {
		log.info("username={}, age={}", data.getUsername(), data.getAge());
		
		return data;
	}
	
	
	
}




```

<br>
<hr>
<br>

# HTTP 메시지 컨버터
> 뷰 템플릿으로 HTML을 생성해서 응답하는 것이 아니라, HTTP API처럼 JSON 데이터를 HTTP 메시지 바디에서 직접 읽거나 쓰는 경우 HTTP 메시지 컨버터를 사용하면 편리하다.

<br>

## @ResponseBody 사용 원리

![ResponseBody](https://user-images.githubusercontent.com/74396651/179907932-ddd7b62c-19c9-4895-b549-d21993ad6ea7.png)

- 참고로 응답의 경우 클라이언트의 HTTP Accept 해더와 서버의 컨트롤러 반환 타입 정보, 둘을 조합하여 HTtpMessageConverter가 선택된다.

<br>

## 스프링 MVC는 다음의 경우에 HTTP 메시지 컨버터를 적용한다.
- HTTP 요청 : @RequestBody, HttpEntity(RequestEntity)
- HTTP 응답 : @ResponseBody, HttpEntity(ResponseEntity)

### HTTP 메시지 컨버터 인터페이스

![인터페이스](https://user-images.githubusercontent.com/74396651/179908471-ce63d317-e231-48cc-9d96-0145983c3a62.png)

<br>

### 스프링 부트 기본 메시지 컨버터
- 스프링 부트는 다양한 메시지 컨버터를 제공하는데, 대상 클래스 타입과 미디어 타입 둘을 체크해서 사용여부를 결정한다. 만약 만족하지 않으면 다음 메시지 컨버터로 우선순위가 넘어간다.
- #### ByteArrayHttpMessageConverter : byte[] 데이터를 처리한다.
   - 클래스 타입: byte[] , 미디어타입: */* ,
   - 요청 예) @RequestBody byte[] data
   - 응답 예) @ResponseBody return byte[] 쓰기 미디어타입 application/octet-stream

<br>

- #### StringHttpMessageConverter : String 문자로 데이터를 처리한다.
   - 클래스 타입: String , 미디어타입: */*
   - 요청 예) @RequestBody String data
   - 응답 예) @ResponseBody return "ok" 쓰기 미디어타입 text/plain

<br>

- #### MappingJackson2HttpMessageConverter : application/json
   - 클래스 타입: 객체 또는 HashMap , 미디어타입 application/json 관련
   - 요청 예) @RequestBody HelloData data
   - 응답 예) @ResponseBody return helloData 쓰기 미디어타입 application/json 관련

<br>

### HTTP 요청 데이터 읽기 흐름
- HTTP 요청이 오고, 컨트롤러에서 @RequestBody , HttpEntity 파라미터를 사용한다.
- 메시지 컨버터가 메시지를 읽을 수 있는지 확인하기 위해 canRead() 를 호출한다.
   - 대상 클래스 타입을 지원하는가.
   - 예) @RequestBody 의 대상 클래스 ( byte[] , String , HelloData )
- HTTP 요청의 Content-Type 미디어 타입을 지원하는가.
   - 예) text/plain , application/json , */*
- canRead() 조건을 만족하면 read() 를 호출해서 객체 생성하고, 반환한다.

<br>

### HTTP 응답 데이터 생성 흐름
- 컨트롤러에서 @ResponseBody , HttpEntity 로 값이 반환된다. 
- 메시지 컨버터가 메시지를 쓸 수 있는지 확인하기 위해 canWrite() 를 호출한다.
   - 대상 클래스 타입을 지원하는가.
   - 예) return의 대상 클래스 ( byte[] , String , HelloData )
- HTTP 요청의 Accept 미디어 타입을 지원하는가.(더 정확히는 @RequestMapping 의 produces )
   - 예) text/plain , application/json , */*
- canWrite() 조건을 만족하면 write() 를 호출해서 HTTP 응답 메시지 바디에 데이터를 생성한다.

<br>
<hr>
<br>

## 요청 맵핑 핸들러 어댑터 구조
> 그렇다면 HTTP 메시지 컨버터는 스프링 MVC 어디쯤 사용되는 것일까?

<br>

### Spring MVC 구조

![Spring MVC](https://user-images.githubusercontent.com/74396651/179909206-69b1b22e-41a5-42f4-a6da-bf1b9cf7ad2f.png)

<br>

### RequestMappingHandlerAdpater 동작 방식

![RequestMappingHandlerAdpater](https://user-images.githubusercontent.com/74396651/179909284-a48a24d7-7240-4162-8a0c-a3f1f3507b38.png)

<br>

###  ArgumentResolver(요청)
- 생각해보면, 애노테이션 기반의 컨트롤러는 매우 다양한 파라미터를 사용할 수 있었다.
- HttpServletRequest , Model 은 물론이고, @RequestParam , @ModelAttribute 같은 애노테이션
- 그리고 @RequestBody , HttpEntity 같은 HTTP 메시지를 처리하는 부분까지 매우 큰 유연함을 보여주었다.
- 이렇게 파라미터를 유연하게 처리할 수 있는 이유가 바로 ArgumentResolver 덕분이다.
- 애노테이션 기반 컨트롤러를 처리하는 RequestMappingHandlerAdapter 는 바로 이 ArgumentResolver 를 호출해서 컨트롤러(핸들러)가 필요로 하는 다양한 파라미터의 값(객체)을 생성한다. 
- 그리고 이렇게 파리미터의 값이 모두 준비되면 컨트롤러를 호출하면서 값을 넘겨준다.
- 스프링은 30개가 넘는 ArgumentResolver 를 기본으로 제공한다.
- 가능한 요청 목록 : https://docs.spring.io/spring-framework/docs/current/reference/html/web.html#mvc-ann-arguments
<br>
  
#### 동작방식
- ArgumentResolver 의 supportsParameter() 를 호출해서 해당 파라미터를 지원하는지 체크하고, 
- 지원하면 resolveArgument() 를 호출해서 실제 객체를 생성한다. 그리고 이렇게 생성된 객체가 컨트롤러 호출시 넘어가는 것이다.
- 그리고 원한다면 여러분이 직접 이 인터페이스를 확장해서 원하는 ArgumentResolver 를 만들 수도 있다. 


<br>

### ReturnValueHandler(응답)
- HandlerMethodReturnValueHandler 를 줄여서 ReturnValueHandler 라 부른다.
- ArgumentResolver 와 비슷한데, 이것은 응답 값을 변환하고 처리한다.
- 컨트롤러에서 String으로 뷰 이름을 반환해도, 동작하는 이유가 바로 ReturnValueHandler 덕분이다.
- 스프링은 10여개가 넘는 ReturnValueHandler 를 지원한다.
- 예) ModelAndView , @ResponseBody , HttpEntity , String
- 가능한 응답 목록 : https://docs.spring.io/spring-framework/docs/current/reference/html/web.html#mvc-ann-arguments


<br>

### HTTP 메시지 컨버터 위치

![image](https://user-images.githubusercontent.com/74396651/179910304-b19a1374-b7ea-41ec-803a-72237d95241c.png)

<br>

- HTTP 메시지 컨버터를 사용하는 @RequestBody 도 컨트롤러가 필요로 하는 파라미터의 값에 사용된다.
- @ResponseBody 의 경우도 컨트롤러의 반환 값을 이용한다.
- 요청의 경우 @RequestBody 를 처리하는 ArgumentResolver 가 있고, HttpEntity 를 처리하는 ArgumentResolver 가 있다. 
- 이 ArgumentResolver 들이 HTTP 메시지 컨버터를 사용해서 필요한 객체를 생성하는 것이다. 
- 응답의 경우 @ResponseBody 와 HttpEntity 를 처리하는 ReturnValueHandler 가 있다. 그리고 여기에서 HTTP 메시지 컨버터를 호출해서 응답 결과를 만든다.
- 스프링 MVC는 @RequestBody @ResponseBody 가 있으면 RequestResponseBodyMethodProcessor (ArgumentResolver)를, HttpEntity 가 있으면 HttpEntityMethodProcessor (ArgumentResolver)를 사용한다.

#### 확장
> 스프링은 다음을 모두 인터페이스로 제공한다. 따라서 필요하면 언제든지 기능을 확장할 수 있다.
- HandlerMethodArgumentResolver
- HandlerMethodReturnValueHandler
- HttpMessageConverter
- 스프링이 필요한 대부분의 기능을 제공하기 때문에 실제 기능을 확장할 일이 많지는 않다. 기능 확장은WebMvcConfigurer 를 상속 받아서 스프링 빈으로 등록하면 된다. 
- 실제 자주 사용하지는 않으니 실제 기능 확장이 필요할 때 WebMvcConfigurer 를 검색해보자.


<br>
<br>
<br>
<br>

참조 : 인프런 김영한님 강의


<br>
<br>
<br>
<br>
<br>
참조 : 인프런 김영한님 강의


