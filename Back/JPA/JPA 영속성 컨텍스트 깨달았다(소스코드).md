# 깨달았다...
# jpa는 Transaction안에서 놀고 1차 캐시 안에서 뛰어다닌다.
## @ManyToOne
- 기본이 eager(즉시로딩)
- fk다 fk.

## @OneToMany
- 기본이 fetch(지연로딩)
- fk랑 양방향 관계

# 우선 코드부터

# Entity

- ## Student
```java
package com.jpa.pratice.domain;

import lombok.Builder;
import lombok.Getter;
import lombok.NoArgsConstructor;

import javax.persistence.*;
import java.util.List;


@Entity
@Getter @NoArgsConstructor
public class Student {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long studentId; // spring data jpa가 카멜 케이스를 인식해 student_id로 만들어 준다.

    private String name;
    private int age;
    private int score;

//    @ManyToOne(fetch = FetchType.LAZY)
    @ManyToOne
    @JoinColumn(name="class_id")
    private MathClass mathClass; // class_id 컬럼으로 만들어 줌

    @Builder
    public Student(String name, int age, int score, MathClass mathClass) {
        this.name = name;
        this.age = age;
        this.score = score;
        this.mathClass = mathClass;
        mathClass.getStudentList().add(this); // 연관관계 편의
    }
}

```
<br>

- ## MathClass
```java
package com.jpa.pratice.domain;

import lombok.Builder;
import lombok.Getter;
import lombok.NoArgsConstructor;

import javax.persistence.*;
import java.util.ArrayList;
import java.util.List;

@Entity
@Getter @NoArgsConstructor
public class MathClass {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long classId; // spring data jpa가 카멜 케이스를 인식해 class_id로 만들어준다.
    private String className;
    private int studentNum;

//    @OneToMany(mappedBy = "mathClass", fetch = FetchType.EAGER) // 해결법 1
    @OneToMany(mappedBy = "mathClass")
    private List<Student> studentList = new ArrayList<>();

    @Builder
    public MathClass(String className, int studentNum) {
        this.className = className;
        this.studentNum = studentNum;
    }
}

```

<br>

# Service, Repository
- ## service는 repositoy를 단순 위임, 메서드는 단순하다.
- ### save() 
   - em.persist
- ### find()
   - em.find(id)  

<br>

# TestCode
```java
package com.jpa.pratice.service;

import com.jpa.pratice.domain.MathClass;
import com.jpa.pratice.domain.Student;
import org.hibernate.LazyInitializationException;
import org.junit.jupiter.api.Assertions;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.transaction.annotation.Transactional;


@SpringBootTest
//@Transactional // 해결방법 2
public class StudentServiceTest {

    @Autowired
    private StudentService studentService;

    @Autowired
    private MathClassService mathClassService;

//    @Autowired
//    private EntityManager em;

    @Test
//    @Transactional // 해결방법 2
    public void save() throws Exception{
        //given
        MathClass mathClass = MathClass.builder()
                .studentNum(10)
                .className("룰루")
                .build();
        MathClass mathClass2 = MathClass.builder()
                .studentNum(30)
                .className("하하하")
                .build();

        // persist(insert query)
        mathClassService.save(mathClass);
        mathClassService.save(mathClass2);

        // persist(insert query)
        studentService.save(Student.builder()
                .age(20)
                .score(70)
                .name("test1")
                .mathClass(mathClass)
                .build());
       studentService.save(Student.builder()
                .age(30)
                .score(80)
                .name("test2")
                .mathClass(mathClass)
                .build());
       studentService.save(Student.builder()
                .age(40)
                .score(90)
                .name("test3")
                .mathClass(mathClass2)
                .build());

        //when

        System.out.println("============= em.find =============");

        // find(select query)
        Student student1 = studentService.find(1L);
        Student student2 = studentService.find(2L);
        Student student3 = studentService.find(3L);

        System.out.println("============= getMathClass =============");


        //then

        // 단순 클래스를 가져오는 것은 1차 캐시를 건들지 않는다. 필드에 접근해야 1차 캐시를 건든다.
        MathClass mc1 = student1.getMathClass();
        MathClass mc2 = student2.getMathClass();
        MathClass mc3 = student3.getMathClass();

        MathClass mc4 = mathClassService.find(1L);
        MathClass mc5 = mathClassService.find(2L);

        System.out.println("============= mathClass.getStudentNum =============");

        System.out.println("숫자1 : " + mc1.getStudentNum());
        System.out.println("숫자2 : " + mc2.getStudentNum());
        System.out.println("숫자3 : " + mc3.getStudentNum());
        System.out.println("같은 클래스인지 확인-1 : " + mathClass);
        System.out.println("같은 클래스인지 확인-2 : " + mathClass2);
        System.out.println("같은 클래스인지 확인1 : " + mc1.equals(mathClass));
        System.out.println("같은 클래스인지 확인2 : " + mc2);
        System.out.println("같은 클래스인지 확인3 : " + mc3);
        System.out.println("같은 클래스인지 확인4 : " + mc4);
        System.out.println("같은 클래스인지 확인5 : " + mc5);

        // 1차 캐시에 MathClass가 없는데 조회하려고 하니 에러 발생 (org.hibernate.LazyInitializationException)
//        Assertions.assertThrows(LazyInitializationException.class, () -> mc.getStudentList());


        System.out.println("============= mathClass.getStudentList =============");
        for (Student st : mc1.getStudentList()) {
            System.out.println("이름1 : " + st.getName());
            System.out.println("나이1 : " + st.getAge());
        }
        for (Student st : mc2.getStudentList()) {
            System.out.println("이름2 : " + st.getName());
            System.out.println("나이2 : " + st.getAge());
        }
        for (Student st : mc3.getStudentList()) {
            System.out.println("이름3 : " + st.getName());
            System.out.println("나이3 : " + st.getAge());
        }
    }
}


```

