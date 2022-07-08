# 빈 생명주기(LifeCycle)
- 스프링의 IoC 컨테이너는 Bean 객체들을 책임지고 의존성을 관리한다. 
- 객체들을 관리한다는 것은 객체의 생성부터 소멸까지 개발자가 아닌 컨테이너가 대신 해준다는 뜻이다.

<br>

# 빈 생명주기 콜백
- 콜백, 콜백함수란 특정 이벤트가 발생했을 때 해당 메소드가 호출되는 것이다. 즉 조건에 따라 실행될 수도 실행되지 않을 수도 있다는 뜻이다.
- 데이터베이스 커넥션 풀이나, 네트워크 소켓처럼 애플리케이션 시작 시점에 필요한 연결을 미리 해두고, 애플리케이션 종료 시점에 연결을 모두 종료하는 작업을 진행하려면 객체의 초기화와 종료 작업이 필요하다.
- 스프링 빈은 객체를 생성하고, 의존관계 주입이 다 끝난 다음에야 필요한 데이터를 사용할 수 있는 준비가 완료된다. 
- 따라서 초기화 작업은 의존관계 주입이 모두 완료되고 난 다음에 호출해야 한다. 
- 그런데 개발자가 의존관계 주입이 모두 완료된 시점을 어떻게 알 수 있을까?
- 스프링은 의존관계 주입이 완료되면 스프링 빈에게 콜백 메서드를 통해서 초기화 시점을 알려주는 다양한 기능을 제공한다. 
- 또한 스프링은 스프링 컨테이너가 종료되기 직전에 소멸 콜백을 준다. 따라서 안전하게 종료 작업을 진행할 수 있다.

<br>
<hr>
<br>


## 빈 생명주기 콜백 3가지
- 인터페이스 InitializingBean, DisposableBean 방식
   - 인터페이스를 implements 하여 afterPropertiesSet(), destroy() 메소드를 오버라이드한다.
   - 스프링 전용 인터페이스이기 때문에 스프링에 의존하며 외부 라이브러리에 적용할 수 없다는 단점이 있다.
- 빈 등록 초기화, 소멸 메소드 지정
   - 메서드 이름을 자유롭게 줄 수 있고 스프링 코드에 의존하지 않는다.
   - 메서드 이름에 맞춰 실행되기 때문에 외부 라이브러리에도 초기화, 종료 메서드를 적용할 수 있다.
   - @Bean의 destroyMethod는 기본값이 (inferred)(추론)으로 등록되어 있다. 이 추론 기능은 close, shutdown이라는 이름의 메소드를 자동으로 호출해준다.
   - 대부분의 라이브러리는 close, shutdown이라는 이름의 종료 메서드를 사용하기 때문에 직접 스프링 빈으로 등록할 때 종료 메서드를 따로 적어주지 않아도 잘 동작한다.
   - 추론 기능을 사용하기 싫다면 destroyMethod = "" 처럼 공백으로 초기화한다.
- 어노테이션 @PostConstruct, @PreDestroy
   - 최신 스프링에서 권장하는 방법이다.
   - 스프링 기술이 아닌 java 표준 기술이기 때문에 스프링이 아닌 다른 컨테이너에서도 동작한다.
   - 외부 라이브러리에 적용하지 못한다는 단점이 있다.

<br>

### 스프링 빈의 이벤트 라이프사이클
<code>스프링 컨테이너 생성 -> 스프링 빈 생성 ->  의존관계 주입 -> 초기화 콜백 사용 -> 소멸전 콜백 -> 스프링 종료 </code>
- 초기화 콜백(Post) : 빈이 생성되고 의존관계 주입이 완료된 후! 호출
- 소멸전 콜백(Pre) : 빈이 소멸되기 직전에! 호출

<br>
<hr>
<br>

