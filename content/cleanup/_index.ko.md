---
title: "실습 리소스 정리"
weight: 100
pre: "<b>6. </b>"
---

{{% notice warning %}}
이 실습을 마치면 사용한 AWS 계정에 비용이 추가로 발생하지 않도록 사용한 리소스를 삭제해야 합니다. 리소스를 삭제하기 위해 기존의 **Administrator** (관리자) 계정으로 AWS 관리 콘솔에 로그인 합니다.
{{% /notice %}}

1. AWS CloudFormation을 사용하여 생성한 모든 AWS 리소스를 정리합니다. 사전에 CloudFormation에서 만든 리소스에서 변경 사항이 있는 것들을 삭제합니다.
    - IAM에 생성된 **lf-bank-analyst** 유저에 추가한 두 개의 정책(AmazonRedsfhitReadOnlyAccess, AmazonRedshiftQueryEditor)을 우측 X 버튼을 클릭하여 모두 연결 해제 합니다.
![ResourceDelete01](/images/resource_delete_01.png)
    - S3 버킷안에 생성된 객체를 일괄 삭제합니다.
![ResourceDelete01](/images/resource_delete_02.png)

2. CloudFormation의 **Lake-Formation-Workshop** 스택을 선택하고 **Delete** 버튼을 클릭하여 Stack을 삭제합니다.
![ResourceDelete01](/images/resource_delete_03.png)
![ResourceDelete02](/images/resource_delete_04.png)

3. Redshift 콘솔을 열고 생성한 Redshift 클러스터를 선택하고, Action에서 **Delete** 버튼을 클릭하여 삭제 합니다.
![ResourceDelete03](/images/resource_delete_05.png)
