# Spring Boot 2.7.x 버전 
> 전체 Release Notes 목록은 이 곳에서 확인할 수 있습니다.
- https://luvstudy.tistory.com/tag/Release%20Notes
- https://github.com/spring-projects/spring-boot/wiki/Spring-Boot-2.7-Release-Notes

<br>

- Upgrading from Spring Boot 2.6
  - @SpringBootTest Property Source Precendence
  - properties attribute 또는 @TestPropertySource annotation을 사용하여 @SpringBootTest 가 추가한 test property source가 이제 command line property source 위에 추가되었습니다.
  - (동일한 property name을 사용한) properties 와 args 를 병행하여 @SpringBootTest 를 사용하였다면 변경해야 할 수 있습니다.

- New Flyway Modules
  - Spring Boot 2.7은 Flyway (8.0에서) 8.5로 upgrade 됩니다.
  - 이후에 많은 database에 대한 Flyway 지원이 신규 module로 추가되었습니다.
  - flyway-firebird (Firebird)
  - flyway-mysql (MariaDB and MySQL)
  - flyway-sqlserver (SQL Server)
  - Flyway를 사용하여 위 database 중 하나의 schema를 관리하는 경우 적절한 신규 module dependency를 추가하세요.

- H2 2.1
  - Spring Boot 2.7이 H2 2.1.120으로 upgrade 되었습니다.
  - H2 2.x는 이전 버전과 호환되지 않으며 여러 보안 취약점을 수정합니다.
  - 변경 사항 및 upgrade 처리 방법에 대한 자세한 내용은 H2 changelog 및 migration guide를 참조하세요.

- jOOQ
  - Java 8 및 H2 2.x와 호환되는 jOOQ의 오픈소스 버전은 없습니다.
  - Java 11을 사용하는 경우 jooq.version 속성을 사용하여 jOOQ 3.16 이상으로 업그레이드하는 것을 고려하세요.
  - Java 8을 사용 중이고 업그레이드할 수 없는 경우 jOOQ Professional Edition을 구입하거나 H2에서 다른 데이터베이스로 마이그레이션 하거나 최후의 수단으로 H2 1.4.x로 다운그레이드 하는 것을 고려하세요.

- Microsoft SQL Server JDBC Drive 10
  - Spring Boot 2.7은 MSSQL driver를 v9에서 v10으로 업그레이드했습니다.
  - 업데이트된 driver는 이제 기본적으로 기존 application을 손상시킬 수 있는 암호화를 활성화합니다.
  - 이 문서의 주요 변경 사항 섹션에서 변경 사항에 대해 읽을 수 있습니다.
  - 추천하는 조언은 서버에 신뢰할 수 있는 인증서를 설치하거나 encrypt=false 를 포함하도록 JDBC connection URL을 업데이트하는 것입니다.

- OkHttp 4
  - OkHttp 3가 더 이상 유지되지 않아 Spring Boot 2.7에서 OkHttp4로 upgrade 되었습니다.
  - 이 upgrade의 일환으로 OkHttp의 version을 제어하는 데 사용되는 property가 okhttp3.version 에서 okhttp.version 으로 변경되었습니다.
  - OkHttp4는 OkHttp3 이전 버전과 호환되도록 설계되었습니다.
  - application 사정상 또는 다른 이유로 OkHttp3를 계속 사용하려면 빌드에서 okhttp.version property를 설정하세요.

- Separate Dependency Management for netty-tcnative Removed
  - netty-tcnative 에 대한 별도의 dependency management가 Netty의 bom에서 제공하는 dependency management를 위해 제거되었습니다.
  - 이렇게 하면 netty-tcnative 의 version이 Netty가 기본적으로 사용하는 version과 맞춰지게 됩니다.
  - 이 변경으로 인해 더 이상 netty-tcnative.version property를 netty-tcnative 의 version을 override 하는 데 사용할 수 없습니다.
  - 따로 dependency management를 제공하여 version을 계속 무시할 수 있지만 Netty의 default version과 맞춰진 상태를 유지하는 것이 좋습니다.

- spring.mongodb.embedded.features Configuration Property Removed
  - Embedded Mongo 3.5이 Mongo feature 구성에 대한 지원을 중단했습니다.
  - 이를 반영하여 spring.mongodb.embedded.features configuration property가 제거되었습니다.
  - Mongo를 시작하는 데 사용되는 command line을 변경하기 위해 feature를 지정하는 advanced configuration의 경우 custom MongoConfig bean을 대신 제공해야 합니다.
  - Servlet-specific Mustache Properties
  - Servlet과 관련된 다음 Mustache 관련 property들이 deprecated 되었습니다.
