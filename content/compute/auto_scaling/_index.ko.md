---
date: 2020-06-01
title: "오토 스케일링 웹 서비스 배포"
weight: 220
pre: "<b>2-2. </b>"
---

{{% notice note %}}
AWS Auto Scaling은 애플리케이션을 모니터링하고 용량을 자동으로 조정하여, 최대한 저렴한 비용으로 안정적이고 예측 가능한 성능을 유지합니다. AWS Auto Scaling을 사용하면 몇 분 만에 손쉽게 여러 서비스 전체에서 여러 리소스에 대해 애플리케이션 규모 조정을 설정할 수 있습니다.
{{% /notice %}}

이제 앞서 Networking Lab에서 생성한 네트워크 인프라 위에 부하에 따라 자동적으로 Scale out/in 이 가능하고 고가용성이 보장되는 웹 서비스를 배포해 보겠습니다. 여기서는 이전 장에서 만들어 둔 웹 서버 AMI와 VPC Lab에서 구성한 네트워크 인프라를 사용할 예정입니다. 

## Application Load Balancer 구성
AWS Elastic Load Balancing은 Application Load Balancer, Network Load Balancer, Classic Load Balancer의 세 가지 유형의 로드 밸런서를 지원합니다. 본 실습에서는 HTTP 요청을 부하 분산 처리하는 Application Load Balancer를 구성하고 설정하도록 해 보겠습니다.

### ALB 생성
1. **EC2 관리 콘솔**로 들어갑니다. 좌측 메뉴에서 스크롤을 내려 **Load Balancing** 항목 아래 **Load Balancers** 를 클릭하고, 가운데 위의 **Create Load Balancer** 를 클릭합니다.

2. Select load balancer type 창에서 **Application Load Balancer** 의 **Create** 버튼을 클릭합니다.
 
3. **[Step 1: Configure Load Balancer]** 에서 로드 밸런서의 이름을 정해 줍니다. 여기서는 **Name**에 `Web-ALB`로 이름을 지정해 줍니다. 다른 설정들은 기본값으로 둡니다. 기본 설정은 인터넷으로부터 80 port로 들어오는 HTTP 연결을 ALB가 Listening 하도록 되어 있습니다.

4. 스크롤을 조금 내리면, 가용 영역을 설정하는 란이 나옵니다. 사전에 구성한 **VPC**가 `VPC-Lab` 으로 선택되어 있는지 확인한 후, 로드 밸런서가 사용할 **Availability Zones** 의 `ap-northeast-2a`를 선택하고, `Public subnet A`를 찾아 지정합니다. 마찬가지로 `ap-northeast-2c`도 선택하고 `Public subnet C`를 지정해 줍니다. 우측 하단 **Next: Configure Security Settings**을 눌러 다음 단계로 넘어갑니다.

5. **[Step 2: Configure Security Settings]** 에서 HTTPS를 사용하는 보안 리스너를 구성하라는 경고 메시지가 나옵니다. 이 실습에서는 편의를 위하여 HTTP 리스너를 사용할 예정입니다. 바로 **Next: Configure Security Groups** 버튼을 눌러 넘어갑니다.

6. **[Step 3: Configure Security Groups]** 에서 ALB에 적용될 보안 그룹을 설정합니다. 먼저 **Assign a security group** 란에 `Create a new security group` 를 선택하고, **Security group name** 에 `Web-ALB-SG` 를 입력합니다. 그리고 **Type** 드롭다운 메뉴에서 `HTTP`를 찾아 선택합니다. 완료되었으면 **Next: Configure Routing** 버튼을 눌러 다음 단계로 넘어갑니다.

7. **[Step 4: Configure Routing]** 에서 위에서 설정한 리스너가 트래픽을 넘겨줄 대상 그룹을 설정합니다. 현재 우리는 아직 트래픽을 받아 처리해 줄 인스턴스가 없는 상태입니다. 일단 **Name**만 `Web-TG` 로 만들고 **Next: Register Targets** 버튼을 누르고 다음 단계로 넘어갑니다.

