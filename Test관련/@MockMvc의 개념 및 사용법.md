# @MockMvc
> &nbsp; MockMvc란 Spring MVC 테스트 유틸리티 클래스이다. Controller 테스트 코드를 작성하지 않는다면, PostMan 혹은 Request를 발생시킬 수 있는 도구를 사용해 직접 호출해 서버를 디버깅해야 한다. 하지만 MockMvc를 이용할 경우 편하게 테스트할 수 있다.

<br>

## @SpringBootTest or @WenMvcTest
> &nbsp;찾아보니 @WebMvcTest와 @SpringBootTest 어노테이션을 사용하는 두 가지 세팅 방법이 존재한다. @SpringBootTest는 ApplicationContext 전체(모든 bean)을 불러온다. 즉, Ben으로 등록된 객체를 모두 메모리에 올린다. <br><br> &nbsp;@WebMvcTest는 테스트에 필요한 레이어만 지정할 수 있다. 즉, 테스트에 필요한 레이어만 지정할 수 있다. 즉 테스트에 필요한 Bean을 직접 세팅해 사용한다.

<br>



https://velog.io/@jkijki12/Spring-MockMvc


흠ㅎ므
