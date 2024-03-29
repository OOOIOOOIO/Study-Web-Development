# JPA 영속성 컨텍스트(Persistence Context)
- JPA를 이해하는데 가장 중요한 용어이다. "엔티티를 영구 저장하는 환경"이라는 뜻이다.
- 어플리케이션과 DB 사이에서 객체를 보관하는 가상 저장소같은 역할을 한다.
- EntityManagerFactory는 하나만 존재해야 하며 애플리케이션 전체에서 공유한다. 
- EntitiyManagerFactory에서 EntityManager를 생성해 영속성 컨텍스트에 접근하며 와 EntityTransaction를 통해 트랜잭션을 얻을 수 있다.
- EntityManger는 쓰레드 간 절대 공유하면 안된다.
- 영속성 컨텍스트는 EntityManager를 생성할 때 만들어지며 EntiyManager를 통해 접근하고 관리한다.
- EntityManager에 Entity를 저장하거나 조회하면 EntityManager는 영속성 컨텍스트에 이를 보관하고 관리한다.
- Spring에서는 EntityManager를 주입하여 사용하면 같은 트렌잭션의 범위에 있는 EntityManager는 같은 영속성 컨텍스트에 접근한다. 
   - J2EE, 스프링 프레임워크 같은 컨테이너 환경은 Entity Manager와 영속성 컨텍스트의 관계가 N:1이 가능하다. 
   - J2SE 환경은 1:1이다.

