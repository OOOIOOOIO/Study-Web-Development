> AWS로 배포하면서 항상 헷갈리는 부분...

# VPN(Virtual Private Network)란

<img width="648" alt="image" src="https://user-images.githubusercontent.com/74396651/233285284-cf95dac5-7a6f-4447-83ce-45114b2dc72f.png">

VPN은 한국어로 "가상 사설망"이라고 한다. 가상이라는 단어에서 알 수 있듯 실제 사설망이 아닌 가상의 사설망이다. 만약 위 그림과같이 회사의 네트워크가 구성되어있고 보안상의 이유로 직원간 네트워크를 분리하고 싶다면 기존 인터넷선 선공사도 다시해야하고 건물의 내부선을 다 뜯어고쳐야하며 다시 전용선을 깔아야 한다. 이를 위해 가상의 망, VPN을 사용하게 된다.

<br>

<img width="711" alt="image" src="https://user-images.githubusercontent.com/74396651/233285501-2ac92cdc-a707-4ba8-8a48-e9fef73e0ad2.png">

네트워크A와 네트워크B가 실제로 같은 네트워크상에 있지만 논리적으로 다른 네트워크인 것처럼 동작하는 것을 VPN이라고 한다.

<hr>

# VPC(Virtual Private Cloud)
VPC는 독립적인 가상 네트워크 공간으로 사용자의 설정에 따라 자유롭게 구성할 수 있는 공간이다.
### VPC가 없는 구조

<img width="614" alt="image" src="https://user-images.githubusercontent.com/74396651/233285985-e9c828f4-f1b7-47c1-86ff-6cd32235e577.png">

VPC가 없다면 EC2 인스턴스들이 서로 거미줄처럼 연결되고 인터넷과 연결된다. 이런 구조는 시스템의 복잡도를 엄청나게 끌어올릴 뿐만 아니라 하나의 인스턴스만 추가되도 모든 인스턴스를 수정해야하는 불편함이 생긴다. 마치 인터넷 전용선을 다시 까는 것과 같습니다.

### VPC를 적용한 구조

<img width="656" alt="image" src="https://user-images.githubusercontent.com/74396651/233286473-d89590d8-2c84-4317-abb8-771522bb30d9.png">

VPC를 적용하면 위 그림과같이 VPC별로 네트워크를 구성할 수 있고 각각의 VPC에따라 다르게 네트워크 설정을 줄 수 있다. 또한 각각의 VPC는 완전히 독립된 네트워크처럼 작동하게 된다.

## VPC를 구축하는 과정

<img width="675" alt="image" src="https://user-images.githubusercontent.com/74396651/233287124-8d655196-7281-41f9-ae03-26ad3a0fa7a6.png">

VPC를 구축하기위해서는 VPC의 아이피범위를 RFC1918이라는 사설아이피대역에 맞추어 구축해야 한다. 사설아이피란 인터넷을 위해 사용하는것이 아닌 우리끼리 사용하는 아이피주소 대역이다.
> 누군가 “안방에서 리모컨좀 가져다달라”하면 저는 옆집을 가는게 아닌 우리집에 있는 “안방”으로 찾아갈 것이다. 안방이 프라이빗아이피(사설아이피) 우리집 주소가 퍼블릭아이피이다.

안방”, “작은방”, “큰방”처럼 내부에서 쓰는 주소를 사설아이피 대역이라고 부르며 내부 네트워크내에서 위치를 찾아갈 때 사용한다. 물론 내 친구나 혹은 동생의 친구가 집으로 찾아올때는 도로명주소(퍼블릭아이피)를 알려주면 되고 같은집(우리집)에사는(동일한 네트워크 상 존재하는) 내가 동생에게 갈 때는 동생방(사설아이피)로 찾아가면 된다.

- VPC에서 사용하는 사설 아이피 대역
  - 10.0.0.0 ~ 10.255.255.255(10/8 prefix)
  - 172.16.0.0 ~ 172.31.255.255(182.16/12 prefix)
  - 192.168.0.0 ~ 192.168.255.255(192.168/16 prefix)