```
spring.mustache.allow-request-override
spring.mustache.allow-session-override
spring.mustache.cache
spring.mustache.content-type
spring.mustache.expose-request-attributes
spring.mustache.expose-session-attributes
spring.mustache.expose-spring-macro-helpers

아래와 같이 대체되었습니다.

spring.mustache.servlet.allow-request-override
spring.mustache.servlet.allow-session-override
spring.mustache.servlet.cache
spring.mustache.servlet.content-type
spring.mustache.servlet.expose-request-attributes
spring.mustache.servlet.expose-session-attributes
spring.mustache.servlet.expose-spring-macro-helpers
```

- Default Indices Options on Auto-configured ReactiveElasticsearchTemplate
  - auto-configure 된 ReactiveElasticsearchTemplate 의 default indices option이 Spring Data Elasticsearch와 맞춰지도록 변경되었습니다.
  - 이전에는 기본값이 strictExpandOpenAndForbidClosed 였습니다.
  - 이제 strictExpandOpenAndForbidClosedIgnoreThrottled 입니다.
  - 이전 indices option을 복원하려면 따로 reactiveElasticsearchTemplate bean을 정의하세요.

- @Bean
```
ReactiveElasticsearchTemplate reactiveElasticsearchTemplate(ReactiveElasticsearchClient client,
        ElasticsearchConverter converter) {
    ReactiveElasticsearchTemplate template = new ReactiveElasticsearchTemplate(client, converter);
    template.setIndicesOptions(IndicesOptions.strictExpandOpenAndForbidClosed());
    return template;
}
```

- MongoDB Property Precedence
  - 이전에는 spring.data.mongodb.uri 가 spring.data.mongodb.host 및 spring.data.mongodb.port 같은 동등한 개별 property와 함께 구성된 경우 exception이 발생했습니다.
  - uri property는 이제 개별 property보다 우선합니다.
  - 즉, spring.data.mongodb.uri 가 설정되면 다른 것은 무시됩니다.
  - 이는 spring.redis.url 과 같은 다른 유사한 property의 동작과 일치합니다.

- Running Your Application in the Maven Process
  - Maven Plugin의 spring-boot:run 및 spring-boot:start goal이 default로 forked process로 application을 실행합니다.
  - plugin의 fork attribute를 사용하여 이 동작을 비활성화할 수 있습니다.
  - 이 attirbute는 대체 없이 deprecated 되었습니다.

- Ordered Exit Code Generators
  - ExitCodeGenerator 는 이제 Orderd 구현 및 @Order annotation을 기반으로 정렬됩니다.
  - 생성된 첫 번째 non-zero exit code가 사용됩니다.

- Metric Tag Keys Renamed
  - camelCase 로 있던 metric tag key가 모두 소문자 및 . 구분자를 사용하라는 Micrometer의 권장 사항을 준수하도록 변경되었습니다.
  - 다음 metric과 tag key가 변경됩니다.

- Metric	Old Tag Key	New Tag Key
```
application.ready.time	main-application-class	main.application.class
application.started.time	main-application-class	main.application.class
cache.*	cacheManager	cache.manager
http.client.requests	clientName	client.name
```
  - 이전 이름을 복원해야 하는 경우 tag key를 수정하기 위해 map(Id) method를 구현하는 MeterFilter bean을 정의하세요.

- Support for Elasticsearch’s RestHighLevelClient is Deprecated
  - Elasticsearch의 RestHighLevelClient가 deprecated 되었습니다.
  - 이에 맞춰 RestHighClient 에 대한 Spring Boot의 auto-configuration 도 deprecated 되었습니다.
  - 가능한 경우 auto-configure 된 low-level RestClient를 대신 사용해야 합니다.
  - 또는 새 client를 수동으로 구성하는 것이 좋습니다.

- R2DBC Driver Changes
  - Borca release에서 PostgreSQL용 driver인 r2dbc-postgresql의 group ID가 io.r2dbc 에서 org.postgresql 로 변경되었습니다.
  - MySQL용 driver인 r2dbc-mysql 이 제거되었습니다.
  - r2dbc-maridadb 로 교체하는 것을 고려하세요.

