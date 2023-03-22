# 생성

ssh-keygen -t rsa -f ~/.ssh/rsa-gcp-key -C "polite159";     

## permision denied

![image](https://user-images.githubusercontent.com/74396651/226815877-3ff834ce-a7dd-4cd6-a6ca-b0aa7b181e47.png)
- pub : chmod 644로 권한 바꿈
- 그냥 : chmod 400

## UNPROTECTED PRIVATE KEY FILE
- rsa key 파일(pub 말고 그냥) 권한 오류 chmod 400으로 변경
- 
# 실행

ssh -i ~/.ssh/rsa-gcp-key polite159@외부아이피
