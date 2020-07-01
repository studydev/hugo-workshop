---
date: 2020-06-01
title: "RDS 인스턴스 생성"
weight: 320
pre: "<b>3-2. </b>"
---

{{% notice note %}}
RDS에서 사용할 보안 그룹이 생성되었으므로, 이제 RDS Aurora(MySQL 호환) 인스턴스를 생성 하겠습니다.
{{% /notice %}}
[AWS Management Console에서 RDS(Relational Database Service)](https://console.aws.amazon.com/rds)로 이동하십시오.

1. 대시보드에서 **Create Database** 를 선택하여 RDS 인스턴스의 생성을 시작합니다.
 
2. 사용할 RDS 인스턴스의  데이터베이스 엔진을 선택합니다. Amazon RDS에서는 Open Source기반의 Database 엔진 및 상용 Database 엔진을 선택할 수 있습니다. 본 Lab에서는 Amazon에서 제공하는 **MySQL 호환 Database 엔진인 Amazon Aurora** 를 사용하겠습니다. 데이터베이스 생성 방식 선택 란에서 **Standard Create** 을 선택합니다.
 
    Engine type을 **Amazon Aurora**로, Version은 **Aurora (MySQL)-5.6.10a** 을 선택합니다.
 

3. 스크롤을 내려 **Template** 란으로 이동하고, **Production** 을 선택합니다.  

4. **Settings** 항목에서는 RDS 인스턴스의 식별을 위한 정보 및 관리자 정보를 지정합니다. 아래의 정보를 입력하세요.

    | 키 | 값 |
    |----------|--------------------|
    | DB cluster identifier | `rdscluster` |
    | Master username | `awsuser` |
    | Master password | `awspassword` |

5. **DB instance size** 와 **Availability & durability** 항목이 아래와 같은지 확인합니다. **메모리 최적화 인스턴스 클래스와 다른 가용 영역에 읽기 전용 복제 노드를 구성** 하는 것이 기본값으로 설정되어 있습니다.
 
    
6. **Connectivity** 란에서 네트워크 및 보안을 설정합니다. Virtual private cloud (VPC) 란에 앞서 생성한 VPC-Lab을 선택하고, **Additional connectivity configuration** 을 클릭하여 RDS 인스턴스가 운영될 VPC, 서브넷, VPC 외부로부터의 접근 허용여부 및 보안 그룹을 지정합니다. 아래의 내용대로 설정하시면 됩니다.

    | 키 | 값 |
    |----------|--------------------|
    | Virtual private cloud (VPC) | `VPC-Lab` |
    | Subnet group | `Create new DB subnet group` |
    | Publicly accessible | `No` |
    | VPC security group | Choose existing: `DB SG` (***Default의 경우 옆의 X표를 클릭하여 삭제***) |
    | 데이터베이스 포트 (Database Port) | `3306` |

7. 아래로 스크롤을 내려, **Additional configuration** 을 클릭합니다. 아래와 같이 데이터베이스 옵션을 설정합니다.

    | 키 | 값 |
    |----------|--------------------|
    | DB instance identifier | `awsdb` |
    | Initial database name | `immersionday` |
    | DB cluster parameter group | `default.aurora5.6` |
    | DB parameter group | `default.aurora5.6` |

8. 이후 항목인 **Backup**, **Encryption**, **Backtrack**, **Monitoring**, **Log exports** 등 항목은 모두 기본값을 그대로 사용하고, **Create database** 를 눌러 Database를 생성합니다.

    이제 새로운 RDS 인스턴스가 생성됩니다. 이 작업은 5분이상 소요될 수 있습니다. DB 인스턴스의 상태가 ***Available*** 이 되면 RDS 인스턴스를 사용할 수 있습니다.

9. **RDS** 콘솔에서 좌측 **Databases** 메뉴를 선택하여, 생성한 RDS DB 인스턴스의 상태가 ***Available*** 인지 확인하세요.
