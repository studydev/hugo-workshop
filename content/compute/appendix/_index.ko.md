---
date: 2020-06-01
title: "부록 - 추가적인 EC2 개념들"
weight: 240
pre: "<b>2-4. </b>"
---

{{% notice warning %}}
프리 티어 유지를 위해서 인스턴스 타입을 ***t2.micro*** 에서 변경하지 않습니다.
{{% /notice %}}

## 새로운 키 쌍(Key Pair) 생성
EC2를 SSH로 접근하기 위해서는 Key Pair를 생성해야 합니다. 다음의 과정은 SSH Key Pair를 생성하는 과정을 설명합니다.

{{% notice info %}}
키 쌍(Key Pairs)는 각 리전(Region)별로 독립적으로 관리됩니다.
{{% /notice %}}

1. AWS Management Console에 로그인하고, [Amazon EC2 Console](https://console.aws.amazon.com/ec2)을 여십시오.

2. AWS Management Console 화면 우측 상단의 사용 리전이 여러분이 사용 하고자 의도한 리전이 맞는지 확인하십시오. 

3. 네트워크 및 보안 항목에 있는 **키 페어**을 선택하십시오. SSH Key Pair를 관리할 수 있는 페이지가 보여지게 됩니다.

4. 새로운 SSH Key Pair를 생성하기 위하여 화면 상단 또는 화면 중간(키 페어가 없는 경우)의 **키 페어 생성** 버튼을 클릭 하십시오.

5. 키 페어 생성 윈도우에서, ***AWS-ImmersionDay-Lab*** 형식으로 키 페어 이름을 지정하고 **키 페어 생성**을 클릭 하십시오. 키페어의 이름은 사용자가 식별하기 쉽도록 목적과 용도에 따라 지정이 가능합니다. *PuTTY를 사용하시는 경우(Windows) 파일 형식에서 아래쪽 ppk를 선택해 주시고, 그 외 ssh를 이용하시는 경우(Mac 또는 Linux) 파일 형식을 pem으로 선택해 주십시오.*

6. 새로운 키 페어가 생성되고, 최초 1회에 한하여 사용자의 PC로 다운로드 됩니다. pem 파일 형식을 선택하셨던 경우, 다운로드 되는 파일 이름은 ***AWS-ImmersionDay-Lab.pem*** 입니다. ppk 파일 형식을 선택하셨다면, ***AWS-ImmersionDay-Lab.ppk*** 가 다운로드 됩니다.

7. 다운로드 된 키 페어 파일을 안전한 위치에 보관하시기 바랍니다.

{{% notice warning %}}
다운로드된 키 페어 파일은 이후 Lab과정에서 생성하는 EC2 인스턴스에 지정하는데 사용됩니다. 또한, 이후 EC2 인스턴스에 로그인하기 위해서도 반드시 필요합니다.
{{% /notice %}}

## SSH를 통한 Linux OS 접근
AWS상에서 운영되는 Linux 인스턴스는 일반적인 On-Premise의 Linux에 접근하던 방식과 다른 인증 방식을 기본적으로 사용하고 있습니다. 일반적인 ID + Password 방식의 OS 사용자 인증이 아닌 보안성이 강화된 ID + Private Key/Public Key 방식의 인증을 사용하고 있습니다.

EC2 인스턴스에 접속할 수 있는 방법은 현재 3가지입니다. 
1. 키 페어를 이용하여 SSH로 접속하는 방법
2. Systems Manager의 Session Manager를 이용하여 접속하는 방법
3. 브라우저 기반의 SSH 연결을 이용하여 인스턴스 연결

이번 Module에서는 EC2에 지정된 키 페어를 통하여 앞서 생성한 EC2 인스턴스에 SSH로 접속하는 과정을 실습하겠습니다.

사용자의 Private Key는 Key Pair 생성과정에서 최초 1회에 한하여 사용자가 Download 할 수 있으며, 배포과정에서 EC2 Linux 인스턴스에는 지정된 Key Pair에 해당하는 Public Key가 설치됩니다. 이후 사용자는 자신만의 Private Key를 이용하여 EC2 Linux 인스턴스에 SSH 인증 및 접속이 가능하게 됩니다.

접속하는 사용자가 사용하는 운영체제 및 SSH Client Tool의 종류에 따라 사전 준비 작업이 필요할 수 있습니다. 사용자의 단말 운영체제가 Windows기반이고, 사용하는 SSH Client Tool 이 **PuTTY**인 경우, 초기 키 페어를 생성할 때 .ppk 형태로 다운로드하여 사용하는 것이 편리합니다.

### Windows Laptop 사용자를 위한 PuTTY 사용 방법

AWS는 표준 인증서 포맷인 PEM(Privacy-enhanced Electronic Mail)형식으로 제공되나, Windows Laptop 사용자들이 주로 사용하는 대표적인 Freeware SSH Client인 PuTTY는 PEM 형식 인증서를 직접 지원하지 않고 있습니다.(그 외 대부분의 SSH Client들은 PEM인증서를 기본적으로 지원합니다.)

따라서, 사용자가 Windows Laptop기반에서 PuTTY를 사용하는 경우, EC2 인스턴스에 SSH 연결을 위하여 PuTTY가 지원하는 PPK(.ppk, PuTTY Private Key Files) 파일 형식으로 Private Key를 다운로드 받아야 합니다.


### PuTTY에서 PPK 인증서를 지정하고 연결하기

PuTTY를 실행하고, 연결할 Host에 ***ec2-user@EC2_PUBLIC_IP_ADDRESS/FQDN*** 의 형식으로 지정합니다.

**Connection > SSH > Auth** 메뉴의 **Private key file for authentication:** 항목의 **Browse**를 선택하여 PPK형식의 인증서 파일(Private Key)을 지정 하십시오. 향후, 접속한 EC2 인스턴스에서 VPC내부의 다른 EC2 인스턴스로 SSH연결을 할 경우, **Allow agent forwarding** 기능을 선택하십시오.

**Open**을 선택하면, 인증서 Cache여부에 대한 질문 후 SSH Session이 연결 됩니다.

이제 개인용 인증서(Private Key)를 지정하여 SSH연결이 가능합니다. 기본적인 Linux 명령을 이용하여 OS의 구성 등을 확인하십시오.


## 인스턴스 타입의 변경

EBS Volume을 사용하는 EC2 인스턴스는 간단한 절차를 통하여 인스턴스의 타입(CPU/Memory 용량 및 주된 사용 분야에 따라서)을 변경 할 수 있습니다. 본 Lab에서는 필요하지 않으나, 아래와 같은 과정을 통하여 인스턴스의 타입을 사용자가 원하는 유형으로 변경 할 수 있습니다.

AWS Management Console에서 변경하고자 하는 인스턴스를 선택하고, 마우스 오른쪽 버튼을 클릭하여 **인스턴스 상태** 하위의 **중지** (종료가 아님에 주의하십시오!)를 선택합니다. 
 

중지될 인스턴스를 확인하고 **예, 중지**를 선택하십시오.

 

인스턴스가 중지되면, 마우스 오른쪽 버튼을 눌러 **인스턴스 설정**을 선택하고 하위의 **인스턴스 유형 변경**을 선택하십시오.

 

변경하고자 하는 인스턴스의 유형을 선택하고 **적용**을 클릭 하십시오.
 
변경이 완료되면, **인스턴스 상태** 하위의 **시작**을 선택하여 변경된 인스턴스 유형으로 시작합니다.
