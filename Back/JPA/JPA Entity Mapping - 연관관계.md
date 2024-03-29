# Entity Mapping - 연관관계
- 객체와 데이터베이서의 테이블이 어떻게 맵핑되는지를 이해해야 한다.

<hr>

# JPA 목적
- "객체 지향 프로그래밍과 데이터베이스 사이의 패러다임 불일치를 해결" 이라는 것

<hr>

# 연관관계 정의 규칙
- 방향 
    - 단방향
    - 양방향(객체 참조)
- 연관관계의 주인
    -  양방향 관계일 경우, 연관 관계에서 누가 관리하는지
- 다중성
    - 다대일(N:1) : @ManyToOne
    - 일대다(1:N) : @OneToMany
    - 일대일(1:1) : @OneToOne
    - 다대다(N:M) : ManyToMany

<br>
<hr>
<br>

## 방향 
- 테이블
    - DB에서는 외래 키 하나로 양 쪽 모두 테이블 조인이 가능(딱히 방향이 있지는 않다.)
- 객체
    - 객체는 참조용 필드가 있는 곳에서만 다른 객체를 참조할 수 있다.(방향이 있다.)
    - 단방향 : 두 객체 사이에 하나의 객체만 참조용 필드를 갖고 있는 경우
    - 양방향 : 두 객체 모두 서로 참조용 필드를 갖고 있는 경우
       - 엄밀하게는 두 객체가 단방향 참조를 가지고 있어 양방향 관계처럼 사용하는 것이다.

<br>
<hr>
<br>

## 연관 관계의 주인
- 테이블은 외래 키 하나로 두 테이블이 연관관계를 맺는다.
- 객체 양방향 관계는 A -> B, B -> A 처럼 참조가 2군데이다.
- 두 객체가 양방향 관계일 경우 연관관계의 주인(외래 키를 관리할 곳)을 지정해야 한다.
- 연관 관계의 주인을 지정하는 것은 두 단방향 관계 중 제어의 권한(외래 키를 비롯한 테이블 레코드를 저장, 수정, 삭제, 처리)을 갖는 실질적인 관계가 어떤 것인지 JPA에게 알려준다고 생각하면 편하다.
- 연관관계의 주인은 연관관계를 갖는 두 객체 사이에서 조회, 저장, 수정, 삭제를 할 수 있지만, 연관관계의 주인이 아닌 경우 조회만 가능하다.
- 비즈니스 로직을 기준으로 연관관계의 주인을 선택하면 안된다.
- 연관관계의 주인은 외래 키의 위치를 기준으로 정해야한다
   - TIP 
     - DB를 만들 때 외래 키가 있는 곳을 무조건 연관관계의 주인으로 정하면 된다.
     - 주인 : ex) @ManyToOne @JoinColumn(name = "외래 키 컬럼명")
     - 그외 : ex) @OneToMany(mappedBy = "객체명"), mappedBy 속성으로 주인 지정

