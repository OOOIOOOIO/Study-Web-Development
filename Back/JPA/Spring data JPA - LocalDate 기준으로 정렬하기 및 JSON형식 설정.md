# 정리
- 우선 Repository 에서 날짜 기준으로 정렬하기 위해선 OrderBy필드명Desc로 기입해야한다.(OrderBy 없이 바로 Desc 썼는데LocalDate에는 desc 프로퍼티가 없다고 에러남!
- 그리고 잘 정렬되서 나왔는데 Json의 LocalDate 타입이 [2023, 3, 3] 이런 식으로 배열로 나왔다. 그래서 DTO에 JsonFormat을 사용해 포맷을 지정해줬다.

## Controller
 <img width="1129" alt="image" src="https://user-images.githubusercontent.com/74396651/224638681-12218cc0-6041-46ee-b0f2-6c98be8e33ad.png">

## DTO

<img width="1058" alt="image" src="https://user-images.githubusercontent.com/74396651/224638486-52943c08-4809-4f99-aa7a-643d7efcba56.png">


## Service

<img width="1175" alt="image" src="https://user-images.githubusercontent.com/74396651/224638405-a4b05013-7974-4c9e-b311-f5c903c561a1.png">


## Repository

<img width="807" alt="image" src="https://user-images.githubusercontent.com/74396651/224638329-6ec42f76-23d5-4f00-8bba-5920a2134a6d.png">