- Migrating From WebSecurityConfigurerAdapter to SecurityFilterChain
  - Spring Boot 2.7은 WebSecurityConfigurerAdapter 가 사용되지 않는 Spring Security 5.7로 업그레이드됩니다.
  - WebSecurityConfigurerAdapter 없이 Spring Security를 구성하고 @WebMvcTest 와 같은 Spring Boot의 slice test를 사용하는 경우 security configuration class @Import 를 통해 SecurityFilterChain bean을 테스트에 사용할 수 있도록 application을 변경해야 할 수 있습니다.
  - 자세한 내용은 reference documentation을 참조하세요.

- Building Jars With Maven Shade Plugin and Gradle Shadow Plugin
  - Spring Boot 2.7은 auto-configuration 및 management context class를 검색하는 방법을 변경했습니다.
  - 이제 각각 META-INF/spring/org.springframework.boot.autoconfigue.AutoConfiguration.imports 및 META-INF/spring.org.springframework.boot.actuate.autoconfigure.web.ManagementContextConfiguration.import 라는 이름의 파일에 선언됩니다.

- Configuration of Maven Shade Plugin
  - maven-shade-plugin 을 사용 중이고 spring-boot-starter-parent 에 의존하지 않는 경우 다음 AppendingTransformer configuration을 추가하세요.
```
<transformer implementation="org.apache.maven.plugins.shade.resource.AppendingTransformer">
  <resource>META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports</resource>
</transformer>
<transformer implementation="org.apache.maven.plugins.shade.resource.AppendingTransformer">
  <resource>META-INF/spring/org.springframework.boot.actuate.autoconfigure.web.ManagementContextConfiguration.imports</resource>
</transformer>
Configuration of Gradle Shadow Plugin
.import 파일을 추가하려면 다음 구성을 추가하십시오.

tasks.withType(ShadowJar).configureEach {
    append("META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports")
    append("META-INF/spring/org.springframework.boot.actuate.autoconfigure.web.ManagementContextConfiguration.imports")
}
```

- MIMEPull
  - org.jvnet.mimepull:mimepull 에 대한 dependency management가 제거되었습니다.
  - mimepull 에 대한 dependency를 선언한 경우 해당 선언에 맞는 버전을 추가하세요.

- Deprecations from Spring Boot 2.5
  - Spring Boot 2.5에서 deprecated 되었던 class, method, property들이 이번 relelase에서 제거되었습니다.
  - upgrade 하기 전에 deprecated 된 method를 호출하지 않는지 확인하세요.

- Minimum Requirements Changes
  - 없음

- New and Noteworthy
  - configuration에 대한 전체 변경 사항은 configuration changelog를 참고하세요.

- New Spring GraphQL starter
  - Spring Boot 2.7은 새로운 spring-boot-starter-graphql starter와 함께 Spring GraphQL project에 대한 지원을 제공합니다.
  - Spring Boot 참조 문서의 GraphQL section에서 더 많은 정보를 찾을 수 있습니다.

- Support for RabbitStreamTemplate
  - spring.rabbitmq.stream.name property를 사용하여 stream name을 설정하면 RabbitStreamTemplate 이 auto-configured 됩니다.
  - auto-configuration을 유지하면서 추가 instance를 customize 하기 위해 RabbitTemplateConfigurer 와 유사한 RabbitStreamTemplateConfigurer 가 제공됩니다.

- Hazelcast @SpringAware Support
  - auto-configure 된 Hazelcast embedded server는 이제 default로 SpringManagerContext 를 사용합니다.
  - 이것은 Hazelcast에 의해 instance화 된 object에 Spring managed bean을 주입하는 것을 가능하게 합니다.
  - HazelcastConfigCustomizer callback interface도 도입되었으며 Hazelcast server 구성을 추가로 조정하는 데 사용할 수 있습니다.

- Operating System Information in Info endpoint
  - OsInfoContributor 는 application이 실행 중인 운영 체제에 대한 일부 정보를 노출할 수 있습니다.
```
{
  "os": {
    "name": "Linux",
    "version": "5.4.0-1051-gke",
    "arch": "amd64"
  }
}
이 신규 contributor는 default로 비활성화되어 있습니다.
management.info.os.enabled property를 사용하여 활성화할 수 있습니다.
```

- Java Vendor Information in Info endpoint
  - 기존 JavaInfoContributor 는 vendor 별 버전을 포함하여 vendor 정보에 대한 전용 section을 제공하도록 개선되었습니다.
  - top-level vendoer 단순 attribute가 아니라 이제 name 과 version attribute가 있는 전용 object입니다.
