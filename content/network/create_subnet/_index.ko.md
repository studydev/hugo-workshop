---
date: 2020-06-30
title: "VPC에서 추가 서브넷 생성"
weight: 120
pre: "<b>1-2. </b>"
---

{{% notice note %}}
고가용성 및 Fault Tolerant한 네트워크 서비스 구성을 위해, VPC의 Availability Zone C를 사용하겠습니다. 여기에 퍼블릭, 프라이빗 서브넷을 하나씩 더 추가하고, 라우팅 테이블을 설정하겠습니다.
{{% /notice %}}

### 다른 가용영역(az-northeast-2c)에 퍼블릭 서브넷 생성
VPC 콘솔 왼쪽 화면의 **Subnet** 을 클릭하고, 상단의 **Create subnet** 을 클릭합니다.  
**Create Subnet** 란에서 Name tag, VPC, Availability Zone, IPv4 CIDR block을 아래의 값으로 지정해 줍니다.

| 키 | 값 |
|----------|--------------------|
| Name tag | `Public subnet C` |
| VPC | VPC란을 클릭해서 `VPC-Lab` 태그가 설정된 VPC를 선택합니다. |
| Availability Zone | `ap-northeast-2c` |
| IPv4 CIDR block | `10.0.20.0/24` |

우측 하단의 **Create** 버튼을 누르고 `Puiblic subnet C` 서브넷을 생성합니다.

### 다른 가용영역(az-northeast-2c)에 프라이빗 서브넷 생성
마찬가지로 프라이빗 서브넷인 `Private subnet C` 도 생성해 보겠습니다.

| 키 | 값 |
|----------|--------------------|
| Name tag | `Private subnet C` |
| VPC | VPC란을 클릭해서 `VPC-Lab` 태그가 설정된 VPC를 선택합니다. |
| Availability Zone | `ap-northeast-2c` |
| IPv4 CIDR block | `10.0.200.0/24` |

**Create** 버튼을 눌러 `Private subnet C` 도 생성합니다.

### 생성된 서브넷 확인
VPC의  **Subnet** 메뉴를 클릭하고, 필터에 `VPC-Lab` 을 입력하면 아래와 같이 4개의 서브넷을 볼 수 있습니다.

### 라우팅 테이블 수정
**Routing Table** 을 선택하면 생성된 모든 라우팅 테이블을 확인할 수 있으며, 라우팅 테이블을 변경하거나 해당 라우팅 테이블에 Subnet을 연결(Association)하는 작업을 할 수 있습니다. 여기서는 새로 생성한 두 개의 서브넷에 적합한 라우팅 테이블을 연결해 주겠습니다.

먼저, VPC 메뉴 좌측의 **Route Tables** 을 클릭합니다. 가운데 검색 창을 클릭 후 리소스 속성에서 **VPC** 를 선택하여, 방금 생성한 **VPC ID** 를 찾아 필터를 겁니다.
  
필터링이 되고 나면 두 개의 라우팅 테이블이 보일 겁니다. 일단 **Explicit subnet associations** 에 서브넷이 연결되어 있는 라우팅 테이블부터 살펴보겠습니다. 해당 라우팅 테이블을 클릭하면, 아래에 세부 사항이 표시됩니다.
 
**Routes** 탭을 눌러, 이 라우팅 테이블의 설정을 확인하겠습니다.
 
*목적지가 VPC 내부(10.0.0.0/16)인 경우 로컬 게이트웨이(local)로* 트래픽을 라우팅하고, 그 외 다른 *모든 목적지(0.0.0.0/0)의 트래픽을 인터넷 게이트웨이(igw-xxx)로* 보내는 라우팅 테이블입니다. 인터넷과 바로 통신이 가능한 라우팅 구성이므로, 퍼블릭 서브넷들에 적용되어야 하는 라우팅 테이블이네요. **Subnet Association** 탭을 눌러 보겠습니다.

서브넷 연결을 확인해 보니, *10.0.10.0/24* 의 주소 공간을 갖는 *Public subnet A* 만 해당 라우팅 테이블에 연결되어 있는 것을 볼 수 있습니다. 우리가 새로 만든 *Public subnet C* 역시 해당 라우팅 테이블의 규칙을 따라 0.0.0.0/0 으로의 트래픽을 인터넷 게이트웨이로 보내야 합니다. **Edit subnet associations** 을 눌러 *Public Subnet C* 도 해당 라우팅 테이블에 연결해 보겠습니다.

**Edit subnet associations** 창의 가운데 서브넷 ID란을 보면, Public subnet C가 선택되지 않은 것을 보실 수 있습니다. **Public subnet C 의 좌측 체크박스** 를 눌러 연결 설정을 해 준 뒤, 우측 하단의 **Save** 버튼을 누릅니다.
 
이제 해당 라우팅 테이블에 Public subnet A, C가 연결된 것을 확인할 수 있습니다. 추후 혼선을 방지하기 위해, 라우팅 테이블의 **Name** 을 눌러 **Public route** 라고 라우팅 테이블에 이름을 붙여 주겠습니다.
 

이제 프라이빗 서브넷들을 위한 라우팅 테이블을 수정해 보겠습니다. 현재 보이는 두 개의 라우팅 테이블 중, **이름이 없는 라우팅 테이블** 을 클릭하고 **라우트(Routes)** 탭을 선택합니다.

*목적지가 VPC 내부(10.0.0.0/16)인 경우 로컬 게이트웨이(local)로* 트래픽을 라우팅하고, 그 외 다른 *모든 목적지(0.0.0.0/0)의 트래픽을 NAT 게이트웨이(nat-xxx)* 로 보내는 라우팅 테이블입니다. NAT 게이트웨이를 사용하도록 한 서브넷이므로 프라이빗 서브넷들에 적용되어야 하는 라우팅 테이블이네요. **Subnet Associations** 탭을 눌러 보겠습니다.

서브넷 연결을 확인해 보니 아무 서브넷도 연결되어 있지 않네요. **Edit subnet associations** 을 눌러 *Private subnet A, C* 를 해당 라우팅 테이블에 연결하겠습니다.
 

앞서 퍼블릭 서브넷을 연결했던 것과 마찬가지로, Private subnet A와 C의 **체크 박스** 를 클릭하고 우측 하단 **Save** 버튼을 누릅니다. 프라이빗 서브넷 2개가 해당 라우팅 테이블에 잘 연결되었는지 확인 후, Name을 눌러 **Private route** 라고 라우팅 테이블의 이름을 지정해 줍니다.

자, 이제 기본적인 네트워크 구성이 완료되었습니다. 현재까지 구성한 자원들을 개념적으로 도면에 표시해 보면 아래 그림과 같습니다.