<br>

# 깨달은 점
- 1차 캐시에 있는 엔티티의 식별자와 조회하고자 하는 엔티티의 식별자가 같은 경우 같은 타입의 엔티티이다.(이것 또한 같은 Transaction 안이여야 한다.) 골때리네... 
- 1차 캐시(영속성 컨텍스트)에서 엔티티를 가져오고 싶다면 이미 1차 캐시 안에 존재하던가 Transactioin 안이여야 한다.
- 이 때문에 eager로 모두 한번에 가져오던지 lazy라면 transaction 내에서 조회든 뭐든 실행해야 한다.
- 엔티티를 조회할 경우 1차 캐시를 뒤지고 없으면 DB에 접근한다는 뜻인데, transaction 내가 아니라면 바로 에러가 발생한다.
- 이미 transaction이 끝나면 영속성 컨텍스트가 종료되어 1차 캐시 또한 없어진다.
- 트랜잭션이 끝난 후 entitiy에 접근하고자 한다면 <code>org.hibernate.LazyInitializationException: failed to lazily initialize a collection of role: com.jpa.pratice.domain.MathClass.studentList, could not initialize proxy - no Session</code>가 발생한다.
- Transaction 바깥에서 proxy 객체를 조회하려고 하면 마찬가지로 에러가 발생한다.
     - Transation이 끝나면 영속성 컨텍스트도 종료된다.    
- 요청이 끝나면 영속성 컨텍스트에서 1차 캐시가 사라진다. 이 의미는 같은 트랜잭션이 아니라면 계속 쿼리가 나간다는 뜻이고 LAZY로 설정한 proxy객체든 
- ##그래서 결론은 같은 Transaction 내에서 처리하자.....

<br>

# 1차 캐시과 트랜잭션으로 인한 에러

![image](https://user-images.githubusercontent.com/74396651/201977561-27e630b9-120b-472a-bfb6-580b92e55302.png)
- 영속성 컨텍스트에 없는데 가져오려고 할 경우에 발생한다. lazy라 필요할 때 가져와야 하는데 1차 캐시에도 없고 transaction도 없고 난리다 난리..
- 해결하려면 Eager로 한번에 가져오던가 혹은 Transaction 내에서 해당 코드를 가져오면 된다.


<br>

## 같은 트랜잭션 안
![image](https://user-images.githubusercontent.com/74396651/201990380-30e08df2-708f-461d-9f20-554b220c105f.png)

## 각각 다른 트랜잭션
![image](https://user-images.githubusercontent.com/74396651/201990571-eb6efddc-74cb-4105-9ca7-f575a888dd1a.png)

- 골때리네...
