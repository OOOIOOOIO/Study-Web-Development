# Proxy
> &nbsp;JPA 구현체들은 연관된 객체들을 처음부터 데이터베이스에서 조회하는 것이 아니라, 실제 사용하는 시점에 데이터베이스에서 조회할 수 있다. 이것이 가능한 이유는 프록시 때문이다. 프록시를 통해서 즉시로딩(EAGER)과 지연로딩(LAZY)을 사용하여 보다 유연하게 개발할 수 있다.<br><br>
> &nbsp;JPA 표준 명세는 지연 로딩 구현방법을 JPA 구현체에 위임한다. 하이버네이트는 지연로딩(LAZY)을 지원하는 방법으로 프록시 방법과 바이트코드 수정 방법 이 두 가지 방법을 제공한다.(바이트코드를 수정하는 방법은 복잡하다.)

<br>

## JPA에서의 Proxy
- em.find -> DB를 통해 실제 엔티티 객체를 조회
- em.getReference() -> DB 조회를 미루는 가짜(프록시) 엔티티 객체 조회
- 실제 클래스를 상속 받아 만들어지며 겉 모양이 같다.
- 사용하는 입장에서 진짜 객체인지 프록시 객체인지 신경쓰지 않고 사용하면 된다.
![image](https://user-images.githubusercontent.com/74396651/203362563-8947fa64-de9e-4a00-8bc2-98f4c94cf14f.png)

- 프록시 객체는 실제 객체의 참조(target)를 보관하고 있다.
- 프록시 객체르 호출하면 프록시 객체는 실제 객체의 메소드를 호출한다.
![image](https://user-images.githubusercontent.com/74396651/203362774-76e478e9-9fd0-47a4-9db8-b782492ab813.png)

- 프록시 객체는 처음 사용할 때 한 번만 초기화 된다.
- 프록시 객체를 초기화 할 때, 프록시 객체가 실제 엔티티로 변하는 것이 아니다. 초기화가 되면 프록시 객체를 통해 실제 엔티티로 접근하는 것이다.
- 프록시 객체는 엔티티 객체의 식별자(PK) 값을 보관하고 있다. 프록시 객체를 통해 엔티티를 조회할 때 식별자(PK) 값을 파라미터로 전달하는데, 프록시 객체는 식별자 값을 가지고 있기 때문에 만약 식별자 값을 조회하는 .getId()를 호출하더라도 프록시는 초기화 되지 않는다.
    - 단, 엔티티 접근 방식을 프로퍼티(@Access(AccessType.PROPERTY))로 설정한 경우에만 초기화 하지 않고
    - 엔티티 접근 방식을 필드(@Access(AccessType.FIED))로 설정하면 JPA는 프록시객체를 초기화 한다. 

![image](https://user-images.githubusercontent.com/74396651/203364081-45a9843b-db63-47c2-a468-e77bbe830fc9.png)

- 프록시 객체는 원본 엔티티를 상속받기 때문에 타입 체크시 주의해야 한다.
   - == 대신 instance of를 사용해야 한다.
- 영속성 컨텍스트에 찾는 엔티티가 이미 있다면 em.getReference()를 호출해도 프록시가 아닌 실제 엔티티를 반환한다. 
- 영속성 컨텍스트 안이 아닌 준영속 상태일 때 프록시를 초기화하면 문제가 발생한다.(Transaction 내에서 사용하자 꼭! 그렇지 않으면  org.hibernate.LazyInitializationException 예외가 발생한다.)

<br>

## 프록시 확인하기
- ### 프록시 인스턴스의 초기화 여부 확인
    - PersistenceUnitUtil.isLoaded(Object entity)

- ### 프록시 클래스 확인하는 방법
    - entity.getClass.getName() 출력 -> javasist... or HibernateProxy가 나온다!
- ### 프록시 강제 초기화
    - org.hibernate.Hibernate.initialize(entity)
- ### 참고, JPA 표준은 강제 초기화가 없다.
    - 강제 호출 : member.getName()처럼 직접 값에 접근


<br>
<hr>
<br>

# 즉시로딩, 지연로딩
> &nbsp;쉽게 말해 엔티티를 조회할 때 객체(FK)를 어떻게 가져올까! 하는 방식이다.

<br>

## 지연 로딩
- ### fetch = FetchType.LAZY

![image](https://user-images.githubusercontent.com/74396651/203367711-78af3b20-4be0-4805-91a6-0bfcc3296a51.png)

![image](https://user-images.githubusercontent.com/74396651/203367766-1d16050f-d296-48c0-8029-e2fd74893a16.png)

![image](https://user-images.githubusercontent.com/74396651/203367829-f875b75a-9ae6-452b-a562-beba0f79f7c6.png)

<br>

## 즉시 로딩
- ### fetch = FetchType.EAGER
![image](https://user-images.githubusercontent.com/74396651/203367234-7e9f381c-1321-4874-903c-6913a15451fa.png)

![image](https://user-images.githubusercontent.com/74396651/203367467-56ec4662-c190-42fc-8a94-4b97e43cb387.png)

![image](https://user-images.githubusercontent.com/74396651/203367506-5ceb279f-1ebc-4675-81fd-3ce2bdf62fe5.png)

<br>

## 프록시와 즉시로딩 주의점
- 가급적 모든 연관관계에서 지연 로딩만 사용하라!(특히 실무에서)
- 즉시 로딩을 적용하면 예상하지 못한 SQL이 발생한다.
- 즉시 로딩은 JPQL에서 N+1문제를 일으킨다.
     - 하나를 조회했는데 추가로 N개의 select 쿼리가 나가는 것!  
- @ManyToOne, @OneToOne은 기본이 EAGER다. --> 꼭 LAZY로 설정하기
- @OneToMany, @ManyToMany는 기본이 LAZY다.
- JPQL fetch join이나 엔티티 그래프 기능으로 끌어오면 된다! 그러니 LAZY로 안전하게 개발하자!

<br>
<hr>
<br>

# CASCADE




<br>
<br>
<br>
<br>

참조 : 인프런 김영한님 강의
