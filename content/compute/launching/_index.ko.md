---
date: 2020-06-01
title: "웹 서버 인스턴스의 시작"
weight: 210
pre: "<b>2-1. </b>"
---

{{% notice note %}}
이 과정에서는 기본 Amazon Linux 인스턴스를 시작하고, 초기화 과정에서 Apache/PHP Web Server를 자동으로 구성하도록 합니다.
{{% /notice %}}

## 인스턴스 생성
1. 좌측 메뉴 상단의 **EC2 Dashboard**를 클릭합니다.

2. **Launch instance** 버튼을 누르고, 메뉴에서 **Launch instance** 를 선택합니다.

3. **[Step 1. Choose AMI]** 의 **Quick Start** 탭에서 `Amazon Linux 2 AMI`를 선택하십시오. *Amazon Linux AMI*가 아니라 `Amazon Linux 2 AMI` 입니다.

4. **[Step 2. Choose Instance Type]** 에서 `t2.micro`를 선택하고 **Next: Configure Instacne Details**을 클릭 하십시오.
 
5. **[Step 3. Configure Instance]** 에서 아래와 같이 설정합니다.  

| 키 | 값 |
|----------|--------------------|
| Network | `VPC-Lab` 태그가 붙어 있는 VPC를 선택합니다. |
| Subnet | 드롭 다운 메뉴에서 `Public subnet A`를 찾아 선택합니다. |
| Auto-assign Public IP | `Enable` |

나머지 모든 값들은 기본값을 사용하고, 화면 하단의 **Advanced Details**를 클릭하여 확장하십시오. **User data** 입력란에 아래의 내용을 입력한 후, **Next: Add Storage**를 선택 하십시오.  
`#include https://go.aws/38GIqcB `  
또는 아래와 같이 쉘 스크립트를 직접 입력한 후 **Next: Add Storage**를 선택 하십시오. 
```
#!/bin/sh
yum -y install httpd php mysql php-mysql
chkconfig httpd on
systemctl start httpd
if [ ! -f /var/www/html/bootcamp-app.tar.gz ]; then
   cd /var/www/html
   wget https://s3.amazonaws.com/awstechbootcamp/GettingStarted/bootcamp-app.tar.gz
   tar xvfz bootcamp-app.tar.gz
   chown apache:root /var/www/html/rds.conf.php
fi
yum -y update
```

{{% notice tip %}}
사용자 데이터(User Data)는 최초 인스턴스 생성시 실행되는 사용자 정의 초기화 스크립트 입니다.
{{% /notice %}}

6. **[Step 4. Add Storage]** 에서 인스턴스에 할당하는 OS Volume(EBS Volume)을 정의합니다. 기본 값인 8GB(SSD Type)를 그대로 사용합니다. **Next: Add Tags**를 클릭하여 다음 단계로 진행합니다.
 
7. **[Step 5: Add Tags]** 에서 인스턴스를 식별할 수 있는 다양한 정보를 추가할 수 있습니다. 태그 정보를 통하여 사용자는 인스턴스의 용도, 목적, 비용 관련 정보 등을 손쉽게 확인 할 수 있습니다. **Add Tag**를 클릭하고, 키와 값을 아래와 같이 입력합니다. 완료되면 **Next: Configure Security Group**을 클릭하십시오.

| 키 | 값 |
|----------|--------------------|
| Key | `Name` |
| Value | `Web server for custom AMI` |

8. **[Step 6: Configure Security Group]** 에서는 새로운 보안 그룹을 만들거나 기존에 만들어진 보안 그룹을 선택 할 수 있습니다. 보안 그룹은 방화벽 정책으로 허용하고자 하는 프로토콜과 주소를 지정하게 됩니다. Lab을 위하여 새로운 보안 그룹을 생성하고 이름을 지정합니다.

