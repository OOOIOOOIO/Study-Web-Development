# TestRestTemplate
- TestRestTemplate은 REST 방식으로 개발한 API의 TEST를 최적화하기 위해 만들어진 클래스이다.
- HTTP 요청 후 데이터를 응답 받을 수 있는 템플릿 객체이며 ResponseEntity와 함께 자주 사용된다.
- Header와 Content-Type 등을 설정하여 API를 호출할 수 있다.

<br>
<hr>
<br>

## 사용 조건
- @SpringBootTest는 여러 환경의 웹 환경을 만들어 테스트를 할 수 있다.
- @Autowired의 기본 설정은 required=true 이므로 빈을 찾아서 주입해야한다.
- 하지만 TestRestTemplate은 열려있는 포트가 없다면 빈으로 등록할 수 없기 때문에 에러가 난다.
- 그래서 <code>@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)</code>를 설정해 호스트가 사용하지 않는 랜덤
 포트를 열어준다. 그리고 TestRestTemplate을 열려있는 포트로 자동으로 등록한다.
 
 ```java
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT) // 호스트가 사용하지 않는 랜덤 포트를 테스트에 사용하겠다는 것을 의미
public class PostsApiControllerTest {

    @LocalServerPort // 랜덤 로컬 port
    private int port;

    @Autowired // REST 방식으로 개발한 API Test를 최적화 하기 위해 만들어진 클래스
    private TestRestTemplate restTemplate;
}
```
 
<br>
<hr>
<br>

## 자주 사용하는 기능
- testRestTemplate.getForObject(URI url, Class<T> responseType)
   - 기본 http 헤더를 사용하여 결과를 ResponseEntity로 반환받는다.
```java
ResponseEntity<Long> responseEntity = restTemplate.getForEntity(url, Long.class);
```
<br>
  
- testRestTemplate.postForObject(URI url, Object request, Class<T> responseType)
   - 기본 http 헤더를 사용하여 결과를 ResponseEntity로 반환받는다.
```java
ResponseEntity<Long> responseEntity = restTemplate.postForEntity(url, requestDTO, Long.class);
```
<br>
  
- testRestTemplate.exchange(URI url, HttpMethod method, HttpEntity<?> requestEntity,  Class<T> responseType)
   - update할 때 주로 사용된다. 결과를 ResponseEntity로 반환받으며 Http header를 변경할 수 있다.
```java
// given
HttpEntity<ProductUpdateRequestDto> requestEntity = new HttpEntity<>(productUpdateRequestDto);

// when
ResponseEntity<Long> responseEntity = this.testRestTemplate.exchange(url, HttpMethod.PUT, requestEntity, Long.class);
```

<br>
<hr>
<br>

## MockMvc와의 차이점
- 가장 큰 차이점은 Servlet Container를 사용하느냐 사용하지 않느냐이다. 
   - MockMvc는 Servlet Container를 생성하지 않는 반면 @SpringBootTest + TestRestTemplate은 Servlet Container를 사용한다. 
   - 그래서 마치 서버가 동작하는 것처럼 테스트를 수행할 수 있다.
- 두번째 차이점은 테스트하는 관점이다.
   - MockMvc는 서버 입장에서 구현한 API를 통해 비즈니스 로직이 문제없이 수행되는지 테스트를 하는 것이라면
   - TestRestTemplate은 클라이언트 입장에서 RestTemplate을 사용하듯이 테스트하는 것이다.


# 참조
https://enterkey.tistory.com/275
