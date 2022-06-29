# 스프링 컨테이너와 스프링 빈
- ApplicationContext를 스프링 컨테이너라고 하며 언터페이스이다.
- 스프링 컨테이너는 XML 혹은 어노테이션 기반의 Java 클래스를 사용해 만들 수 있다.
- 스프링 컨테이너는 자바 객체의 생명 주기를 관리하며, 생성된 자바 객체들에게 추가적인 기능을 제공하는 역할을 한다.
- 여기서 말하는 자바 객체를 스프링 빈이라 부르며 IoC와 DI의 원리가 스프링 컨테이너에 적용된다.
- 개발자가 new 연산자, 인터페이스 호출, 팩토리 호출 방식을 통해 객체를 생성하고 소멸시킬수 있지만 이 역할을 스프링 컨테이너가 대신 해준다(IoC).
- 또한 객체들 간의 의존 관계를 스프링 컨테이너가 알아서 주입해준다(DI).

<br>
<hr>
<br>

## 스프링 컨테이너 생성 및 스프링 빈 등록 : XML 방식

<br>

- 등록 방법
   - src/main/resources 에 Spring Bean Configuration File을 생성해 코드를 작성한다.

<br>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

	<bean id="memberService" class="com.core.member.MemberServiceImpl">
		<constructor-arg name="memberRepository" ref="memberRepository"></constructor-arg>
	</bean>
	
	<bean id="memberRepository" class="com.core.member.MemoryMemberRepository"></bean>
	
	<bean id="orderService" class="com.core.order.OrderServiceImpl">
		<constructor-arg name="memberRepository" ref="memberRepository"></constructor-arg>
		<constructor-arg name="discountPolicy" ref="discountPolicy"></constructor-arg>
	</bean>
	
	<bean id="discountPolicy" class="com.core.discount.RateDiscountPolicy"></bean>
</beans>

```

<br>
<hr>
<br>

## 스프링 컨테이너 생성 및 스프링 빈 등록 : Java 방식

```
package com.core.hello;
/*
 * 구현체의 객체를 생성하고 연결하는 책임을 가지는 별도의 설정 클래스(팩토리 메서드(factoryMethod) 방법)
 * 
 * 생성자를 통해 주입시켜 줄 때 지나가는 통로 느낌
 * 
 * application의 실제 동작에 필요한 구현 객체를 생성한다.
 * 
 * 사용용도를 변경하고 싶다면 클라이언트 코드는 변경하지 않고 AppConfig 클래스만 변경하면 된다! AppConfig를 사용함으로써 OCP와 dIP 둘 다 지킬 수 있다!
 * 
 */

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
	
	// 여기서 객체를 생성한 후 생성자에 주입시켜준다!
	// 어떤 객체를 설정할지 여기서 결정하면 된다!
	// 역할을 한눈에 볼 수 있게 개발한다!
	// @Bean을 붙이면서 Spring Bean으로 사용한다!없애면 순수 자바코드!
	// @Bean이 붙은 메서드 명이 스프링 빈의 이름으로 사용된다!

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

<br>
<hr>
<br>

## 스프링 컨테이너와 스프링 빈이 잘 등록 되었는지 테스트하기 : Junit5 사용

