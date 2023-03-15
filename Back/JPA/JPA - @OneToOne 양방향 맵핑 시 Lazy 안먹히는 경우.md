> 후...왜 셀렉 쿼리가 나갈까 했더니만... 까먹었었다.. [참고](https://1-7171771.tistory.com/143)
## Lazy Loading
- JPA의 유일한 단점은 사용하기 쉬운만큼 성능적인 측면에서 발생할 수 있는 이슈를 간과하기 쉽다는 것인데, 성능이 안나올때 가장 먼저 고려해봐야할 부분이 즉시로딩(EAGER LOADING)으로 설정된 Fetch 전략이 있는지 확인하는 것이다.
- 하지만 @OneToOne 매핑시 Fetch 전략을 Lazy로 설정해도 EAGER로 동작하는 경우가 있다. 어떤 경우에 이러한 문제점이 발생하는지, 해결책은 무엇인지 예시를 통해 알아보도록 하자

## 단방향 맵핑
![image](https://user-images.githubusercontent.com/74396651/225357029-c06399c2-2116-42f8-92d8-58deea73be18.png)
![image](https://user-images.githubusercontent.com/74396651/225357075-8ffcd8eb-ddb9-4834-8f4f-cf9c83d02384.png)


<br>

## 양방향 맵핑
![image](https://user-images.githubusercontent.com/74396651/225357188-d117b161-e808-46b3-9d11-ee47839c2475.png)
![image](https://user-images.githubusercontent.com/74396651/225357242-b494de9d-9be2-4edf-bdef-bb1c30efa436.png)
![image](https://user-images.githubusercontent.com/74396651/225357298-5020a452-2436-445b-9e3c-6635ab1d7ff7.png)
![image](https://user-images.githubusercontent.com/74396651/225357338-f216aa83-b5b8-4db3-a603-9382fa44f6e5.png)
![image](https://user-images.githubusercontent.com/74396651/225357390-ac94e9c5-2b81-4d01-8f4c-25add68e52f3.png)