한번 설정된 아이피대역은 수정할 수 없으며 각 VPC는 하나의 리전에 종속된다. 각각의 VPC는 완전히 독립적이기때문에 만약 VPC간 통신을 원한다면 VPC 피어링 서비스를 고려해볼 수 있다.

### 서브넷
<img width="717" alt="image" src="https://user-images.githubusercontent.com/74396651/233289533-49330ee5-588a-4120-9685-286cc6fe3ae2.png">

VPC를 만들었다면 이제 서브넷을 만들 수 있다. 서브넷은 VPC를 잘개 쪼개는 과정이다. 서브넷은 VPC안에 있는 VPC보다 더 작은단위이기때문에 연히 서브넷마스크가 더 높게 되고 아이피범위가 더 작은값을 갖게 된다. 서브넷을 나누는 이유는 더 많은 네트워크망을 만들기 위해서
이다.
> 각각의 서브넷은 가용영역안에 존재하며 서브넷안에 RDS, EC2와같은 리소스들을 위치시킬 수 있다.

### 라이팅 테이블과 라우터

<img width="720" alt="image" src="https://user-images.githubusercontent.com/74396651/233290072-a6f7f0e9-9636-47f7-b7a5-8eadb3dc18dd.png">

네트워크 요청이 발생하면 데이터는 우선 라우터로 향하게 된다. 라우터란 목적지이고 라우팅테이블은 각 목적지에 대한 이정표이다. 데이터는 라우터로 향하게되며 네트워크 요청은 각각 정의된 라우팅테이블에 따라 작동한다. 서브넷A의 라우팅테이블은 172.31.0.0/16 즉 VPC안의 네트워크 범위를 갖는 네트워크 요청은 로컬에서 찾도록 되어 있다. 또한 그 이외 외부로 통하는 트래픽을 처리할 수 없습니다. 이때 인터넷 게이트웨이를 사용한다.

### 인터넷 게이트웨이

<img width="636" alt="image" src="https://user-images.githubusercontent.com/74396651/233290773-cf852b86-f855-4708-adbe-785b85fcf03d.png">

인터넷게이트웨이는 VPC와 인터넷을 연결해주는 하나의 관문이다. 서브넷B의 라우팅테이블을 잘보면 0.0.0.0/0으로 정의되어있습니다. 이뜻은 요청이 들어오는 모든 트래픽에 대하여 IGA(인터넷 게이트웨이) A로 향하라는 뜻이다. 라우팅테이블은 가장 먼저 목적지의 주소가 172.31.0.0/16에 매칭되는지를 확인한 후 매칭되지 않는다면 IGA A로 보낸다.
- 인터넷과 연결되어 있는 서브넷을 퍼블릭 서브넷
- 인터넷과 연결되어 있지 않은 서브넷을 프라이빗 서브넷이라고 한다.

### 네트워크 ACL과 보안그룹

<img width="729" alt="image" src="https://user-images.githubusercontent.com/74396651/233292201-6a3726e4-4ee7-4bba-b48d-3d30ad4d48b3.png">

- 보안그룹
네트워크 ACL과 보안그룹은 방화벽과 같은 역활을 하며 인바운드 트래픽과 아웃바운드 트래픽 보안정책을 설정할 수 있다. 먼저 보안그룹은 Stateful한 방식으로, 동작하는 보안그룹은 모든 허용을 차단하도록 기본 설정이 되어 있으며 필요한 설정은 따로 허용해주어야 한다. 또한 보안그룹은 네트워크 ACL과 다르게 각각 별도의 트래픽을 설정할 수 있으며 서브넷에도 설정할 수 있으며 각각의 EC2인스턴스에도 적용할 수 있습니다.

- 네트워크 ACL
네트워크 ACL은 Stateless하게 작동하며 모든 트래픽을 기본 설정이 되어 있기 때문에 불필요한 트래픽을 막도록 적용해야 한다. 서브넷단위로 적용되며 리소스 별로는 설정할 수 없습니다. 네트워크 ACL과 보안그룹이 충돌한다면 보안그룹이 더 높은 우선순위를 갖는다.

### NAT 게이트웨이