## 인터페이스 InitializingBean, DisposableBean 방식 : Junit5
```java
package com.core.lifecycle;

import org.junit.jupiter.api.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.ConfigurableApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

public class BeanLifeCycleTest {
	
	
	@Test
	public void lifeCycleTest() {
		// 여기서 스프링 빈이 생성되면서 객체가 생성된다! 
		ConfigurableApplicationContext ac = new AnnotationConfigApplicationContext(LifeCycleConfig.class);
		System.out.println("==================빈 등록 전 후=================!!!");
		
		NetworkClient networkClient = ac.getBean(NetworkClient.class);
		
		System.out.println("==================빈 종료 전 후=================!!!");
		
		// ConfigurableApplicationContext 인터페이스에만 가능, ApplicationContext 자식임
		ac.close(); // 스프링 빈 라이프사이클 종료
	}
	
	
	
	
	@Configuration
	static class LifeCycleConfig {
		
		@Bean
		public NetworkClient networkClient() {
			NetworkClient networkClient = new NetworkClient();
			networkClient.setUrl("http://hello.dev");
			return networkClient;
		}
		
	}
	
	
}
/*
생성자 호출, url : null
afterPropertiesSet!!!!!!!!!!!!!!!!!!!
connect : http://hello.dev
call  : http://hello.dev / message : 초기화 연결 메세지
==================빈 등록 전 후=================!!!
==================빈 종료 전 후=================!!!
13:41:07.105 [main] DEBUG org.springframework.context.annotation.AnnotationConfigApplicationContext - Closing org.springframework.context.annotation.AnnotationConfigApplicationContext@212b5695, started on Fri Jul 08 13:41:06 KST 2022
destroy!!!!!!!!!!!!!!!!!!!!
close : http://hello.dev
*/
```

```java
package com.core.lifecycle;

import org.springframework.beans.factory.DisposableBean;
import org.springframework.beans.factory.InitializingBean;


public class NetworkClient implements InitializingBean, DisposableBean{
	private String url;

	public NetworkClient() {
		System.out.println("생성자 호출, url : " + url);
		
	}


	public void setUrl(String url) {
		this.url = url;
	}
	
	
	// 서비스 시작시 호출
	public void connect() {
		System.out.println("connect : " + url);
		
		
	}
	
	
	public void call(String message) {
		System.out.println("call  : " + url + " / message : " + message);
	}
	
	
	//서비스 종료시 호출
	public void disconnect() {
		System.out.println("close : " + url);
	}


	// 의존관계 주입이 끝나면 호출하겠다.
	@Override
	public void afterPropertiesSet() throws Exception {
		System.out.println("afterPropertiesSet!!!!!!!!!!!!!!!!!!!");
		connect();
		call("초기화 연결 메세지");
		
	}

	// 빈이 종료(close)될 때 호출이 된다.
	@Override
	public void destroy() throws Exception {
		System.out.println("destroy!!!!!!!!!!!!!!!!!!!!");
		disconnect();
	}
	
}

```

<br>
<hr>
<br>

## 빈 등록 초기화, 소멸 메소드 지정
```java
package com.core.lifecycle;

import org.junit.jupiter.api.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.ConfigurableApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

public class BeanLifeCycleTest2 {
	
	
	@Test
	public void lifeCycleTest() {
		// 여기서 스프링 빈이 생성되면서 객체가 생성된다!
		ConfigurableApplicationContext ac = new AnnotationConfigApplicationContext(LifeCycleConfig.class);
		System.out.println("==================빈 등록 전 후=================!!!");
		
		NetworkClient2 networkClient = ac.getBean(NetworkClient2.class);
		
		System.out.println("==================빈 종료 전 후=================!!!");
		
		// ConfigurableApplicationContext 인터페이스에만 가능, ApplicationContext 자식임
		ac.close(); // 스프링 빈 라이프사이클 종료
	}
	
	
	
	
	@Configuration
	static class LifeCycleConfig {
		
		@Bean(initMethod = "init", destroyMethod = "close") // destroyMethod
		public NetworkClient2 networkClient() {
			NetworkClient2 networkClient2 = new NetworkClient2();
			networkClient2.setUrl("http://hello.dev");
			return networkClient2;
		}
		
	}
	
	
}

```

