
# IAM > 사용자 > 사용자 추가

<img width="1676" alt="image" src="https://user-images.githubusercontent.com/74396651/230345117-257b478a-5b45-471f-abaa-3cd6bd906da5.png">

<img width="1319" alt="image" src="https://user-images.githubusercontent.com/74396651/230345331-3209dc94-d003-4902-8ba2-2a4429e1b8ea.png">

<img width="1325" alt="image" src="https://user-images.githubusercontent.com/74396651/230345961-98d9c18b-7e0b-4c5c-aa4a-fbb4bfd3ac16.png">

- AmazonS3FullAccess
- AWSCodeDeployFullAccess

<img width="1312" alt="image" src="https://user-images.githubusercontent.com/74396651/230346207-63411058-9821-4604-8928-ab87331509e6.png">

# IAM > 보안 자격 증명 > 아래로 스크롤 > 액세스 키 > 액세스 키 만들기

<img width="1341" alt="image" src="https://user-images.githubusercontent.com/74396651/230347081-d3d511e8-c15e-4204-8541-6edb14001e3b.png">

<hr>

# S3 버킷 생성
<img width="1695" alt="image" src="https://user-images.githubusercontent.com/74396651/230348917-c1ff3c3f-fee5-426a-8eb4-3052f1f51e31.png">

<img width="873" alt="image" src="https://user-images.githubusercontent.com/74396651/230349554-16c3d704-d562-483f-93f8-1fb3ad36e583.png">

<img width="840" alt="image" src="https://user-images.githubusercontent.com/74396651/230350346-ef36d792-0151-42da-ae89-bf9b14f17634.png">
- IAM 사용자로 바은 키를 사용하여 접근한다.

<img width="879" alt="image" src="https://user-images.githubusercontent.com/74396651/230349936-bbcffd49-8e07-4e46-8310-52101e5f21de.png">

<img width="1340" alt="image" src="https://user-images.githubusercontent.com/74396651/230350412-312ed74c-1910-4777-8a48-b947635e65cc.png">


<hr>

# EC2에 IAM 역할 추가(CodeDeploy용), IAM > 역할 > 역할 만들기

<img width="1364" alt="image" src="https://user-images.githubusercontent.com/74396651/230375556-6ac76895-00fa-40a3-a067-acc7e6a1826a.png">

<img width="1361" alt="image" src="https://user-images.githubusercontent.com/74396651/230377659-a4364c97-3319-4225-8c17-fe72defddb09.png">

<img width="1338" alt="image" src="https://user-images.githubusercontent.com/74396651/230377778-fc7f3b8d-189d-44f1-8517-10c5aed948a8.png">

<hr>

# EC2에 역할 등록(CodeDeploy)

<img width="1455" alt="image" src="https://user-images.githubusercontent.com/74396651/230378613-322ed63e-2375-4a6e-bbda-41ddd31bbaab.png">

<img width="894" alt="image" src="https://user-images.githubusercontent.com/74396651/230378752-fd217156-6366-4f44-8c35-7aae16a8ab11.png">

<img width="992" alt="image" src="https://user-images.githubusercontent.com/74396651/230379122-b47d76da-9664-46ca-b570-3bde1727e949.png">


<hr>

# EC2에 CodeDeploy 에어전트 설치

1. ec2 접속
2. apt 저장소 업데이트
```
sudo apt-get update
```
4. ruby 설치
```
sudo apt-get install ruby
```
5. wget 설치
```
sudo apt-get install wget
```
6. Agent 설치
```
# Seoul region
wget https://aws-codedeploy-ap-northeast-2.s3.ap-northeast-2.amazonaws.com/latest/install

# 권한 변경
chmod +x ./install

# 설치 진행
sudo ./install auto

# codedeploy-agent 상태 확인
sudo service codedeploy-agent status

# codedeploy-agent 서비스 시작
sudo service codedeploy-agent start

```

<hr>

# IAM CodeDeploy 역할 생성

<img width="1175" alt="image" src="https://user-images.githubusercontent.com/74396651/230382359-858921a0-b8ec-4091-9c6c-4c71fe6d563e.png">

<img width="1296" alt="image" src="https://user-images.githubusercontent.com/74396651/230382452-5399501a-a557-48fa-bcc7-8d143973179b.png">

<img width="1344" alt="image" src="https://user-images.githubusercontent.com/74396651/230382626-efa14d40-87b8-47a8-b8b8-0549d4ea48d6.png">

<hr>

# CodeDeploy 생성
<img width="1436" alt="image" src="https://user-images.githubusercontent.com/74396651/230383527-0de1cf2e-ae4e-4ab8-8d46-94fa2d6929f3.png">

<img width="876" alt="image" src="https://user-images.githubusercontent.com/74396651/230383678-29317e9f-cf3c-4e6a-97ac-f848a1ca6ba6.png">
- 애플리케이션 이름은 프로젝트 이름과 같게

# CodeDeploy 배포 그룹 생성 -> CodeDeploy 설정 

<img width="1357" alt="image" src="https://user-images.githubusercontent.com/74396651/230383845-d1f54360-e9e1-42da-b598-e103059083a6.png">

<img width="917" alt="image" src="https://user-images.githubusercontent.com/74396651/230384041-7c53b8a3-f03b-47d5-bd38-4f09bf9a2afb.png">

<img width="917" alt="image" src="https://user-images.githubusercontent.com/74396651/230384126-1ff81d95-04cb-4940-a242-dfd689d746db.png">

<img width="831" alt="image" src="https://user-images.githubusercontent.com/74396651/230384312-6b6c7c30-3a14-4b49-9181-4dc757f0bb8d.png">

<img width="1356" alt="image" src="https://user-images.githubusercontent.com/74396651/230385568-89defd56-1c54-4296-8fb4-66c5c56578c5.png">

<hr>

# Travis CI, S3, CodeDeploy 연동 및 연동 확인

# EC2에 S3에 zip 파일을 저장할 디렉토리 생성
> Travis CI에서 build가 끝나면 S3에 zip 파일이 전송되고 zip 파일은 ec2의 home/ubuntu/app/step2/zip 경로로 복사되어 압출이 풀리는 플로우
<img width="555" alt="image" src="https://user-images.githubusercontent.com/74396651/230387005-839af040-0625-4ee8-947c-9b16b29760e8.png">

# AWS CodeDeploy 설정 : appspec.yml

<img width="462" alt="image" src="https://user-images.githubusercontent.com/74396651/230388639-e22c359d-ce8f-40e9-83eb-079c14d1f4a2.png">

# .travis.yml에 추가

<img width="852" alt="image" src="https://user-images.githubusercontent.com/74396651/230390100-8cfc1839-1d54-461f-8eed-5680d00008ad.png">

- IAM 사용자 하나로 쭉 가는구나

<hr>

## Travis CI
![image](https://user-images.githubusercontent.com/74396651/230390744-aa1fb00b-2e8a-4f11-95f5-1f6971a325fe.png)

## S3
<img width="1336" alt="image" src="https://user-images.githubusercontent.com/74396651/230390879-1a0ed076-e698-4fbf-a4f5-54d64b9796df.png">

## CodeDeploy
<img width="1328" alt="image" src="https://user-images.githubusercontent.com/74396651/230391077-2e92df2d-df4f-4ec9-a9c4-6e8d086d225b.png">

## EC2

<img width="720" alt="image" src="https://user-images.githubusercontent.com/74396651/230391281-71bc1228-dcb2-4172-b082-e56d2d1d21ba.png">

<hr>







