> 문득... 동기화를 공부하면서 어... 스프링도 멀티 쓰레드 환경일텐데... 뭐지? 싶어서 구글링을 막 해버렸다..

<br>

# 멀티쓰레드 환경과 스프링 빈
> &nbsp;“SpringFramework” 기반으로 만든 “어플리케이션”은 기동시 “ApplicationContext“라는 “Static Sharing Pool“를 생성 합니다.
좀더 쉽게 설명을 하면 하나의 “싱글톤” 패턴 방식의 구현된 오브젝트가 생성 됩니다. (정확히는 “싱글톤 패턴”은 아닙니다. 이해를 돕기 위해서)<br>
> &nbsp;이렇게 생성된 “ApplicationContext” 영역에 “POJO(Plain Old Java Object) 클래스”들의 오브젝트들이 등록이 됩니다.(“POJO” 클래스는 그냥 “new 하면 스스로 생성”이 가능한 클래스의 형태를 말합니다.)
> &nbsp;이렇게 등록된 오브젝트를 “Spring Bean“이라고 합니다. 비록 “POJO” 클래스에 “static“으로 선언을 하지 않더라도, “ApplicationContext”가 이미 “Sharing“이 되어 있기때문에 “당연히 등록된 Bean”로“멀티 Thread 환경“에서 서로 공유를 하게 됩니다.
(물론 prototype일 경우는 매번 생성을 합니다.)

<br>
<hr>
<br>

# 구조
![image](https://user-images.githubusercontent.com/74396651/220053382-b36fa5ea-7143-478b-8540-30273ad52b1f.png)
- 위에 그림을 보면 “JVM”에서 하나의 공유 “ApplicationContext”가 생성이 되며 “Spring Bean #1”, “Spring Bean #2”, “Spring Bean #3” 등 3개의 “Bean”이 등록이 되어 있고, “1번부터 10번 Thread 모두 동일한 Bean 오브젝트“를 사용을 합니다.
- 문제는 개발자들이 Spring Bean의 멤버변수 또한 멀티쓰레드 환경에서 공유가 된다는 것을 간과한다는 것입니다. 이러한 실수는 “Thread Safe” 한 프레임웍(Servlet, Netty Handler, Camel Router, Spring Controller)들에 대한 오해 때문입니다.
- 문서를 보면 당연히 “Thread Safe“하다고 명시된 부분만을 확인하지, 실제 어디까지 “Thread Safe“하지 않는지 확인을 잘 안한다는 것입니다.
- 대부분 public 메서드”안에서 선언된 로컬 변수가 “Thread Safe”한거지 “멤버변수” 까지 “Thread Safe” 하다라는 것입니다. 즉 정확한 확인이 필요 합니다.


<br>
<hr>
<br>

# 예제
```java
@RestController
public class UserController {

    private final UserRepository userRepository;
    private Long n = 0L;

    public UserController(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    @GetMapping(value = "/hello")
    @Transactional
    public Long getUser() throws NotFoundException, InterruptedException {
        return userRepository.save(new User(++n,"user"+n)).orElseThrow(() -> new NotFoundException("존재하지 않습니다."));
    }
}
```
- 위 예시는 단순히 repository에 저장하는 코드다. 
- "/hello" 요청이 들어올 때마다 user가 저장되고 id값이 하나씩 증가한다.

![image](https://user-images.githubusercontent.com/74396651/220053973-9411c2fe-022e-465b-934a-b88259ed1da0.png)

<hr>

- 위 사진에서는 문제없이 잘 저장이 된 것을 확인할 수 있다.
- 하지만 다수의 요청이 동시에 들어왔을 때를 확인해보면 아래 사진처럼 id값이 중복되는 것을 확인할 수 있다.
- 즉 Thread-Safe 하지 않다는 것이다.

![image](https://user-images.githubusercontent.com/74396651/220054273-5f70ccba-9cdf-4af5-9147-a36eac1ec14b.png)

<br>
<hr>
<br>

## 그동안 어떻게 문제없이 사용했을까
> &nbsp;그 이유는 객체가 상태를 가지지 않는 불변객체였기 때문이다. 보통 @Component, @Controller, @Repository와 같은 곳에서 사용하는 객체들은 "주입"을 받아서 사용하고 내부적으로 멤버 변수를 가지고 변화시키지 않는다.<br>
> &nbsp;스프링빈을 Thread-Safe하게 사용하기 위해서는 스프링 빈으로 주입되는 클래스는 모두 불변 객체여야 한다.<br>
> &nbsp;아니면 스프링빈의 스코프를 Prototype으로 만들면 해당 빈이 불릴 때마다 새로운 객체가 생성되게 된다.
> 왠만하면 “스프링”에서 “멤버변수”는 “Injection“에 사용하는 “bean“일 경우만 사용하도록 권고 한다.


<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

[참고1](https://doflamingo.tistory.com/44)
[참고2](https://beyondj2ee.wordpress.com/2013/02/28/%EB%A9%80%ED%8B%B0-%EC%93%B0%EB%A0%88%EB%93%9C-%ED%99%98%EA%B2%BD%EC%97%90%EC%84%9C-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B9%88-%EC%A3%BC%EC%9D%98%EC%82%AC%ED%95%AD/)