```java
package com.core.lifecycle;


public class NetworkClient2{
	private String url;

	public NetworkClient2() {
		System.out.println("생성자 호출, url : " + url);
		
	}


	public void setUrl(String url) {
		this.url = url;
	}
	
	
	// 서비스 시작시 호출
	public void connect() {
		System.out.println("connect : " + url);
		
	}
	
	
	public void call(String message) {
		System.out.println("call  : " + url + " / message : " + message);
	}
	
	
	//서비스 종료시 호출
	public void disconnect() {
		System.out.println("close : " + url);
	}


	// 의존관계 주입이 끝나면 호출하겠다.
	public void init() throws Exception {
		System.out.println("init!!!!!!!!!!!!!!!!!!!");
		connect();
		call("초기화 연결 메세지");
		
	}

	// 빈이 종료(close)될 때 호출이 된다.
	public void close() throws Exception {
		System.out.println("close!!!!!!!!!!!!!!!!!!!!");
		disconnect();
	}
	
}

```

<br>
<hr>
<br>

## 어노테이션 @PostConstruct, @PreDestroy
```java
package com.core.lifecycle;

import org.junit.jupiter.api.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.ConfigurableApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

public class BeanLifeCycleTest3 {
	
	
	@Test
	public void lifeCycleTest() {
		// 여기서 스프링 빈이 생성되면서 객체가 생성된다!
		ConfigurableApplicationContext ac = new AnnotationConfigApplicationContext(LifeCycleConfig.class);
		System.out.println("==================빈 등록 전 후=================!!!");
		
		NetworkClient3 networkClient = ac.getBean(NetworkClient3.class);
		
		System.out.println("==================빈 종료 전 후=================!!!");
		
		// ConfigurableApplicationContext 인터페이스에만 가능, ApplicationContext 자식임
		ac.close(); // 스프링 빈 라이프사이클 종료
	}
	
	
	
	
	@Configuration
	static class LifeCycleConfig {
		
		@Bean
		public NetworkClient3 networkClient() {
			NetworkClient3 networkClient3 = new NetworkClient3();
			networkClient3.setUrl("http://hello.dev");
			return networkClient3;
		}
		
	}
	
	
}

```

```java
package com.core.lifecycle;

import javax.annotation.PostConstruct;
import javax.annotation.PreDestroy;

public class NetworkClient3{
	private String url;

	public NetworkClient3() {
		System.out.println("생성자 호출, url : " + url);
		
	}


	public void setUrl(String url) {
		this.url = url;
	}
	
	
	// 서비스 시작시 호출
	public void connect() {
		System.out.println("connect : " + url);
		
	}
	
	
	public void call(String message) {
		System.out.println("call  : " + url + " / message : " + message);
	}
	
	
	//서비스 종료시 호출
	public void disconnect() {
		System.out.println("close : " + url);
	}


	// 의존관계 주입이 끝나면 호출하겠다.
	@PostConstruct
	public void init() throws Exception {
		System.out.println("init!!!!!!!!!!!!!!!!!!!");
		connect();
		call("초기화 연결 메세지");
		
	}

	// 빈이 종료(close)될 때 호출이 된다.
	@PreDestroy
	public void close() throws Exception {
		System.out.println("close!!!!!!!!!!!!!!!!!!!!");
		disconnect();
	}
	
}

```

<br>
<hr>
<br>

## 빈 스코프
- 빈 스코프란 빈이 존재할 수 있는 범위를 뜻한다. 스프링은 다음과 같은 스코프들을 지원한다.

<br>
-싱글톤(singletone)(스프링 IoC 컨테이너 당 하나의 인스턴스만 사용한다.)
   - 기본 스코프이다. 스프링 컨테이너의 시작과 종료까지 유지되는 가장 넓은 범위의 스코프이다.

- 프로토타입(Protype)(매번 새로운 빈을 정의해서 사용한다.)
   - 스프링 컨테이너는 프로토타입 빈의 생성과 의존관계 주입까지만 관여하고 더는 관리하지 않는다.
   - 스프링 빈을 조회할 때 생성되고 사용되면 버려진다.

