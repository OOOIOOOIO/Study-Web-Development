# Logging
- 스프링 부트 라이브러리를 사용하며 스프링 부트 로깅 라이브러리(sprig-boot-starter-logging)가 함께 포함된다.
- 스프링 부트 로깅 라이브러리는 기본으로 다음 로깅 라이브러리를 사용한다.
   - SLF4J - http://www.slf4j.org
   - Logbackk - http://logback.qos.ch

- 로그 라이브러리는 Logback, Log4J, Log4J2 등등 수 많은 라이브러리가 있는데, 그것을 통합해서인터페이스로 제공하는 것이 바로 SLF4J 라이브러리다.
- 쉽게 이야기해서 SLF4J는 인터페이스이고, 그 구현체로 Logback 같은 로그 라이브러리를 선택하면 된다.
- 실무에서는 스프링 부트가 기본으로 제공하는 Logback을 대부분 사용한다

<br>

## application.properties 설정
```
# 전체 로그 레벨 설정(default info)
#logging.level.root=info

# hello.springmvc 패키지와 그 하위 로그 레벨 설정
# 심각도 : trace -> debug -> info -> warn -> error
logging.level.hello.springmvc=debug

```

<br>

## 코드
```java
package hello.springmvc.basic;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import lombok.extern.slf4j.Slf4j;

@RestController 
@Slf4j
public class LogTestController {

	// slf4j 사용!
//	private final Logger log = LoggerFactory.getLogger(getClass());
	
	@RequestMapping("/log-test")
	public String logTest() {
		String name = "Spring";
		
		System.out.println("name = " + name);
		
		
		// log level 정하기! format 형식으로 사용하는구나! 
		// "+" 연산으로 사용하면 안된다. 먼저 연산이 되기 때문에 의미없는 메모리 낭비가 일어난다!
		// 로그 레벨 trace(높) --> error(낮)
		log.trace(" trace log={}", name); 
		log.debug(" debug log={}", name); // 개발 서버 주로 이용
		log.info(" info log={}", name);  // 운영 서버 주로 이용, default level
		log.warn(" warn log={}", name);
		log.error(" error log={}", name);
		
		return "ok"; // RestController라서 뷰가 아닌 그냥 HTTP body(string data)로 넘어간다
				
	}
			
		
}

/*
console

name = Spring
2022-07-19 10:45:43.555 DEBUG 31672 --- [nio-9090-exec-1] hello.springmvc.basic.LogTestController  :  debug log=Spring
2022-07-19 10:45:43.557  INFO 31672 --- [nio-9090-exec-1] hello.springmvc.basic.LogTestController  :  info log=Spring
2022-07-19 10:45:43.557  WARN 31672 --- [nio-9090-exec-1] hello.springmvc.basic.LogTestController  :  warn log=Spring
2022-07-19 10:45:43.557 ERROR 31672 --- [nio-9090-exec-1] hello.springmvc.basic.LogTestController  :  error log=Spring

*/

```
### @RestController
- @Controller 는 반환 값이 String 이면 뷰 이름으로 인식된다. 그래서 뷰를 찾고 뷰가 랜더링 된다.
- @RestController 는 반환 값으로 뷰를 찾는 것이 아니라, HTTP 메시지 바디에 바로 입력한다. 

<br>

## 테스트
- LEVEL: TRACE > DEBUG > INFO > WARN > ERROR
- 개발 서버는 debug 출력한다.
- 운영 서버는 info 출력한다.

<br>

## 옳바른 로그 사용법
<code>log.debug("data="+data)</code>
-로그 출력 레벨을 info로 설정해도 해당 코드에 있는 "data="+data가 실제 실행이 되어 버린다. 
-결과적으로 문자 더하기 연산이 발생한다.

<code>log.debug("data={}", data)</code>
-로그 출력 레벨을 info로 설정하면 아무일도 발생하지 않는다. 따라서 앞과 같은 의미없는 연산이 발생하지 않는다.

<br>

## 로그 사용시 장점
- 쓰레드 정보[안에 있는 정보] , 클래스 이름 같은 부가 정보를 함께 볼 수 있고, 출력 모양을 조정할 수 있다.
- 로그 레벨에 따라 개발 서버에서는 모든 로그를 출력하고, 운영서버에서는 출력하지 않는 등 로그를 상황에 맞게 조절할 수 있다.
- 시스템 아웃 콘솔에만 출력하는 것이 아니라, 파일이나 네트워크 등, 로그를 별도의 위치에 남길 수 있다.(/resource 안에 Logback xml파일 별도 설정) 
- 특히 파일로 남길 때는 일별, 특정 용량에 따라 로그를 분할하는 것도 가능하다.
- 성능도 일반 System.out보다 좋다. (내부 버퍼링, 멀티 쓰레드 등등) 그래서 실무에서는 꼭 로그를 사용해야 한다. 그리고 사용법 틀리면 욕 먹는다!!!

## 참고
- SLF4J - http://www.slf4j.org
- Logback - http://logback.qos.ch
- 스프링 부트가 제공하는 로그 기능은 다음을 참고하자.
   - https://docs.spring.io/spring-boot/docs/current/reference/html/spring-bootfeatures.html#boot-features-loggin



<br>
<br>
<br>
<br>
<br>

참조 : 김영한님 스프링 강의
