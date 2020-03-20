---
title: "S3 스토리지 설정"
weight: 43
pre: "<b>4-3. </b>"

---

데이터가 수집되는 Amazon S3 버킷을 데이터 레이크의 스토리지로 등록합니다. 

1. 왼쪽 탐색 창에서 **Data lake locations**을 선택하고 **Register location**을 클릭 합니다.
![DataLakeStorage01](/images/data_lake_storage_01.png)

2. Amazon S3 경로는 기존 CloudFormation을 통해 생성된 S3 버킷의 경로를 입력합니다. 

3. IAM 역할의 경우 Lake Formation에서 생성한 기본 역할 (**AWSServiceRoleForLakeFormationDataAccess**)을 유지하여 Lake Formation이 S3 버킷에 데이터를 작성할 수 있도록 합니다. 다음 화면과 같이 설정 되었으면 **Register location** 버튼을 클릭합니다. 
![DataLakeStorage02](/images/data_lake_storage_02.png)

4. 다음과 같이 **lf-bank_db-bucket**이 데이터 레이크의 위치로 등록 되었는지 확인합니다. 
![DataLakeStorage03](/images/data_lake_storage_03.png)