```
{
  "java": {
    "vendor": {
       "name": "Eclipse Adoptium",
        "version": "Temurin-17.0.1+12"
    },
    "..."
}
모든 vendor가 java.vendor.version system property를 노출하는 것이 아니므로 version attribute가 null 일 수 있습니다.
```

- Accessing the Authenticated Principal in RSocket Handler Methods
  - RSocket handler method가 이제 @Authenticated Princial 을 inject 할 수 있습니다.


- Opaque Token Introspection Without the OIDC SDK
  - OAuth2 resource server에서 불투명 token 검사를 사용하는 경우 auto-configure 된 introspector는 더 이상 com.nimbusds:oauth2-oidc-sdk 에 대한 dependency를 필요로 하지 않습니다.
  - SDK의 다른 용도에 따라 application에서 dependency를 제거할 수 있습니다.

- @DataCouchbaseTest
  - Spring Data Couchbase를 사용하는 application test를 위한 신규 @DataCouchbaseTest annotation이 도입되었습니다.
  - 자세한 내용은 업데이트된 reference documentation을 참조하세요.

- @DataElasticsearchTest
  - Spring Data Elasticsearch를 사용하는 application 테스트를 위한 신규 @DataElasticsearchTest annotation이 도입되었습니다.
  - 자세한 내용은 업데이트 된 reference documentation을 참조하세요.

- Auto-configuration for SAML2 logout
  - Spring Security의 SAML2 지원을 사용하는 경우 configure property 들을 통해 RP-initiated 또는 AP-initiated logout을 구성할 수 있습니다.
  - 자세한 내용은 업데이트 된 reference documentation을 참조하세요.

- Changes to Auto-configuration
  - Auto-configuration Registration
  - 자체 auto-configuration을 사용하는 경우 등록을 spring.factories 에서 META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports 파일로 이동해야 합니다.
  - 각 줄에는 auto-configuration의 완전한 이름이 포함됩니다.
  - 예제는 included auto-configuartion을 참조하세요.
  - 이전 버전과의 호환성을 위해 spring.factories 의 항목은 여전히 적용됩니다.

- New @AutoConfiguration Annotation
  - 신규 @AutoConfiguration annotation이 도입되었습니다.
  - @Configuration 을 대체하여 새로운 META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.import 파일에 나열된 top-level auto-configuration class에 annotation을 달아야 합니다.
  - 편의를 위해 @AutoConfiguration 은 after, afterNames, before 및 beforeNames 속성을 통한 auto-configuration ordering도 지원합니다.
  - 이는 @AutoConfigureAfter, @AutoConfigureBefore의 대체하여 사용할 수 있습니다.

- Test Slice Configuration
  - 자체 test-slice 를 생성했다면 등록을 spring.factories 에서 META-INF/spring/<테스트 슬라이스 주석의 이름>.import 아래로 옮겨야 합니다.
  - format은 "Auto-configuration Registration" section에서 설명한 새 파일과 동일합니다. (이전 섹션 참조)
  - 예제는 included test slices를 참조하세요.

- FailureAnalyzer Injection
  - FailureAnalyzer 구현은 이제 이러한 값 중 하나 또는 둘 모두를 parameter로 사용하는 constructor를 제공하여 현재 application context의 BeanFactory 및 Environment 에 access 할 수 있습니다.
  - FailureAnalyzer 에서 BeanFactoryAware 를 구현하여 Beanfactory 를 주입하고 EnvironmentAware 를 구현하여 Environment 를 주입하는 지원은 deprecated 되었으며 향후 release에서 제거됩니다.

- Redis Sentinel Username Support
  - spring.redis.sentinel.username property를 사용하여 sentinel에 대한 인증을 위한 사용자 이름 지정 지원이 추가되었습니다.

- Overriding Built-in Sanitization
  - SanitizingFunction bean은 이제 순서대로 호출되며 function이 SantizableData 값을 변경하면 중지됩니다.
  - SanitizingFunction bean이 값을 삭제하지 않으면 build-in key-based sanitization이 수행됩니다.
  - function은 @Order annotation 또는 Ordered 구현을 통해 정렬됩니다.

- Docker Image Building
  - Podman Support
  - Maven 및 Gradle plugin은 이제 Cloud Native Buildpack을 사용하여 image를 build 할 때 Docker engine의 대안으로 Podman container engine 사용을 지원합니다.
  - 자세한 내용은 업데이트된 Gradle 및 Maven reference documentation을 참조하세요.

