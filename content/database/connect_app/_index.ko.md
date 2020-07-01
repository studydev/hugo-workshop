---
date: 2020-06-01
title: "웹앱 서버와 RDS 연결"
weight: 330
pre: "<b>3-3. </b>"
---

{{% notice note %}}
앞선 Lab에서 생성한 Web Server 인스턴스에는 RDS 사용을 위한 단순한 주소록을 생성하는 코드가 포함되어 있습니다. EC2 Web Server에서 RDS를 사용하기 위하여 RDS의 Endpoint URL(DNS FQDN)을 먼저 확인해야 합니다.
{{% /notice %}}

1. Amazon RDS 콘솔의 좌측 메뉴에서 **Databases**를 선택하고, 생성한 RDS Aurora 클러스터의 이름, **rdscluster**를 선택하십시오. **Connectivity & security** 탭에서 RDS Aurora의 엔드포인트(Endpoint)를 확인할 수 있습니다. **Type**이 **Writer**인 Endpoint name을 복사해 둡니다.

2. VPC Lab에서 생성한 ALB의 DNS 이름을 확인하고, Web Browser로 접속하십시오. 

3. 위의 메뉴 중 **RDS**를 선택하여 RDS 연결에 필요한 정보를 입력합니다. RDS 인스턴스 정보에서 복사해 둔 Write Endpoint URL을 입력하고, RDS 인스턴스 생성시 입력한 Database, 사용자 이름 및 암호를 입력한 후 **Submit**을 선택하십시오. 
    
    | 키 | 값 |
    |----------|--------------------|
    | Endpoint | `RDS의 Writer endpoint name` (ex. rdscluster.cluster-<임의의 번호>.ap-northeast-2.rds.amazonaws.com) |
    | Database | `immersionday` |
    | Username | `awsuser` |
    | Password | `awspassword` |

4. 입력한 정보가 정확하다면 자동으로 지정된 Database(immersionday)에 간단한 **address**라는 테이블이 생성되고, 2개의 Sample Record(Row)가 추가됩니다. 10초를 기다리면 자동으로 화면이 전환됩니다. 추가된 Sample Record가 정상적으로 보여지는지 확인하세요.
  
5. 이제 Web Application Server와 RDS Aurora가 정상적으로 연결되었습니다. PHP로 구성된 Web Page상에서 **Edit**, **Remove** 또는 **Add Contact** Link를 사용하여 RDS 데이터베이스에 데이터를 수정/삭제 및 추가가 가능한지를 확인하십시오. 여기서는 샘플로 아래와 같은 사용자를 추가해 보겠습니다.
