## 1. GCP 프로젝트 생성
- vm, sql : resion : us-central1(아이오와)
## 2. VM 머신 생성

## 2-1. 생성 후

![image](https://user-images.githubusercontent.com/74396651/225280266-1c4f3805-28c3-45a3-a174-03f757865c97.png)

## 3. 고정 IP 할당

![image](https://user-images.githubusercontent.com/74396651/225280899-dced18a4-6b7b-462f-b9ea-59936032223e.png)

## 3-1
- 34.170. 로 시작하는 "와부" IP에 "예약 선택" 후 이름, 설명 기입

![image](https://user-images.githubusercontent.com/74396651/225281386-d74dd70a-1a5d-4078-a145-d9a48a3d0e0f.png)

-  고정 IP이 설정이 끝났으며 해당 IP를 따로 저장한다. (아래 SQL 접근 허용 IP에 사용한다.)

## 4. SQL 진입 후 인스턴스 생성

![image](https://user-images.githubusercontent.com/74396651/225282165-827146bf-85e6-4e27-91fc-46410875becd.png)

![image](https://user-images.githubusercontent.com/74396651/225282300-7ce02006-e427-4689-b0d8-fb2b85fe01ad.png)

![image](https://user-images.githubusercontent.com/74396651/225282382-70bc4243-8c96-4662-a07a-e41b2667bcb1.png)

![image](https://user-images.githubusercontent.com/74396651/225282451-ff4a2c8f-81b3-40de-87b7-2424f40f2895.png)

## 5. 사용자 계정 추가

![image](https://user-images.githubusercontent.com/74396651/225286125-d3d7b189-5c3e-4875-8e37-c3c85bfad1f1.png)

![image](https://user-images.githubusercontent.com/74396651/225286260-ed92f0d7-f837-4954-a6bf-2e2a729bd6b4.png)


## 6. 연결

![image](https://user-images.githubusercontent.com/74396651/225286375-fe55d63c-2b8c-4984-a938-2fb1683038f3.png)


![image](https://user-images.githubusercontent.com/74396651/225286645-83088329-550b-4bfb-ac25-e9f6ce0d3558.png)

- 정 부분을 보시면 아무리 공개 IP라 되어있더라도, 네트워크 승인 혹은 Cloud SQL Proxy를 사용해야 연결이 가능합니다. 
- 저희는 네트워크 승인을 진행토록 하겠습니다. 
- 네트워크 추가를 클릭합니다.