- 웹 관련 스코프(각 클라이언트 마다 한 개의 빈만 사용한다.)
   - request : HTTP request가 들어올 때 생성되며 client에게 전달되면 소멸되는 스코프이다.
   - session : HTTP ession이 생성되고 소멸될 때까지 유지되는 스코프이다. 
   - application : ServletContect와 같은 범위로 유지되는 스코프이다.
   - websocket : 

<br>
<hr>
<br>


## PrototypeTest : Junit5

![image](https://user-images.githubusercontent.com/74396651/177924890-7ba829e9-6016-488b-871e-8ad75664d91a.png)

![image](https://user-images.githubusercontent.com/74396651/177924927-794039a9-e2ee-47e0-88f5-b47d9fc5bc9b.png)


```java
package com.core.scope;

import static org.assertj.core.api.Assertions.assertThat;

import javax.annotation.PostConstruct;
import javax.annotation.PreDestroy;

import org.junit.jupiter.api.Test;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.annotation.Scope;

public class PrototypeTest {

	@Test
	public void prototypeBeanFind() {
		AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(PrototypeBean.class);
    
		System.out.println("====find prototypeBean 1=====");
		PrototypeBean prototypeBean = ac.getBean(PrototypeBean.class); // 이 때 빈이 생성된다.
    
		System.out.println("====find prototypeBean 2=====");
		PrototypeBean prototypeBean2 = ac.getBean(PrototypeBean.class); // 이 때 빈이 생성된다.
    
		System.out.println("singletonBean : " + prototypeBean);
		System.out.println("singletonBean2 : " + prototypeBean2);
		
		assertThat(prototypeBean).isNotSameAs(prototypeBean2);
		
		// 종료시키고 싶다며 직접 종료 시켜야 한다.!
		prototypeBean.destroy();
		prototypeBean2.destroy();
		
		// close가 안된다. @PreDestroy가 실행되지 않는다. prototype은 스프링 컨테이너에서 스프링 빈을 조회할 때 빈이 생성이 되고 바로 버려진다.
		ac.close();
		
	}
	
	@Scope("prototype")
	static class PrototypeBean{
		
		@PostConstruct
		public void init() {
			System.out.println("=============init==============");
		}
		
		@PreDestroy
		public void destroy() {
			System.out.println("=============destroy==============");
			
		}
	}
  
  /*
  console
  
  ====find prototypeBean 1=====
  =============init==============
  ====find prototypeBean 2=====
  =============init==============
  singletonBean : com.core.scope.PrototypeTest$PrototypeBean@6c2ed0cd
  singletonBean2 : com.core.scope.PrototypeTest$PrototypeBean@7d9e8ef7
  =============destroy==============
  =============destroy==============
  14:24:18.111 [main] DEBUG org.springframework.context.annotation.AnnotationConfigApplicationContext - Closing org.springframework.context.annotation.AnnotationConfigApplicationContext@212b5695, started on Fri Jul 08 14:24:17 KST 2022

  */
}

```

<br>
<hr>
<br>

## SingletonTest : Junit5

![image](https://user-images.githubusercontent.com/74396651/177924805-bc09bdd3-9682-4213-bc62-7ecd0721c3ae.png)


```java
package com.core.scope;

import static org.assertj.core.api.Assertions.assertThat;

import javax.annotation.PostConstruct;
import javax.annotation.PreDestroy;

import org.junit.jupiter.api.Test;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.annotation.Scope;

public class SingletonTest {

	@Test
	public void singletonBeanFind() {
		AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(SingletonBean.class);
		SingletonBean singletonBean = ac.getBean(SingletonBean.class);
		SingletonBean singletonBean2 = ac.getBean(SingletonBean.class);
		System.out.println("singletonBean : " + singletonBean);
		System.out.println("singletonBean2 : " + singletonBean2);
		
		assertThat(singletonBean).isSameAs(singletonBean2);
		
		ac.close();
		
	}
	
	@Scope("singleton") // 기본이 싱글톤이다!
	static class SingletonBean{
		
		@PostConstruct
		public void init() {
			System.out.println("=============init==============");
		}
		
		@PreDestroy
		public void destroy() {
			System.out.println("=============destroy==============");
			
		}
	}
  /*
  console
  
  =============init==============
  singletonBean : com.core.scope.SingletonTest$SingletonBean@6c2ed0cd
  singletonBean2 : com.core.scope.SingletonTest$SingletonBean@6c2ed0cd
  14:28:24.492 [main] DEBUG org.springframework.context.annotation.AnnotationConfigApplicationContext - Closing org.springframework.context.annotation.AnnotationConfigApplicationContext@212b5695, started on Fri Jul 08 14:28:24 KST 2022
  =============destroy==============  
  */
}

```

<br>
<hr>
<br>

## Provider : Scope과 의존관계 주입의 해결사!
```java
package com.core.scope;

import static org.assertj.core.api.Assertions.assertThat;

import javax.annotation.PostConstruct;
import javax.annotation.PreDestroy;
import javax.inject.Provider;

import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.ObjectProvider;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.annotation.Scope;

/*
 * 
 *  Prototype 빈은 조회시에 생성되고 바로 버려진다. 하지만 Singleton 빈은 스프링 컨테이너가 종료될 때까지 유지된다!
 * 하지만 Singleton 안에 Prototype을 주입시킨다면 생성시점에 주입되어 사라지지 않는다.
 * 그러나 Prototye 빈을 사용하는 목적에 어긋나기 때문에 목적에 맞게 사용하기 위해 ObjectProvider<PrototypeBean>, Provider<PrototypeBean>를 사용한다!!!
 * 
 */


public class SingletonWithPrototypeTest1 {
	
	
	
	@Test
	@DisplayName("프로토타입 빈")
	public void prototypeFind() {
		AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(PrototypeBean.class);
		
		PrototypeBean prototypeBean1 = ac.getBean(PrototypeBean.class);
		prototypeBean1.addCount();
		assertThat(prototypeBean1.getCount()).isEqualTo(1);
		
		PrototypeBean prototypeBean2 = ac.getBean(PrototypeBean.class);
		prototypeBean2.addCount();
		assertThat(prototypeBean2.getCount()).isEqualTo(1);
		
	}
	
	@Test
	@DisplayName("싱글톤 빈에 프로토 타입 빈 주입시키기")
	public void singletonClientUsePrototype() {
		
		AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(ClientBean.class, PrototypeBean.class);
		
		ClientBean clientBean1 = ac.getBean(ClientBean.class);
		int count1 = clientBean1.logic();
		assertThat(count1).isEqualTo(1);
		
		
		ClientBean clientBean2 = ac.getBean(ClientBean.class);
		int count2 = clientBean2.logic();
//		assertThat(count2).isEqualTo(2);
		assertThat(count2).isEqualTo(1);

		
	}
	
	
	
	@Scope("singleton")
	static class ClientBean{
//		private final PrototypeBean prototypeBean; // 생성시점에 주입되어 사라지지 않는다.
		
		// DI가 아니라 DL을 사용하고 싶을 때 사용한다!!, 대신 주입시키는 것이다! 
		@Autowired
		private ObjectProvider<PrototypeBean> prototypeBeanProvider; // 원하는 스프링 빈을 찾아주는 역할, DL
		private Provider<PrototypeBean> prototypeBeanProvider2; // java8의 provider
		
//		@Autowired
//		public ClientBean(PrototypeBean prototypeBean) {
//			this.prototypeBean = prototypeBean;
//		}
		
		
		
		public int logic() {
			PrototypeBean prototypeBean = prototypeBeanProvider.getObject(); // 대신 조회하여 항상 새로운 프로토타입 빈을 생성한다.
			PrototypeBean prototypeBean2 = prototypeBeanProvider2.get();
			
			prototypeBean.addCount();
			int count = prototypeBean.getCount();
			
			return count;
		}
		
	}
	
	@Scope("prototype")
	static class PrototypeBean {
		 private int count = 0;
		
		 public void addCount() {
			 count++;
		 }
		 
		 public int getCount() {
			 return count;
		 }
		 
		 @PostConstruct
		 public void init() {
			 System.out.println("PrototypeBean.init " + this);
		 }
		 
		 @PreDestroy
		 public void destroy() {
			 System.out.println("PrototypeBean.destroy");
		 }
	}
}


```

<br>

## 정리
- 그러면 프로토타입 빈을 언제 사용할까? 매번 사용할 때 마다 의존관계 주입이 완료된 새로운 객체가 필요하면 사용하면 된다. 
- 그런데 실무에서 웹 애플리케이션을 개발해보면, 싱글톤 빈으로 대부분의 문제를 해결할 수 있기 때문에 프로토타입 빈을 직접적으로 사용하는 일은 매우 드물다.
- ObjectProvider , JSR330 Provider 등은 프로토타입 뿐만 아니라 DL이 필요한 경우는 언제든지 사용할 수 있다

<br>
<hr>
<br>

## 웹 스코프
- gradle 추가 <code>implementation 'org.springframework.boot:spring-boot-starter-web'</code>
- 혹시 포트가 겹치면 포트 변경해주자!

![image](https://user-images.githubusercontent.com/74396651/177924541-fd6683a0-a055-4f88-87db-d569a0f036fb.png)
- @Scope(value = "request", proxyMode = ScopedProxyMode.TARGET_CLASS) 를 사용해 프록시 객체 사용
- 클라이언트가 myLogger.logic() 을 호출하면 사실은 가짜 프록시 객체의 메서드를 호출한 것이다.
- 가짜 프록시 객체는 request 스코프의 진짜 myLogger.logic() 를 호출한다.
- 가짜 프록시 객체는 원본 클래스를 상속 받아서 만들어졌기 때문에 이 객체를 사용하는 클라이언트 입장에서는 사실 원본인지 아닌지도 모르게, 동일하게 사용할 수 있다(다형성)

<br>

## Controller
```java
package com.core.web;

import javax.servlet.http.HttpServletRequest;

import org.springframework.beans.factory.ObjectProvider;
import org.springframework.context.annotation.ScopedProxyMode;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

import com.core.common.MyLogger;

import lombok.RequiredArgsConstructor;

@Controller
@RequiredArgsConstructor
public class LogDemoController {
	private final LogDemoService logDemoService;
	private final MyLogger myLogger; // proxy 객체가 의존관계 주입이 되기 때문에 사용할 수 있다!(지연처리)
	
	//  MyLogger의 scope은 request이기 때문에 http요청이 와야 생성이 된다. 하지만 생성자로 바로 주입하기 때문에 예외가 발생한다.
	// 이를 해결하기 위해 앞서 원하는 빈을 찾을 수 있는 Provider를 주입시키면 된다.(http 요청이 와야 controller로 오는 것이기 때문에 provider로 지연시킬 수 있는 것이다!)
	//private final ObjectProvider<MyLogger> myLoggerProvider;
	
										
	
	
	@RequestMapping("/log-demo")
	@ResponseBody // view 화면이 없이 데이터를 반환하고 싶을 때! ajax, ...
	public String logDemo(HttpServletRequest request) throws InterruptedException {
//		MyLogger myLogger = myLoggerProvider.getObject();
		String requestURL = request.getRequestURL().toString();
		myLogger.setRequestURL(requestURL);
		
		System.out.println("myLogger : " + myLogger.getClass());
		myLogger.log("controller test");
		Thread.sleep(1000);
		logDemoService.logic("testID");
		
		return "OK";
	}
}


/*

console

[54c7915f-afba-49db-85f3-3742f40d2d99] request scope bean create : com.core.common.MyLogger@84fc595
[54c7915f-afba-49db-85f3-3742f40d2d99][http://localhost:9090/log-demo] controller test
[54c7915f-afba-49db-85f3-3742f40d2d99][http://localhost:9090/log-demo] service id : testID
[54c7915f-afba-49db-85f3-3742f40d2d99] request scope bean close : com.core.common.MyLogger@84fc595

[3428e9fe-9b46-4070-b3f7-bc9425b5caca] request scope bean create : com.core.common.MyLogger@3924ffcd
[3428e9fe-9b46-4070-b3f7-bc9425b5caca][http://localhost:9090/log-demo] controller test
[3428e9fe-9b46-4070-b3f7-bc9425b5caca][http://localhost:9090/log-demo] service id : testID
[3428e9fe-9b46-4070-b3f7-bc9425b5caca] request scope bean close : com.core.common.MyLogger@3924ffcd

 */

```

## Service
```java
package com.core.web;

import org.springframework.beans.factory.ObjectProvider;
import org.springframework.stereotype.Service;

import com.core.common.MyLogger;

import lombok.RequiredArgsConstructor;

@Service
@RequiredArgsConstructor
public class LogDemoService {
	
	private final MyLogger myLogger; // proxy 객체가 의존관계 주입이 되기 때문에 사용할 수 있다!(지연처리)
	
	//  MyLogger의 scope은 request이기 때문에 http요청이 와야 생성이 된다. 하지만 생성자로 바로 주입하기 때문에 예외가 발생한다.
	// 이를 해결하기 위해 앞서 원하는 빈을 찾을 수 있는 아이를 주입시키면 된다.
//	private final ObjectProvider<MyLogger> myLoggerProvider;
	
	public void logic(String id) {
//		MyLogger myLogger = myLoggerProvider.getObject();
				
		myLogger.log("service id : " + id);
	}
}

```

## Compnent
```java
package com.core.common;

import java.util.UUID;

import javax.annotation.PostConstruct;
import javax.annotation.PreDestroy;

import org.springframework.context.annotation.Scope;
import org.springframework.context.annotation.ScopedProxyMode;
import org.springframework.stereotype.Component;

@Component
@Scope(value = "request", proxyMode = ScopedProxyMode.TARGET_CLASS)
// HTTP request 요청 당 새로운 request scope 객체가  할당된다!
// proxyMode = ScopedProxyMode.TARGET_CLASS : MyLogger의 가짜 프록시 클래스를 만들어두고 HTTP request와 상관 없이 가짜 프록시 클래스를 다른 빈에 미리 주입해 둘 수 있다.
// proxyMode를 사용하면 Provider를 사용할 필요가 없다!!
	
public class MyLogger {
	
	private String uuid;
	private String requestURL;
	
	
	 public void setRequestURL(String requestURL) {
		 this.requestURL = requestURL;
	 }
	 
	 
	 public void log(String message) {
		 System.out.println("[" + uuid + "]" + "[" + requestURL + "] " + message);
	 }
	 
	 @PostConstruct
	 public void init() {
		 // 전세계에 있는 id 만들기
		 uuid = UUID.randomUUID().toString();
		 System.out.println("[" + uuid + "] request scope bean create : " + this);
	 }
	 
	 @PreDestroy
	 public void close() {
		 
		 System.out.println("[" + uuid + "] request scope bean close : " + this);
		 
	 }
	

}

```

## 참고
- 객체의 생성과 초기화를 분리하자.
   - 생성자는 필수 정보(파라미터)를 받고, 메모리를 할당해서 객체를 생성하는 책임을 가진다. 
   - 반면에 초기화는 이렇게 생성된 값들을 활용해서 외부 커넥션을 연결하는등 무거운 동작을 수행한다.
   - 따라서 생성자 안에서 무거운 초기화 작업을 함께 하는 것 보다는 객체를 생성하는 부분과 초기화 하는 부분을 명확하게 나누는 것이 유지보수 관점에서 좋다.
   - 물론 초기화 작업이 내부 값들만 약간 변경하는 정도로 단순한 경우에는 생성자에서 한번에 다 처리하는게 더 나을 수 있다.






