# 생성

ssh-keygen -t rsa -f ~/.ssh/rsa-gcp-key -C "polite159";     

## permision denied

![image](https://user-images.githubusercontent.com/74396651/226815877-3ff834ce-a7dd-4cd6-a6ca-b0aa7b181e47.png)
- pub : chmod 644로 권한 바꿈
- 그냥 : chmod 400

## UNPROTECTED PRIVATE KEY FILE
- rsa key 파일(pub 말고 그냥) 권한 오류 chmod 400 or 600으로 변경

<br>

# 접속1(key)
ssh -i ssh키위치 아이디@외부아이피
ssh -i ~/.ssh/rsa-gcp-key polite159@외부아이피

# 접속2(vim으로 config 파일)

## vim 파일 만들기
- vim ~/.ssh/config
![image](https://user-images.githubusercontent.com/74396651/229997409-2942d4ce-0726-4832-8444-6b0f5492107f.png)

## 권한변경 및 접속하기
chmod 700 ~/.ssh/config
ssh HostName 으로 접속하기
