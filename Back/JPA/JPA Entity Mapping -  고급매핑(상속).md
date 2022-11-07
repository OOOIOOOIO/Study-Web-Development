# 상속관계 매핑
- 관계형DB는 상속 관계가 없다.
- 슈퍼타입, 서브타입 관계라는 모델링 기법이 객체의 상속과 유사하다.
   - 즉, 상속관계 매핑은 객체의 상속 구조와 DB의 슈퍼타입-서브타입 관계를 매핑한 것이다.  

<br>

## 슈퍼타입-서브타입 논리 모델을 실제 물리 모델로 구현하는 방법
- 조인 전략 : 각각 테이블로 변환
- 단일 테이블 전략 : 통합 테이블로 변환
- 구현 클래스마다 테이블 전략 : 서브타입 테이블로 변환

<br>

## 주요 어노테이션
- @Inheritance(strategy-InheritanceType.XXX)
   - JOINED : 조인전략
   - SINGLE_TABle : 단일 테이블 전략
   - TABLE_PER_CLASS : 구현 클래스마다 테이블 전략
- @DiscriminatorColumn(name-"DTYPE)
- @DiscriminatorValue("XXX")

![image](https://user-images.githubusercontent.com/74396651/200245699-d0d30541-c601-4ab8-afd8-a6df85c8365f.png)

![image](https://user-images.githubusercontent.com/74396651/200247664-87981829-edfc-46a6-bd51-18f3a75b9fe6.png)


<br>

## 조인 전략
- 장점
   - 테이블 정규화
   - 외래 키 참조 무결성 제약조건 활용가능
   - 저장공간 효율화
- 단점
   - 조회시 조인을 많이 사용한다. -> 성능 저하
   - 쿼리 복잡
   - 데이터 저장시 INSERT SQL 2번 호출

![image](https://user-images.githubusercontent.com/74396651/200246040-61d5f87c-4586-4abd-a2cb-c801b7a54343.png)


<br>

## 단일 테이블 전략
- 장점
   - 조인이 필요 없어 일반적으로 성능이 빠르다.
   - 쿼리 단순 
- 단점
   - 자식 엔티티가 매핑한 컬럼은 모두 null 허용
   - 단일 테이블에 모든 것을 저장하므로 테이블이 커질 수 잇다.
   - 상황에 따라 오히려 느려질 수 있다.
![image](https://user-images.githubusercontent.com/74396651/200246072-88e50e65-9569-4c6e-9d1c-1701fcfb9098.png)

<br>

## 구현 클래스마다 테이블 전략
- 장점
   - 서브 타입을 명확하게 구분해서 처리할 때 효과적
   - not null 제약조건 사용 가능 
- 단점
   - 여러 자식 테이블을 함께 조회할 때 성능이 느리다.(UNION SQL 필요)
   - 자식 테이블을 통합해서 쿼리짜기가 어렵다.
   - 
![image](https://user-images.githubusercontent.com/74396651/200246326-d062da77-1b90-4c3c-93e9-cd039d81eefc.png)

<br>
<hr>
<br>

# @MappedSuperclass
- 테이블과 관계 없고 단순히 엔티티들이 공통으로 사용하는 매핑 정보를 모으는 역할이다.
   - 공통 매핑 정보가 필요할 때 사용
   - 상속관계 매핑이 아니다
   - 엔티티가 아니다.(단순 인터페이스 어노테이션임)
   - 테이블과 매핑하는 것이 아닏.
   - 부모 클래스를 상속 받는 자식 클래스에 매핑 정보(컬럼)만 제공
- 조회, 검색 불가(em.find(BaseEntity) 불가)
- 직접 생성해서 사용할 일이 없으므로 추상 클래스를 권장
- @Entity 클래스는 같은 @Entity나 @MappedSuperclass로 지정한 클래스만 상속(extends) 가능하다.

![image](https://user-images.githubusercontent.com/74396651/200246592-42a44dd6-ee6b-4e75-b6b4-58e1745308e8.png)

![image](https://user-images.githubusercontent.com/74396651/200246668-28de9fad-8da1-4534-bde6-b1dddd7cc5e8.png)

![image](https://user-images.githubusercontent.com/74396651/200246776-a2953305-f2c4-447e-bfce-6f1255647f83.png)


