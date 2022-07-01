# ComponentScan
- 지금까지 스프링 빈을 xml의 <Bean> 혹은 Java의 @Bean을 통해 설정 정보 파일 혹은 클래스에 등록을 하였다. 하지만 @Componet 어노테이션과 @ComponentScan 어노테이션을 통해 
쉽고 간단하게 스프링 빈을 등록할 수 있다.

<br>

## ComponentScan의 기본 대상

- @Component
- @Controller
- @Service
- @Repository
- @Configuration

<br>
<hr>
<br>

## Component 클래스 : 구현체 클래스
```java

@Component
public class MemoryMemberRepository implements MemberRepository {

}



@Component
public class RateDiscountPolicy implements DiscountPolicy {}



@Component
public class MemberServiceImpl implements MemberService {}



@Component
public class OrderServiceImpl implements OrderService {}

```
- @Coponent 어노테이션을 통해 스프링 빈으로 등록한다.
- @Autowired 어네테이션을 통해 의존관계를 주입하는데, 이는 뒤에서 한번에 보자!

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
- basePackages = "com.core"를 통해 스캔을 어디서부터 시작할 건지 설정할 수 있다. 만약 설정하지 않았다면 @ComponentScan이 등록된 클래스가 있는 패키지가 시작 패키지이다.
   - 가능하면 설저 정보 클래스의 위치를 프로젝트의 최상단에 둘 것을 추천한다. 최근 스프링 부트도 이를 기본으로 제공한다.