8. **[Step 5: Register Targets]** 단계입니다. 하지만 앞서 말씀드린 것처럼 현재 등록할 타겟이 없습니다. **Next: Review** 를 눌러 넘어갑니다.
 
9. **[Step 6: Review]** 에서 현재까지 구성한 설정을 확인해 볼 수 있습니다. 이상이 없는 것을 확인한 후, 오른쪽 아래 **Create** 버튼을 클릭하여 생성 설정을 완료합니다.

10. 로드 밸런서 생성이 완료되었습니다. **Close** 를 눌러 생성 화면을 종료합니다.

## 시작 템플릿 구성
ALB를 만들었으니 이제 로드 밸런서 뒤에 인스턴스들을 배치할 차례입니다. Auto Scaling Group에서 시작할 Amazon EC2 인스턴스를 구성하려면 **시작 템플릿**, **시작 구성**, 또는 **EC2 인스턴스** 를 사용할 수 있습니다. 여기서는 시작 템플릿을 사용하여 Auto Scaling 그룹을 생성해 보겠습니다. 

시작 템플릿은 한 리소스 내의 모든 시작 파라미터를 한꺼번에 구성하도록 되어 있어 인스턴스 생성에 필요한 단계 수를 줄입니다. 또한 시작 템플릿은 Auto Scaling, 스팟 집합, 스팟 및 온디맨드 인스턴스에 대한 지원을 통해 Best Practice 를 더욱 쉽게 구현할 수 있습니다. 이를 통해 비용을 보다 편리하게 관리하고 보안을 향상시키며, 배포 오류 위험을 최소화하는 데 도움이 됩니다.

시작 템플릿에는 AMI 및 인스턴스 유형과 같이 Amazon EC2가 인스턴스를 시작하는 데 필요한 정보가 포함되어 있습니다. Auto Scaling 그룹은 이를 참조하여 확장(Scale out) 이벤트가 발생할 때 새로운 인스턴스들을 추가하게 됩니다. 만약 Auto Scaling 그룹에서 시작할 EC2 인스턴스의 구성을 변경해야 한다면, 시작 템플릿의 새 버전을 생성하여 Auto Scaling 그룹에 지정하면 됩니다. 필요에 따라 Auto Scaling 그룹에서 EC2 인스턴스를 시작하는 데 사용하는 시작 템플릿의 특정 버전을 선택할 수도 있습니다. 이 설정은 언제든지 변경할 수 있습니다.  

### 보안 그룹 생성
시작 템플릿을 만들기 전에, 시작 템플릿을 통해 생성되는 인스턴스들이 사용할 보안 그룹을 만들어 보겠습니다. EC2 콘솔의 좌측 메뉴에서 **Network & Security** 칸의 **Security Groups**를 선택하고, 우측 상단의 **Create Security Group**을 클릭합니다.

1. 보안 그룹 생성 화면에서 아래 내용들을 채워 넣습니다.

| 키 | 값 |
|----------|--------------------|
| Security Group Name | `ASG-Web-Inst-SG` |
| Description | `HTTP Allow` |
| VPC | `VPC-Lab` |

2. 스크롤을 내려 인바운드 룰을 수정해 줍니다. 먼저 **Add rule** 을 눌러 인바운드 룰 수정 창을 추가하시고, **Type**에는 `HTTP`를 입력합니다. **Source**로는 검색창에 `ALB`라고 입력하시면 ALB 생성 시 만든 보안 그룹이 검색됩니다. 이를 클릭하여, ALB에서 들어오는 HTTP 트래픽만 받도록 보안 그룹을 구성해 줍니다.

| 키 | 값 |
|----------|--------------------|
| Type | HTTP |
| Source | `Web-ALB-SG` (클릭하고 나면 sg-xxxx 형태로 변경됨) |

3. Outbound rules는 기존 설정 그대로 두고, 오른쪽 아래 **Create Security Group** 버튼을 눌러 보안 그룹을 생성해 줍니다. 이를 통해 인터넷에서 ALB를 통해 인스턴스로 들어오는 HTTP 연결(TCP 80)에 대해서만 트래픽을 허용하는 보안 그룹을 생성하였습니다.