- Cache2k Support
  - Check2k 에 대한 dependency management 및 auto-configuration이 추가되었습니다.
  - 기본 cache 설정은 Check2kBuilderCustomizer bean을 정의하여 customize 할 수 있습니다.

- Simplified Registration of Jackson Mixins
  - Jackson에 대한 auto-configuration은 이제 @JsonMixin 으로 annotation이 달린 class에 대해 application package를 scan 합니다.
  - 발견된 모든 class는 auto-configuration 된 ObjectMapper 를 사용하여 Mixin으로 자동 등록됩니다.

- Web Server SSL Configuration Using PEM-encoded Certificates
  - property server.ssl.certificate 및 server.ssl.certificate-private-key 와 optinal server.ssl.trust-certificate 및 server.ssl.trust-certificate-private-key 를 사용하여 PEM-encoded certificate 및 private key file과 함께 SSL을 사용하도록 Embedded web server를 구성할 수 있습니다.
  - Management endpoint는 유사한 management.server.ssl.* property 들을 사용할 수 있습니다.
  - 예제는 documentation을 참조하세요.
  - 이것은 Java KeyStore file로 SSL을 구성하는 대신 제공됩니다.

- Dependency Upgrades
  - Spring Boot 2.7에서 여러 Spring project가 새 버전으로 변경되었습니다.
```
Spring Data 2021.2
Spring HATEOAS 1.5
Spring LDAP 2.4
Spring Security 5.7
Spring Session 2021.2
수많은 타사 dependency 도 업데이트되었으며 그중 주목할 만한 것은 다음과 같습니다.

Elasticsearch 7.17
Flyway 8.5
H2 2.1
Infinispan 13
Json 2.9
Json Path 2.7
Kafka 3.1
MariaDB 3.0
Micrometer 1.9
MongoDB 4.5
OkHTTP 4.9
REST Assured 4.5
R2DBC Borca
Miscellaneous

위에 나열된 변경 사항 외에도 다음과 같은 사소한 조정 및 개선 사항이 많이 있습니다.

spring.kafka.listener.idle-partition-event-interval property를 사용하여 kafka idlePartitionEventInterval 을 구성할 수 있습니다.
spring.kafka.template.transaction-id-prefix property를 사용하여 KafkaTemplate transactionIdPrefix property를 설정할 수 있습니다.
server.netty.max-keep-alive-requests property를 사용하여 Netty maxKeepAliveRequests 를 구성할 수 있습니다.
@DataJdbcTest 는 AbstractJdbcConfiguration bean을 자동으로 scan 합니다.
SAML 2.0 login을 사용할 때 UserDetailService bean은 더 이상 auto-configure 되지 않습니다.
spring.batch.jdbc.isolation-level-for-create property를 사용하여 Spring Batch의 transaction isolation을 구성할 수 있습니다.
Spring MVC metric을 기록하는데 사용되는 filter는 이제 자신의 FilterRegistrationBean<WebMvcMetricsFilter> bean을 정의하여 교체할 수 있습니다.
DatabaseDriver.MARIADB 의 ID가 mysql에서 mariadb 로 변경되었습니다.
spring-boot-loader 에서 RandomAccessDataFile 에 의해 반환된 InputStream 은 이제 available() 을 구현합니다.
Spring Kafka의 immediateStop 은 spring.kafka.listener.immediate-stop property를 사용하여 구성할 수 있습니다.
새로운 property spring.mustache.reactive.media-types 는 reactive mustache view에서 지원하는 media type을 구성하는데 사용할 수 있습니다.
Elasticsearch RestClientBuilder 및 RestClient bean은 이제 elasticsearch-rest-cilent 가 classpath에 있을 때 auto-configure 됩니다.
elasticsearch-rest-high-level-client 가 classpath에 있을 경우에도 RestHighLevelClient bean이 이전과 같이 auto-configure 되지만 RestHighLevelClient 에 대한 지원은 더 이상 사용되지 않습니다.
Deprecations in Spring Boot 2.7
spring.factories 에서 auto-configuration을 로드하는 것은 deprecated 되었습니다.
자세한 내용은 위를 참조하세요.
DatabaseDriver.GAE
spring.security.saml2.relyingparty.registration.{id}.identityprovider 아래의 property가 spring.security.saml2.relyingparty.registration.{id}.assertingparty 로 이동되었습니다.
  

  
<br>
<br>
<br>
[참고](https://luvstudy.tistory.com/180)
