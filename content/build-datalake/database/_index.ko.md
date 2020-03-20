---
title: "데이터베이스 생성"
weight: 42
pre: "<b>4-2. </b>"

---

Lake Formation 콘솔에서 새 데이터베이스를 생성합니다. 

1. 왼쪽 탐색 창에서 **Databases**를 선택하고 **Create database** 버튼을 클릭합니다.
![DataLakeDatabase01](/images/data_lake_database_01.png)

2. Bank 데이터베이스 소스에서 데이터를 수집하여 데이터 레이크를 구축할 계획이므로 데이터베이스의 이름을 `bank_db`로 지정하겠습니다. 

3. **Location**은 CloudFormation을 통해 생성 된 S3 데이터 레이크 경로를 선택합니다. S3 경로는 CloudFormation 스택의 Outputs 탭을 통해 확인할 수도 있습니다.

4. **“Use only IAM access control for new tables in this database”** 사용 옵션 선택을 **취소** 합니다.

5. 다음과 같이 설정을 확인하고 **Create database** 버튼을 클릭합니다.
![DataLakeDatabase02](/images/data_lake_database_02.png)
 
6. 데이터베이스 생성이 성공하면 다음과 같이 표시됩니다.
![DataLakeDatabase03](/images/data_lake_database_03.png)

