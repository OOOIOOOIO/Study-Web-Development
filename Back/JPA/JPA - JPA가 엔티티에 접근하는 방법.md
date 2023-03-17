# JPA가 엔티티에 접근하는 방법(@Access)

- 필드 접근 : @Access(AccessType.FIELD)
  - private으로 선언되엇더라도 reflection으로 접근 가능
- 프로퍼티 접근 : @Access(AccessType.PROPERTY)
  - getter를 사용해 접근
- @Access 어노테이션이 붙어있지 않다면 @Id의 위치로 접근 방식이 결정된다.
  - 필드에 붙어있는 경우 : 필드 접근

![image](https://user-images.githubusercontent.com/74396651/225780332-00dc8a6f-33eb-4e0b-996c-d88966edab71.png)


<br>

## 필드접근 방식을 권장하는 이유

1. 코드의 가독성
읽기 쉬운 코드는 중요하다.
필드에 어노테이션을 매핑하는 것이 getter에 붙이는 것에 비해서 훨씬 보기 편하다.

2. 애플리케이션(비즈니스로직)에서 호출하지 말아야할 getter 혹은 setter를 구현하는 문제
특히 primary key를 자동생성하는 @GenerateValue 어노테이션을 붙힌 id에 setter를 설정했을 때 문제가 발생한다.
persistence provider에게 맡겨야 하는데, 프로그래머가 조작할 수 없도록 해야한다.
또한 getter를 구현하지 않음으로써. @ManyToMany 관계 또는 양방향 관계에서 LIst를 직접적으로 접근하지 못하지 못하도록 하고 메서드를 유연하게 구현할 수 있다.

3. getter와 setter의 유연한 구현
persistence provider가 getter와 setter를 호출해서 데이터에 접근하지 않기
때문에, 유효성 검사 혹은 추가적인 비즈니스로직을 구현할 수 있다.
```
@Entity
public class Book {
 
    @ManyToOne
    Publisher publisher;
  
    public void setPublisher(Publisher p) {
        this.publisher = p;
    }
  	// optinal 사용(JPA에서는 optional을 제공하지 않음)
    public Optional<Publisher> getPublisher() {
        return Optional<Publisher>.ofNullable(this.publisher);
    }
  
    
}
```

4. 유틸리티 메서드에 @Transient 어노테이션을 부착하지 않아도 된다.
필드 타입으로 접근하게 되면 영속성 상태를 엔티티의 필드에서 정의하기 때문에 JPA가 엔티티의 모든 메서드들을 무시한다.

5. 프록시를 사용할 때 생기는 버그를 피할 수 있다.
하이버네이트는 -ToOne 관계에 지연 로딩에서 프록시를 사용한다.
프로퍼티 접근 방식을 사용하게 되면 함정에 빠질 수 있다.
프로퍼티 접근 방식을 사용할 때, 하이버네이트는 프록시 객체를 getter를 호출 할때 초기화 한다.
하지만 많은 equals() 메서드와 hashCode()의 구현이 필드에 직접적으로 접근한다.
이 때, 프록시 객체의 필드들이 아직 초기화되지 않는 문제가 있다.

```
@Entity
public class Book {
 
    @NaturalId
    String isbn;
 
    ...
  
    @Override
    public int hashCode() {
    return Objects.hashCode(isbn);
    }
 
    @Override
    public boolean equals(Object obj) {
    if (this == obj)
        return true;
    if (obj == null)
        return false;
    if (getClass() != obj.getClass())
        return false;
    Book other = (Book) obj;
    return Objects.equals(isbn, other.isbn);
    }
}

```


<br>
<br>
<br>

[참고](https://velog.io/@cmsskkk/JPA-Access)
