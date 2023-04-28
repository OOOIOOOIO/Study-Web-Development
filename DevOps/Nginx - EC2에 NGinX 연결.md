 # EC2에 NGinX 설치(Unbutu)
```
sudo apt-get install nginx

```

<hr>

# 명령어
```

# 시작
sudo service nginx start / sudo systemctl start nginx

# 재시작
sudo service nginx restart

# 중지
sudo service nginx stop

# 상태 확인
sudo systemctl status nginx

# 설정 reload
sudo service nginx reload

# 환경설정 파일 찾기
sudo find / -name nginx.conf


```

<img width="1090" alt="image" src="https://user-images.githubusercontent.com/74396651/230545433-624c7e80-7f6b-476c-b07e-feec29bbd478.png">

<hr>

# 버전 확인
```
sudo dpkg -l nginx
```

<img width="875" alt="image" src="https://user-images.githubusercontent.com/74396651/230545681-d7c4b292-9a5f-4406-9d1d-740af95d3995.png">

# 웹에서 확인(포트번호 없이 도메인 주소만)

![image](https://user-images.githubusercontent.com/74396651/230546458-f83a450f-ef65-4a5f-86ca-f09f299fd9d9.png)

<hr>

# EC2에 nginx 포트 인바운드 보안그룹 추가

<img width="1531" alt="image" src="https://user-images.githubusercontent.com/74396651/230545983-340dfdca-df80-44ff-a660-a621a30c3f91.png">

<hr>

# 소셜로그인 시 리다이렉션 주소 추가(:9999 삭제)

![image](https://user-images.githubusercontent.com/74396651/230546080-9e938e41-18cd-4883-b216-2b40d672eb9a.png)

![image](https://user-images.githubusercontent.com/74396651/230546280-97114b3c-431b-4c79-ac1e-b20d44d02f6a.png)

<hr>

 
 
 ## Nginx 설정 파일로 이동
 ````
 cd /etc/nginx
 
 vim nginx.conf
 ````
 
## Nginx 기본 설정

![image](https://user-images.githubusercontent.com/74396651/235079171-80611d38-63af-463a-8b47-edb1329849a7.png)

- 텍스트 Core 모듈	: 코어 모듈은 대부분 환경 설정 파일의 최상단에 위치하며 한번만 사용할 수 있다. nginx의 기본적인 동작 방식을 정의한다.
- Http 블록 :	웹서버에 대한 동작을 설정하는 영역으로, server 블록과 location 블록의 루트 블록이다.
- Server 블록	: 하나의 웹사이트를 선언하는 데 사용된다. 가상 호스팅(Virtual Host)의 개념이다.
- Location 블록	: server 블록 내에서 특정 URL을 처리하는 방법을 정의한다.
- Events 블록	: 주로 네트워크 동작에 관련된 설정하는 영역으로, 이벤트 모듈을 사용한다.
















