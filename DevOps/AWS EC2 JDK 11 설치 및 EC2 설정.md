[Amazon Linux 인스턴스 사용 설명서](https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/set-time.html)

## 다운 가능한 JDK 보기
```
yum list java*jdk-devel
```

![image](https://user-images.githubusercontent.com/74396651/209340424-0bdb99d2-9426-4c29-bdf3-1c64e34c4aec.png)

<hr>

## JDK 11 설치
- Amazon에서 제공하는 OpenJDK인 Amazon Coretto를 다운받아 설치

```
# 1. aws coreetto 다운로드

sudo curl -L https://corretto.aws/downloads/latest/amazon-corretto-11-x64-linux-jdk.rpm -o jdk11.rpm

# 2. jdk11 설치

sudo yum localinstall jdk11.rpm

==================================================

# jdk version 선택

sudo /usr/sbin/alternatives --config java

# java 버전 확인

java --version

# 다운받은 설치키트 제거

rm -rf jdk11.rpm

==================================================
# 이전 버전 제거하기

yum list installed | grep "java" # yum 설치 리스트 확인
# java-1.8.0-openjdk-headless.x86_64    1:1.8.0.222.b10-0.47.amzn1   @amzn-updates
# java-11-amazon-corretto-devel.x86_64  1:11.0.7.10-1                installed

sudo yum remove java-1.8.0-openjdk-headless.x86_64 

```
## 1. aws coreetto 다운
![image](https://user-images.githubusercontent.com/74396651/209340980-b8a9ed92-7e67-44d0-8140-6cb4440ce797.png)

<hr> 

## 2. java 설치
![image](https://user-images.githubusercontent.com/74396651/209341205-92e91f0d-4ae2-418a-8dfd-6e0016f4bf32.png)

<hr> 

## 3. java 버전 확인
![image](https://user-images.githubusercontent.com/74396651/209341313-ab718ecf-9aaf-47a2-99c5-ff261cddd4a4.png)


<hr> 

## 타임존 변경
- EC2 서버의 기본 타임존은 UTC이다. 세계 표준 시간이므로 한국의 시간대가 아니다.(한국과 9시간 차이가 발생)
- 이 때문에 EC2 서버에서 수행되는 Java 어플케이션에서 생성되는 시간 모두 9시간 차이가 나기 때문에 꼭 설정해주어야 한다.

```
# 시간대 보기

ls /usr/share/zoneinfo

# zone 변경
# nano 파일 저장 : Ctrl + O -> Ctrl + X

sudo nano /etc/sysconfig/clock

"Asia/Seoul" 로 변경

# timezone 변경

sudo ln -sf /usr/share/zoneinfo/Asia/Seoulsu /etc/localtime

# 재부팅(인스턴스 페이지에서 인스턴스를 선택하고 인스턴스 상태, 인스턴스 재부팅을 차례로 선택)

sudo reboot

# 날짜 확인

date
```
![image](https://user-images.githubusercontent.com/74396651/209349688-4ea09d5f-4cf8-49ba-ae06-30c155a01f3c.png)


```
- reboot 했는데 Connection timeout 떠서 식겁...
```
![image](https://user-images.githubusercontent.com/74396651/209346227-12dfbe24-d094-463f-a35f-07b42b51c7f5.png)


<hr>


## 호스트 네임 변경(퍼블릭 DNS 이름 없이 호스트 네임 변경)

```
# 호스트네임변경

sudo hostnamectl set-hostname learning-springboot-webservice

# i 누르고 수정
# esc 누르고 :wq 누르고 나가기
# 여기서 HOSTNAME=learning-springboot-webservice 작성

sudo vim /etc/sysconfig/network
```
![image](https://user-images.githubusercontent.com/74396651/209349177-ff251aff-a403-4421-9bb2-0d69d1a7eba0.png)

```
# 호스트 주소 적용
# 여기서 127.0.0.1 hostname기입

sudo vim /etc/hosts
```
![image](https://user-images.githubusercontent.com/74396651/209349339-8ed7241a-2fc1-4f69-b067-1d4668030df2.png)

```
# 재부팅(인스턴스 페이지에서 인스턴스를 선택하고 인스턴스 상태, 인스턴스 재부팅을 차례로 선택)

sudo reboot

# 호스트네임 확인

hostname
```
![image](https://user-images.githubusercontent.com/74396651/209349516-f2d58ab5-af90-4866-ae43-8577e87e058b.png)

```
# 호스트 잘 등록되었는지
# 80포트로 접근이 안된다는 에러가 발생하면 성공!
# 80포트로 실행된 서비스가 없다는 뜻, curl 호스트 이름으로는 실행은 잘 된다는 뜻!

curl learning-springboot-webservice
```
![image](https://user-images.githubusercontent.com/74396651/209349959-855654de-0c3f-41f2-b5e6-b059e27d0edb.png)

<hr>

## 기본적인 EC2 설정은 완료되었다! 이제 AWS RDS를 생성하러 가보자!
[AWS RDS]()

