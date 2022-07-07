# ComponentScan
- 지금까지 스프링 빈을 xml의 <Bean> 혹은 Java의 @Bean을 통해 설정 정보 파일 혹은 클래스에 등록을 하였다. 하지만 @Componet 어노테이션과 @ComponentScan 어노테이션을 통해 
쉽고 간단하게 스프링 빈을 등록할 수 있다.

<br>

## ComponentScan의 기본 대상

- @Component : 컴포넌트 스캔에서 사용
- @Controller : 스프링 MVC 컨트롤러에서 사용, 스프링 MVC 컨트롤러로 인식한다.
- @Service : 스프링 비즈니스 로직에서 사용, 사실 특별한 처리는 없지만 개발자들이 핵심 비즈니스 로직이겠구나~ 라고 인식하는데 도움이 된다.
- @Repository : 스프링 데이터 접근 계층에서 사용, 데이터 계층의 예외를 스프링 예외로 변환해준다.
- @Configuration : 스프링 설정 정보에서 사용

<br>
<hr>
<br>

## Component 클래스 : 구현체 클래스
```java

@Component
public class MemoryMemberRepository implements MemberRepository {//TODO}


	
@Component
public class RateDiscountPolicy implements DiscountPolicy {//TODO}



@Component
public class MemberServiceImpl implements MemberService {//TODO}



@Component
public class OrderServiceImpl implements OrderService {//TODO}

```
- @Coponent 어노테이션을 통해 스프링 빈으로 등록한다.
- @Autowired 어네테이션을 통해 의존관계를 주입하는데, 이는 뒤에서 한번에 보자!

<br>

## 의존관계 주입
```java
@Component
public class MemberServiceImpl implements MemberService{

	// * Spring이 알아서 파라미터에 주입시켜준당 이거 까먹지말기 *
	// 생성자 주입 생성자를 통해 주입시켜 줄 때, final을 사용하여 값이 무조건 들어온다는 것을 암시할 수 있다!
	
	
//	@Autowired 3. 필드 주입
	private MemberRepository memberRepository;
	
	
	
	
	
	// 2. Setter 주입 : 선택, 변경 가능성이 있는 의존관계에서 사용한다. 굳이 잘 사용하지는 않는다.
	@Autowired
	public void setMemberRepository(MemberRepository memberRepository) {
		System.out.println("Setter 주입 : " + memberRepository);
		this.memberRepository = memberRepository;
	}
	
	
	// 1. 생성자 주입 : 불편, 필수 의존관계에서 사용한다. 만약 생성자가 1개만 있다면 Autowired를 사용하지 않아도 알아서 등록해준다!
	@Autowired// 자동으로 의존관계 주입!, getBean(MemberRepository.class)와 같다고 보면 된다.
	public MemberServiceImpl(MemberRepository memberRepository) {
		System.out.println("생성자 주입 : " + memberRepository);
		this.memberRepository = memberRepository;
	}
	
	/*
	console
	
	생성자 주입 : com.core.member.MemoryMemberRepository@44e3a2b2
	Setter 주입 : com.core.member.MemoryMemberRepository@44e3a2b2

	 */
	

}	
	
```	
- 생성자 주입
   - 생성자를 통해 의존 관계를 주입받는 방법이다.
   - 생성자 호출시점(스프링 빈을 등록할 때)에 딱 1번만 호출되는 것을 보장받는다.
   - 만약 생성자가 딱 1개만 있다면 @Autowired를 생략해도 된다.
   - 불변, 필수 의존관계에 사용하며 요즘은 대부분 생성자 주입을 사용한다.<br>
	- 


- 수정자 주입(setter)
   - setter라 불리는 필드의 값을 변경하는 수정자 메서드를 통해 의존관계를 주입하는 방법이다.
   - 선택, 변경 가능성이 있는 의존관계에 사용된다.
   - 자바빈 프로퍼티 규약(setter, getter) 방식을 사용하는 방법이다.<br>

- 필드 주입
   - 필드(변수)에 바로 주입하는 방법이다.
   - 코드가 간결하지만 외부에서 변경이 불가능해 테스트가 힘든 단점이 있다.
   - DI 프레임워크(Spring, ..)가 없으면 아무것도 할 수 없다.
   - 되도록 사용하지 말자! 만약 사용하고 싶다면 테스트 코드 혹은 @Configuration 같은 스프링 설정 클래스에서만 특별한 용도로 사용하자!<br>

- 일반 메서드 주입
   - 음 그냥 메서드 명만 바꿔서 하는 거라 setter랑 별 다를게 없다.
	
	
### 참고
- @Autowired 의 기본 동작은 주입할 대상이 없으면 오류가 발생한다. 주입할 대상이 없어도 동작하게 하려면 @Autowired(required = false) 로 지정하면 된다.
- 생성자 주입을 사용해야 하는 이유 : 불변!!!
   - 대부분의 의존관계 주입은 한번 일어나면 애플리케이션 종료시점까지 의존관계를 변경할 일이 없다. 
   - 오히려 대부분의 의존관계는 애플리케이션 종료 전까지 변하면 안된다.(불변해야 한다.)
   - 수정자 주입을 사용하면, setXxx 메서드를 public으로 열어두어야 한다.
   - 누군가 실수로 변경할 수 도 있고, 변경하면 안되는 메서드를 열어두는 것은 좋은 설계 방법이 아니다.
   - 생성자 주입은 객체를 생성할 때 딱 1번만 호출되므로 이후에 호출되는 일이 없다. 따라서 불변하게 설계할 수 있다

<br>
<hr>
<br>
	
## ComponentScan 클래스
	
```java
package com.core;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.FilterType;

import com.core.member.MemberRepository;
import com.core.member.MemoryMemberRepository;


@Configuration
@ComponentScan(
        //@Configuration 내부에 Component가 있음. 우리는 Configuration 에서 Bean 으로 등록해줬기 때문에 충돌할 수 있으므로 filter로 뺐다.
        excludeFilters = @ComponentScan.Filter(type = FilterType.ANNOTATION, classes = Configuration.class),
        basePackages = "com.core" 
                      // com.core 아래에 있는 애들을 다 보겠다, xml 설정에서 해주듯이 여기서도 해줘야된다.{"com.core.member", "com.core.order", ..} 여러개도 가능
        						 // default로 현재 클래스의 패키지이다. 현재 default는 com.core
        						// 따라서 권장하는 방법은 패키지 위치를 지정하지 않고 설정 정보 클래스의 위치를 프로젝트 최상단에 두는 것!
		)
public class AutoAppConfig {;}

```

- Configuration을 통해 스프링 컨테이너를 생성하고
- ComponentScan을 통해 @Component 어노테이션이 붙은 클래스를 스프링 빈으로 등록한다.
- excludeFilters = @ComponentScan.Filter(type = FilterType.ANNOTATION, classes = Configuration.class)를 통해 스캔을 할 때 제외할 요소를 설정할 수 있다.
- includeFilters = @ComponentScan.Filter(type = FilterType.ANNOTATION, classes = MyIncludeComponent.class)를 통해 스캔을 할 때 포함할 요소를 설정할 수 있다.
- basePackages = "com.core"를 통해 스캔을 어디서부터 시작할 건지 설정할 수 있다. 만약 설정하지 않았다면 @ComponentScan이 등록된 클래스가 있는 패키지가 시작 패키지이다.
   - 가능하면 설저 정보 클래스의 위치를 프로젝트의 최상단에 둘 것을 추천한다. 최근 스프링 부트도 이를 기본으로 제공한다.


<br>
<hr>
<br>

## ComponentScan 테스트 : Junit5
```java
package com.core.scan;

import static org.assertj.core.api.Assertions.assertThat;
import static org.junit.jupiter.api.Assertions.assertThrows;

import org.junit.jupiter.api.Test;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

import com.core.AutoAppConfig;
import com.core.member.MemberService;


public class AutoAppConfigTest {

	@Test
	public void basicScan() {
		
		// Spring Container 생성
		AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(AutoAppConfig.class);
		
		// Spring Bean 꺼내오기
		MemberService memberService = ac.getBean(MemberService.class);
		
		assertThat(memberService).isInstanceOf(MemberService.class);
		
	}
}

```

<br>
<hr>
<br>
	
## 조회한 빈이 모두 필요한 경우 Map, List 사용
```java
public class AllBeanTest {

	
	@Test
	public void findAllBean() {
		ApplicationContext ac = new AnnotationConfigApplicationContext(AutoAppConfig.class, DiscountService.class);
		DiscountService discountService = ac.getBean(DiscountService.class);
		
		Member member = new Member(1L, "userA", Grade.VIP);
		
		int discountPrice = discountService.discount(member, 10000, "fixDiscountPolicy");
		
		assertThat(discountService).isInstanceOf(DiscountService.class);
		assertThat(discountPrice).isEqualTo(1000);
		
		int rateDiscountPrice = discountService.discount(member, 20000, "rateDiscountPolicy");
		assertThat(rateDiscountPrice).isEqualTo(2000);
	}
	
	
	static class DiscountService{
		
		private final Map<String, DiscountPolicy> policyMap;
		private final java.util.List<DiscountPolicy> policies;
		
		// 알아서 Map과 List에 주입시켜준다. 생성자가 1개만 있기 때문에 @Autowired 생략 가능!
		public DiscountService(Map<String, DiscountPolicy> policyMap, List<DiscountPolicy> policies) {
			this.policyMap = policyMap;
			this.policies = policies;
			System.out.println("policyMap : " + policyMap);
			System.out.println("policies : " + policies);
			
		}
		
		
		 public int discount(Member member, int price, String discountCode) {
			 DiscountPolicy discountPolicy = policyMap.get(discountCode);
			 
			 System.out.println("discountCode = " + discountCode);
			 System.out.println("discountPolicy = " + discountPolicy);
			 
			 return discountPolicy.discount(member, price);
		 }
		
	}
}	
```

<br>
<hr>
<br>

## 옵션처리(null 처리)

```java
package com.core.autowired;

import java.util.Optional;

import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.lang.Nullable;

import com.core.AppConfig;
import com.core.member.Member;

/*
 * 
 *
	옵션 처리
	
 주입할 스프링 빈이 없어도 동작해야 할 때가 있다.
그런데 @Autowired 만 사용하면 required 옵션의 기본값이 true 로 되어 있어서 자동 주입 대상이 없으면 오류가 발생한다.
자동 주입 대상을 옵션으로 처리하는 방법은 다음과 같다.
	
@Autowired(required=false) : 자동 주입할 대상이 없으면 수정자 메서드 자체가 호출 안됨
org.springframework.lang.@Nullable : 자동 주입할 대상이 없으면 null이 입력된다.
Optional<> : 자동 주입할 대상이 없으면 Optional.empty 가 입력된다
	
 */

public class AutowiredTest {
	
	@Test
	public void AutowiredOption() {
		ApplicationContext ac = new AnnotationConfigApplicationContext(TestBean.class); // 컴포넌트스캔하는 것 처럼 @Configuration이 없어도 TestBean을 스프링 빈으로 인식한다.
		

	}
	
	
	static class TestBean{
		// 1 : @Autowired(required = false) / true가 기본값
		@Autowired(required = false)
		public void setNoBean1(Member noBean1) {
			System.out.println("noBean1 : " + noBean1);
		}
		
		// 2 : @Nullable
		@Autowired
		public void setNoBean2(@Nullable Member noBean2) {
			System.out.println("noBean2 : " + noBean2);
		}
		
		// 3 : Optional<Member>
		@Autowired
		public void setNoBean3(Optional<Member> noBean3) {
			System.out.println("noBean3 : " + noBean3);
		}
		
		
	}
	
}

```
- 주입할 스프링 빈이 없어도 동작해야 할 때가 있다.
- 그런데 @Autowired 만 사용하면 required 옵션의 기본값이 true 로 되어 있어서 자동 주입 대상이 없으면 오류가 발생한다.
- 자동 주입 대상을 옵션으로 처리하는 방법은 다음과 같다.
   - @Autowired(required=false) : 자동 주입할 대상이 없으면 수정자 메서드 자체가 호출 안됨
   - org.springframework.lang.@Nullable : 자동 주입할 대상이 없으면 null이 입력된다.
   - Optional<> : 자동 주입할 대상이 없으면 Optional.empty 가 입력된다
	
<br>
<hr>
<br>

## 참고
- 만약 자동 빈 등록(@Component)와 수동 빈 등록이 충돌이 일어날 경우 수동 빈이 자동 빈을 오버라이딩한다.(수동 빈이 등록된다!)
- 만약 같은 이름의 빈이 여러개 등록되어 충돌이 일어날 경우 onflictingBeanDefinitionException 예외가 발생한다!!! 조심하장
- @Autowired는 타입 매칭이다. 타입 매칭의 결과가 2개 이상일 때 필드 명, 파라미터 명으로 빈 이름을 매칭한다.
- @Autowired를 통해 주입할 경우 타입이 같은 여러개의 스프링 빈이 매칭될 때 이렇게 해결할 수 있다.
   - @Autowired 필드 명 매칭 -> 필드 명을 사용할 구현체 명으로 설정하는 법
   - @Qualifier 사용 -> 컴포넌트(빈)마다 추가 구분자를 붙여주는 방법이다. 
	```java
	// @Qualifier 사용 법
	@Component
	@Qualifier("mainDiscountPolicy")
	public class RateDiscountPolicy implements DiscountPolicy {}
	
	
	@Component
	@Qualifier("fixDiscountPolicy")
	public class FixDiscountPolicy implements DiscountPolicy {}
	
	// 생성자 주입(수정자 주입도 똑같이 사용하면 된다.
	@Autowired
	public OrderServiceImpl(MemberRepository memberRepository, @Qualifier("mainDiscountPolicy") DiscountPolicy discountPolicy) {
 		this.memberRepository = memberRepository;
 		this.discountPolicy = discountPolicy;
	}
	
	```
	
   - @Primary 사용 -> 우선순위를 준다. 같은 타입의 여러 빈이 있더라도 우선권이 있는 빈이 주입된다.
	```java
	
	@Component
	@Primary
	public class RateDiscountPolicy implements DiscountPolicy {}
	
	
	@Component
	public class FixDiscountPolicy implements DiscountPolicy {}
	
	// 생성자 주입(수정자 주입도 똑같이 사용하면 된다.
	@Autowired
	public OrderServiceImpl(MemberRepository memberRepository,DiscountPolicy discountPolicy) {
 		this.memberRepository = memberRepository;
 		this.discountPolicy = discountPolicy;
	}
	
	```
	- @Qualifier가 @Primary보다 우선권이 높으며 예를 들어 메인 데이터베이스의 커넥션을 획득하는 스프링 빈은 @Primary를 통해 편리하게 조회하고 서브 데이터베이스에선
	@Qualifier를 통해 명시적으로 획득하는 방식으로 사용하여 코드를 깔끔하게 유지할 수 있다.

	
