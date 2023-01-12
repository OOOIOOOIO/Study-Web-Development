# 스프링 프레임워크에서 request에 대한 life cycle
![image](https://user-images.githubusercontent.com/74396651/211505966-17405f4f-642c-4fc1-b6f4-e4a715bed114.png)

> request -> filter -> servlet -> interceptor -> controller<br>
> response <- filter <- servlet <- interceptor <- controller

<hr>

# Filter
> &nbsp;요청과 으답을 거른 뒤 정제하는 역할을 한다. 서플릿 필터는 DispatcherServlet 이전에 실행이 되는데 필터가 동작하도록 지정된 자원의 앞단에서 요청내용을 변경하거나, 여러거지 체크를 수행할 수 있다.

<br>

## Filter 사용예시
> &nbsp;필터는 주로 요청에 대한 인증, 권한 체크 등을 수행하는데 쓰입니다. 예를 들면 헤더를 검사해 인증 토큰 검사 혹은 유효성 검사 등 및 인코딩 변환 처리, XSS 방어 등의 요청에 사용할 수 있습니다.

<br>

## 필터 인터페이스 3가지 메소드 및 설정
```java
@Slf4j
public class FirstFilter implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        log.info("FirstFilter가 생성 됩니다.");
    }

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        log.info("==========First 필터 시작!==========");
        filterChain.doFilter(servletRequest, servletResponse);
        log.info("==========First 필터 종료!==========");
    }

    @Override
    public void destroy() {
        log.info("FirstFilter가 사라집니다.");
    }
}
```
- init() : 필터가 생성될 때 수행되는 메소드
- doFilter() : Request, Response가 필터를 거칠 때 수행되는 메소드
- destory() : 필터가 소명될 때 수행되는 메소드

<br>

### Filter를 사용하기 위해선 Spring Bean으로 등록해야 한다.
```java
@Configuration
public class AppConfig{

    @Bean
    public FilterRegistrationBean firstFilterRegister() {
        FilterRegistrationBean registrationBean = new FilterRegistrationBean(new FirstFilter());
        return registrationBean;
    }
}
```
- 필터를 빈으로 등록하깅 위해 스프링 설정에 FilterRegistrationBean을 이용해 직접 만든 필터를 등록할 수 있다.

<br>

### Controller 테스트
```java
@Slf4j
@RestController
public class MyController {
    @GetMapping("/")
    public String hello() {
        log.info("Hello Garden!!!");
        return "Hello";
    }
}
```
### 로그
![image](https://user-images.githubusercontent.com/74396651/211510155-076d2389-f660-4a20-8bca-e4625d83407b.png)

<br>
<hr>
<br>

### 두번쨰 필터 등록
```java
@Slf4j
public class SecondFilter implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        log.info("SecondFilter가 생성 됩니다.");
    }

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        log.info("==========Second 필터 시작!==========");
        filterChain.doFilter(servletRequest, servletResponse);
        log.info("==========Second 필터 종료!==========");
    }

    @Override
    public void destroy() {
        log.info("SecondFilter가 사라집니다.");
    }
}
```

<br>

### Spring Bean 등록
```java
@Configuration
public class AppConfig{

    @Bean
    public FilterRegistrationBean firstFilterRegister() {
        FilterRegistrationBean registrationBean = new FilterRegistrationBean(new FirstFilter());
        registrationBean.setOrder(1);
        return registrationBean;
    }

    @Bean
    public FilterRegistrationBean secondFilterRegister() {
        FilterRegistrationBean registrationBean = new FilterRegistrationBean(new SecondFilter());
        registrationBean.setOrder(2);
        return registrationBean;
    }
}
```
- FilterRegistrationBean.setOrder()를 통해 필터의 동작 순서를 결정할 수 있다.

![image](https://user-images.githubusercontent.com/74396651/211511147-0accaf58-e16b-43c1-bcf3-166e32358a3f.png)

- 동작 순서
   - FirstFilter -> SecondFilter -> Controller
   - FirstFilter <- SecondFilter <- Controller 


<br>
<hr>
<hr>

# Interceptor
> &nbsp;요청에 대한 작업 전 / 후로 가로챈다고 보면 된다. 필터는 "스프링 컨텍스트 외부"에 존재하여 스프링과 무관한 자원에 대해 동작한다. 하지만 인터셉터는 스프링의 DispatcherServlet이 컨트롤러를 호출하기 전, 후로 끼어들기 때문에 스프링 컨텍스트(Context) 내부에서 Controller(Handler)에 관한 요청과 응답에 대해 처리한다.<br> 스프링의 모든 빈 객체에 접근할 수 있다.

<br>

## Interceptor 사용예시
> &nbsp; 로그인, 권한, 프로그램 실행시간 계산, 로그확인 등에 사용할 수 있다.

<br>

## Interceptor 3가지 메소드 및 설정
- preHandel()
   - 컨트롤러가 호출되기 전에 실행된다.
   - 컨트롤러가 실행 이전에 처리해야 할 작업이 있는 경우 혹은 요청정보를 가공하거나 추가하는 경우 사용한다.
   - 실행되어야 할 '핸들러'에 대한 정보를 인자값으로 받기 때문에 '서블릿 필터'에 비해 세밀하게 로직을 구성할 수 있다.
   - 리턴값이 boolean이다. 
      - true일 경우 preHandle() 실행 후 핸들러에 접근한다.
      - false일 경우 작업을 중단하고 이후의 컨트롤러와 인터셉터는 실행되지 않는다.
- postHandle()
   - 핸들러가 실행은 완료되었지만 아직 View가 생성되기 이전 상태에서 호출된다.
   - ModelAndView 타입의 정보를 인자값으로 받는다. 따라서 Controller에서 View 정보를 전달하기 위해 작업한 Model 객체의 정보를 참조하거나 조작할 수 있다.
   - preHandle()에서 리턴값이 false인 경우 실행되지 않는다.
   - 적용중인 인터셉터가 여러개인 경우 preHandle()은 역순으로 호출된다.
   - 비동기적 요청처리 시에는 처리되지 않는다.
- afterCompletion()
   - 모든 View에서 최종 결과를 생성하는 일을 포함한 모든 작업이 완료된 후에 실행된다.
   - 요청 처리 중에 사용한 리소스를 반환해주기 알맞은 메소드이다.
   - preHandle()에서 리턴값이 false인 경우 실행되지 않는다.
   - 적용중인 인터셉터가 여러개인 경우 preHandle()은 역순으로 호출된다.
   - 비동기적 요청 처리시에 처리되지 않는다.


스프링에서 제공하는 org.springframework.web.servlet.HandlerInterceptor 인터페이스를 구현 및 org.springframework.web.servlet.handler.HandlerInterceptorAdapter 추상클래스를 오버라이딩을 통해 자신만의 인터셉터를 만들수 있다. HandlerInterceptorAdapter 추상클래스 경우  HandlerInterceptor 인터페이스를 상속받아 구현되었다.



<hr>

[필터](https://derekpark.tistory.com/101) <br>
[인터셉터](https://popo015.tistory.com/115)<br>
[참고](https://gardeny.tistory.com/35)



