```java
package com.core.beanfind;

import org.junit.jupiter.api.Disabled;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.config.BeanDefinition;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

import com.core.hello.AppConfig;

public class ApplicationContextInfoTest {

	// Spring 컨테이너 등록!
	AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);
	
	@Test
	@Disabled
	@DisplayName("모든 빈 출력하기")
	public void findAllBean() {
		String[] beanDefinitionNames = ac.getBeanDefinitionNames();
		
		for(String beanDefinitionName : beanDefinitionNames) {
			Object bean = ac.getBean(beanDefinitionName);
			System.out.println("bean = " + beanDefinitionName + " / object = " +bean);
					
			
		}
	}
	
	@Test
//	@Disabled
	@DisplayName("애플리케이션 빈 출력하기")
	public void findApplicationBean() {
		String[] beanDefinitionNames = ac.getBeanDefinitionNames();
		
		for(String beanDefinitionName : beanDefinitionNames) {
			BeanDefinition beanDefinition = ac.getBeanDefinition(beanDefinitionName);
			System.out.println("======== : " + beanDefinition);
			System.out.println("-------- : " + beanDefinition.getRole());
			System.out.println("++++++++ : " + BeanDefinition.ROLE_APPLICATION);
			
			// BeanDefinition.ROLE_APPLICATION : 내가 등록한 빈들, 외부 라이브러리 등등
			if(beanDefinition.getRole() == BeanDefinition.ROLE_APPLICATION) {
				Object bean = ac.getBean(beanDefinitionName);
				System.out.println("bean = " + beanDefinitionName + " / object = " +bean);
				
			}
		}
	}
}

/*
console

======== : Generic bean: class [com.core.hello.AppConfig$$EnhancerBySpringCGLIB$$eea811e5]; scope=singleton; abstract=false; lazyInit=null; autowireMode=0; dependencyCheck=0; autowireCandidate=true; primary=false; factoryBeanName=null; factoryMethodName=null; initMethodName=null; destroyMethodName=null
-------- : 0
++++++++ : 0
bean = appConfig / object = com.core.hello.AppConfig$$EnhancerBySpringCGLIB$$eea811e5@5b202a3a
======== : Root bean: class [null]; scope=; abstract=false; lazyInit=null; autowireMode=3; dependencyCheck=0; autowireCandidate=true; primary=false; factoryBeanName=appConfig; factoryMethodName=memberService; initMethodName=null; destroyMethodName=(inferred); defined in com.core.hello.AppConfig
-------- : 0
++++++++ : 0
bean = memberService / object = com.core.member.MemberServiceImpl@10b9db7b
======== : Root bean: class [null]; scope=; abstract=false; lazyInit=null; autowireMode=3; dependencyCheck=0; autowireCandidate=true; primary=false; factoryBeanName=appConfig; factoryMethodName=memberRepository; initMethodName=null; destroyMethodName=(inferred); defined in com.core.hello.AppConfig
-------- : 0
++++++++ : 0
bean = memberRepository / object = com.core.member.MemoryMemberRepository@9ef8eb7
======== : Root bean: class [null]; scope=; abstract=false; lazyInit=null; autowireMode=3; dependencyCheck=0; autowireCandidate=true; primary=false; factoryBeanName=appConfig; factoryMethodName=orderService; initMethodName=null; destroyMethodName=(inferred); defined in com.core.hello.AppConfig
-------- : 0
++++++++ : 0
bean = orderService / object = com.core.order.OrderServiceImpl@34cdeda2
======== : Root bean: class [null]; scope=; abstract=false; lazyInit=null; autowireMode=3; dependencyCheck=0; autowireCandidate=true; primary=false; factoryBeanName=appConfig; factoryMethodName=discountPolicy; initMethodName=null; destroyMethodName=(inferred); defined in com.core.hello.AppConfig
-------- : 0
++++++++ : 0
bean = discountPolicy / object = com.core.discount.RateDiscountPolicy@6ee660fb
*/
```

<br>
<hr>
<br>

## 스프링 빈 조회시 같은 타입이 여러개인 경우 : Junit5 

- 타입으로 조회시 같은 타입의 스프링 빈이 둘 이상일 경우 오류가 발생한다. 그러므로 빈 이름을 꼭 넣어주자!

```java
package com.core.beanfind;

import static org.assertj.core.api.Assertions.assertThat;
import static org.junit.jupiter.api.Assertions.assertThrows;

import java.util.Map;

import org.junit.jupiter.api.Disabled;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.NoUniqueBeanDefinitionException;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import com.core.hello.AppConfig;
import com.core.member.MemberRepository;
import com.core.member.MemoryMemberRepository;

public class ApplicationContextSameBeanFindTest {

	// Spring 컨테이너 등록!
	AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(SameBeanConfig.class);
	
	@Test
	@Disabled
	@DisplayName("타입으로 조회시 같은 타입이 둘 이상이 있으면, 중복 오류가 발생한다.")
	public void findByTypeDuplicate() {
		MemberRepository bean = ac.getBean(MemberRepository.class);
		
		assertThrows(NoUniqueBeanDefinitionException.class, () -> ac.getBean(MemberRepository.class));
	}
	
	
	@Test
	@Disabled
	@DisplayName("타입으로 조회시 같은 타입이 둘 이상이 있으면, 빈 이름을 지정해주면 된다.")
	public void findBeanByName() {
		MemberRepository member1 = ac.getBean("memberRepository1", MemberRepository.class);
		
		assertThat(member1).isInstanceOf(MemberRepository.class);
	}
	
	@Test
//	@Disabled
	@DisplayName("특정 타입을 모두 조회하기")
	public void findAllBeanByType() {
		Map<String, MemberRepository> beansOfType = ac.getBeansOfType(MemberRepository.class);
		
		for(String key : beansOfType.keySet()) {
			System.out.println("key = " + key + "/ value = " + beansOfType.get(key));
		}
		
		assertThat(beansOfType.size()).isEqualTo(2);
	}
	
	// Spring Container 만들기
	@Configuration
	static class SameBeanConfig{ // Static으로 만들어줘서 메모리에 바로 올리고 분리하기
		
    // Spring Bean 등록
		@Bean
		public MemberRepository memberRepository1() {
			return new MemoryMemberRepository();
		}
		
		@Bean
		public MemberRepository memberRepository2() {
			return new MemoryMemberRepository();
		}
	}
	
}

/*
console

key = memberRepository1/ value = com.core.member.MemoryMemberRepository@56f0cc85
key = memberRepository2/ value = com.core.member.MemoryMemberRepository@62e20a76

*/

```

