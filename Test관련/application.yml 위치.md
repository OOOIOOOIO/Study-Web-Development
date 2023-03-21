## 패키지 구조

<img width="434" alt="image" src="https://user-images.githubusercontent.com/74396651/226576434-38be1a48-265b-497a-a700-b1726a5d84e0.png">

## application.yml 파일

```java
server:
  port: 9999

spring:
  mvc:
    pathmatch:
      matching-strategy: ant_path_matcher # swagger 에러 해결
  profiles:
    include:
      - test-db
      - test-oauth
      - test-cloud
```