### 시작 템플릭 생성
1. EC2 콘솔에 접속, 좌측 메뉴에서 **Launch Templates**을 찾아 선택하고, **Create Launch Template**을 클릭합니다.

2. 시작 템플릿 설정을 하나하나 진행해 보겠습니다. 먼저 Launch template name과 Template version description을 아래와 같이 설정하고, Auto Scaling guidance의 Provide guidance…. 항목의 체크박스를 선택합니다. 이 체크박스를 선택하여 생성하는 템플릿이 Amazon EC2 Auto Scaling에서 활용되도록 설정합니다.

| 키 | 값 |
|----------|--------------------|
| Launch template name | `Web` |
| Template version description | `Immersion Day Web Instances Template – Web only` |
| Auto Scaling guidance | **Provide guidance to help me set up a template that I can use with EC2 Auto Scaling** `체크 박스 클릭` |

3. 아래로 스크롤을 내려 시작 템플릿 콘텐츠를 설정합니다. **Amazon Machine Image(AMI)** 란에는 이전 EC2 Hands-on Lab에서 만든 AMI를 찾아 설정합니다. 검색 창에 `Web Server v1` 이라고 입력하여 찾으시거나, 스크롤을 내려 내 AMI 란에서 찾으실 수 있습니다. 다음으로 인스턴스 유형에는 `t2.micro`를 입력하여 선택해 줍니다. 서비스용 웹 서버를 올리는 것이므로 SSH 접근은 하지 않을 예정입니다. 따라서 키 페어는 사용하지 않습니다.

| 키 | 값 |
|----------|--------------------|
| AMI | `Web Server v1` |
| Instance Type | `t2.micro` |

4. 다른 부분들은 기본값으로 두고, Network Settings 부분을 보겠습니다. 먼저 **Networking platform** 란에서 `Virtual Private Cloud(VPC)`를 선택합니다. 보안 그룹 란에는 검색 창에 `ASG-Web-Inst-SG`를 입력하여 앞서 만든 보안 그룹을 선택해 주겠습니다.

| 키 | 값 |
|----------|--------------------|
| Networking platform | `Virtual Private Cloud(VPC` |
| Security groups | `ASG-Web-Inst-SG` |

5. Storage는 별도 설정 없이 기본값을 따르겠습니다. 아래로 내려가 Instance tags를 정의하겠습니다. **Add tag**를 누르고, **Key**에 `Name`을, **Value**에 `Web Instance`를 입력해 줍니다. `Tag volumes`도 클릭하여 선택해 줍니다.

| 키 | 값 |
|----------|--------------------|
| Key | `Virtual Private Cloud(VPC` |
| Value | `ASG-Web-Inst-SG` |
| Tag Volumes | **volumes** `체크 박스 클릭` |

6. 이외의 다른 설정들은 모두 기본값으로 두고, 우측 하단의 **Create launch template** 버튼을 눌러 시작 템플릿을 생성합니다.


7. 다음 단계에서 **View launch template** 를 누르시면 아래와 같이 시작 템플릿이 생성된 것을 확인하실 수 있습니다.


## Auto Scaling Group 구성
자, 이제 Auto Scaling Group 을 만들어 보겠습니다. 

1. 먼저 EC2 콘솔로 들어가 좌측 메뉴 맨 하단의 **Auto Scaling Groups**를 선택합니다. 그리고 **Create Auto Scaling group** 버튼을 눌러 *Auto Scaling Group*을 생성합니다.
 

2. Step 1에서는 먼저 Auto Scaling 그룹의 이름을 지정합니다. 여기서는 `Web-ASG`라고 지정해 보겠습니다. 이후 아래의 시작 템플릿 란에서 방금 만든 템플릿, `Web`을 선택해 줍니다. 시작 템플릿의 기본 설정들이 아래로 펼쳐져 보입니다. 확인 후 우측 하단 **Next** 버튼을 누릅니다.

