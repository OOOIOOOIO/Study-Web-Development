# OAuth2.0으로 구글 로그인

<hr>

## 사이트 접속
 - https://console.cloud.google.com/getting-started?pli=1 

<br>

## 구글 플랫폼 프로젝트 선택

![image](https://user-images.githubusercontent.com/74396651/199977257-76dfc28f-e98b-4897-b85f-7a172510fe66.png)

<br>

## 새 프로젝트 클릭

![image](https://user-images.githubusercontent.com/74396651/199977461-efb7a3e8-3781-4687-ba00-fe6c71fda742.png)

<br>

## 프로젝트 이름 입력 후 생성

![image](https://user-images.githubusercontent.com/74396651/199977768-fc128110-5f91-4b4d-8cec-40b86d122be5.png)

<br>

## 설정 페이지 이동 : API 및 서비스

![image](https://user-images.githubusercontent.com/74396651/199977944-e65e95c7-6f55-4f66-9ecd-74ab0b568916.png)

<br>

## 사용자 인증 정보 : 프로젝트 선택

![image](https://user-images.githubusercontent.com/74396651/199978103-4a6e8187-bc07-430d-9120-2b4b6e8504bf.png)

<br>

## 사용자 인증 정보 만들기 : OAuth 클라이언트 ID

![image](https://user-images.githubusercontent.com/74396651/199978223-8a7c1181-20db-44a0-9b4a-e4daf6f31333.png)

<br>

## 동의 화면 구성

![image](https://user-images.githubusercontent.com/74396651/199978385-dfa59aa4-a6a1-48be-a744-63ee50279a6e.png)

<br>

![image](https://user-images.githubusercontent.com/74396651/199979027-31aa407d-8642-4632-9623-c3d381d512e7.png)
- User Type
   - 내부 : G Suite 사용자만 앱을 사용할 수 있다.
   - 외부 : Google 계정이 있는 모든 사용자가 앱을 사용할 수 있다.

<br>

## OAuth 동의 화면
![image](https://user-images.githubusercontent.com/74396651/199982445-e10cc70c-517c-45fe-8e3d-fb824ebae6e2.png)

- 앱 정보의 앱 이름, 사용자 지원 이메일만 우선 작성
- 앱 도메인은 일단 스킵

<br>

## 범위
![image](https://user-images.githubusercontent.com/74396651/199982724-75485a05-59de-4730-b7e7-52d59ca1c008.png)

- email
- profile
- openid 가 기본.

<br>

## 테스트 사용자는 스킵

<br>

## OAuth 클라이언트 ID
![image](https://user-images.githubusercontent.com/74396651/199983540-bbef1d78-cf14-46fe-bd41-5e19a083671b.png)

<br>

## 웹 선택
![image](https://user-images.githubusercontent.com/74396651/199983595-6553fd09-9b4c-433c-b93d-301a5e8a4c45.png)

<br>

## 승인된 리디렉션 URI
![image](https://user-images.githubusercontent.com/74396651/199983842-26408e1b-f34a-46de-8475-d2ac0a4deada.png)

<br>

## 생성 완료
![image](https://user-images.githubusercontent.com/74396651/199983963-78869b3a-647d-428f-a823-78ed52e2358e.png)

<br>

## 확인하기
![image](https://user-images.githubusercontent.com/74396651/199984260-74953f63-b146-4270-83ee-71b093bf03f3.png)
- 클릭해서 확인할 수 있다.

<br>
<hr>
<br>

# 클라이언트 ID, 클라이언트 보안 비밀번호를 프로젝트에 적용

- application-oauth.properties 생성
![image](https://user-images.githubusercontent.com/74396651/199986150-9437d439-bb53-4aae-86d4-ad14a2139d55.png)

<br>

![image](https://user-images.githubusercontent.com/74396651/199986251-c48b38a7-d111-4bed-ae40-0fc2a4a2673f.png)

<br>

- application.properties에 프로필 등록
![image](https://user-images.githubusercontent.com/74396651/199986912-fd78b8c2-e229-40c2-8e07-d45109c80deb.png)

<br>

<br>
<hr>
<br>

# gitignore 등록

![image](https://user-images.githubusercontent.com/74396651/199987227-6b7f16dd-f047-448b-93fb-976cda276f35.png)

- 추가한 뒤 commit 했을 때 추가되지 않으면 성공!