**Security group name**에 **Immersion Day - Web Server**를 입력 후, **Add Rule**를 선택하여 Web Service를 위한 TCP/80도 함께 허용합니다. 추가된 행의 **Type**에 **HTTP**를 지정하면 됩니다. 소스 주소의 **0.0.0.0/0**은 모든 네트워크에서의 접근을 의미합니다.

**Review and Launch**을 클릭하여  전체 구성을 확인합니다.

9.	**[Step 7. Review]** 에서 앞서 구성한 정보를 확인하고 **Launch**을 선택하여 인스턴스를 시작하십시오.
 
10.	키 페어 선택 화면에서 **Proceed without a key pair**를 선택합니다. 아래 체크박스를 선택 후, **Launch Instances**를 클릭합니다.
 
11.	화면 하단의 **인스턴스 보기**를 선택하여 EC2인스턴스의 목록을 확인할 수 있습니다. 인스턴스의 시작이 완료되면 인스턴스가 구동되고 있는 가용 영역, 외부에서 접근 가능한 IP 및 DNS 정보를 확인 할 수 있습니다.


## 웹 서비스 접속

1. 인스턴스의 **Status Check** 결과가 **2/2 checks passed**가 될 때 까지 대기 하십시오.  초기화가 완료되면 **2/2 checks passed**로 변경됩니다.

2. 새로운 웹 브라우저 탭을 열고 URL 주소 입력하는 영역에, EC2 인스턴스의 **퍼블릭 DNS 또는 IPv4 퍼블릭 IP를 입력**하십시오. 아래와 같이 페이지가 보여지면 웹 서버 인스턴스가 정상적으로 구성된 것입니다.


## 인스턴스 접속

1. EC2 인스턴스 콘솔로 들어갑니다. 접속하고자 하는 인스턴스를 선택한 뒤, 가운데 **Connect** 버튼을 누릅니다.
 	
2. **Connect your instance** 창에서 **Connection method**를 EC2 Instance Connect로 선택한 뒤, 오른쪽 아래 **Connect** 버튼을 누릅니다.
 
잠시 기다리고 나면, 아래와 같이 브라우저 기반 SSH 콘솔을 사용하실 수 있습니다. 테스트 후 창을 닫으면 됩니다.


## 커스텀 AMI 생성
AWS EC2 콘솔에서는 생성된 인스턴스를 이용하여 이미지를 만들고, 추후 인스턴스 생성 시 이 이미지를 사용할 수 있습니다. 이를 커스텀 AMI라고 부릅니다. 
여기서는 앞서 생성한 웹 서버 인스턴스를 이용하여 AMI를 만들어 보겠습니다.

EC2 콘솔에서 아까 생성한 인스턴스를 선택하고, **Actions** -> **Image** -> **Create Image** 를 선택합니다.
 

Create Image 창에서 아래와 같이 입력합니다.

| 키 | 값 |
|----------|--------------------|
| Image name | `Web Server v1` |
| Image description | `LAMP web server AMI` |

이후 **Create image**를 눌러 이미지를 생성합니다.
 

이미지 생성 요청이 완료되었다는 창에서 **Close**를 누르고, 좌측 콘솔 메뉴에서 IMAGES 아래 AMIs 버튼을 찾아 클릭합니다. 방금 요청한 AMI가 생성 중인 것을 보실 수 있습니다.
 

생성이 완료되면 아래와 같이 **Status**가 **Available**로 변경됩니다.

## 인스턴스 종료(Terminate)

커스텀 AMI 생성을 완료하였으니 이제 기존의 EC2 인스턴스를 종료(Termination)해 보도록 하겠습니다.
1. EC2 대시보드에서 **Instances** 탭으로 들어갑니다.
2. 삭제하고자 하는 인스턴스를 선택합니다. 이후 **Actions** -> **Instance state** -> **Terminate** 를 클릭합니다.
3. 경고 창이 뜨면, **Yes, Terminate**를 눌러 삭제합니다.
4. Instance State가 **Shutting down**으로 변경됩니다.
5. 이후 곧 **terminated**로 변경됩니다. 삭제가 완료되었습니다.