# DTO의 사용 범위
> 이제 JPA를 사용하다보니...proxy, Lazy Loading 등 transation 범위가 중요하다는 것을 느끼고... 고민하게 되었다....후음... OSIV를 키면 되겠지만.. 성능이...이제 정리를 시작해보자.

<br>

## DTO(Data Transfer Object)
> DTO란 레이어(계층)간 데이터 교환을 위해 사용하는 객체(Java Bean)이다. Spring MVC 패턴을 사용한다면 Controller, Service, Repository 레이어가 있다는 것을 알 것이고! DDD 아키텍쳐를 사용한다면...후응 이것도 공부해야하는데.... 아무튼 레이어에서 혹은 레이어 사이사이에서 단순 데이터를 담은 객체라고 생각하면 편하다.

![image](https://user-images.githubusercontent.com/74396651/208408992-a816480d-b09f-456b-9242-fa02dbcf95f3.png)

![image](https://user-images.githubusercontent.com/74396651/208409548-50bcdbc3-0d70-4018-bec6-9251e08a19a0.png)

<br>

## MVC 패턴
> MVC 패턴은 Model, View, Controller 등 세가지 역할로 구분하는 디자인 패턴이다. 비즈니스 처리 로직 영역(Model), UI 영역(View)은 서로 알지 못하며 Controller가 중간에서 다리 역할을 한다.MVC 패턴의 장점은 Model과 View를 분리함으로써 서로의 의존성을 낮추고 독립적인 개발을 가능하게 한다. Controller는 View와 도메인 Model의 데이터를 주고 받을 때 별도의 DTO 를 주로 사용합니다. 도메인 객체를 View에 직접 전달할 수 있지만, 민감한 도메인 비즈니스 기능이 노출될 수 있으며 Model과 View 사이에 의존성이 생기기 때문이다. 

![image](https://user-images.githubusercontent.com/74396651/208410805-7760efb9-3aa8-497b-9d21-587f2965e093.png)


<br>

### 도메인 Model을 그대로 response할 경우
- 도메인 Model의 모든 속성이 외부에 노출된다. 이는 민감한 정보가 노출되는 보안 문제와 직결된다!
- UI 계층에서 Model의 메서드를 호출하거나 상태를 변경시킬 위험이 존재한다.
- Model과 View의 의존도가 높아져 둘 중 하나의 변경사항에 따라 영향을 끼치는 불상사가 발생한다.

> 그렇기 때문에 DTO를 따로 만들어줘야 이런 불상사가 일어나지 않는다!!
<br>
<hr>
<br>

## Layered Architecture

![image](https://user-images.githubusercontent.com/74396651/208410805-7760efb9-3aa8-497b-9d21-587f2965e093.png)

> MVC 패턴에서 Controller가 도메인 Model 객체들의 조합을 통해 프로그램의 작동 순서나 방식을 제어하는데, 어플리케이션의 규모가 커진다면 Controller는 중복되는 코드가 많아지고 비대해질 것이다.
> Layered Architecture는 유사한 관심사들을 레이어로 나눠서 추상화하여 수직적으로 배열하는 아키텍처이다. 하나의 레이어는 자신에게 주어진 고유한 역할을 수행하고, 인접한 다른 레이어와 상호작용합니다. 이렇게 시스템을 레이어로 나누면 시스템 전체를 수정하지 않고도 특정 레이어를 수정 및 개선할 수 있어 재사용성과 유지보수에 유리할 것이다.

## DTO의 변환 위치 
> request, response할 때 DTO를 이용하는 것을 알았다. 그렇다면 DTO를 domain 모델과 어디서 변환해야하는지 고민이 생길 수 밖에 없다! 대부분 DTO와 Domain간의 변환 위치를 Controller(표현계층)에서 수행하는데 꼭 이렇게 해야하는지에 대한 의문이 생길 수 있다. 

## 나의 해결책
> domain은 철저히 도메인 영역에서만 사용 및 노출해야지 그 외 영역에서 도메인을 노출시키는 건 위험하다고 생각한다. 
- Service에서 넘어오는 도메인 객체는 View에 필요없는 정보까지 같이 넘어오기 때문에 위험하다.
- 마틴 파울러 Service 레이어란 어플리케이션의 경계를 정의하고 "도메인을 캡슈로하 하는 역할"이라고 정의했다. 즉 서비스 레이어란 도메인을 보호하는 레이어라는 뜻이다. 도메인 모델이 Presentation layer까지 노출된다면 Controller의 변경을 촉발하는 문제로 흘러갈 수도 있고 API라면 API 스펙이 달라지는 문제로 흘러갈 수도 있고 무엇보다 목적에 어긋난다.

> 따라서 나의 경우는 Service에서 domain -> dto로 변환을 해 controller에 넘겨주는 것이 안전하다고 생각한다. 하지만 controller와 service 사이에 Mapper클래스를 만들어 Mapper에서 변환해주어도 상관없다! [Mapper 라이브러리](https://its-ward.tistory.com/entry/Spring-DTO%EC%99%80-Mapper)




