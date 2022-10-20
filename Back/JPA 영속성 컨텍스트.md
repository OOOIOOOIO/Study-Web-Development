

# JPA의 CRUD

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


            // update, 값이 바뀌면 알아서 해줌
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

# JPA 영속성 컨텍스트
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

            // 영속, 1차 캐쉬, update, delete는 persist를 할 필요 없다.
            em.persist(member1);
            em.persist(member2);

            // db에 sql 반영이 된다(commit이 되면 완전 db에 저장됨), flush를 해도 1차 캐쉬는 유지된다.
            // db에 동기화를 한다고 생각하면 된다.
            em.flush();

            // 진짜 커밋, 자동으로 flush 호출
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
