# Spring Boot 응답 데이터
> 스프링(서버)에서 응답 데이터를 만드는 방법은 크게 3가지 이다.
- 정적 리소스
   - 웹 브라우저에 정적인 HTML, css, js를 제공할 때는 정적 리소스를 사용한다.
   - 스프링 부트는 클래스패스의 다음 디렉토리에 있는 정적 리소스를 제공한다.
   - 경로 : /static, /public, /resource, /META-INF/resources
   - src/main/resources는 리소스를 보관하는 곳이며 클래스패스의 시작 경로이다. 따라서 이 디렉토리에 리소스를 넣어두면 스프링 부트가 정적 리소스로 서비스를 제공한다.
   - 정적 리소스는 해당 파일을 변경 없이 그대로 서비스하는 것이다. ex) <code>http://localhost:9090/hello-form.html</code>

<br>

- 뷰 템플릿(여기선 타임리프)
   - 웹 브라우저에 동적인 HTML을 제공할 때 뷰 템플릿을 사용한다. 
   - 뷰 템플릿을 거쳐서 HTML이 생성되고, 뷰가 응답을 만들어서 전달한다.
   - 일반적으로 HTML을 동적으로 생성하는 용도로 사용하지만, 다른 것들도 가능하다. 뷰 템플릿이 만들 수 있는 것이라면 뭐든지 가능하다.
   - 경로 : src/main/resources/templates

<br>

- HTTP 메세지(body) 사용
   - HTTP API를 제공하는 경우에는 HTML이 아니라 데이터를 전달해야 하므로, HTTP 메세지 바디에 JSON 같은 형식으로 데이터를 실어 보낸다. 

<br>

### 스프링 부트 타임리프 관련 추가 설정
- https://docs.spring.io/spring-boot/docs/2.4.3/reference/html/appendix-application-properties.html#common-application-properties-templating

<br>
<hr>
<br>

# 응답 데이터 종류
## String을 반환하는 경우 - View or HTTP 메시지
- @ResponseBody 가 없으면 response/hello 로 뷰 리졸버가 실행되어서 뷰를 찾고, 렌더링 한다.
- @ResponseBody 가 있으면 뷰 리졸버를 실행하지 않고, HTTP 메시지 바디에 직접 response/hello 라는 문자가 입력된다.
- 여기서는 뷰의 논리 이름인 response/hello 를 반환하면 다음 경로의 뷰 템플릿이 렌더링 되는 것을 확인할 수 있다.
- 실행: templates/response/hello.html

<br>

## Void를 반환하는 경우
- @Controller 를 사용하고, HttpServletResponse , OutputStream(Writer) 같은 HTTP 메시지 바디를 처리하는 파라미터가 없으면 요청 URL을 참고해서 논리 뷰 이름으로 사용한다.
- 요청 URL: /response/hello
- 실행: templates/response/hello.html
- 참고로 이 방식은 명시성이 너무 떨어지고 이렇게 딱 맞는 경우도 많이 없어서, 권장하지 않는다.

<br>

## HTTP 메시지
- @ResponseBody , HttpEntity 를 사용하면, 뷰 템플릿을 사용하는 것이 아니라, HTTP 메시지 바디에
- 직접 응답 데이터를 출력할 수 있다



<br>
<hr>
<br>


# HTTP API 메시지 바디에 직접 입력
```java
package hello.springmvc.basic.response;

import hello.springmvc.basic.HelloData;
import lombok.extern.slf4j.Slf4j;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.*;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@Slf4j
@Controller// ,@ResponseBody를 class 레벨에 붙일 수 있다. 근데 둘이 합친게 @RestController이다!
//@RestController
public class ResponseBodyController {

	
	/*
	 * 서블릿을 직접 다룰 때 처럼 HttpServletResponse 객체를 통해서 HTTP 메시지 바디에 직접 ok 응답 메시지를 전달한다.
	 * response.getWriter().write("ok")
	 */
	@GetMapping("/response-body-string-v1")
	public void responseBodyV1(HttpServletResponse response) throws IOException {
		response.getWriter().write("ok");
	}
	
	
	
	 /*
	 * HttpEntity, ResponseEntity(Http Status 추가)
	 * ResponseEntity 엔티티는 HttpEntity 를 상속 받았는데, 
	 * HttpEntity는 HTTP 메시지의 헤더, 바디정보를 가지고 있다. 
	 * ResponseEntity 는 여기에 더해서 HTTP 응답 코드를 설정할 수 있다.
	 * HttpStatus.CREATED 로 변경하면 201 응답이 나가는 것을 확인할 수 있다
	 */
	 @GetMapping("/response-body-string-v2")
	 public ResponseEntity<String> responseBodyV2() {
		 return new ResponseEntity<>("ok", HttpStatus.OK);
	 }
	 
	 
	/*
	 * ResponseBody 를 사용하면 view를 사용하지 않고, HTTP 메시지 컨버터를 통해서 HTTP 메시지를 직접 입력할 수 있다.
	 * ResponseEntity 도 동일한 방식으로 동작한다
	 */
	 @GetMapping("/response-body-string-v3")
	 @ResponseBody
	 public String responseBodyV3() {
		 return "ok";
	 }
	 
	 
	 
	/* 
	 * ResponseEntity 를 반환한다. HTTP 메시지 컨버터를 통해서 JSON 형식으로 변환되어서 반환된다
	 * 만약 HTTP 상태코드를 동적으로 바꾸고 싶다면  @ResponseBody 대신 ResponseEntity 타입을 사용하라!
	 */
	 @GetMapping("/response-body-json-v1")
	 public ResponseEntity<HelloData> responseBodyJsonV1() {
		 HelloData helloData = new HelloData();
		 helloData.setUsername("userA");
		 helloData.setAge(20);
		 return new ResponseEntity<>(helloData, HttpStatus.OK); 
	 }
	 
	/*
	 * ResponseEntity 는 HTTP 응답 코드를 설정할 수 있는데, 
	 * @ResponseBody 를 사용하면 이런 것을 설정하기 까다롭다.
	 * @ResponseStatus(HttpStatus.OK) 애노테이션을 사용하면 응답 코드도 설정할 수 있다.
	 */
	 
	 @GetMapping("/response-body-json-v2")
	 @ResponseStatus(HttpStatus.OK) // 싱테코드를 바꿀 수 있다.
	 @ResponseBody
	 public HelloData responseBodyJsonV2() {
		 HelloData helloData = new HelloData();
		 helloData.setUsername("userA");
		 helloData.setAge(20);
		 return helloData;
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

