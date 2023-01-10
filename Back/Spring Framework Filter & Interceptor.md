# 스프링 프레임워크에서 request에 대한 life cycle
![image](https://user-images.githubusercontent.com/74396651/211505966-17405f4f-642c-4fc1-b6f4-e4a715bed114.png)

> request -> filter -> servlet -> interceptor -> controller<br>
> response <- filter <- servlet <- interceptor <- controller

<hr>

# Filter

## Filter 사용예시
> &nbsp;필터는 주로 요청에 대한 인증, 권한 체크 등을 수행하는데 쓰입니다. 예를 들면 헤더를 검사해 인증 토큰을 검사 혹은 유효성 검사 등에 사용할 수 있습니다.

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







<hr>

[필터](https://derekpark.tistory.com/101) <br>
[참고](https://gardeny.tistory.com/35)



















