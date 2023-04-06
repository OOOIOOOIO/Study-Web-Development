# Travis CI란
Travis CI는 github에서 제공하는 무료 CI 서비스이다. Jenkins의 경우 설치하여 사용해야 하지만 Travis CI는 오픈소스 웹 서비스이기 때문에 오픈 소스인 경우 무료로 사용이 가능하다. 

## 사용법
1. 깃헙으로 로그인

![image](https://user-images.githubusercontent.com/74396651/230341631-9548cc5f-0c6b-4619-a70e-ccf0742c79f7.png)

2. 프로필 > settins > repositories 진입

![image](https://user-images.githubusercontent.com/74396651/230341824-5879009c-9ade-4f68-a14d-83f472f40322.png)

3. 깃헙에서 적용할 레포 선택 -> travis에서 할 건 끝

![image](https://user-images.githubusercontent.com/74396651/230342087-7cd7bd8f-20fd-48c4-aa3a-47f0c72c0205.png)

<hr>

4. ide에서 .travis.yml 설정

<img width="249" alt="image" src="https://user-images.githubusercontent.com/74396651/230342530-c4255c57-7cf3-4c04-aa3f-7f09923fc676.png">

<img width="593" alt="image" src="https://user-images.githubusercontent.com/74396651/230342693-8b8c498e-b030-4f97-a1c3-10c32485bb33.png">

5. 설정 완료 후 commit-push할 경우 아래와 같이 build됨(이메일도 옴)

![image](https://user-images.githubusercontent.com/74396651/230344378-864822b7-59bb-4e5f-b083-dbbf3df79cc7.png)

![image](https://user-images.githubusercontent.com/74396651/230344465-0534461e-4b77-4c93-9579-739c858e099e.png)

<img width="379" alt="image" src="https://user-images.githubusercontent.com/74396651/230344625-d7c91e5d-7fb5-4e6f-aec5-98845a13c79b.png">