![image](https://user-images.githubusercontent.com/74396651/197515361-a56aa133-dba0-4255-9da5-e6d35a9ae080.png)

<br>

## Entity
- DB Table 객체, 테이블의 스키마와 같게 만들어야 한다.

<br>

## EntityManagerFactory
- EntityManager를 찍어내는 곳. DB 당 딱 1하나만 생성하여 사용한다.
-  EntityManagerFactory는 Thread Safe해서 여러 스레드가 동시에 접근해도 안전하므로 서로 다른 쓰레드 간 공유하여 사용한다.
    - #### 동시성(Concurrency) : 사용자가 체감하기에 동시에 수행하느 것처럼 보이지만, 사실 사용자가 체감할 수 없는 짧은 시간단위로 작업들이 번갈아가며 수행되는 것이다.
    - #### 병렬(Parallelism) : 실제로 동시에 여러 작업이 수행되는 개념이다.
- WAS가 종료되는 시점에 EntityManagerFactory.close()를 통해닫아준다.
    - 그래야 내부적으로 Connection Pooling에 대한 자원이 반납된다. 
- Hibernate에서는 SessionFactory이다.

<br>

## EntityManager
- 엔터티 매니저는 엔터티를 저장하는 메모리상의 데이터베이스라고 생각하면 될 것같다. 
- Transactional 단위로 실행된다.
- 엔터티 매니저는 엔터티를 저장하고 수정하고 삭제하고 조회하는 등 엔터티와 관련된 모든일을 한다. 
- 하지만 위의 EntityManagerFactory는 달리 EntityManager는 Thread Safe하지 않기때문에 동시성 문제가 발생하므로 스레드간에 절대 공유하면 안 된다. 
- 그래서 일반적으로 EntityManager를 EntityManagerFactory를 이용하여 생성한것을 사용하는것이 아닌 스프링에서 관리하는 EntityManager를 아래와 같이 선언하여 사용한다. 
- 이렇게되면 스프링에서 알아서 EntityManager를 Proxy로 깜싼 EntityManager를 생성 하여 주입해주기 때문에 Thread-Safety를 보장 한다.
- Hibernate에서는 Session이다.(no session 에러가 뜬다)
```java
/*
@PersistenceContext
--> JPA 스펙에서 제공하는 영속성 컨텍스트를 주입하는 표준 어노테이션
어노테이션을 붙여주면 Factory를 만들고 EntityManager를 반환하는 과정을 생략할 수 있다.
@Autowired와 비슷하다고 생각하면 된다.
*/

@PersistenceContext
private EntityManager entityManager

/* 
Spring Data Jpa를 사용할 경우 생성자 주입을 통해 주입할 수 있다.

private EntityManager em;
*/
```

<br>

## Entity 생명주기
- #### 비영속(new/transient)
   - 영속성 컨텍스트와 전혀 관계가 없는 새로운 상태
- #### 영속(managed)
   - 영속성 컨텍스트에 관리되는 상태
- #### 준영속(detached)
   - 영속성 컨텍스트에 저장되어 있다 분리된 상태
- #### 삭제(removed)
   - 삭제된 상태

![image](https://user-images.githubusercontent.com/74396651/197519857-1b298af5-409a-4cdd-93e1-972dda386615.png)

<br>

## 영속성 컨텍스트의 이점
- 1차 캐시
   - persist를 통해 영속성 컨텍스트 1차 캐시에 저장할 수 있으며
   - 만약 1차 캐시에 entity가 존재할 경우 db에 접근하지 않고 1차 캐시에서 데이터를 가져온다.
   - 1차 캐시의 경우 요청이 종료되면 영속성 컨텍스트를 지우면서 삭제된다.
- 동일성(identity) 보장
   - 1차 캐시로 반복 가능한 읽기(REPEATABLE READ) 등급의 트랜잭션 격리 수준을 데이터베이스가 아닌 애플리케이션 차원에서 제공한다.
- ### 트랜잭션을 지원하는 쓰기 지연(transaction write-behind)

![image](https://user-images.githubusercontent.com/74396651/197521420-45691b91-9eb0-4e93-a244-08291cbccebc.png)

![image](https://user-images.githubusercontent.com/74396651/197521463-a45c4136-59e8-425b-af74-683a951a26e5.png)

<br>

- 변경 감지(Dirty Checking)
   - setter로 수정만 해도 commit할 때 알아서 update를 해준다.

![image](https://user-images.githubusercontent.com/74396651/197521531-363ddc8c-1ac4-4126-afc7-9771508ca898.png)

<br>

- 지연 로딩(Lazy Loading)

<br>

## flush가 발생하는 경우
- 변경 감지
- 수정된 엔티티 쓰기 지연 SQL 저장소에 등록
- 쓰기 지연 SQL 저장소의 쿼리를 데이터베이스에 전송(등록, 수정, 삭제 쿼리)

<br>

## 영속성 컨텍스트를 플러시하는 방법
- 직접 호출 : em.flush()
- 플러쉬 자동 호출 : 트랜잭션 commit
- 플러쉬 자동 호출 : JPQL 쿼리 

## 플러쉬 모드 옵션
<code>em.setFlushMode(FlushModeType.COMMIT)</code>
- FlushModeType.AUTO : 커밋이나 JPQL 쿼리를 실행할 때 플러쉬 실행(기본값)
- FlushModeType.COMMIT : 커밋할 때만 플러쉬
 
<br>

## 플러쉬
- 영속석 컨텍스트를 비우지 않는다.
- 영속성 컨텍스트의 변경내용을 데이터베이스에 동기화 한다.(db에 반영만)
- 트랜잭션이라는 작업 단위 안에서 커밋 직전에만 동기화 하면 된다.
   
<br>

# JPA의 CRUD 코드

```java
package hellojpa;

import javax.persistence.*;
import java.util.List;

/*
JPA는 트랜잭션이 중요하다. 모든 데이터의 변경은 트랜잭션 단위로 수행해야 한다.
 */
public class JpaMain {
    public static void main(String[] args) {
        // persistence.xml에 설정한 DB 불러오기. name = hello
        // 여기서 Entity를 읽고 db에 Table을 생성해준다.
        // 하나만 생성해서 애플리케이션 전체에서 공유한다.
        EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");
        
        // DB 연결, Java Connection 이랑 비슷하다고 생각하면 된다.  Collection 처럼 사용할 수 있따.
        // EntityManager는 쓰레드 간에 절대 공유하면 안된다. 사용하고 버려야 한다.
        EntityManager em = emf.createEntityManager();

        // Transaction 얻기
        EntityTransaction tx = em.getTransaction();

        // Transaction 시작
        tx.begin();

        // code
        Member member = null;
        try {
            // insert
            member = new Member();
            member.setId(1L);
            member.setName("helloA");
            
            
            em.persist(member);


            // JPQL : 객체지향 쿼리 언어, Query로 select, SQL 문법 대부분 지원
            // JPQL은 엔티티 객체를 대상으로
            // SQL은 데이터베이스 테이블을 대상으로 쿼리를 짠다.
            List<Member> query = em.createQuery("select m from Member as m", Member.class)
                    .setFirstResult(0) // offset
                    .setMaxResults(5) // limit
                    .getResultList();



            // delete
            em.remove(findMember);


            // update, 값이 바뀌면 알아서 commit할 때 처리된다.
            findMember.setName("bye1");


            // 커밋, Transaction 종료
            tx.commit();
        } catch (Exception e) {
            // 실패시 롤백
            tx.rollback();
            throw new RuntimeException(e);
        }finally {
            // DB 커넥션 닫기
            em.close();
        }

        emf.close();
    }

}

```

# JPA 영속성 컨텍스트 테스트 
```java
package hellojpa;

import javax.persistence.EntityManager;
import javax.persistence.EntityManagerFactory;
import javax.persistence.EntityTransaction;
import javax.persistence.Persistence;

public class JpaEntiyManager {
    public static void main(String[] args) {
        // persistence.xml에 설정한 DB 불러오기. name = hello
        // 여기서 Entity를 읽고 db에 Table을 생성해준다.
        // 하나만 생성해서 애플리케이션 전체에서 공유한다.
        EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");

        // DB 연결, Java Connection, Collection이랑 비슷하다고 생각하면 된다.
        // EntityManager는 쓰레드 간에 절대 공유하면 안된다. 사용하고 버려야 한다.
        EntityManager em = emf.createEntityManager();

        // Transaction 얻기
        EntityTransaction tx = em.getTransaction();

        // Transaction 시작
        tx.begin();

        try {
            Member member1 = new Member(10L, "A");
            Member member2 = new Member(100L, "B");

            // insert, 영속성 컨텍스트(1차 캐쉬)에 등록, update, delete는 persist를 할 필요 없다.
            // 아직 db에 동기화는 X
            em.persist(member1);
            em.persist(member2);

            // select, 1차 캐시에서 조회
            Member findMember1 = em.find(Member.class, 10L); 
            Member findMember2 = em.find(Member.class, 10L); 
            System.out.println(findMember1 == findMember2); // 동일성 보장

            // db에 sql 반영이 된다(commit이 되면 완전 db에 저장됨), flush를 해도 1차 캐쉬는 유지된다.
            // db에 동기화를 한다고 생각하면 된다.
            em.flush();

            // 진짜 커밋(DB에 반영), 자동으로 flush 호출
            tx.commit();
        }catch (Exception e){
            tx.rollback();
        }
        finally {
            // 커넥션 닫기
            em.close();
        }

    }
}

```
