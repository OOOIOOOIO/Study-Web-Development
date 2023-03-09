# 싱글톤 컨테이너에 

- 대부분의 스프링 애플리케이션은 웹 애플리케이션이다. 물론 웹이 아닌 애플리케이션도 얼마든지 개발할 수 있다. 웹 애플리케이션의 특징은 보통 여러 고객이 동시에 요청을 한다는 점이다.

<hr>

![스프링을 사용하지 않은 DI 컨테이너](https://user-images.githubusercontent.com/74396651/176580307-541f4a16-fd33-48f3-a5c8-2d346735b52d.png)

<br>

## 스프링을 사용하지 않은 DI 컨테이너 테스트: Junit5
```java

@Test
@DisplayName("스프링 없는 순수한 DI 컨테이너")
public void pureContainer() {

  AppConfig appConfig = new AppConfig();

  // 1. 조회 : 호출할 때마다 객체를 생성
  MemberService memberService1 = appConfig.memberService();

  // 2. 조회 : 호출할 때마다 객체를 생성
  MemberService memberService2 = appConfig.memberService();

  // 참조값이 다른 것을 확인
  System.out.println("memberService : " + memberService1);
  System.out.println("memberService : " + memberService2);

  // memberService1 != memberService2 확인
  assertThat(memberService1).isNotSameAs(memberService2);

}

/*
console

memberService : com.core.member.MemberServiceImpl@145f66e3
memberService : com.core.member.MemberServiceImpl@3023df74

*/
  
```
- 이렇게 순수 Java DI 컨테이너인 Appconfig는 요청을 할 때 마다 객체를 새로 생성한다.
- 만약 고객 트래픽이 초당 100이 나오면 초당 100개 객체가 생성되고 소멸된다. -> 메모리 낭비가 심하다!
- 이를 해결하기 위해 해당 객체가 딱 1개만 생성되고 공유되도록 설계하면 된다. -> 싱글톤 패턴!

<br>
<hr>
<br>

## 싱글톤 패턴
- 싱글톤 패턴이란 클래스의 인스턴스(객체)가 딱 1개만 생성되는 것을 보장하는 디자인 패턴이다.
- 그래서 외부를 통해 또 다른 객체 인스턴스들이 생성되지 못하도록 막아야 한다.
   - private 생성자를 사용해 외부에서 임의로 new 키워드를 사용하지 못하도록 막는다!

<br>

## 싱글톤 패턴을 적용한 클래스
```java
package com.core.singleton;

public class SingleTonService {
	

	// static 영역에 객체를 딱 1개만 생성한다.
	private static final SingleTonService instance = new SingleTonService();
	
	
	// 외부에서 객체가 필요하면 getInstance()를 통해 조회하도록 한다.
	public static SingleTonService getInstance() {
		return instance;
	}
	
	// 생성자를 private으로 만듦으로써 외부에서 new를 못하게 막아준다 -> 객체 하나만 쓰게 만들 수 있다! -> 싱글톤 패턴!
	private SingleTonService() {;}
	
	public void logic() {
		System.out.println("싱글톤 객체 로직 호출");
	}
	
}

```

<br>

## 싱글톤 패턴을 사용한 테스트 : Junit5
```java

@Test
@DisplayName("싱글톤 패턴을 적용한 객체 사용")
public void singletonServiceTest() {
  // private 생성자로 외부에서 new를 통해 새로운 객체를 생성하지 못하게 막아두었다. -> 싱글톤 패턴!!!!!
  SingleTonService singletonService1 = SingleTonService.getInstance();
  SingleTonService singletonService2 = SingleTonService.getInstance();

  System.out.println("싱글톤 : " + singletonService1);
  System.out.println("싱글톤 : " + singletonService2);

  // isSameAs : == 와 같다(주소값 비교)
  // isEqualTo : equals(값 비교)
  assertThat(singletonService1).isSameAs(singletonService2);
  /*
  console

  싱글톤 : com.core.singleton.SingleTonService@7c1e2a9e
  싱글톤 : com.core.singleton.SingleTonService@7c1e2a9e
  */

}

```

<br>

## 싱글톤 패턴의 문제점
- 싱글톤 패턴을 적용하면 고객의 요청이 올 때 마다 객체를 생성하는 것이 아니라, 이미 만들어진 객체를 공유하여 효율적으로 사용할 수 있다. 하지만 싱글톤 패턴은 다음과 같은 문제점을 가지고 있다.
   - 싱글톤 패턴을 구현하는 코드 자체가 많이 들어간다.
   - 의존관계상 클라이언트가 구체 클래스에 의존한다. -> DIP를 위반
   - 클라이언트가 구체(구현) 클래스에 의존하여 OCP 원칙을 위반할 가능성이 높다.
   - 테스트하기가 어렵다.
   - 내부 속성을 변경하거나 초기화 하기 어렵다.
   - private 생성자로 자식 클래스를 만들기 어렵다.
   - 결론적으로 유연성이 떨어진다.
   - 안티패턴으로 불리기도 한다.

<br>
<hr>
<br>


# 싱글톤 컨테이너
- 스프링(싱글톤) 컨테이너는 싱글톤 패턴의 문제점을 해결하면서 객체 인스턴스를 싱글톤으로 관리한다.
- 스프링 컨테이너는 싱글톤 패턴을 적용하지 않아도 객체 인스턴스를 싱글톤으로 관리한다.
- 스프링 컨테이너는 싱글톤 컨테이너 역할을하며 싱글톤 객체를 생성하고 관리하는 기능을 싱글톤 레지스트리라 한다.
- 스프링 컨테이너의 이런 기능 덕분에 싱글톤 패턴의 모든 단점을 해결하면서 객체를 싱글톤으로 유지할 수 있다.

![image](https://user-images.githubusercontent.com/74396651/176591129-81457efb-4bf9-4eda-8e47-511ed8e440a8.png)


<br>

## 싱글톤 컨테이너를 사용한 테스트 : Junit5
```java
@Test
@DisplayName("스프링 컨테이너와 싱글톤")
public void springContainer() {
//		AppConfig appConfig = new AppConfig();

  // Spring Container를 사용하면 한번 생성된 객체를 계속 사용한다!!(자동으로 싱글톤!) / 설정에 따라 매번 새로운 객체를 사용할 수도 있다!
  AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);


  // 1. 조회 : 호출할 때마다 객체를 생성
  MemberService memberService1 = ac.getBean("memberService", MemberService.class);

  // 2. 조회 : 호출할 때마다 객체를 생성
  MemberService memberService2 = ac.getBean("memberService", MemberService.class);

  // 참조값이 같은 것을 확인
  System.out.println("memberService : " + memberService1);
  System.out.println("memberService : " + memberService2);

  // memberService1 == memberService2 확인
  assertThat(memberService1).isSameAs(memberService2);	

  /*
  console

  memberService : com.core.member.MemberServiceImpl@4e858e0a
  memberService : com.core.member.MemberServiceImpl@4e858e0a
  */

}
```

<br>

## 싱글톤 방식의 주의점
- 싱글톤 패턴이든, 스프링 같은 싱글톤 컨테이너를 사용하든 객체 인스턴스를 하나만 생성하여 공유하는 싱글톤 방식은 여러 클라이언트가 하나의 같은 객체를 공유하기 때문에 싱글톤 객체는 상태를
유지(stateful)하게 설계하면 안된다.
- 무상태(stateless)로 설계해야 한다.
   - 특정 클라이언트에 의존적인 필드가 있으면 안된다.
   - 특정 클라이언트가 값을 변경할 수 있는 필드가 있으면 안된다.
   - 가급적 읽기만 가능해야 한다.
   - 필드 대신에 자바에서 공유되지 않는 지역변수, 파라미터, ThreadLocal 등을 사용해야 한다.

- 스프링 빈의 필드에 공유값을 설정하면 정말 큰 장애가 발생할 수 있다.

## 문제점 예시
```java
package com.core.singleton;

public class StatefulService {
	
	private int price; // 상태를 유지하는 필드
	
	// 잘못된 코드
	// price는 지금 공유되는 필드! 같은 객체를 사용하기 때문에 order가 호출될 때마다 price가 변경되어 멀티쓰레드를 사용할 경우 문제가 생긴다!
	// 따라서 스프링 빈(싱글톤 패턴)은 항상 무상태(stateless)로 설계하고 공유필드를 조심하자!
	public void order(String name, int price) {
		System.out.println("name : " + name + "/ price : " + price);
		this.price = price; // 여기가 문제!
	}
	
	public int getPrice() {
		return price;
	}
	
	
	// 옳바른 코드, 지역변수, 파라미터, ThreadLocal을 사용해야한다!!!
	public int order2(String name, int price) {
		System.out.println("name : " + name + "/ price : " + price);
		return price;
	}
	
}
```

<br>

## 코드 테스트 : Junit5
```java
package com.core.singleton;

import org.assertj.core.api.Assertions;
import org.junit.jupiter.api.Test;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

public class StatefulServiceTest {
	
	@Test
	public void statefulServiceSingleton() {
		
		AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(TestConfig.class);
		
		StatefulService statefulService1 = ac.getBean("statefulService", StatefulService.class);
		StatefulService statefulService2 = ac.getBean("statefulService", StatefulService.class);
		
		
		// ThreadA : A 사용자 10000원 주문
		statefulService1.order("userA", 10000);
		
		// ThreadB : B 사용자 20000원 주문
		statefulService2.order("userB", 20000);
		
		// ThreadA 사용자A 주문 금액 조회
		int price = statefulService1.getPrice();
		System.out.println("userA price : " + price);
		
		
		// 옳바른 코드, 
		int userAAA = statefulService1.order2("userAAA", 1000);
		int userBBB = statefulService1.order2("userBBB", 3000);
		System.out.println("userAAA : " + userAAA);
		
		// 잘못된 코드 검사! 10000 != 20000
		Assertions.assertThat(statefulService1.getPrice()).isEqualTo(20000);
		
		Assertions.assertThat(userAAA).isEqualTo(1000);
		
	}
	
	@Configuration
	static class TestConfig{
		
		@Bean
		public StatefulService statefulService() {
			return new StatefulService();
		}
	}
  
  
  /*
  console
  // 잘못된 코드 결과 : price 필드가 공유되는데 클라이언트의 요청에 따라 값이 변경되기 때문에 
  // 공유필드는 조심해서 사용해야 한다! 스프링 빈은 항상 무상태(stateless)로 설계하자!
  name : userA / price : 10000
  name : userB / price : 20000
  userA price : 20000
  
  // 옳은 코드 결과
  name : userAAA / price : 1000
  name : userBBB / price : 3000
  userAAA : 1000
  
  */
}

```

<br>
<hr>
<br>

# Configuration과 싱글톤 그리고 바이트코드 조작 라이브러리(CGLIB)

##
```java
package com.core.singleton;

import org.assertj.core.api.Assertions;
import org.junit.jupiter.api.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

import com.core.hello.AppConfig;
import com.core.member.MemberRepository;
import com.core.member.MemberService;
import com.core.member.MemberServiceImpl;
import com.core.member.MemoryMemberRepository;
import com.core.order.OrderService;
import com.core.order.OrderServiceImpl;

public class ConfigurationSingletonTest {

	/*
	 * 
	 * 서로 다른 @Bean에서 생성한 객체가 같은지 확인하는 테스트 
	 */
	
	@Test
	public void configurationTest() {
		ApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);
		
		MemberServiceImpl memberService = ac.getBean("memberService", MemberServiceImpl.class);
		OrderServiceImpl orderService = ac.getBean("orderService", OrderServiceImpl.class);
		MemberRepository memberRepository = ac.getBean("memberRepository", MemberRepository.class);
		
		MemberRepository memberRepository1 = memberService.getMemberRepository();
		MemberRepository memberRepository2 = orderService.getMemberRepository();
		
		System.out.println("memberService.getMemberRepository() : " + memberRepository1);
		System.out.println("orderService.getMemberRepository() : " + memberRepository2);
		System.out.println("MemberRepository : " + memberRepository);
		
		Assertions.assertThat(memberRepository1).isSameAs(memberRepository);
		Assertions.assertThat(memberRepository2).isSameAs(memberRepository);
		
		
	}
	
	@Test
	public void configurationDeep() {
		
		ApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);
		
		AppConfig bean = ac.getBean(AppConfig.class);
		
		System.out.println("bean : " + bean.getClass()); // 클래스면ㅇ에 ~~CGLIB가 붙으면 스프링이 CGLIB라는 바이트코드 조작 라이브러리를 사용해 새로운 클래스를 만들어 객체로 등록한다.
		
	}
  
  
  /*
  console
  
  memberService.getMemberRepository() : com.core.member.MemoryMemberRepository@3b00856b
  orderService.getMemberRepository() : com.core.member.MemoryMemberRepository@3b00856b
  MemberRepository : com.core.member.MemoryMemberRepository@3b00856b
  
  */
}

```

<hr>

```java
package com.core.hello;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import com.core.discount.DiscountPolicy;
import com.core.discount.FixDiscountPolicy;
import com.core.discount.RateDiscountPolicy;
import com.core.member.MemberRepository;
import com.core.member.MemberService;
import com.core.member.MemberServiceImpl;
import com.core.member.MemoryMemberRepository;
import com.core.order.OrderService;
import com.core.order.OrderServiceImpl;


// Spring 사용!
@Configuration
public class AppConfig {
	

	// @Bean memberService() 호출 -> new MemberServiceImpl() 객체 생성 -> memberRepository() 호출 -> new MemoryMemberRepository 객체 생성
	// @Bean orderService() 호출 -> new OrderServiceIMpl() 객체 생성 -> memberRepository() 호출 -> new MemoryMemberRepository 객체 생성
	// --> 둘다 MemoryMemberRepository 객체를 생성하니 싱글톤이 깨지는 것일까?! --> 아니다! 한번 생성한 객체는 다시 호출되도 똑같은 객체이다!
	
	// 호출 흐름
	
	// 예상 테스트 결과
	// call memberService
	// call memberRepository
	// call ordererService
	// call memberRepository
	// call memberRepository
	
	// 실제 테스트 
	// call memberService
	// call memberRepository
	// call ordererService
	// -> ~~CGLIB가 내부에서 이미 객체가 있다면 그것을 찾아서 반환해주고 없으면 새로 등록하는 동적으로 코드를 만들어준다.(@Counfiguration가 CGLIB를  해주는 것이다!)
	
	@Bean
	public MemberService memberService() {
		System.out.println("call memberService");
		return new MemberServiceImpl(memberRepository());
		
	}

	@Bean
	public MemberRepository memberRepository() {
		System.out.println("call memberRepository");
		return new MemoryMemberRepository();
	}
	
	@Bean
	public OrderService orderService() {
		System.out.println("call orderService");
		return new OrderServiceImpl(memberRepository(), discountPolicy());
	}
	
	@Bean
	public DiscountPolicy discountPolicy() {
//		return new FixDiscountPolicy();
		return new RateDiscountPolicy();
	}
	
}

```

- 스프링 컨테이너는 싱글톤 레지스트리다. 따라서 스프링 빈이 싱글톤이 되도록 보장해주어야 한다. 그런데 스프링이 자바 코드까지 어떻게 하기는 어렵다. 
저 자바 코드를 보면 분명 3번 호출되어야 하는 것이 맞다. 그래서 스프링은 클래스의 바이트코드를 조작하는 라이브러리를 사용한다.
- 스프링이 CGLIB라는 바이트코드 조작 라이브러리를 사용해서 AppConfig 클래스를 상속받은 임의의 다른 클래스를 만들고, 그 다른 클래스를 스프링 빈으로 등록한 것이다.

<br>

## CGLIB 예상 코드
```java
@Bean
public MemberRepository memberRepository() {
 
 if (memoryMemberRepository가 이미 스프링 컨테이너에 등록되어 있으면?) {
  return 스프링 컨테이너에서 찾아서 반환;
 }
 else { //스프링 컨테이너에 없으면
  기존 로직을 호출해서 MemoryMemberRepository를 생성하고 스프링 컨테이너에 등록
  return 반환
 
 }
 
}

```
- @Bean이 붙은 메서드마다 이미 스프링 빈이 존재하면 존재하는 빈을 반환하고, 스프링 빈이 없으면 생성해서 스프링 빈으로 등록하고 반환하는 코드가 동적으로 만들어진다. 덕분에 싱글톤이 보장되는 것이다.
- @Bean만 사용해도 스프링 빈으로 등록되지만, 싱글톤을 보장하지 않는다. memberRepository() 처럼 의존관계 주입이 필요해서 메서드를 직접 호출할 때 싱글톤을 보장하지 않는다.
- 스프링 설정 정보는 항상 @Configuration 을 사용하자


참조 : 인프런 김영한님 강의
