# 스프링 프레임워크에서 request에 대한 life cycle
<img width="795" alt="image" src="https://user-images.githubusercontent.com/74396651/227755173-b034ff94-0591-4e49-bda4-092def1c5bb5.png">


> request -> filter -> servlet -> interceptor -> controller<br>
> response <- filter <- servlet <- interceptor <- controller

<hr>

# Filter
> &nbsp;요청과 으답을 거른 뒤 정제하는 역할을 한다. 서플릿 필터는 DispatcherServlet 이전에 실행이 되는데 필터가 동작하도록 지정된 자원의 앞단에서 요청내용을 변경하거나, 여러거지 체크를 수행할 수 있다.

![image](https://user-images.githubusercontent.com/74396651/227753861-ef0c2740-9974-4a7f-bbc3-08c7786cea8a.png)


## 개념
- Dispatcher Servlet 전/후 ServletRequest/ServletResponse 객체 변경 및 조작 수행 가능
- WAS 내의 ApplicationContext에서 등록된 필터가 실행
- WAS 구동 시 FilterMap이라는 배열에 등록되고, 실행 시 Filter chain을 구성하여 순차적으로 실행
- Spring Context 외부에 존재하여 Spring과 무관한 자원에 대해 동작
- 일반적으로 web.xml에 설정
- 예외 발생 시 Web Application에서 예외 처리
    - ex. 인코딩 변환, XSS 방어 등
<br>

## Filter 사용예시
> &nbsp;필터는 주로 요청에 대한 인증, 권한 체크 등을 수행하는데 쓰입니다. 예를 들면 헤더를 검사해 인증 토큰 검사 혹은 유효성 검사 등 및 인코딩 변환 처리, XSS 방어 등의 요청에 사용할 수 있습니다.

<br>

## 필터 인터페이스 3가지 메소드 및 설정

- init() : 필터가 생성될 때 수행되는 메소드
    - 필터 객체를 초기화하고 서비스에 추가하기 위한 메서드이다. 웹 컨테이너가 1회 init 메소드를 호출하여 필터 객체를 초기화하면 이후의 요청들은 doFilter를 통해 처리된다
- doFilter() : Request, Response가 필터를 거칠 때 수행되는 메소드
    - url-pattern에 맞는 모든 HTTP 요청이 디스패처 서블릿으로 전달되기 전에 웹 컨테이너에 의해 실행되는 메소드이다. doFilter의 파라미터로는 FilterChain이 있는데, FilterChain의 doFilter 통해 다음 대상으로 요청을 전달하게 된다. chain.doFilter() 전/후에 우리가 필요한 처리 과정을 넣어줌으로써 원하는 처리를 진행할 수 있다. 
- destory() : 필터가 소명될 때 수행되는 메소드
    - 필터 객체를 서비스에서 제거하고 사용하는 자원을 반환하기 위한 메소드이다. 이는 웹 컨테이너에 의해 1번 호출되며 이후에는 이제 doFilter에 의해 처리되지 않는다. 

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


![image](https://user-images.githubusercontent.com/74396651/227753881-a344c793-af99-47f2-9efd-b8263099f019.png)

<br>

## 개념
- Dispatcher Servlet 이후 Controller 호출 전/후에 끼어들어 기능 수행
- Spring Context 내부에서 Controller의 요청과 응답에 관여하며 모든 Bean에 접근 가능
- 일반적으로 servlet-context.xml에 설정
- 예외 발생 시 @ControllerAdvice에서 @ExceptionHandler를 사용해 예외 처리
    - ex. 로그인 체크, 권한 체크, 로그 확인 등

<br>

## Interceptor 사용예시
> 스프링의 모든 빈 객체에 접근할 수 있다. 인터셉터는 여러 개를 사용할 수 있고 로그인 체크, 권한 체크, 프로그램 실행 시간 계산 작업 로그 확인 등의 업무처리에 사용한다.


<br>

## Interceptor 3가지 메소드 및 사용법
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

<br>

### HandlerInterceptor 구현
```java
public class CustomInterceptor implements HandlerInterceptor {
 
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)
            throws Exception {
        
        System.out.println("preHandle1");
        
        
        
        return true;
    }
 
    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler,
            ModelAndView modelAndView) throws Exception {
        
        System.out.println("postHandle1");
        
        
    }
 
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex)
            throws Exception {
        
        System.out.println("afterCompletion1");
        
        
    }    
    
}
```

### Interceptor 등록 및 패턴 설정
```java
@Configuration
public class WebConfig implements WebMvcConfigurer {

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new AuthCheckInterceptor())
                .addPathPatterns("/**")
                .excludePathPatterns("/api/auth/**");
    }
}
```

> 스프링에서 제공하는 org.springframework.web.servlet.HandlerInterceptor 인터페이스를 구현 및 org.springframework.web.servlet.handler.HandlerInterceptorAdapter 추상클래스를 오버라이딩을 통해 자신만의 인터셉터를 만들수 있다. HandlerInterceptorAdapter 추상클래스 경우 Spring 5.3 부터 Deprecatetd 되었다. 따라서   HandlerInterceptor 인터페이스 혹은 AsyncHandlerIntercptor 인터페이스를 상속받아 구현한다.

#### AsyncHandlerInterceptor
> AsyncHandlerInterceptor는 afterConcurrentHanding을 추가로 구현해야한다.<br>
> Servlet 3.0 부터 비동기 요청이 가능해졌다.<br>
> 비동기 요청 시 PostHandle과 afterCompletion이 수행되지 않고 afterConcurrentHandlingStarted가 수행됩니다. 
```java
public interface AsyncHandlerInterceptor extends HandlerInterceptor {

   default void afterConcurrentHandlingStarted(HttpServletRequest request, HttpServletResponse response,
         Object handler) throws Exception {
   }

}
```

### 참고, preHandle이 2번 탈 경우
```
![image](https://user-images.githubusercontent.com/74396651/227768989-ad15c8b2-a01d-44d8-91f2-e30674ad6b81.png)

```
[참고](https://www.logicbig.com/tutorials/spring-framework/spring-web-mvc/async-intercept.html)



<br>
<hr>
<br>

## Filter와 Interceptor 비교

![image](https://user-images.githubusercontent.com/74396651/227754002-0f12825e-f57b-4835-b940-bd15f1197f64.png)


- Filter는 WAS단에 설정되어 Spring과 무관한 자원에 대해 동작하고, Interceptor는 Spring Context 내부에 설정되어 컨트롤러 접근 전, 후에 가로채서 기능 동작
- Filter는 doFilter() 메소드만 있지만, Interceptor는 pre와 post로 명확하게 분리
- Interceptor의 경우 AOP 흉내 가능
- handlerMethod(@RequestMapping을 사용해 매핑 된 @Controller의 메소드)를 파라미터로 제공하여 메소드 시그니처 등 추가 정보를 파악해 로직 실행 여부 판단 가능
- 필터와 인터셉터는 각각 관리되는 컨테이너와 Request/Response의 조작 가능 여부가 다르고, 그에 따라 용도가 다르다.
- 필터는 Request와 Response를 조작할 수 있지만 인터셉터는 조작할 수 없다.

<br>
- 필터에서는 기본적으로 스프링과 무관하게 전역적으로 처리해야 하는 작업들을 처리할 수 있다. 대표적인 예시로 보안과 관련된 공통 작업이 있다. 필터는 인터셉터보다 앞단에서 동작하기 때문에 전역적으로 해야 하는 보안 검사(XSS 방어 등)를 하여 올바른 요청이 아닐 경우 차단을 할 수 있다. 그러면 스프링 컨테이너까지 요청이 전달되지 못하고 차단되므로 안정성을 더욱 높일 수 있다. 또한 필터는 이미지나 데이터의 압축이나 문자열 인코딩과 같이 웹 애플리케이션에 전반적으로 사용되는 기능을 구현하기에 적당하다. Filter는 다음 체인으로 넘기는 ServletRequest/ServletResponse 객체를 조작할 수 있다는 점에서 Interceptor보다 훨씬 강력한 기술이다.
- 인터셉터에서는 클라이언트의 요청과 관련되어 전역적으로 처리해야 하는 작업들을 처리할 수 있다. 대표적으로 세부적으로 적용해야 하는 인증이나 인가와 같이 클라이언트 요청과 관련된 작업 등이 있다. 예를 들어 특정 그룹의 사용자는 어떤 기능을 사용하지 못하는 경우가 있는데, 이러한 작업들은 컨트롤러로 넘어가기 전에 검사해야 하므로 인터셉터가 처리하기에 적합하다. 또한 인터셉터는 필터와 다르게 HttpServletRequest나 HttpServletResponse 등과 같은 객체를 제공받으므로 객체 자체를 조작할 수는 없다. 대신 해당 객체가 내부적으로 갖는 값은 조작할 수 있으므로 컨트롤러로 넘겨주기 위한 정보를 가공하기에 용이하다. 예를 들어 JWT 토큰 정보를 파싱 해서 컨트롤러에게 사용자의 정보를 제공하도록 가공할 수 있는 것이다. 그 외에도 우리는 다양한 목적으로 API 호출에 대한 정보들을 기록해야 할 수 있다. 이러한 경우에 HttpServletRequest나 HttpServletResponse를 제공해주는 인터셉터는 클라이언트의 IP나 요청 정보들을 포함해 기록하기에 용이하다.

<br>
<br>
<br>
<br>
<br>
<br>

[필터](https://derekpark.tistory.com/101) <br>
[인터셉터](https://popo015.tistory.com/115)<br>
[참고](https://gardeny.tistory.com/35)<br>
[참고2](https://carnival.tistory.com/77)



















