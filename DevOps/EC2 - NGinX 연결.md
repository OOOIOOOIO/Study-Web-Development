 # EC2에 NGinX 설치
```
sudo apt-get install nginx

```

<hr>

# 시작 및 설치 확인
```

# 시작
sudo systemctl start nginx  / sudo service nginx start

# 확인
sudo systemctl status nginx

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

 # Nginx와 spinrg boot 연동
 
 ## Nginx 설정 파일로 이동
 ````
 cd /etc/nginx
 
 vim nginx.conf
 ````
 
### http 블럭 안에
















