---
date: 2020-06-01
title: "데이터베이스 – Amazon Aurora"
weight: 300
pre: "<b>3. </b>"
---

{{% notice note %}}
AWS에서 사용하실 수 있는 여러 Database 옵션 중, Amazon RDS(Relational Database Service)는 구성과 운영이 간편하고 확장이 손쉬운 클라우드 기반의 데이터베이스 서비스입니다. Amazon RDS는 비용 효율적이고 손쉽게 용량을 조절할 수 있으며, 시간 소모가 많은 관리 작업을 줄여 사용자가 비지니스와 어플리케이션에 보다 집중할 수 있게 합니다.
{{% /notice %}}

## Overview

본 Lab은 앞서 수행한 Networking, Computing Lab을 전제로 합니다. 앞선 Lab에서 생성된 Web Instance가 본 Lab에서 생성하는 RDS를 사용하게 됩니다.

## 목표 구성도
본 Database Lab은 VPC Lab을 통해 생성한 VPC 내에 RDS Aurora 인스턴스를 배포하고, 이미 생성된 Auto Scaling Group 내 인스턴스의 Web Service(Apache+PHP)가 RDS Aurora(MySQL)를 사용할 수 있도록 구성합니다. 이후 해당 인스턴스를 이용하여 새로운 커스텀 AMI를 생성하고, Auto Scaling Group에서 새로운 AMI를 사용하도록 업데이트합니다. 이후 Web Browser를 통하여 RDS DB에 저장된 단순한 주소록에 연락처를 추가/수정/삭제 하는 테스트를 진행해 봅니다.

## 실습 순서
본 Lab은 다음과 같은 순서로 진행됩니다.

[3-1. VPC 보안 그룹 생성](./create_sg)  
3-2. RDS 인스턴스 생성  
3-3. 웹앱 서버와 RDS 연결  
3-4. 오토 스케일링 그룹 업데이트  
3-5. RDS 관리 기능  
3-6. 도전 과제 - RDS Aurora 연결  