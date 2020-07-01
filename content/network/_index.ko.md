---
date: 2020-06-01
title: "네트워크 – Amazon VPC"
weight: 100
pre: "<b>1. </b>"
---

{{% notice note %}}
본 Hands-On Lab에서는 VPC Wizard를 통하여 Public 및 Private Subnet을 2개의 AZ에 각각 하나씩 생성하고, 하나의 Public Subnet에 NAT 게이트웨이를 구성합니다. 이후 라우팅 테이블을 설정하여 트래픽 흐름을 정의하게 됩니다. 이와 같은 작업을 통해 추후 고가용성이 보장된 EC2 기반의 웹 서비스 환경을 위한 기본 네트워킹 구성을 완료합니다.
{{% /notice %}}

{{% notice tip %}}
Lab Guide에 삽입된 Screenshot 들은 Lab 수행을 돕기 위하여 작성되었습니다. Lab 수행 중 생성하는 각각의 요소들의(VPC, NAT Gateway 및 EIP 등)  식별자(ID)는 사용자 계정마다 다르다는 것을 인지 하시기 바랍니다.
{{% /notice %}}

## 목표 구성도
본 실습을 통하여 구축하고자 하는 최종 구성도는 아래와 같습니다.

## 실습 순서
1. VPC 생성
2. VPC에서 추가 서브넷 생성





