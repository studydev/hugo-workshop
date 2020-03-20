---
title: "데이터 보안 설정"
weight: 47
pre: "<b>4-7. </b>"

---

AWS Lake Formation의 주요 기능 중 하나는 데이터 레이크의 보안을 중앙에서 통제 할 수 있는 것입니다. Lake Formation은 IAM 기반의 권한 모델을 보강하는 자체 권한 모델을 제공합니다.
 
다음 실습에서는 데이터 레이크로 수집 및 큐레이션된 bank 데이터에 대한 세분화된 엑세스 정책을 **bank_db** 카탈로그의 테이블 및 컬럼을 통해 정의합니다.

본 실습에서 사용할 사용자는 이미 CloudFormation을 통해 생성되어 있습니다. 
- **Admin (lf-admin)**: 데이터 레이크 관리자 권한이 있는 사용자입니다. 관리자는 데이터 카탈로그 및 데이터 저장 위치에 액세스 할 수 있고 Lake Formation의 모든 기능을 사용할 수 있는 추가 권한을 보유합니다.
- **Bank Analyst (lf-bank-analyst)**: 은행 거래와 관련된 테이블에 모두 엑세스 할 수 있는 사용자 입니다. 단 이 사용자는 고객 테이블에서 개인 식별 정보 (이메일, 생년월일, 주소 등)에 해당하는 데이터 액세스는 제한 됩니다.
- **Credit Card Analyst (lf-card-analyst)**: 신용 카드 거래와 관련된 테이블에 모두 액세스 할 수 있는 사용자 입니다. 단 이 사용자는 고객 테이블에서 개인 식별 정보 (이메일, 생년월일, 주소 등)에 해당하는 데이터 액세스는 제한 됩니다.
- **Supervisor (lf-supervisor)**: 은행 및 신용카드 거래와 관련된 모든 테이블에 액세스 할 수 있는 사용자 입니다. 이 사용자는 고객의 개인 식별 정보 또한 액세스 할 수 있습니다.

----

### Bank Analyst 사용자

1. Lake Formation 콘솔 왼쪽 탐색 창에서 **Data Permissions**을 선택 합니다. 
![DataLakeSecurity01](/images/data_lake_security_01.png)

2. **Grant** 버튼을 클릭합니다. 
![DataLakeSecurity02](/images/data_lake_security_02.png)

3. **Grant Permissions** 팝업 창이 뜨면, IAM users and roles drop-down 옵션에서 **lf-bank-analyst**을 선택합니다. Database는 **bank_db**를 선택하고, Table은 **dim_accounts**, **dim_branches**, **dim_date**와 **fact_bank_transactions**을 선택합니다. 그리고 Table permissions은 **Select** 권한만 선택하고 **Grant**를 클릭하여 권한을 부여합니다. 
![DataLakeSecurity03](/images/data_lake_security_03.png)

4. 추가 권한을 부여하기 위해 Data permissions에서 **Grant** 버튼을 클릭합니다. 
![DataLakeSecurity02](/images/data_lake_security_02.png)

5. **Grant Permissions** 팝업 창이 뜨면, IAM users and roles drop-down 옵션에서 **lf-bank-analyst**을 선택합니다. Database는 **bank_db**를 선택하고, Table은 **dim_customers**을 선택하되 개인정보 (PII)가 포함된 컬럼(생일, 주소, 거주지, 우편번호, 이메일, 성별 등)은 Exclude(제외)합니다. 그리고 Table permissions은 **Select** 권한만 선택하고 **Grant**를 클릭하여 권한을 부여합니다.
![DataLakeSecurity04](/images/data_lake_security_04.png)
![DataLakeSecurity05](/images/data_lake_security_05.png)

6. **dim_customers** 테이블의 경우 **lf-bank-analyst** 사용자는 개인 식별 정보(PII)가 포함되어 있지 않은 컬럼만 조회 할 수 있습니다.

----

### Credit Card Analyst 사용자

1. Data permissions에서 **Grant** 버튼을 클릭합니다. 
![DataLakeSecurity02](/images/data_lake_security_02.png)

2. Grant Permissions 팝업 창이 뜨면, IAM users and roles drop-down 옵션에서 **lf-card-analyst**을 선택합니다. Database는 **bank_db**를 선택하고, Table은 **dim_credit_cards**, **dim_date**와 **fact_card_transactions**을 선택합니다. 그리고 Table permissions은 **Select** 권한만 선택하고 **Grant를** 클릭하여 권한을 부여합니다. 
![DataLakeSecurity06](/images/data_lake_security_06.png)

3. 추가 권한을 부여하기 위해 Data permissions에서 **Grant** 버튼을 클릭합니다. 
![DataLakeSecurity02](/images/data_lake_security_02.png)

4. Grant Permissions 팝업 창이 뜨면, IAM users and roles drop-down 옵션에서 **lf-card-analyst**을 선택합니다. Database는 **bank_db**를 선택하고, Table은 **dim_customers**을 선택하되 개인정보 (PII)가 포함된 컬럼은 **Exclude**(제외)합니다. 그리고 Table permissions은 **Select** 권한만 선택하고 **Grant를** 클릭하여 권한을 부여합니다.
![DataLakeSecurity07](/images/data_lake_security_07.png)
![DataLakeSecurity05](/images/data_lake_security_05.png)

5. **dim_customers** 테이블의 경우 **lf-card-analyst** 사용자는 개인 식별 정보(PII)가 포함되어 있지 않는 컬럼만 조회 할 수 있습니다.

----

### Supervisor 사용자

1.	Data permissions에서 **Grant** 버튼을 클릭합니다.
![DataLakeSecurity02](/images/data_lake_security_02.png)

2.	Grant Permissions 팝업 창이 뜨면, IAM users and roles drop-down 옵션에서 **lf-supervisor**을 선택합니다. Database는 **bank_db**를 선택하고, Table은 **모든 fact 테이블과 dimension 테이블을 선택**합니다. 그리고 Table permissions은 **Select** 권한만 선택하고 **Grant**를 클릭하여 권한을 부여합니다.
![DataLakeSecurity08](/images/data_lake_security_08.png)
지금까지 총 3명의 사용자의 대한 데이터 액세스 권한을 부여하였고, 이제 분석 단계에서 Amazon Athena 콘솔로 이동하여 각 사용자의 권한을 테스트 해 볼 수 있습니다.