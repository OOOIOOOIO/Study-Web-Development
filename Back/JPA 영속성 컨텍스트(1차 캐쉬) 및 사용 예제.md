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

## EntityManager
- 엔터티 매니저는 엔터티를 저장하는 메모리상의 데이터베이스라고 생각하면 될 것같다. 
- 엔터티 매니저는 엔터티를 저장하고 수정하고 삭제하고 조회하는 등 엔터티와 관련된 모든일을 한다. 
- 하지만 위의 EntityManagerFactory는 달리 Thread Safe하지 않기때문에 동시성 문제가 발생하므로 스레드간에 절대 공유하면 안 된다. 
- 그래서 일반적으로 EntityManager를 EntityManagerFactory를 이용하여 생성한것을 사용하는것이 아닌 스프링에서 관리하는 EntityManager를 아래와 같이 선언하여 사용한다. 
- 이렇게되면 스프링에서 알아서 EntityManager를 Proxy로 깜싼 EntityManager를 생성 하여 주입해주기 때문에 Thread-Safety를 보장 한다.
- Hibernate에서는 Session이다.
```java
/*
@PersistenceContext
--> JPA 스펙에서 제공하는 영속성 컨텍스트를 주입하는 표준 어노테이션
어노테이션을 붙여주면 Factory를 만들고 EntityManager를 반환하는 과정을 생략할 수 있다.
@Autowired와 비슷하다고 생각하면 된다.
*/

@PersistenceContext
private EntityManager entityManager
```

<br>

## EntityManagerFactory
- EntityManager를 찍어내는 곳.
-  EntityManagerFactory는 Thread Safe해서 여러 스레드가 동시에 접근해도 안전하므로 서로 다른 쓰레드 간 공유하여 사용한다.
    - 동시성(Concurrency) : 사용자가 체감하기에 동시에 수행하느 것처럼 보이지만, 사실 사용자가 체감할 수 없는 짧은 시간단위로 작업들이 번갈아가며 수행되는 것이다.
    - 병렬(Parallelism) : 실제로 동시에 여러 작업이 수행되는 개념이다.
- Hibernate에서는 SessionFactory이다.
    
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


            // select
            Member findMember = em.find(Member.class, 1L); //
            System.out.println("findMember = " + findMember.getId());
            System.out.println("findMember = " + findMember.getName());


            // Query로 select, JPQL 객체지향 쿼리 언어, SQL 문법 대부분 지원
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
