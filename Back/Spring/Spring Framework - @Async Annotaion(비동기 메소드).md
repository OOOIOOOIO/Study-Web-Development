# @Async Annotation
> &nbsp; @Async Annotation은 Thread Pool을 활용하는 비동기 메소드를 지원하는 Annotation으로 Spring에서 제공한다.  

### 기존 Java의 비동기 방식으로 메소드 구현
```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class GillogAsync {

    static ExecutorService executorService = Executors.newFixedThreadPool(5);

    public void asyncMethod(final String message) throws Exception {
        executorService.submit(new Runnable() {
            @Override
            public void run() {
                // do something
            }            
        });
    }
}
```
- java.util.concurrent.ExecutorService을 활용해서 비동기 방식의 method를 정의 할 때마다, Runnable의 run()을 재구현해야 하는 등 동일한 작업들의 반복이 잦았다.

<hr>

### @Async 사용
- @Async 어노테이션을 활용하면 손쉽게 비동기 메소드 작성이 가능하다. 만약 Spring Boot에서 간단히 사용하고 싶다면 단순히 Application Class에 @EnableAsync 어노테이션을 추가한다.
```java
@EnableAsync
@SpringBootApplication
public class SpringBootApplication {
    ...
}
```

 비동기로 작동하길 원하는 method 위에 Annotation을 붙여준다.

```java
public class GillogAsync {

    @Async
    public void asyncMethod(final String message) throws Exception {
        ....
    }
}
```

<br>
<hr>
<br>

위와 같이 사용은 간단하지만 @Async 기본설정인 SimpleAsyncTaskExcecutor를 사용한다. 

