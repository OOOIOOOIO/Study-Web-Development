> OAuth2로 소셜 로그인을 하면서 Security와 많이 친해졌지만 그래도 한번 기본 흐름은 정리를 해야할 것 같아 정리한다. 

# Spring Security란
Spring Security는 Spring 기반의 애플리케이션의 보안(인증과 권한, 인가 등)을 담당하는 스프링 하위 프레임워크이다. Spring Security는 '인증'과 '권한'에 대한 부분을 Filter 흐름에 따라 처리하고 있다. Filter는 Dispatcher Servlet으로 가기 전에 적용되므로 가장 먼저 URL 요청을 받지만, Interceptor는 Dispatcher와 Controller사이에 위치한다는 점에서 적용 시기의 차이가 있다. 또한 Filter는 Spring Context 밖에 있기 때문에 예외처리를 하귀 위해선 예외처리용 필터를 하나 빼야한다.<br>
Spring Security는 보안과 관련해서 체계적으로 많은 옵션을 제공해주기 때문에 개발자 입장에서는 일일이 보안관련 로직을 작성하지 않아도 된다는 장점이 있다.

<br>

## 특징
- 인증과 권한 부여에 대한 지원과 유연한 확장
- 세션 고정, 클릭재킹, 사이트 간 요청 위조 등의 공격으로부터 보호
- 모든 URL을 가로채어 인증을 요구한다.
- 서블릿 API 통합
```
- 세션고정
 - 사용자의 세션 ID라고도 하는 세션 식별자에 영향을 준 다음 이를 사용하여 계정에 액세스할 수 있게 하는 것
- 클릭재킹
 - 웹 사용자가 자신이 클릭하고 있다고 인지하는 것과 다른 어떤 것을 클릭하게 속이는 악의적인 기법
- 서블릿
 - 클라이언트가 어떠한 요청을 하며 그에 대한 결과를 다시 전송해주는 역할을 하는 자바 프로그램
```
<br>

## 인증관련 Architecture