### 양방향 연결
![image](https://user-images.githubusercontent.com/74396651/199026717-ed9d7a99-2883-49a3-92da-8bad5e194d7c.png)

<br>
<hr>
<br>

## 다중성

### 다대일[N : 1] : @ManyToOne
- 다대일 단방향
    - 가장 많이 사용하는 연관관계
    - 다대일의 반대는 일대다이다.

![image](https://user-images.githubusercontent.com/74396651/199695450-7ab3ef1c-2fd2-448e-92b4-4a9b590e51fc.png)

<br>

- 다대일 양방향
    - 외래 키가 있는 쪽이 연관관계의 주인(테이블 짜는거 생각하면 편함)
    - 양쪽을 서로 참조하도록 개발

![image](https://user-images.githubusercontent.com/74396651/199695611-ffad5513-a7f0-4f2b-bf47-eab55485e243.png)

<hr>

### 일대다[1 : N] : @OneToMany
- 일대다 단반향
    - 일대다 단방향은 일대다(1 : N)에서 일(1)이 연관관계의 주인
    - 테이블 일대다 관계는 항상 다(N) 쪽에 외래키가 있다.
    - 객체와 테이블 차이 때문에 반대편 테이블의 외래키를 관리하는 특이한 구조
    - @ManyToOne처럼 @JoinColum을 꼭 사용해주어야 한다. 그렇지 않으면 조인 테이블 방식(중간에 테이블을 하나 추가)을 사용한다.
    - ### 단점
       - 엔티티가 관리하는 외래 키가 다른 테이블에 있다.
       - 연관관계 관리를 위해 추가로 UPDATE SQL 쿼리가 나간다.
    - 고로 일대다 단방향 매핑보다는 다대일 양방향 매핑을 사용하자.(권장하지 않는다.)

![image](https://user-images.githubusercontent.com/74396651/199698071-08cdef16-dc6c-4ae1-a7d7-eefc86f32746.png)

<br>

- 일대다 양방향 
     - 이런 매핑은 공식적으로 존재하지 않는다.
     - @JoinColumn(insertable=false, updatable=false)
     - 읽기 전용 필드를 사용해 양방향처럼 사용한다.
     - 다대일 양방향을 사용하자.

<br>
<hr>
<br>

### 일대일[1 : 1] 관계(@OneToOne)
- 일대일 관계는 그 반대도 일대일이여야 한다.
- 주 테이블이나 대상 테이블 중에 외래 키를 아무 곳이나 선택할 수 있다.
     - 주 테이블에 fk
     - 대상 테이블에 fk
- 외래 키에 DB 유니크 제약조건을 추가해야 한다.

<br>
- 일대일 : 주 테이블에 외래키 단방향
     - 다대일(@ManyToOne) 단방향 매핑과 유사

![image](https://user-images.githubusercontent.com/74396651/199908959-aaddfc74-81e9-40e9-b42d-8e1e6a315b2e.png)

<br>

- 일대일 : 주 테이블에 외래키 양방향
     - 다대일 양방향 매핑처럼 외래키가 있는 곳이 연관관계의 주인
     - 반대편은 mappedBy 적용(읽기 전용)

![image](https://user-images.githubusercontent.com/74396651/199909089-26b52382-2ef7-4498-9e1e-71161f116801.png)

- 일대일 : 대상 테이블에 외래키 단방향
     - 단방향 관계는 JPA에서 지원 X
     - 양방향 관계는 지원함

![image](https://user-images.githubusercontent.com/74396651/199909605-4a543e57-855c-4875-9aa7-91c29eec93a9.png)

<br>

- 일대일 : 대상 테이블에 외래 키 단방향 정리
     - 사실 일대일 주 테이블에 외래키 양방향 매핑 방법과 같다.

![image](https://user-images.githubusercontent.com/74396651/199909747-d1f8ebea-252d-471d-895a-1387019a67df.png)

<br>
<hr>
<br>

### 다대다[N : M](@ManyToMany)
- 관계형 데이터베이스는 정규화된 테이블 2개로 다대다 관계를 표현할 수 없다.
     - 연결 테이블을 추가해 1 : N <-- ㅁ --> N : 1 관계로 풀어야 한다.
![image](https://user-images.githubusercontent.com/74396651/199910263-9de23eb9-fdbe-4317-bb2b-491ef2aee058.png)


- 객체는 컬렉션을 사용해 객체 2개로 다대다 관계가 가능하다
![image](https://user-images.githubusercontent.com/74396651/199910285-9a6ba9bb-8c95-4200-9d26-841ccfd4b362.png)


- @ManyToMany 사용
- @JoinTable로 연결 테이블 지정
- 다대다 매핑 : 단방향, 양방향 가능

#### 한계
- 편리해 보이지만 실부에서 사용하지 않는다.
- 연결 테이블이 단순히 연결만 하고 끝나지 않는다.(정보 저장, 조회 등등)
- 주문시간, 수량, 등 데이터가 들어올 수 있다.

<br>

#### 한계 극복
- 연결 테이블용 엔티티 추가(연결 테이블을 엔티티로 승격)
- @ManyToMany --> @OneToMany + @ManyToOne 으로 변경

![image](https://user-images.githubusercontent.com/74396651/199910552-e6dedf69-712b-4c39-9349-47343d1adc3f.png)













