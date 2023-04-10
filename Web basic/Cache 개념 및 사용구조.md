> 레디스를 들어가기 전에 간단하게 리마인드 개념정리
[참고1 짱](https://codeahoy.com/2017/08/11/caching-strategies-and-how-to-choose-the-right-one/)
[참고2](https://velog.io/@tyjk8997/%EC%BA%90%EC%8B%9C%EC%99%80-%EA%B6%81%EA%B8%88%ED%95%9C%EC%A0%90)

# Cache
Cache란 자주 사용하는 데이터나 값을 미리 복사해 놓는 임시 장소를 가리킨다. 아래와 같은 저장공간 계층 구조에서 확인할 수 있듯이, 캐시는 저장 공간이 작고 비용이 비싼 대신 빠른 성능을 제공한다. 


![image](https://user-images.githubusercontent.com/74396651/230900689-54d8e096-1576-4227-9ead-0921e5e49fa8.png)

<br>

## Cache를 사용하는 경우
- 접근 시간에 비히 원래 데이터를 접근하는 시간이 오래 걸리는 경우(서버의 균일한 API 데이터)
- 반복적으로 동일한 결과를 돌려주는 경우(이미지나 썸네일 등)

Cache에 데이터를 미리 복사해 놓으면 계산이나 접근 시간 없이 더 빠른 속도로 데이터에 접근할 수 있다. 결국 Cache란 반복적으로 데이터를 불러오는 경우에, 지속적으로 DBMS 혹은 서버에 요청하는 것이 아니라 Memory에 데이터를 저장하였다가 불러다 쓰는 것을 의미한다. Enterprise급 Application에서는 DBMS의 부하를 줄이고, 성능을 높이기 위해 캐시(Cache)를 사용한다. 원하는 데이터가 캐시에 존재할 경우 해당 데이터를 반환하며, 이러한 상황을 Cache Hit라고 한다. 하지만 원하는 데이터가 캐시에 존재하지 않을 경우 DBMS 또는 서버에 요청을 해야하며 이를 Cache Miss라고 한다. 캐시는 저장공간이 작기 때문에, 지속적으로 Cache Miss가 발생하는 데이터의 경우 캐시 전략에 따라서 저장중인 데이터를 변경해야 한다.

<br>

## 어떤 정보를 Cache에 담아야 하나
모든 데이터를 캐시에 담기에는 캐시라는 저장 공간은 작다. 그렇기 때문에 파레토의 법칙에 해당하는 소수의 데이터를 선별해야한다. 이때 사용되는 것이 지역성이다.

![image](https://user-images.githubusercontent.com/74396651/230902595-553580a4-05cc-447c-8368-1df83a01a345.png)

### 시간적 지역성
특정 데이터가 한번 접근되었을 경우, 가까운 미래에 또 한번 데이터에 접근할 가능성이 높은 것을 말한다. 메모리 상의 같은 주소에 여러 차례 쓰기를 수행할 경우 상대적으로 작은 크기의 캐시를 사용해도 효율성을 꾀할 수 있다.

### 공간적 지역성
특정 데이터와 가까운 주소가 순서대로 접근되었을 경우를 공간적 지역성이라고 한다.CPU 캐시나 디스크 캐시의 경우 한 메모리 주소에 접근할 때 그 주소뿐 아니라 해당 블록을 전부 캐시에 가져오게 된다. 이때 메모리 주소를 오름차순이나 내림차순으로 접근한다면, 캐시에 이미 저장된 같은 블록의 데이터를 접근하게 되므로 캐시의 효율성이 크게 향상된다.

### 순차 지역성
공간 지역성과 함꼐 설명되기도 한다. 데이터가 순차적으로 엑세스되는 경향을 보인다. 프로그램 내의 명령어가 순차적으로 구성된다.


<br>

## Cache의 필요성 : 파레토의 법칙
파레토 법칙이란 80퍼센트의 결과는 20퍼센트의 원인으로 인해 발생한다는 법칙이다.

![image](https://user-images.githubusercontent.com/74396651/230902042-39614be3-47f1-45ea-9d38-14ab73643bb1.png)

 <br>
 
 ## Cache 사용 구조
 
 ### 일반적인 구조
 ![image](https://user-images.githubusercontent.com/74396651/230903176-3c9e21e4-5b81-40d1-a51c-5f40aabdc21f.png)
1. Clien로부터 요청을 받는다
2. Cache에서 조회한다.
3. DB에서 읽어 온다.
4. Cache에 저장한다.
 
 ### 1. Look Aside Cache(Lazy Loading)
 
 ![image](https://user-images.githubusercontent.com/74396651/230904937-3d8f873c-82ab-4a3a-a653-616d1cd81a5c.png)

 
look aside cache는 캐시를 한번 접근하여 데이터가 있는지 판단한 후, 있다면 cache의 데이터를 사용한다. 만약 없다면 실제 DB 또는 API를 호출하는 로직으로 구현됩니다. 대부분의 cache를 사용한 개발이 해당 프로세스를 따르고 있다. 스프링 데이터 레디스 문서에서는 TTL(Time to Live)이라는 기능을 제공한다. redis 데이터의 만료기간을 설정하여, TTL기간이 지나면 캐시 데이터가 없어지게 된다. TTL을 설정함으로써 자연스럽게 캐시 값이 없어지고, 자가진단 관련 데이터 조회 시 DB 데이터를 캐시에 저장하게되면서 최신의 DB 데이터를 캐시에 담을 수 있게 된다.

1. Cache에 Data 존재 유무 확인
2. Data가 있다면 cache의 Data 사용
3. Data가 없다면 실제 DB Data 사용
4. DB에서 가져온 Data를 Cache에 저장


### Read Through 구조
![image](https://user-images.githubusercontent.com/74396651/230905271-619af358-5765-4f9e-980c-4c5375ffdea0.png)


캐시에 데이터가 저장되어있는지 보고 만일 저장되어 있다면 해당 데이터를 반환합니다. 캐시에 데이터가 없는 경우 DB 질의를 통해 데이터를 찾아 캐시에 저장한 뒤 클라이언트에 반환합니다. DB가 캐시에 데이터를 저장하기 때문에 애플리케이션단에서 관리할 수 없습니다.


### 2. Write Back
![image](https://user-images.githubusercontent.com/74396651/230905332-47d5df60-4795-4f6d-b5f1-d75e4d35094b.png)

write back은 cache를 다르게 이용하는 방법이다. DB는 접근 횟수가 적을수록 전체 시스템의 퍼포먼스가 향상된다. 데이터를 쓰거나 많은 데이터를 읽게되면 DB에서 Disk를 접근하게 된다. 이렇게 되면 Application의 속도 저하가 일어날 수 있다. 따라서 write back은 데이터를 cache에 모으고 일정한 주기 또는 일정한 크기가 되면 한번에 처리하는 것이다.
 
1.Data를 Cache에 저장
2. Cache에 있는 Data를 일정 기간동안 Check
3. 모여있는 Data를 DB에 저장
4. Cache에 있는 Data 삭제

### Write Through 구조
![image](https://user-images.githubusercontent.com/74396651/230905301-57ff6246-649b-4b01-b72d-2dc24511729f.png)

캐시와 DB에 항상 동시에 데아터를 기록하는 방식이다. 항상 최신의 데이터가 유지되지만, 쓰기 작업이 많을 경우 캐시와 DB에 매번 통신하는 비용이 발생한다.

