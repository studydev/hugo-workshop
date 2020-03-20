---
title: "데이터 검증"
weight: 45
pre: "<b>4-5. </b>"

---

1. Lake Formation의 워크플로우를 통해 Bank 데이터베이스의 원본 데이터가 자동으로 추출 되어 S3 데이터 레이크에 csv 형식으로 저장되었습니다.
![DataLakeVerify01](/images/data_lake_verify_01.png)
![DataLakeVerify02](/images/data_lake_verify_02.png)

2. 그리고 수집된 데이터는 자동으로 카탈로그화 되어 **bank_db**의 테이블 목록에 표시됩니다.
![DataLakeVerify03](/images/data_lake_verify_03.png)

3. 데이터가 잘 수집되었는지 간단하게 카탈로그화 된 테이블의 샘플 데이터를 확인해 보겠습니다. 예시로 **csv_bank_db_banking_transactions** 테이블을 클릭합니다.
![DataLakeVerify04](/images/data_lake_verify_04.png)

4. **Action** 메뉴에서 **View Data**를 클릭하면 Amazon Athena 콘솔로 이동하게 됩니다.
![DataLakeVerify05](/images/data_lake_verify_05.png)

5. Athena를 처음 사용하는 경우 쿼리 결과를 저장할 S3 위치를 설정하라는 메시지가 표시됩니다. 링크를 클릭합니다.
![DataLakeVerify06](/images/data_lake_verify_06.png)

6. CloudFormation 스택의 Outputs 탭 화면에 출력된 Athena 쿼리 결과 S3 버킷 경로를 입력하고, 버킷 이름 맨 끝에 “/”를 입력하고 저장합니다. (예시: `s3://lf-athena-query-results-xxxxxxxx/`)
![DataLakeVerify07](/images/data_lake_verify_07.png)

7. 수집 된 각 테이블의 샘플 데이터를 확인하려면 테이블 이름 옆에 **Preview table** 옵션을 선택하여 샘플 쿼리를 실행할 수 있습니다.
![DataLakeVerify08](/images/data_lake_verify_08.png)
{{% notice tip %}}
Lake Formation의 Blueprint는 데이터 수집 프로세스를 위해 몇 개의 임시 테이블을 생성하며 모든 임시 테이블 이름은 접두사(prefix)로 밑줄(_)로 시작합니다 (예: _csv_bank_db_customers, _temp_csv_bank_db_customers). 계속해서 진행되는 실습에서는 모든 임시 테이블을 제외합니다.
{{% /notice %}}