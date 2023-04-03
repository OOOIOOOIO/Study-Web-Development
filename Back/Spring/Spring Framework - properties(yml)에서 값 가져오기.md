## properties or yml 파일의 값 가져오기
> 하.... AWS에 Spring boot를 배포하다 계속 외부 yml에 있는 jwt.refreshExpireMin 을 못가져왔다. 분명 로컬에선 잘 주입되는데 왜... 도대체 왜!! 알고보니 vim 파일 띄어쓰기 오류였다. 하...항상 @Value만 사용하다 이게 안먹히나 싶어 다른 방법을 사용해서 주입해보았는데 잘 돌아가서 이번에 정리한다.

<hr>

## @Value 사용하기


### yml 파일
```
jwt:
  refreshExpireMin: 604800000 # 7일(7 * 24 * 60 * 60 * 1000)

```

### 자바 클래스에서 사용
```java

@Service
public class RefreshTokenService {
    @Value("${jwt.refreshExpireMin}")
    private Long refreshTokenMin;
    
  
}

```

<hr>

### 새로운 방식 : @ConfigurationProperties 사용
> Mockito를 사용한 단위 테스트에선 Spring의 Application Context가 로딩되지 않기 때문에 테스트를 수행하면서 activeEnv가 주입되지 않아 NullPointerException이 발생한다! 

- @ConfigurationProperites를 사용한다.
- prifix는 위 yml 파일을 보면 refrech~를 가져오고 싶을 때 앞에 있는 속성을 적어준다!

```java
package com.gdsc.side.api.config.jwt;

import lombok.Getter;
import lombok.Setter;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.context.annotation.Configuration;
import org.springframework.stereotype.Component;

//@Component
@Configuration
@ConfigurationProperties(prefix = "jwt")
@Getter
@Setter
public class JwtInfoProperties {
    private String secret;
    private Long expireMin;
    private Long refreshExpireMin;


}


```

### @ConfigurationProperties를 사용해야 하는 이유
>@Value를 통한 프로퍼티 주입은 나쁜 습관이지만 해당 포스트에서 중요한 부분이 아니니 간략하게 알아봅시다. 링크에선 아래 2가지 이유를 들어 @Value가 좋지 못한 Injection이라고 말하고 있다.

1. Configuration이 전체 어플리케이션에 흩어져 있게 됩니다.
각 클래스가 @Value로 Configuration의 일부분을 선택적으로 사용하기 때문이죠.
key의 이름을 변경할 경우 cmd + shift + f로 프로젝트 내에서 검색하여 일일이 수정해야 할 지도 모릅니다.

2. Configuration은 다른 클래스들처럼 캡슐화된 서비스로 제공되어야 합니다. 
이 경우 책임을 1군데에서 가지게 됩니다.
Spring Boot를 사용할 경우 @ConfigurationProperites로 쉽게 주입받을 수 있습니다.



- [자세한 참고1](https://kkambi.tistory.com/210)
- [자세한 참고2](https://programmer93.tistory.com/47)