![image](https://user-images.githubusercontent.com/74396651/220223896-0882183f-7b57-4cad-9d8b-3e9a2d494fc5.png)


본인의 개발 환경에 맞게 커스텀하기 위해선 AsyncConfigurerSupport를 상속받는 Class를 작성하는 것이 좋다.

```java
import java.util.concurrent.Executor;

import org.springframework.context.annotation.Configuration;
import org.springframework.scheduling.annotation.AsyncConfigurerSupport;
import org.springframework.scheduling.annotation.EnableAsync;
import org.springframework.scheduling.concurrent.ThreadPoolTaskExecutor;

@Configuration
@EnableAsync
public class AsyncConfig extends AsyncConfigurerSupport {
    
    @Override
    public Executor getAsyncExecutor() {
        ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
        executor.setCorePoolSize(5);
        executor.setMaxPoolSize(30);
        executor.setQueueCapacity(50);
        executor.setThreadNamePrefix("DDAJA-ASYNC-");
        executor.initialize();
        return executor;
    }
}
```
여기서 설정한 요소들은 아래와 같다.

- @Configuration : Spring 설정 관련 Class로 @Component 등록되어 Scanning 될 수 있다.
- @EnableAsync : Spring method에서 비동기 기능을 사용가능하게 활성화 한다.
- CorePoolSize : 기본 실행 대기하는 Thread의 수**
- MaxPoolSize : 동시 동작하는 최대 Thread의 수
- QueueCapacity : MaxPoolSize 초과 요청에서 Thread 생성 요청시,
- 해당 요청을 Queue에 저장하는데 이때 최대 수용 가능한 Queue의 수,
- Queue에 저장되어있다가 Thread에 자리가 생기면 하나씩 빠져나가 동작
- ThreadNamePrefix : 생성되는 Thread 접두사 지정

<br>
<hr>
<br>

## 주의사항
> @Async Annotation을 사용할 때 아래와 같은 세 가지 사항을 주의하자.
- private method는 사용 불가
- self-invocation(자가 호출) 불가, 즉 inner method는 사용 불가
- QueueCapacity 초과 요청에 대한 비동기 method 호출시 방어 코드 작성

![image](https://user-images.githubusercontent.com/74396651/220224247-b999b7b1-1434-482a-bec1-0154379e6d26.png)

위 주의사항을 아래 사진과 함께 설명을 해보면, @Async의 동작은 AOP가 적용되어 Spring Context에서 등록된 Bean Object의 method가 호출 될 시에, Spring이 확인할 수 있고 @Async가 적용된 method의 경우 Spring이 method를 가로채 다른 Thread에서 실행 시켜주는 동작 방식이다.<br>
이 때문에 Spring이 해당 @Async method를 가로챈 후, 다른 Class에서 호출이 가능해야 하므로, private method는 사용할 수 없는 것이다. 또한 Spring Context에 등록된 Bean의 method의 호출이어야 Proxy 적용이 가능하므로, inner method의 호출은 Proxy 영향을 받지 않기에 self-invocation이 불가능하다. 위 주의사항을 아래 예시 코드와 함께 살펴보자

<br>

### self-invocation(자가 호출) 불가(inner method)
위에서 작성한 AsyncConfig를 사용하는 Spring Project에서 아래와 같이, 같은 Class에 존재하는 method에 @Async Annotation을 작성해 비동기 방식을 사용해보자.

<br>

### 코드
```java
@Controller
@Slf4j
public class InnerMethod {

    @GetMapping("async")
    public String testAsync() {
        log.info("TEST ASYNC");
        for(int i=0; i<50; i++) {
            asyncMethod(i);
        }
        return "";
    }

    @Async
    public void asyncMethod(int i) {
        try {
            Thread.sleep(500);
            log.info("[AsyncMethod]"+"-"+i);
        } catch(InterruptedException e) {
            e.printStackTrace();
        }
    }

}

```

### 로그
![image](https://user-images.githubusercontent.com/74396651/220226030-7c87b90b-ea86-4953-8133-1152cda532c0.png)

로그를 보면 비동기 방식으로 쓰레드가 호출되지 않고 하나의 쓰레드로 순서대로 동작하는 것을 확인할 수 있다. 즉, 하나의 클래스 내에서는 @Async 사용이 불가능하다.

<hr>

### 서로 다른 클래스 코드
```java
@Service
@Slf4j
public class TestService {
    @Async
    public void asyncMethod(int i) {
        try {
            Thread.sleep(500);
            log.info("[AsyncMethod]"+"-"+i);
        } catch(InterruptedException e) {
            e.printStackTrace();
        }
    }
}

@AllArgsConstructor
@Controller
@Slf4j
public Class TestController {

    private TestService testService;

    @GetMapping("async")
    public String testAsync() {
        log.info("TEST ASYNC");
        for(int i=0; i<50; i++) {
            testService.asyncMethod(i);
        }
        return "";
    }
    
}
```

### 로그
![image](https://user-images.githubusercontent.com/74396651/220226330-62db598d-b51f-4478-9108-503067bedb25.png)

로그를 보면 서로 다른 쓰레드가 무작위로 호출하는 것을 볼 수 있다. 또한 AsyncConfig(커스텀)에서 prefix로 작성한 접두사(DDAJA-ASYNC)도 쓰레드에 정상적으로 붙은 것을 확인할 수 있다.


<br>
<hr>
<br>

## QueueCapacity 초과 요청 방어 코드 작성

### AsyncConfig 코드
> 이번엔 AsyncConfig에서 PoolSize와 QueueCapacity를 줄여보고 위 코드를 다시 실행해보자.
```java
@Configuration
@EnableAsync
public class AsyncConfig extends AsyncConfigurerSupport {
    
    @Override
    public Executor getAsyncExecutor() {
        ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
        executor.setCorePoolSize(2);
        executor.setMaxPoolSize(10);
        executor.setQueueCapacity(10);
        executor.setThreadNamePrefix("DDAJA-ASYNC-");
        executor.initialize();
        return executor;
    }
}
```

![image](https://user-images.githubusercontent.com/74396651/220227786-bb0e38e7-09fc-4403-8913-9447a332cf17.png)

#### Exception을 살펴보면 
```
Request processing failed; nested exception is org.springframework.core.task.TaskRejectedException: 
Executor [java.util.concurrent.ThreadPoolExecutor@116e53a0
[Running, pool size = 10, active threads = 10, queued tasks = 10, completed tasks = 0]] 
did not accept task: org.springframework.aop.interceptor.AsyncExecutionInterceptor$$
Lambda$2031/170931344@7ef0a05e] with root cause
```
TaskRejectedException으로 수행된 task는 0으로 설정 thread 수, pool size 는 10, queued 된 tasks = 10개로 최대 수용 가능한 Thread Pool 수와 QueueCapacity 까지 초과된 요청이 들어오자, Task를 Reject하는 Exception이 발생하였다.<br>
TaskRejectedException 발생 시 handling 해주는 방어 코드를 작성해보자.

```java
@AllArgsConstructor
@Controller
public Class TestController {

    private TestService testService;

    @GetMapping("async")
    public String testAsync() {
        log.info("TEST ASYNC");
        try {
            for(int i=0; i<50; i++) {
                testService.asyncMethod(i);
        } catch (TaskRejectedException e) {
            // ....
        }
        return "";
    }
    
}
```


<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

[참고1](https://velog.io/@gillog/Spring-Async-Annotation%EB%B9%84%EB%8F%99%EA%B8%B0-%EB%A9%94%EC%86%8C%EB%93%9C-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0)

