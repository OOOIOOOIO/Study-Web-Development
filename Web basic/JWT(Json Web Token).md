# JWT(Json Web Token)란
> &nbsp;JWT는 개체와 개체 간 정보 전달 및 권한 인가(Authorization)를 위해 Json 형식 토큰이다. URL-safety하기 때문에 Header 또는 URL을 통해 전달한다.

- [JWT 잘 성명해줌](https://escapefromcoding.tistory.com/255#header-parameters)

<br>

# JWT 구조
> &nbsp;JWT는 Header, Payload, Signature로 이루어져있다. 각 부분은 Json형태로 저장되어 있으며, 각각 Base64로 인코딩 되어 있고, "."을 구분자로 각각의 부분을 구분한다.

![image](https://user-images.githubusercontent.com/74396651/204966947-a7b76488-9589-4711-93e5-81a722aa0d56.png)

![image](https://user-images.githubusercontent.com/74396651/204975348-9840dc91-73d6-42a8-aff4-6d296da1593b.png)


- Header : 토큰에 대한 기본정보
- Payload : 전달할 정보
- Signature : 검증된 토큰 정보

<br>

## Header(헤더)
> &nbsp;토큰에 대한 기본 정보가 저장된다.
- alg : Signature를 어떤 알고리즘으로 암호화 했는지 명시한다. ex) SHA256, RSA
- typ : 토큰의 타입을 지정한다. ex) jwt

<br>

## Payload(페이로드)
> &nbsp;전달하려고 하는 정보가 저장되어있다. 크게 3가지로 구분된다.

- ### 등록된 클레임(Registered Claim)
> &nbsp;이름이 이미 정해진 클레임. 등록된 클레임은 모두 선택적으로 사용할 수 있다.
   - iss(issuer) : 토큰 발급자
   - sub(subject) : 토큰 제목
   - aud(audience) : 토큰 대상자
   - exp(expiration) : 토큰 만료 시간, NumericDate 형식으로 저장된다. (ex : 1480849147370)
   - nbf(not before) : 토큰 활성 날짜, 이 날이 지나기전에 토큰이 활성화되지 않는다.
   - iat(isuued at) : 토큰 발급 시간, 토큰 발급 이후의 경과시간을 알 수 있다.
   - jti : JWT의 고유 식별자, 중복적인 처리를 방지하기 위해서 사용한다. 

<br>

- ### 공개 클레임(Public Claim)
> &nbsp;사용자 정의 클레임으로, 공개용 정보를 위해 사용된다.

<br>

- ### 비공개 클레임(Private Claim)
> &nbsp;서버와 클라이언트 간 협의하에 사용되는 클레임이다.

<br>

## Signature(서명)
> &nbsp;토큰의 유효성을 검증할 때 사용되는 서명이다. 서명은 헤더와 페이로드의 값을 각각 BASE64로 인코딩하고 "."을 구분자로 붙여준다. 그리고 비밀키를 이용하여 헤더에 정의한 알고리즘으로 암호화한 후 다시 BASC64로 인코딩한다.


<br>

# JWT 용도
- ## 회원 인증
> &nbsp;회원이 로그인을 하면 서버는 회원정보가 담긴 토큰을 발급해 줍니다. 그리고 회원이 서버에 데이터를 요청할 때 마다 발급 받은 토큰 정보를 함께 전달하여, 인증받은 회원이라는 것을 증명한다.

- ## 정보 전달
> &nbsp;JWT는 Signature 정보를 이용하여 데이터 조작을 방지할 수 있으며, 안전하게 정보를 전달할 수 있다.

<br>

# 완성된 JWT

![image](https://user-images.githubusercontent.com/74396651/208608465-8f66f879-6688-4df0-a260-f406142863e7.png)


<br>

# JWT 장단점

## 장점
- ### Stateless이며 확장성이 있다.
> &nbsp;서버와 클라이언트 간에 상태 정보를 저장하지 않으며 토큰을 가지고 있고 유효하다면 서버로 부터 데이터를 요청할 수 있습니다. 즉 개체간에 연결을 유지하지 않아도 됩니다.<br><br>

- ### 사용자 인증에 필요한 인증 저장소가 필요없다.
> 인증 정보가 토큰 자체에 포함되어있기 때문에 별도의 인증 저장소가 필요없다.<br><br>

- ### 쿠키 불필요
> 쿠키를 전달하지 않아도 되므로 쿠키를 사용함으로써 발생하는 취약점이 사라진다.<br><br>

- ### 독립적인 JWT
> &nbsp;서버를 여러대 사용하거나, 클라이언트 단말의 종류(PC, 모바일 등..)가 다양할 경우에도 토큰만 유효하다면 어떤 단말이든지간에 서버로 부터 데이터를 받을 수 있습니다. <br><br>
> 즉 JWT 인증 시스템을 만들어 두면 플랫폼에 상관없이 회원 인증 시스템 및 정보 전달이 가능합니다. 


<br>

## 단점
- ### 한번 발급된 토큰은 수정 및 삭제할 수 없다.
> &nbsp;JWT는 stateless하기 때문에 토큰에 대한 정보를 관리하지 않습니다. 그래서 한번 만들어진 토큰은 삭제, 수정할 수 없습니다. 만약 토큰이 유출된다면 토큰이 만료될 때 까지 토큰을 가로챈 사용자가 서비스를 이용할 수 있다.

- ### 토큰 길이
> 토큰의 PayLoad에 3종류의 클레임을 저장하기 때문에, 정보가 많아질수록 토큰의 길이가 늘어나 네트워크에 부하를 줄 수 있다.
- ### Payload 인코딩
> &nbsp;Payload 값이 암호화된 값이 아닌 Base64로 인코딩 된 값이라 누구나 확인할 수 있습니다. 그래서 중요한 정보를 Payload에 저장해서는 안된다.


<br>
<br>
<br>
<br>
<br>

참고
https://bamdule.tistory.com/144
https://jwt.io/
