| 키 | 값 |
|----------|--------------------|
| Auto Scaling 그룹 이름 | `Web-ASG` |
| 시작 템플릿 | `Web` |

3. Configure settings단계에서는 Purchasing options and instance types는 기본값으로 두고 네트워크 구성을 설정합니다. **VPC**에는 `VPC-Lab`을 선택하고, **Subnets** 란에 `Private subnet A`와 `Private subnet C`를 골라줍니다. 설정이 완료되면 **Next** 버튼을 누릅니다. 

| 키 | 값 |
|----------|--------------------|
| VPC | `VPC-Lab` |
| Subnets | `Private subnet A`, `Private subnet C` |
 	
4. 다음으로는 로드 밸런싱 설정을 진행합니다. 먼저 **Enable load balancing** 버튼을 누르고, `Application Load Balancer or Network Load Balancer`를 선택합니다. 이후 **Choose a target group for your load balancer**란에서 ALB 생성 시 만든 `Web-TG`를 선택합니다. 맨 아래쪽의 **Monitoring**에서 **Enable group metrics collection within CloudWatch**의 `체크 박스를 선택`합니다. 이를 통해 Auto Scaling 그룹의 상태를 확인할 수 있는 그룹 지표(Metric)을 CloudWatch에서 확인할 수 있도록 합니다. 우측 하단의 **Next** 버튼을 누릅니다.

| 키 | 값 |
|----------|--------------------|
| Enable load balancing | **Application Load Balancer or Network Load Balancer** `체크 박스 클릭` |
| Choose a target group for your load balancer | `Web-TG` |
| Monitoring | **Enable group metrics collection within CloudWatch** `체크 박스 클릭` |

5. Configure group size and scaling policies단계에서는 Auto Scaling 그룹의 Scaling 정책을 구성합니다. **Group size** 란에서 **Desired capacity**, **Minimum capacity**를 각각 `2`로 지정하고, **Maximum capacity**를 `4`로 지정합니다. 인스턴스 수를 평상시 2개로 유지하고, 정책에 따라 최소 2개, 최대 4개까지의 스케일링을 허용합니다.

| 키 | 값 |
|----------|--------------------|
| Desired capacity | `2` |
| Minimum capacity | `2` |
| Maximum capacity | `4` |
 
6. 아래 조정 정책 란에서는 **Target tracking scaling policy**을 선택하고, **Target value**에 `30`을 입력합니다. CPU 평균 사용률이 전체 30%가 유지될 수 있도록 인스턴스 수를 조정하는 정책입니다. 다른 설정들은 모두 기본값으로 두고, 우측 하단 **Next** 버튼을 누릅니다.

| 키 | 값 |
|----------|--------------------|
| Target tracking scaling policy | `체크 박스 선택` |
| Target value | `30` |
 
7. Add notifications단계는 기본값으로 둔 채 **Next**를 눌러 넘어갑니다.
 
8. Add tags 단계에서는 간단히 이름 태그를 지정하겠습니다. **Add tag**를 누르고, **Key**에 `Name`을, **Value**에 `ASG-Web-Instance`를 입력한 후 **Next**을 클릭합니다.

| 키 | 값 |
|----------|--------------------|
| Key | `Name` |
| Value | `ASG-Web-Instance` |
 
9. 이제 마지막 검토 단계입니다. 관련된 설정들을 리뷰한 후, 우측 하단의 **Create Auto Scaling Group** 버튼을 눌러 주세요. 
 

10. Auto Scaling 그룹이 만들어졌습니다. Auto Scaling 그룹 콘솔에서 아래와 같이 생성된 Auto Scaling 그룹을 확인할 수 있습니다.
 

11. Auto Scaling 그룹을 통해 생성된 인스턴스들은 EC2 인스턴스 메뉴에서도 확인해 볼 수 있습니다. 
 

자, 이제 고가용성이 확보된, 부하에 따라 자동적으로 스케일링되는 웹 서비스를 구축하였습니다! 지금까지 만든 서비스의 구성도는 아래와 같습니다:


