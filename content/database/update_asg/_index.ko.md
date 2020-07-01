---
date: 2020-06-01
title: "오토 스케일링 그룹 업데이트"
weight: 340
pre: "<b>3-4. </b>"
---

{{% notice note %}}
이제 Aurora를 사용하도록 구성된 웹 인스턴스를 이용, 새로운 커스텀 AMI를 생성해 보겠습니다. 이 새로 만든 커스텀 AMI로 Launch Template 을 업데이트하고, Auto Scaling Group을 이용하여 RDS 연결 정보를 보유한 새 버전의 인스턴스들을 배포할 계획입니다.
{{% /notice %}}

## 신규 커스텀 AMI 생성
EC2 콘솔로 들어가 좌측 메뉴에서 Instances를 누르고, ASG-Web-Instance를 선택합니다. 이후 ActionsImageCreate Image를 순차적으로 선택합니다.

Image name에 Web Server v2를 입력하고, Image description에 LAMP web server AMI with RDS connection config를 입력합니다. 그리고 Create Image 버튼을 클릭합니다.
 
EC2 콘솔 좌측 메뉴에서 AMIs를 찾아 클릭합니다. 잠시 기다리면 새로운 커스텀 AMI가 available 상태로 바뀔 겁니다.
 

## Launch Template 업데이트
EC2 콘솔 좌측 메뉴에서 Launch Templates를 찾아 선택하고, 우측 화면에서 Web이라는 이름의 Launch template ID를 클릭합니다.

오른쪽 Actions 메뉴를 누르고, Modify template (Create new version)을 클릭합니다.

Modify Template 메뉴에서 Template version description을 아래와 같이 입력하고, Auto Scaling guidance의 Provide… 에 체크박스를 선택합니다.
•	Template version description: Immersion Day Web Instances Template - with RDS connection String
 
스크롤을 내려 Launch template contents 의 Amazon machine image (AMI) 란에서 새로 만든 커스텀 AMI를 선택합니다. 검색 창에 Web Server v2를 입력하면 쉽게 찾을 수 있습니다.
 
이후 다른 설정들은 그대로 두고, 스크롤을 맨 아래로 내려 Create template version을 클릭합니다. 다음 화면에서 View launch template을 누르고, 가운데 Versions 탭을 선택합니다. 아래와 같은 화면이 나올 겁니다.
 
두 개의 버전 중, 방금 생성한 Version 2를 선택하고 Actions  Set default version을 누릅니다.
 
이후 화면에서도 Set as default version을 누릅니다.
 
이제 Launch template의 Default version이 변경된 것이 보입니다.
 
 
## Auto Scaling Group 업데이트
이제 Auto Scaling Group을 업데이트할 차례입니다. EC2 콘솔 좌측 메뉴에서 맨 아래 **Auto Scaling Groups**을 선택합니다. 생성해 둔 `Web-ASG`를 선택한 후, **Edit** 버튼을 누릅니다.

Edit Web-ASG 화면에서 Desired capacity와 Minimum capacity를 모두 2로 변경합니다.

아래 Launch template 버전을 확인해 줍니다. Default (2)로 설정되어 있어야 합니다.

아래로 스크롤을 내려 Advanced configurations 란으로 이동하고, 아래의 Default cooldown을 30 seconds로 변경해 줍니다.

맨 아래 Update 버튼을 누릅니다. 잠시 기다리면, Auto Scaling Group이 새 Launch Template을 이용하여 새로운 인스턴스를 생성하는 걸 볼 수 있습니다.

두 개의 인스턴스가 InService인 것을 확인할 수 있습니다. 그런데 버전을 살펴보니 하나는 예전 Launch template 버전(Version 1)으로 만들어진 인스턴스입니다. 이 예전 버전 인스턴스를 수동으로 종료해 보겠습니다. 
맨 왼쪽, 인스턴스의 ID(i-xxxxx)를 클릭하여 EC2 인스턴스 콘솔로 들어갑니다.

새 탭이 뜨고, 종료할 인스턴스가 이미 선택되어 있습니다. Actions  Instance State  Terminate를 눌러 줍니다. 정말 종료하겠냐는 경고 팝업이 뜨면, Yes, Terminate를 누릅니다.

다시 Auto Scaling Group 메뉴로 들어옵니다. Web-ASG 를 선택하고, 가운데 Instance management 탭을 눌러 보면 현재 3개의 인스턴스가 보입니다. 이를 살펴보면, 수동으로 종료시킨 Version 1 인스턴스가 Terminating 상태이고, 새로이 Version 2 인스턴스가 올라오는 것을 볼 수 있습니다. 

변경이 발생하는 동안 ALB의 DNS를 이용하여 웹 서비스에 접근해 보면, 정상적으로 접근이 되는 것을 볼 수 있습니다. 

인스턴스 추가가 완료되면, 앞서 Computing Lab에서 본 것과 같이 두 개의 인스턴스에 번갈아 접근하게 됩니다. 데모 페이지에서 InstanceId가 변경된다면, Auto Scaling Group 콘솔로 들어가 봅니다. 두 개의 Launch Template Version 2 인스턴스가 정상적으로 동작 중인 것을 확인할 수 있습니다.

데모 웹 페이지로 돌아와, RDS 버튼을 눌러 보겠습니다. 바로 아래와 같이 접근되며, 아까 수정한 내용 역시 반영된 것을 확인할 수 있습니다.

자, 여기까지의 작업을 통해 여러분은 고가용성이 보장된 웹 서비스를 구축하였습니다. 지금까지 구성한 인프라 아키텍처는 아래와 같습니다.
