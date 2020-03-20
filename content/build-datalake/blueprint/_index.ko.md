---
title: "데이터 수집을 위한 Blueprint 설정"
weight: 44
pre: "<b>4-4. </b>"

---

데이터를 담을 데이터 레이크 스토리지가 준비 되었기에 데이터 수집을 위한 Blueprint 설정을 시작할 수 있습니다. Lake Formation의 Blueprint 기능을 사용해 ETL 및 카탈로그 생성 프로세스를 위한 워크플로우를 생성합니다. 2020년 3월 기준, Lake Formation은 MySQL, PostgreSQL, Oracle 및 SQL server와 같은 데이터베이스 소스와 CloudTrail 및 Elastic Load Balancer와 같은 AWS 서비스 로그를 수집하기 위한 두 가지 유형의 Blueprint를 제공합니다.

본 실습에서는 bank 데이터베이스 소스에서 데이터를 수집하기 때문에 Database blueprints를 사용하여 전체 테이블의 데이터를 데이터 레이크로 수집합니다.

1. 왼쪽 탐색 창에서 **Blueprints**를 선택한 다음 **Use blueprints** 버튼을 클릭합니다. 
![DataLakeblueprint01](/images/data_lake_blueprint_01.png)

2. Blueprint 유형으로 **Database snapshot**을 선택합니다.
![DataLakeblueprint02](/images/data_lake_blueprint_02.png)

3. **Database Connection**은 CloudFormation을 통해 생성된 **BankGlueConnector**를 선택하여 RDS에서 실행중인 bank 데이터베이스에 액세스 합니다.

4. **Source data path**에 `bank_db/`를 입력하고, **Exclude pattern**은 기본값으로 유지합니다.
![DataLakeblueprint03](/images/data_lake_blueprint_03.png)
 
5. **Import target** 섹션에서 Target database는 **bank_db**를 선택하고, Target storage location은 이전에 데이터 레이크 위치로 등록한 S3버킷을 선택한 후에 `csv/` prefix를 추가합니다. 그리고 Data format은 **csv** 포맷으로 데이터를 작성할 수 있게 선택합니다. 
![DataLakeblueprint04](/images/data_lake_blueprint_04.png)
![DataLakeblueprint05](/images/data_lake_blueprint_05.png)

6. **Import options** 섹션에서 Workflow name을 `bank-ingest`로 입력하고, IAM role은 **LF-GlueServiceRole**을 선택하고 **Table prefix**로 `csv`를 입력합니다. 그리고 나머지 필드는 기본 값으로 유지합니다.
![DataLakeblueprint06](/images/data_lake_blueprint_06.png)

7. 이제 워크플로우를 생성하기 위해 **Create** 버튼을 클릭합니다. 워크플로우의 상태가 Creating에서 **Successfully created**...로 변경 될 때까지 기다립니다.
![DataLakeblueprint07](/images/data_lake_blueprint_07.png)

8. 새로 생성된 **bank-ingest** 워크플로우를 선택하고, **Actions** 드롭 다운 옵션에서 **Start**를 선택하여 워크플로우를 시작합니다.
![DataLakeblueprint08](/images/data_lake_blueprint_08.png)

9. Bank 데이터베이스를 데이터 레이크로 수집하는 데 몇 분이 소요됩니다. **Last run status**를 확인하면 수집 프로세스의 진행 상태가 표시됩니다. 이 실습에 경우 Discovering 단계는 약 4분 정도 소요되고, Importing 단계는 약 23분 정도 소요됩니다. 다음 빨간색 박스 안에 링크를 클릭해서 따라가면, AWS Glue 서비스 콘솔로 이동하여 워크플로우의 진행 상태를 확인할 수 있습니다.
![DataLakeblueprint09](/images/data_lake_blueprint_09.png)
![DataLakeblueprint10](/images/data_lake_blueprint_10.png)
![DataLakeblueprint11](/images/data_lake_blueprint_11.png)

10. bank-ingest 워크플로우가 성공적으로 완료되면 다음 실습을 진행합니다.
![DataLakeblueprint12](/images/data_lake_blueprint_12.png)

