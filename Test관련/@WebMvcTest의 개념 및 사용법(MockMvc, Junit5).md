# @WebMvcTest
> &nbsp; ApplicationContext를 완전하게 Start 시키지 않고 web layer를 테스트하고 싶을 때 @WebMvcTest를 사용한다. @WebMvcTest는 Present Layer 관련 컴포넌트만 스캔하며 Service, Repository dependency는 빈을 등록하지 않는다. 이들을 사용하고 싶다면 @MockBean으로 더미 객체를 주입 받아 테스트를 진행하자!<br>
> &nbsp; @SpringBootTest와 다른 점은 @SpringBootTest는 모든 빈을 로드하기 때문에 테스트 구동 시간이 오래 걸리고, 테스트 단위가 크기 때문에 디버깅이 어려울 수 있다. Controller관련 레이어만 슬라이스 테스트 하고 싶을 때 @WebMvcTest를 사용한다.

<br>

## 주의점(JPA 설정과 충돌)
> &nbsp; @SpringBootApplication가 붙은 클래스는 자동으로 모든 테스트들의 기본 설정이 적용된다. 테스트 코드는 실행될 때 @SpringBootApplication가 붙은 나의 ~Application.java를 로딩하는데, 테스트 코드에 붙은 @WebMvcTest는 JPA 관련 bean 객체 등을 로딩하지 않아 @EnableJpaAuditing과 설정이 맞지 않는 문제가 발생한다. 쉽게 말해 @WebMvcTest는 Entity를 안불러오는데 @EnableJpaAuditing은 불러와야 해서 서로 충돌한다는 것이다. 때문에 이를 해결하고 싶다면 @EnableJpaAuditing을 따로 Config 패키에 선언한다.<br>

- 만약 JPA 기능까지 한번에 테스트 하고 싶다면 @SpringBootTest + TestRestTemplate을 사용하자!

![image](https://user-images.githubusercontent.com/74396651/202836018-450b23b0-91c0-4231-a58c-5eb258fa26e1.png)
![image](https://user-images.githubusercontent.com/74396651/202836038-9b702a1b-b9ad-4fe7-9f1e-fe01c90a634a.png)

<br>
<hr>
<br>

## Controller 테스트 @WebMvcTest + MvcMock 사용법!(대체적으로 MvcMock을 함께 사용한다.)
- 우선 MockMvc는 웹 API를 테스트할 때 사용. Spring MVC 테스트의 시작점, HTTP GET, POST 등에 대한 API 테스트를 할 수 있다.

### Controller
```java
package com.sh.web;

import com.sh.web.dto.HelloResponseDTO;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

// API TEST
@RestController
public class HelloController {

    @GetMapping("/hello")
    public String hello(){
        return "hello";
    }

    @GetMapping("/hello/dto")
    public HelloResponseDTO helloDTO(@RequestParam("name") String name, @RequestParam("amount") int amount){
        return new HelloResponseDTO(name, amount);
    }
}

```

<br>

### ControllerTest
```java
//@WebMvcTest
@WebMvcTest(controllers = HelloController.class, // 테스트할 controller 명시
            excludeFilters = {
                            @ComponentScan.Filter(type = FilterType.ASSIGNABLE_TYPE, classes = SecurityConfig.class) // SpringConfig 제외
            }
)
public class HelloControllerTest {

    @Autowired
    private MockMvc mvc; // 웹 API를 테스트할 때 사용. Spring MVC 테스트의 시작점, HTTP GET, POST 등에 대한 API 테스트를 할 수 있다.

    @WithMockUser(roles = "USER")
    @Test
    public void helloReturn() throws Exception {
        String hello = "hello";
        
        // return 값 hello
        ResultActions hello1 = mvc.perform(MockMvcRequestBuilders.get("/hello"))
                .andExpect(MockMvcResultMatchers.status().isOk())
                .andExpect(MockMvcResultMatchers.content().string("hello"));


    }

    @WithMockUser(roles = "USER")
    @Test
    void helloDTOReturn() throws Exception {
        String name = "hello";
        int amount = 1000;
  
        // return 값 body(Json형식)
        mvc.perform(MockMvcRequestBuilders.get("/hello/dto")
                        .param("name", name)
                        .param("amount", String.valueOf(amount)))
                .andExpect(MockMvcResultMatchers.status().isOk())
                .andExpect(MockMvcResultMatchers.jsonPath("$.name", Matchers.is(name)))
                .andExpect(MockMvcResultMatchers.jsonPath("$.amount", Matchers.is(amount)));

    }

}
```

<br>
<hr>
<br>

## 

<br>
<hr>
<br>

## Spring Security와 함께 사용하는 법
- #### @WebMvcTest는 컨트롤러 관련 bean만 등록하기 때문에 Security관련 bean은 가져오지 않는다. 때문에 @WebMvcTest를 하기 위해선 관련 설정을 exclude 해주어야 한다. (@EnableWebSecurity(WebSecurityConfigurerAdapter 상속 받기 때문)는 읽지만 그 밑의 @Service 하위 클래스를 읽지 못하기 때문에 그냥 exclude 해준다.)

![image](https://user-images.githubusercontent.com/74396651/202835854-1c8b5bd8-10ce-47d8-a131-29587ae4e716.png)

<br>
<hr>
<br>

- #### 또한 Security를 사용하기 위해선 인증된 사용자를 증명해야 하기 때문에 @WithMockUser()를 사용한다.

![image](https://user-images.githubusercontent.com/74396651/202836001-f6862828-5262-429f-a711-e935f4742762.png)

<br>
<hr>
<br>

- #### 만약 csrf 관련 에러가 생긴다면! [참고](https://velog.io/@cieroyou/WebMvcTest%EC%99%80-Spring-Security-%ED%95%A8%EA%BB%98-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0)

![image](https://user-images.githubusercontent.com/74396651/202836383-62358118-2dce-4994-8dfd-af2a6461da6f.png)

![image](https://user-images.githubusercontent.com/74396651/202836567-83620aa9-a046-45b0-8e5f-a1d50fb6bfa0.png)

<br>
<hr>
<br>

- #### build.gradle에 security-test 추가하기!

![image](https://user-images.githubusercontent.com/74396651/202836138-907b799b-503d-4542-b9a9-6d09061961aa.png)