<br>
<hr>
<br>

## 스프링 빈 조회 - 상속 관계 : Junit5 사용

- 부모 타입으로 조회할 경우 자식 타입도 함께 조회된다.
- 따라서 모든 자바 객체의 최고 부모인 Object 타입으로 조회할 경우 모든 스프링 빈을 조회한다.


![상속 관계](https://user-images.githubusercontent.com/74396651/176361661-04660efa-6b6b-410a-9571-f0d02a7387f8.png)

<br>

```java
package com.core.beanfind;

import static org.assertj.core.api.Assertions.assertThat;
import static org.junit.jupiter.api.Assertions.assertThrows;

import java.util.Map;

import org.junit.jupiter.api.Disabled;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.NoUniqueBeanDefinitionException;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import com.core.discount.DiscountPolicy;
import com.core.discount.FixDiscountPolicy;
import com.core.discount.RateDiscountPolicy;

public class ApplicationContextExtendsFindTest {
	
	// Spring 컨테이너 등록!
	AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(TestConfig.class);
	
	
	@Test
	@Disabled
	@DisplayName("부모 타입으로 조회시, 자식이 둘 이상이 있으면 중복 오류가 발생한다.")
	public void findBeanByParentTypeDuplicate() {
//		DiscountPolicy bean = ac.getBean(DiscountPolicy.class);
		
		assertThrows(NoUniqueBeanDefinitionException.class, () -> ac.getBean(DiscountPolicy.class));
	}
	
	
	@Test
	@Disabled
	@DisplayName("부모 타입으로 조회시, 자식이 둘 이상 있으면 빈 이름을 지정하면 된다.")
	public void findBeanByParentTypeBeanName() {
		DiscountPolicy rateDiscountPolicy = ac.getBean("rateDiscountPolicy", DiscountPolicy.class);
		
		assertThat(rateDiscountPolicy).isInstanceOf(DiscountPolicy.class);
		
	}

	@Test
	@Disabled
	@DisplayName("특정 하위 타입으로 조회")
	public void findBeanBySubType() {
		DiscountPolicy bean = ac.getBean(RateDiscountPolicy.class);
		
		assertThat(bean).isInstanceOf(RateDiscountPolicy.class);
		
	}
	
	@Test
	@Disabled
	@DisplayName("부모 타입으로 조회하기")
	public void findBeanByParentType() {
		
		Map<String, DiscountPolicy> beansOfType = ac.getBeansOfType(DiscountPolicy.class);
		
		assertThat(beansOfType.size()).isEqualTo(2);
		
		for(String key : beansOfType.keySet()) {
			System.out.println("key : " + key + "/ value : " + beansOfType.get(key));
		}
		
		
	}
	
	@Test
//	@Disabled
	@DisplayName("부모 타입으로 모두조회하기")
	public void findBeanByObjectType() {
		
		Map<String, Object> beansOfType = ac.getBeansOfType(Object.class);
		
//		assertThat(beansOfType.size()).isEqualTo(넘나 많음 다 나옴);
		
		for(String key : beansOfType.keySet()) {
			System.out.println("key : " + key + "/ value : " + beansOfType.get(key));
		}
		
	}
	
	// Spring Container 만들기
	@Configuration
	static class TestConfig{
		
		@Bean
		public DiscountPolicy rateDiscountPolicy() {
			return new RateDiscountPolicy();
		}
		
		@Bean
		public DiscountPolicy fixDiscountPolicy() {
			return new FixDiscountPolicy();
		}
	}
	
}

```

<br>
<hr>
<br>

# BeanFactory & ApplicationContext

![BeanFactory & ApplicationContext](https://user-images.githubusercontent.com/74396651/176362898-103336a9-ca41-4f01-969f-d2cc8ff820aa.png)


## BeanFactory
- 스프링 컨테이너의 최상위 인터페이스이며 스프링 빈을 관리하고 조회하는 역할을 담당한다.
- .getBean()을 제공하며 대부분의 기능은 BeanFactory가 제공하는 기능이다.

<br>

## ApplicationContext
- BeanFactory의 기능을 모두 상속받아 제공한다.
- 빈을 관리하고 검색하는 기능 외에 애플리케이션을 개발할 때는 부가기능들이 필요하다.
- 부가기능
   - 메세지소스를 활용한 국제화 기능 : 예를 들어 한국에서 들어오면 한국어로, 영어권에서 들어오면 영어로 출력
   - 환경변수 : 로컬, 개발, 운영 등을 구분해서 처리한다.
   - 애플리케이션 이벤트 : 이벤트를 발행하고 구독하는 모델을 편리하게 지원
   - 파일, 클래스패스, 외부 등에서 리소스를 편리하게 조회한다.

<br>

## 정리
- ApplicationContext는 BeanFactory의 기능을 상속받는다.
- ApplicationContext는 빈 관리기능 + 편리한 부가 기능을 제공한다.
- BeanFactory를 직접 사용할 일은 거의 없다. 부가기능이 포함된 ApplicationContext를 사용한다.
- BeanFactory나 ApplicationContext를 스프링 컨테이너라 한다


# 스프링 빈 설정 메타 정보 - BeanDefinition
- 스프링은 어떻게 이런 다양한 설정 형식을 지원하는 것일까? 그 중심에는 BeanDefinition 이라는 추상화가 있다. 쉽게 이야기해서 역할과 구현을 개념적으로 나눈 것이다!
- XML을 혹은 Java 코드를 읽어서 BeanDefinition을 만들면 된다.
- 스프링 컨테이너는 자바 코드인지, XML인지 몰라도 된다. 오직 BeanDefinition만 알면 된다.
- BeanDefinition 을 빈 설정 메타정보라 한다.
- @Bean , <bean> 당 각각 하나씩 메타 정보가 생성된다.
- 스프링 컨테이너는 이 메타정보를 기반으로 스프링 빈을 생성한다

<br>
  
![BeanDefinition](https://user-images.githubusercontent.com/74396651/176363634-01f63587-8217-440c-86dd-57185290db0a.png)
  
- AnnotationConfigApplicationContext 는 AnnotatedBeanDefinitionReader 를 사용해서 AppConfig.class 를 읽고 BeanDefinition 을 생성한다.
- GenericXmlApplicationContext 는 XmlBeanDefinitionReader 를 사용해서 appConfig.xml 설정 정보를 읽고 BeanDefinition 을 생성한다.
- 새로운 형식의 설정 정보가 추가되면, XxxBeanDefinitionReader를 만들어서 BeanDefinition 을 생성하면 된다.  

## BeanDefinition 정보
  
- BeanClassName: 생성할 빈의 클래스 명(자바 설정 처럼 팩토리 역할의 빈을 사용하면 없음)
- factoryBeanName: 팩토리 역할의 빈을 사용할 경우 이름, 예) appConfig
- factoryMethodName: 빈을 생성할 팩토리 메서드 지정, 예) memberService
- Scope: 싱글톤(기본값)
- lazyInit: 스프링 컨테이너를 생성할 때 빈을 생성하는 것이 아니라, 실제 빈을 사용할 때 까지 최대한 생성을 지연처리 하는지 여부
- InitMethodName: 빈을 생성하고, 의존관계를 적용한 뒤에 호출되는 초기화 메서드 명
- DestroyMethodName: 빈의 생명주기가 끝나서 제거하기 직전에 호출되는 메서드 명
- Constructor arguments, Properties: 의존관계 주입에서 사용한다. (자바 설정 처럼 팩토리 역할의 빈을 사용하면 없음)  

  
참조 : 인프런 김영한님 강의






