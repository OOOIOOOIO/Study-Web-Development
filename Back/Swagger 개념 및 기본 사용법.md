# Swagger
Swagger 는 OAS(Open Api Specification)를 위한 프레임워크이다. 개발자들의 필수 과제인 API 문서화를 쉽게 할 수 있도록 도와주며, 파라미터를 넣어서 실제로 어떤 응답이 오는지 테스트도 할 수 있습니다. 또한, 협업하는 클라이언트 개발자들에게도 Swagger 만 전달해주면 API Path 와 Request, Response 값 및 제약 등을 한번에 알려줄 수 있습니다.

<br>

## OpenAPI와 관계
OpenAPI는 RESTful API 설계를 위한 업계 표준 사양을 나타내고 Swagger는 SmartBear 도구 세트를 나타낸다. 요약하면 Swagger 는 이제 OpenAPI 사양을 구현하기 위한 도구 세트 (Swagger Editor, Swagger UI, SwaggerHub) 가 되었으며, 브랜드명만 변경하지 않은 채 그대로 사용하기로 한 것이다.[Swagger 공식 블로그 포스팅](https://swagger.io/blog/api-strategy/difference-between-swagger-and-openapi/)

<br>

## 적용(Swagger 3.x버전(3.0.0))

### 의존성 추가, build.gradle
```java
dependencies {
    // ..
    implementation 'io.springfox:springfox-boot-starter:3.0.0'
    // ..
}
```
- 사용할 버전은 [여기](https://mvnrepository.com/artifact/io.springfox/springfox-boot-starter)에서 확인할 수 있다.
- Swagger 2.x 버전과 다르게 3.x 버전은 springfox-boot-starter 하나만 추가하면 하위에 필요한 모든 라이브러리가 포함되어 있다.

<br>

### 스프링 2.6이상 null 오류 해결, application.yml ([참고](https://goyunji.tistory.com/137)
```java
spring:
  mvc:
    pathmatch:
      matching-strategy: ant_path_matcher
```

<br>

### Config 추가
```java
package com.sh.excelpractice.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.EnableWebMvc;
import springfox.documentation.builders.ApiInfoBuilder;
import springfox.documentation.builders.PathSelectors;
import springfox.documentation.builders.RequestHandlerSelectors;
import springfox.documentation.service.ApiInfo;
import springfox.documentation.spi.DocumentationType;
import springfox.documentation.spring.web.plugins.Docket;
import springfox.documentation.swagger2.annotations.EnableSwagger2;

@Configuration
//@EnableSwagger2 // Swagger 2.x
@EnableWebMvc // Swagger 3.x
public class SwaggerConfig {

    @Bean
    public Docket api(){
//        return new Docket(DocumentationType.SWAGGER_2) // Swagger 2.x
        return new Docket(DocumentationType.OAS_30) // Swagger 3.x
                .apiInfo(apiInfo())
                .useDefaultResponseMessages(false)
                .select()
                .apis(RequestHandlerSelectors.any()) // 현재 RequestMapping으로 할당된 모든 URL 리스트 추출
                .paths(PathSelectors.ant("/api/**")) // 그 중 /api로 시작하는 URL들만 필터릉
                .build();
    }

    // swagger-ui/index.html 페이지 커스텀
    private ApiInfo apiInfo() {
        return new ApiInfoBuilder()
                .title("성호 스웨거 제목")
                .version("1.0.0")
                .description("성호 스웨거 설명")
                .build();
    }

}

```
- Docket: Swagger 설정의 핵심이 되는 Bean
- apiInfo:Swagger UI 로 노출할 정보
- useDefaultResponseMessages: Swagger 에서 제공해주는 기본 응답 코드 (200, 401, 403, 404). false 로 설정하면 기본 응답 코드를 노출하지 않음, @ApiResponse로 커스텀!
- apis: api 스펙이 작성되어 있는 패키지 (Controller) 를 지정
- paths: apis 에 있는 API 중 특정 path 를 선택

<br>

### Controller에 적용
```
package com.sh.excelpractice.controller;

import com.sh.excelpractice.controller.dto.InfoDto;
import io.swagger.annotations.ApiOperation;
import io.swagger.v3.oas.annotations.Operation;
import io.swagger.v3.oas.annotations.responses.ApiResponse;
import io.swagger.v3.oas.annotations.responses.ApiResponses;
import lombok.RequiredArgsConstructor;
import lombok.extern.slf4j.Slf4j;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
@RequestMapping("/api")
@RequiredArgsConstructor
@Slf4j
public class ExcelV1Controller {

    @Operation(summary = "v1 summary", description = "v1 description")
    @ApiResponses({
            @ApiResponse(responseCode = "200", description = "OK !!"),
            @ApiResponse(responseCode = "400", description = "BAD REQUEST !!"),
            @ApiResponse(responseCode = "404", description = "NOT FOUND !!"),
            @ApiResponse(responseCode = "500", description = "INTERNAL SERVER ERROR !!")
    })

    @GetMapping("/v1/hi")
    public String hi(){

        return "/excel";
    }

    @ApiOperation(value="v1 value", notes="v1 notes")
    @GetMapping("/v1/bye")
    public ResponseEntity<InfoDto> bye(){

        return new ResponseEntity<>(new InfoDto("김성호", "남자", 19980111, 26), HttpStatus.OK);
    }


}

```


<br>
[ApiOperation vs ApiResponse 차이](https://www.baeldung.com/swagger-apioperation-vs-apiresponse)
![image](https://user-images.githubusercontent.com/74396651/220807212-22bd04a6-fb3a-4c54-9c78-ca1cd3f1e9dc.png)


<br>

### 서버 실행 후 접속
> 로컬 실행 후 URL 접속하면 되는데 Swagger 2.x 버전과 다르다.(http://도메인주소:포트번호/swagger-ui/index.html)
- Swagger2: http://localhost:8080/swagger-ui.html
- Swagger3: http://localhost:8080/swagger-ui/index.html


<hr>

## 결과

![image](https://user-images.githubusercontent.com/74396651/220810301-0a866c3d-d86a-4a9f-bd13-42f36dfc793e.png)

![image](https://user-images.githubusercontent.com/74396651/220810182-75ad7003-3eea-4794-b756-cda79dafcdda.png)

![image](https://user-images.githubusercontent.com/74396651/220810206-585c8539-b51f-4c66-b857-f83e1dd27c72.png)

![image](https://user-images.githubusercontent.com/74396651/220810238-4e1e3644-405c-45ef-b180-127714961601.png)


<br>
<hr>
<br>

## Security와 함께 사용하기 

.antMatchers("/swagger-ui/**", "/swagger-resources/**", "/swagger-resources", "/v3/api-docs").permitAll() 