<img width="751" alt="image" src="https://user-images.githubusercontent.com/74396651/233293545-f78e99c1-bd28-43c3-bfbd-5d7589ec8103.png">

NAT 게이트웨이는 프라이빗서브넷이 인터넷과 통신하기위한 아웃바운드 인스턴스이다. 프라이빗 네트워크가 외부에서 요청되는 인바운드는 필요없더라도 인스턴스의 펌웨어나 혹은 주기적인 업데이트가 필요하여 아웃바운드 트래픽만 허용되야할 경우가 있다. 이때 퍼블릭 서브넷상에서 동작하는 NAT 게이트웨이는 프라이빗서브넷에서 외부로 요청하는 아웃바운드 트래픽을 받아 인터넷게이트웨이와 연결한다.

<hr>

## 마무리
VPC의 목적은 다양할 수 있지만 일반적으로 보안을 위해 AWS 리소스간 허용을 최소화하고 그룹별로 손쉽게 네트워크를 구성하기위해 많이 사용한다. 이외에도 독립적인 VPC간 네트워크 통신을 위한 VPC 피어링, 기존 사용하는 온프레미스와 VPC를 연결하는 AWS Diarect Connect, VPC에서 발생하는 로그를 기록하는 VPC FLow Logs와같은 서비스가 있다.

<hr>

## VPN 추가 연결 옵션

### VPC Peering Connection(다른 VPC와 연결)

<img width="797" alt="image" src="https://user-images.githubusercontent.com/74396651/233294838-ea19a47f-e2fb-4a47-a80c-7b8ace5346cb.png">

다른 VPC와 연결하기 위해서는 VPC Peering은 두 VPC 간에 트래픽을 라우팅할 수 있도록 서로 다른 VPC 간의 네트워크 연결을 제공한다.
동일한 네트워크에 속한 것처럼 서로 다른 VPC의 인스턴스 간의 통신이 가능하다. 다른 Region, 다른 AWS 계정의 VPC Peering 연결도 가능합니다. 단, 연결할 VPC 간의 IP 범위가 겹치는 것은 불가능하다.

> VPC Peering 연결 순서는 아래와 같습니다.<br>
> 피어링 요청 → 피어링 요청 수락 → Route table 및 Security group 내 피어링 경로 업데이트

<img width="790" alt="image" src="https://user-images.githubusercontent.com/74396651/233295444-43ff2835-f1c7-4563-8d75-7998fda42490.png">

VPC Peering의 또 다른 특징으로는 전이성 피어링이 불가능하다는 것이다.

> VPC A – VPC B, VPC B – VPC C 간의 피어링 연결이 되어있다고 해도 VPC A – VPC C 간의 연결은 안 된다는 의미.<br>
> 따라서 VPC A와 VPC C와의 연결을 원할 경우 따로 피어링 연결을 해주어야 합니다.


## On-Premise(회사)와 연결(VPN, DX)

<img width="795" alt="image" src="https://user-images.githubusercontent.com/74396651/233296497-d8f50008-039d-412d-99bd-4e4bd7455d33.png">

- AWS Client VPN : VPN 소프트웨어 클라이언트를 사용하여 사용자를 AWS 또는 온프레미스 리소스에 연결하는 방식이다.
- AWS Site-to-Site VPN : 데이터 센터와 AWS Cloud(VPN Gateway / Transit Gateway) 사이에 암호화된 보안 연결 생성하는 방식이다.
- AWS Direct Connect : AWS Cloud 환경으로 인터넷이 아닌 전용 네트워크 연결 생성하는 방식이다. (※AWS 리소스에 대한 최단 경로)

기존 온프레미스와의 연결을 통해 하이브리드 환경 구성이 가능하다.



<br>
<br>
<br>
<br>

[여기 짱](https://medium.com/harrythegreat/aws-%EA%B0%80%EC%9E%A5%EC%89%BD%EA%B2%8C-vpc-%EA%B0%9C%EB%85%90%EC%9E%A1%EA%B8%B0-71eef95a7098)
[굿](https://blog.kico.co.kr/2022/03/08/aws-vpc/)







