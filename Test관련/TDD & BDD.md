# TDD(Test-driven development)

![image](https://user-images.githubusercontent.com/74396651/194557709-1283dc4c-8685-4634-996b-3099676df2c6.png)
> &nbsp; TDD는 테스트를 먼저 작성하고 이를 통과하기 위한 행동들이 개발을 주도하는 것을 목표로 한다.
> TDD는 Kent Beck이 고안해낸 방법론이다!

<br>

# 테스트 코드 작성의 이점
- 단위 테스트는 개발단계 초기에 문제를 발견하게 도와준다.
- 단위 테스트는 개발자가 나중에 코드를 리팩토링하거나 라이브러리 업그레이드 등에서 기존 기능이 올바르게 작동하는지 확인할 수 있다.(ex, 회귀 테스트)
- 단위 테스트는 기능에 대한 불확실성을 감소시킬 수 있다.
- 단위 테스트는 시스템에 대한 실제 문서를 제공한다. 즉, 단위 테스트 자체가 문서로 사용할 수 있다.

<br>

# ETC
- 패키지명
   - 일반적으로 패키지명은 웹 사이트 주소의 역순으로한다.
   - ex) (웹사이트)admin.example.com --> com.example.admin(패키지) 으로 하면 된다.


<br>
<hr>
<br>

# BDD, Behavior Driven Development
![image](https://user-images.githubusercontent.com/74396651/201592241-90b7d275-96d5-4d7a-9ea8-4decb022047c.png)

> &nbsp; BDD는 전혀 새로운 방법이 아니다. Danial Terhorst-North와 Matts가 착안한 방법론으로 BDD의 모든 근간은 TDD에서 착안되었기 때문이다. TDD를 하다 해당 코드를 분석하기 위해 많은 코드들을 분석해야하고 복잡성으로 인해 '누군가가 나에게 이 코드는 어떤 방식으로 짜여졌어! 라고 말해줬으면 좋았을 텐데' 라는 생각을 하다가 행동 중심 개발을 하면 좋겠다고 생각했다.

<br>

### BDD는 애플리케이션이 어떻게 행도해야 하는지에 대한 공통된 이해를 구성하는 방법이다.
- Narrative
> &nbsp;모든 테스트 문장은 Narrative 하게 되어야 한다. 즉, 코드보다 인간의 언어와 유사하게 구성되어야 한다. TDD는 사실 테스트 코드를 이용한 구현에 초점이 맞춰져 있다. 하지만 BDD는 행동과 기능을 개발자가 더 쉽게 이해하게 만드는 것이 목적이다.

<br>

### 모든 테스트 문장은 Given / When / Then 으로 나눠서 작성할 수 있어야 한다.
- Given
   - 테스트를 위해 주어진 상태
   - 테스트 대상에게 주어진 조건
   - 테스트가 동작하기 위해 주어진 환경  
- When
   - 테스트 대상에게 가해진 어떠한 상태
   - 테스트 대상에게 주어진 어떠한 조건
   - 테스트 대상의 상태를 변경시키기 위한 환경 
- Then
   - 앞선 과정의 결과 

<br>

### 즉, 어떤 상태에서 출발(given)하여 상태의 변화를 가했을 때(when) 어떤 상태가 되어야 한다.(then)

<br>

## TDD와 BDD의 비교

![image](https://user-images.githubusercontent.com/74396651/201593644-2d25402c-b71f-480a-9c21-bf9ad9a798d6.png)

<br>

## BDDMockito(Mockito 라이브러리 있는 BDD 지향 개발 api)
