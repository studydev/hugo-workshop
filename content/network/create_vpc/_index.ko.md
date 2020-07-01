---
date: 2020-06-30
title: "VPC 생성"
weight: 110
pre: "<b>1-1. </b>"
---

{{% notice note %}}
Amazon Virtual Private Cloud(Amazon VPC)에서는 사용자가 정의한 가상 네트워크로 AWS 리소스를 시작할 수 있습니다. 이 가상 네트워크는 AWS의 확장 가능한 인프라를 사용한다는 이점과 함께 고객의 자체 데이터 센터에서 운영하는 기존 네트워크와 매우 유사합니다.
{{% /notice %}}

## VPC 서비스로 이동
AWS Management Console로 Login 한 후, Service 메뉴에서 **VPC** 를 선택합니다.

{{% notice tip %}}
만약 아래 스크린샷과 보시고 계신 화면이 다르다면, 좌측 위 **New VPC Experience 토글을 활성화** 해 주십시오.
{{% /notice %}}

### 탄력적 IP 생성
화면 좌측의 VPC Dashboard에서 **탄력적 IP(Elastic IPs)** 를 선택하고 **새 주소 할당(Allocate Elastic IP Address)** 을 클릭 하십시오. 이것은 VPC Wizard를 통하여 생성할 NAT Gateway에 할당하기 위하여 미리 생성하는 것입니다.

탄력적 IP(Elastic IP)는 사용자 계정(Account)에 고정적으로 할당되며, 인스턴스의 상태에 관계 없이 지속적으로 할당되는 IP를 의미하며, Public IP이므로 외부에서 접근이 가능한 IP입니다. NAT Gateway에 사용할 고정된 Public IP를 위하여 생성합니다.

**할당(Allocate)** 을 선택하십시오.
 
사용자 계정(Account)에 할당된 새로운 탄력적 IP(Elastic IP)를 확인 할 수 있습니다.

### VPC 마법사를 이용한 VPC 생성
**VPC 대시보드(Dashboard)** 를 선택하고, **VPC 마법사 시작(Launch VPC Wizard)** 을 클릭하여 VPC 생성 마법사를 시작합니다. 
 
1. **Step 1: Select a VPC Configuration** 화면에서 2번째 Option인 **VPC with Public and Private Subnets** 를 선택하십시오. VPC 마법사가 화면상의 그림과 같이 Private Subnet에 있는 EC2 인스턴스가 Internet에 Access할 수 있도록 NAT Gateway를 자동으로 생성합니다. NAT Gateway에 대해서는 이후에 자세히 다루도록 하겠습니다.

2. **Step 2: VPC with Public and Private Subnets** 설정 화면에서 아래의 값을 **VPC name, Public subnet's IPv4 CIDR, Availability Zone, Public subnet name, Private subnet's IPv4 CIDR, Availability Zone, Private subnet name** 에 입력 하십시오. 퍼블릭 서브넷(Public Subnet)은 EC2인스턴스에 Public IP가 할당되어 Internet Gateway를 통하여 직접 인터넷에 인바운드 / 아웃바운드 접근이 가능한 Subnet이고, 프라이빗 서브넷(Private Subnet)은 Public IP가 할당되지 않으나, NAT Gateway를 통하여 인터넷에 아웃바운드 접근이 가능한 Subnet입니다.

| 키 | 값 |
|----------|--------------------|
| IPv4 CIDR block | `10.0.10.0/16` |
| VPC name | `VPC-Lab` |
| Public subnet’s IPv4 CIDR | `10.0.10.0/24` |
| Availability Zone | `ap-northeast-2a` |
| Public subnet name | `Public subnet A` |
| Private subnet’s IPv4 CIDR | `10.0.100.0/24` |
| Availability Zone | `ap-northeast-2a` |
| Private subnet name | `Private subnet A` |
 
3. NAT Gateway에 할당할 탄력적 IP지정을 위하여 **Elastic IP Allocation ID** 를 선택하십시오. 사용 가능한 탄력적 IP가 표시될 것이며, Lab 도입부에서 NAT Gateway를 위하여 생성한 탄력적 IP(Elastic IP)의 할당 ID(Allocation ID)를 선택합니다. 모든 설정이 지정되었으면 **Create VPC** 를 클릭 하십시오.

4. VPC 생성 마법사는 Subnet과 NAT Gateway를 자동으로 생성합니다. 생성이 완료되면 **확인(OK)** 버튼을 클릭하십시오.


여기까지 완료되었다면, 현재까지 구성된 환경은 아래와 같습니다.
