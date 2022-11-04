# JPA 값 타입 분류
- 기본값 타입
- 임베디드 타입(복합 값 타입)
- 값 타입과 불변 객체
- 값 타입의 비교
- 값 타입 컬렉션

<hr>

# JPA에서의 데이터 타입 분류
- 엔티티 타입
   - @Entity로 정의하는 객체
   - 데이터가 변해도 식별자로 추적 가능
   - ex) 회원 엔티티의 키 or 나이를 변경해도 식별자로 인식 가능
- 값 타입
   - int, Integer, String 처럼 단순히 값으로 사용하는 자바 기본 타입이나 객체
   - 식별자가 없고 값만 있으므로 변경시 추적 불가
   - ex) 숫자 100 -> 200으로 변경하면 완전히 다른 값으로 대체된 것이다.

<br>
<hr>
<br>

# 기본값 타입
- ex) int age, String name, Long length, ...
- 생명주기를 엔티티에 의존한다.
   - 회원을 삭제하면 회원의 이름, 나이, ... 필드도 함께 삭제
 - 값 타입은 공유되면 안된다.
   - 회원 이름 변경시 다른 회원의 이름에 영향을 미치면 안된다.
 - Java의 int, double과 같은 기본 타입(primitive type)은 값을 복사한다.(절대 공유하지 않는다)
 - Java의 Integer, Long과 같은 래퍼 클래스나 String 같은 클래스는 공유 가능한 객체이지만 변경되지 않는다.(다른 객체에 영향을 받지 않는다)

```java
int a = 20;
int b = a;

a = 50;
//--> a = 50, b = 20

Integer c = 40;
Integer d = c;

c = 50;
// --> c = 50, d = 40;

```

<hr>
<br>
<hr>

# 임베디드 타입


# 값 타입 비교

![image](https://user-images.githubusercontent.com/74396651/199898752-1f220e02-c500-41bf-b3ba-aabdd86698d8.png)


# 값 타입 컬렉션
![image](https://user-images.githubusercontent.com/74396651/199896358-70836fd3-ec76-4bcd-aa43-983a7da19be6.png)

![image](https://user-images.githubusercontent.com/74396651/199901671-8aba57a8-8049-45ca-aed5-e897023bc4da.png)






참조
인프런 김영한님 강의