![image](https://user-images.githubusercontent.com/74396651/229701389-edf0a699-ef02-4741-b34b-b7a0745eaf9a.png)

- 요청이 들어오면 인증을 담당하는 필터(AuthenticationFilter)를 거친다. AuthenticationFilter는 사용자의 세션 ID(JSESSIONID)가 Security Context에 있는지 확인한다. Security Context에 세션 ID가 없다면 아래 로직을 수행한다.

1. 사용자가 로그인 정보를 입력하고 인증 요청을 보냄(http request)

2. AuthenticationFilter에서 UsernamePasswordAuthenticationToken을 생성

3. AuthenticationFilter에게 인증용 객체(UsernamePasswordAuthenticationToken)을 AuthenticaionManager 에게 전달.
(Manager는 등록된 AuthenticationProvider를 통해 사용자 정보를 조회)

4. 실제 인증을 할 AuthenticationProvider에게 Authentication객체(UsernamePasswordAuthenticationToken)을 다시 전달한다.

5. DB에서 사용자 인증 정보를 가져올 UserDetailsService 객체에게 사용자 아이디를 넘겨주고 DB에서 인증에 사용할 사용자 정보(사용자 아이디, 암호화된 패스워드, 권한 등)를 UserDetails(인증용 객체와 도메인 객체를 분리하지 않기 위해서 실제 사용되는 도메인 객체에 UserDetails를 상속하기도 한다.)라는 객체로 전달 받는다.

6. AuthenticationProvider는 UserDetails 객체를 전달 받은 이후 실제 사용자의 입력정보와 UserDetails 객체를 가지고 인증을 시도한다.

7.UserDetailsService는 DB에 저장된 회원의 비밀번호와 비교해 일치하면 UserDetails 인터페이스를 구현한 객체를 반환한다.

8. AuthenticationProvider 인터페이스에 의해 사용자가 성공적으로 인증되면, 완전히 채워진 인증개체가 반환된다.

9. AuthenticationManager는 획득한 완전히 채워진 인증개체를 관련 인증 필터(AuthenticationFilter)로 다시 반환한다

10. 인증이 완료되면 사용자 정보를 가진 Authentication 객체를 SecurityContextHolder(spring security의 인메모리 세션저장소)에 담은 이후 성공시 AuthenticationSuccessHandle를 실행한다.(실패시 AuthenticationFailureHandler를 실행한다.)

<br>

## 인증(Authorization)과 인가(Authentication)
- 인증 : 해당 사용자가 본인이 맞는지를 확인하는 절차
- 인가 : 인증된 사용자가 요청한 자원에 접근 가능한지를 결정하는 절차
- Principal(접근 주체) : 보호받는 Resource에 접근하는 대상
- Credential(비밀번호) : Resource에 접근하는 대상의 비밀번호

Spring Security는 기본적으로 인증 절차를 거친 후에 인가 절차를 진행하게 되며, 인가 과정에서 해당 리소스에 대한 접근 권한이 있는지 확인을 하게 된다. Spring Security에서는 이러한 인증과 인가를 위해 Principal을 아이디로, Credential을 비밀번호로 사용하는 Credential 기반의 인증 방식을 사용한다. 

<br>

## 주요 모듈

![image](https://user-images.githubusercontent.com/74396651/229701970-1490ddb8-7236-42d8-8af4-029e6a930725.png)

### SecurityContextHolder
SecurityContextHolder는 보안 주체의 세부 정보를 포함하여 응용프래그램의 현재 보안 컨텍스트에 대한 세부 정보가 저장된다. SecurityContextHolder는 기본적으로 SecurityContextHolder.MODE_INHERITABLETHREADLOCAL 방법과 SecurityContextHolder.MODE_THREADLOCAL 방법을 제공한다.

 

### SecurityContext
Authentication을 보관하는 역할을 하며, SecurityContext를 통해 Authentication 객체를 꺼내올 수 있다. 인증된 사용자의 정보(인증 개체)를 저장하는 공간

 

### Authentication 
Authentication는 현재 접근하는 주체의 정보와 권한을 담는 인터페이스이다. Authentication 객체는 Security Context에 저장되며, SecurityContextHolder를 통해 SecurityContext에 접근하고, SecurityContext를 통해 Authentication에 접근할 수 있다.

![image](https://user-images.githubusercontent.com/74396651/229702789-2beea5a2-2627-4e18-9ec8-ccf7236a8668.png)


### UsernamePasswordAuthenticationToken
UsernamePasswordAuthenticationToken은 Authentication을 implements한 AbstractAuthenticationToken의 하위 클래스로, User의 ID가 Principal 역할을 하고, Password가 Credential의 역할을 한다. UsernamePasswordAuthenticationToken의 첫 번째 생성자는 인증 전의 객체를 생성하고, 두번째 생성자는 인증이 완려된 객체를 생성한다.

![image](https://user-images.githubusercontent.com/74396651/229702851-9c11da54-91e2-4461-87e8-733995052dbc.png)


### AuthenticationProvider 
AuthenticationProvider에서는 실제 인증에 대한 부분을 처리하는데, 인증 전의 Authentication객체를 받아서 인증이 완료된 객체를 반환하는 역할을 한다. 아래와 같은 AuthenticationProvider 인터페이스를 구현해서 Custom한 AuthenticationProvider을 작성해서 AuthenticationManager에 등록하면 된다.

![image](https://user-images.githubusercontent.com/74396651/229702898-0ee64320-c7ea-4514-9a5a-23f1c364820d.png)


### Authentication Manager 
인증에 대한 부분은 SpringSecurity의 AuthenticatonManager를 통해서 처리하게 되는데, 실질적으로는 AuthenticationManager에 등록된 AuthenticationProvider에 의해 처리된다. 인증이 성공하면 2번째 생성자를 이용해 인증이 성공한(isAuthenticated=true) 객체를 생성하여 Security Context에 저장한다. 그리고 인증 상태를 유지하기 위해 세션에 보관하며, 인증이 실패한 경우에는 AuthenticationException를 발생시킨다.

![image](https://user-images.githubusercontent.com/74396651/229703105-e1c3daf4-a7ef-46e5-8e84-84186b240711.png)


AuthenticationManager를 implements한 ProviderManager는 실제 인증 과정에 대한 로직을 가지고 있는 AuthenticaionProvider를 List로 가지고 있으며, ProividerManager는 for문을 통해 모든 provider를 조회하면서 authenticate 처리를 한다.

![image](https://user-images.githubusercontent.com/74396651/229703891-c7f7b082-77ae-4473-8d81-4b68324ac3e8.png)

위에서 설명한 ProviderManager에 우리가 직접 구현한 CustomAuthenticationProvider를 등록하는 방법은 WebSecurityConfigurerAdapter를 상속해 만든 SecurityConfig에서 할 수 있다. WebSecurityConfigurerAdapter의 상위 클래스에서는 AuthenticationManager를 가지고 있기 때문에 우리가 직접 만든 CustomAuthenticationProvider를 등록할 수 있다.(스프링 2.7을 기준으로 WebSecurityConfigurerAdapter는 deprecated 되었다. configure 대신 filterchain을 통해 등록한다.)

![image](https://user-images.githubusercontent.com/74396651/229704056-8c9b1343-4ab1-41a6-86e1-5e377a4ed6b2.png)

![image](https://user-images.githubusercontent.com/74396651/229704955-23a557f6-4543-4ff2-8a43-45926380250b.png)


### UserDetails 
인증에 성공하여 생성된 UserDetails 객체는 Authentication객체를 구현한 UsernamePasswordAuthenticationToken을 생성하기 위해 사용된다. UserDetails 인터페이스를 살펴보면 아래와 같이 정보를 반환하는 메소드를 가지고 있다. UserDetails 인터페이스의 경우 직접 개발한 UserVO 모델에 UserDetails를 implements하여 이를 처리하거나 UserDetailsVO에 UserDetails를 implements하여 처리할 수 있다.

![image](https://user-images.githubusercontent.com/74396651/229705156-78cb1660-479a-4025-b219-3d9807f7a599.png)


### UserDetailsService 
UserDetailsService 인터페이스는 UserDetails 객체를 반환하는 단 하나의 메소드를 가지고 있는데, 일반적으로 이를 구현한 클래스의 내부에 UserRepository를 주입받아 DB와 연결하여 처리한다. UserDetails 인터페이스는 아래와 같다.

![image](https://user-images.githubusercontent.com/74396651/229705194-0cae5420-97dc-4a79-9b60-0894f7b6d6f7.png)


### Password Encoding 
AuthenticationManagerBuilder.userDetailsService().passwordEncoder() 를 통해 패스워드 암호화에 사용될 PasswordEncoder 구현체를 지정할 수 있다.

![image](https://user-images.githubusercontent.com/74396651/229705395-82563973-e781-4b17-998d-1e123090ec90.png)


### GrantedAuthority 
GrantAuthority는 현재 사용자(principal)가 가지고 있는 권한을 의미한다. ROLE_ADMIN나 ROLE_USER와 같이 ROLE_*의 형태로 사용하며, 보통 "roles" 이라고 한다. GrantedAuthority 객체는 UserDetailsService에 의해 불러올 수 있고, 특정 자원에 대한 권한이 있는지를 검사하여 접근 허용 여부를 결정한다.






<br>
<br>
<br>
<br>
<br>
[참고-공식 레퍼런스](https://docs.spring.io/spring-security/reference/reactive/oauth2/client/index.html)<br>
[참고-깃헙](https://github.com/OOOIOOOIO/Web-Security-JWT-login-project/blob/master/src/main/java/com/sh/loginpratice/websecurityjwt/config/jwt/AuthTokenFilter.java)<br>
[참고1](https://mangkyu.tistory.com/76)<br>
[참고2](https://velog.io/@shkim1199/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%8B%9C%ED%81%90%EB%A6%AC%ED%8B%B0-Spring-Security)<br>
[참고3](https://velog.io/@kose/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8-2.7.3-Security)



