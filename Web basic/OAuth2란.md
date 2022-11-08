# OAuth2(Open Authorization, Open Authorization2)
- 인증을 위한 표준 프로토콜이다. 
- 2006년 Twitter와 Google이 정의한 개방형 Authorization 표준이며, API 허가(Authorize)를 목적으로 JSON 형식으로 개발하였다.
- 구글, 페이스북, 카카오, 네이버 등에서 제공하는 Authorization Server를 통해 회원 정보를 인증하고 Access Token을 발급받는다.
- 발급받은 Access Token을 이용해 타사의 API 서비스를 이용할 수 있다.

<br>

## OAuth 역사

#### 1. OAuth 1.0
- [RFC 5849: The OAuth 1.0 Protocol](https://tools.ietf.org/html/rfc5849) 에 정의되어 있다.
- [세션 고정 공격](https://en.wikipedia.org/wiki/Session_fixation)에 취약점이 발견되어 1.0a 개정판이 나오게 되었다.

<br>

#### 2. OAuth 1.0 Revision A
- 1.0 spec의 세션 고정 공격을 해결하기 위해 만들어졌다.
- 1.0a 개정판을 만들었는데, 2.0 인증 프레임워크가 나와 폐기되었다.

<br>

#### 3. OAuth 2.0
- [RFC 6749 : The OAuth 2.0 Authorization Framework](https://datatracker.ietf.org/doc/html/rfc6749)에 정의되어 있다.
- [혼합 공격 Mix-Up Attack](https://danielfett.de/2020/05/04/mix-up-revisited/#:~:text=A%20Mix%2DUp%20Attack%20on,server%20under%20the%20attacker's%20control.)에 취약점을 발견
- 많은 패치와 확장을 거쳐 현재에 이르렀다!

<br>

#### 4. OAuth 2.1 ([현재 개발중](https://oauth.net/2.1/))
- 모든 인증 코드 부여에 대해 PKCE가 필요하다는 것이 포인트다.
- Leagacy 들은 버릴 예정이다(Implicit, Password Credentials)
- 계속 발전 중이다.(2022.11에 들어가봄)


<br>

## 용어 정리
- Resource Owner(자원 소유자)
   - Resource Server의 계정을 소유하고 있는 사용자를 의미한다. 
- Client
   - 구글, 페이스북, 카카오 등 API 서비스를 이용하는 제 3의 서비스
- Authorization Server(권한 서버)
   - 권한을 관리해 주는 서버
   - Access Token, Refresh Token을 발급, 재발급 해주는 역할을 한다.
- Resourc Server
   - OAuth2 서비스를 제공하고, 자원을 관리하는 서버이다.
- Access Token
   - Authorization Server로 부터 발급 받은 인증 토큰
   - Resource Server에 전달하여 서비스를 제공 받을 수 있다.

<br>

## OAuth 2.0 인증 과정

> &nbsp;다양한 클라이언트 환경에 적합한 인증(Authentication) 및 인가(Authorization)의 부여 방법(Grant Type)을 제공하고 그 결과로 클라이언트에게 접근 토큰(Access Token)을 발급하는 것에 대한 구조(프레임워크)이다.

![image](https://user-images.githubusercontent.com/74396651/200621039-b000ea66-3753-4621-8f11-a31071bbe91b.png)

- 사진 참고 : [페이코](https://developers.payco.com/guide/development/start)

<br>

### 다양한 클라이언트 환경
> &nbsp;스마트폰, 모바일 기기 등 인터넷 접근이 쉬워졌으며 2012년 이후 웹과 모바일 환경은 극적으로 변화하였다. 이젠 데스크탑 보다 모바일 장치에서 인터넷에 더 많이 접근하며, 단일 페이지 앱(SPA) 개발은 일반적인 방법으로 널리 통용되고 있다. 수 많은 데이터베이스의 암호가 탈취되는 사건들로 인해 암호를 저장하는 것이 위험하다는 것이 거듭 증명되고 있다.

<br>

### 적합한 인증 및 인가의 부여 방법(Grant Type)을 제공한다.
> &nbsp;변화하는 환경을 위해 OAuth 2.0 Framework는 수 년에 걸친 패치와 사양들을 추가하였다. RFC6749는 Authorization Code, Implicit, Password and Client Credentials 4가지 권한 부여 방법을 진행할 수 있다. <br>
- Authorization Code
   - Web, Mobile App에서 많이 사용한다.
   - Access Token을 위한 권한 인증 코드(Authorization Code)를 받을 때 사용하는 인증 방법 
- Implicit(Legacy)
   - Browser based App, Mobile App에서 많이 사용한다.
   - 권한 인증 코드(Authorization Code)없이 바로 Access Token을 받는다. 
- Password(Legacy)
   - 일반적인 아이디/비밀번호 입력 형태로 Access Token을 받는 방법
   - 보통 자체 서비스에서만 사용되며, 외부 개발자가 사용할 수 없게 하는게 일반적이다.(비밀번호를 직접 입력하는 방법이라 보안에 위배된다.)
- Client Credentials
   - 사용자(User)가 아닌 Client(프로그램)을 인가(Authorization)가 필요할 때 사용하는 방법 

<br>

### Grant Type 변화
- #### [RFC6749](https://datatracker.ietf.org/doc/html/rfc6749)
- Authorization Code
- Implicit
- Resource Owner Password Credentials
 -Client Credentials

- #### [RFC7636](https://datatracker.ietf.org/doc/html/rfc7636)
- PKCE(Proof key for Code Exchange by OAuth Public Clients)
   -Mobile App에 더 나은 방법이 필요하다는 것이 분명 해졌고, Client Secret 없이 Authorization Code Flow을 사용할 수 있는 방법을 제공하기 위해 만들어짐

- #### [RFC8252](https://datatracker.ietf.org/doc/html/rfc8252)
- PKCE for mobile
   - OAuth 2.0 for Native Apps : Native App이 PKCE 확장과 함께 Authorization Code Flow을 사용할 것을 권장

- #### [RFC8628](https://datatracker.ietf.org/doc/html/rfc8628)
- Device Authorization Grant
   - Apple TV 또는 Youtube 스트리밍 비디오 인코더 같은 브라우저가 없거나 키보드가 없는 기기와 함께 OAuth를 사용해야 하는 상황이 생겼다. 이 문제를 해결하기 위해 완전히 새로운 OAuth Grant Type 이 생겨났다
 
### 결론적으로 2021년이 마무리 되는 때에는 아래 3가지 위임 방법(Grant Type)

- Authorization Code + PKCE
- Client Credentials
- Device Authorization
정도로 정리가 된다.
> &nbsp; 만약 OAuth로 개발하고 사용한다면 Authorization Code + PKCE 형태를 사용하느 ㄴ것이 가장 현실적이고 합당한 방법이다.<br>
> &nbsp;Client(프로그램)이 인증받기를 원한다면 Client Credentials를 사용하고 특정 Device가 인증 받기를 원한다면 Device Authorization Grant를 사용하면 된다.


<br>

### PKCE란
- RFC7636에 정의된 PKCE는 Authorization Code flow의 확장팩이다.
- Authorization Code Grant를 사용하는 OAuth 2.0 public client는 인증 코드 가로채기 공격에 취약한데, 이 부분을 해결하기 위해 나온 방법이다. Code Exchange용 Proof Key를 통해 해당 공격에 대응한다.
<br>

### OAuth는 결과로 클라이언트에게 접근 토근(Access Token)을 발급한다.
- JSON 방식의 JWT(JSON Web Tokens)를 Access Token 형식으로 발급 받는다.
- Spring Boot환경에서 Spring Security를 이용한 인증/인가 구현할 경우 최초 로그인 시 OAuth2 인증을 통해 우리의 서버에 접근할 수 있는 JWT토큰을 발급해준다.
- 이후 서버에 접근할 때 Request Header에 발급해준 토큰을 함께 보내면 Spring Security의 UsernamePasswordAuthenticationFilter 앞에 등록된 필터에서 인증정보를 생성해줍니다.


<br><br><br><br>
참조 : https://www.ssemi.net/what-is-the-oauth2/





