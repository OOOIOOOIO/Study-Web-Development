# 설치
> brew install nginx

# 설정
nginx를 설치하면 기본적으로 포트가 8080으로 잡혀있다. homebrew로 설치하면 현재 로그인된 사용자(mac의 user)의 권한으로 진행되는데,
nginx는 root가 아닌 사용자가 nginx를 설치할 경우 1024 포트보다 작은 포트를 사용할 수 없도록 되어 있다.

80 포트를 사용하고 싶다면 설정 파일의 포트를 변경한 후에 설정 적용 시 'sudo'를 붙여야 한다. 포트번호를 80으로 설정해보자

- nginx.cof 파일 수정하기
  - sudo vi/opt/homebrew/etc/nginx/nginx.conf
- 8080 -> 80으로 수정

<img width="555" alt="image" src="https://user-images.githubusercontent.com/74396651/236635678-12ce5190-98ef-4086-bf03-083350c67f2e.png">
