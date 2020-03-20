---
title: "Lake Formation 카탈로그 보안 설정"
weight: 57
---


{{% notice note %}}
이번 실습에서는 Redshift 사용자가 Lake Formation 카탈로그를 통해 데이터 레이크를 쿼리하기 위한 보안 설정을 수행합니다.
{{% /notice %}}

1. [Lake Formation 콘솔](https://console.aws.amazon.com/lakeformation/)로 이동하여 데이터레이크 관리자, `lf-admin` 로 로그인을 합니다. 

2. Lake Formation 왼쪽 탐색 창에서 **Data Permissions**을 클릭 합니다. 
![AalyticRedshiftCatalogSecurity01](/images/analytic_redshift_catalog_security_01.png)

3. **Grant** 버튼을 클릭합니다. 
![AalyticRedshiftCatalogSecurity02](/images/analytic_redshift_catalog_security_02.png)

4. **Grant Permissions** 팝업 창이 뜨면, IAM users and roles drop-down 옵션에서 `RedshiftLakeFormationRole`을 선택합니다. Database는 `bank_db`를 선택하고, Table은 `dim_accounts`, `dim_branches`, `dim_date`와 `fact_bank_transactions`을 선택합니다. 그리고 Table permissions은 **Select** 권한만 선택하고 **Grant**를 클릭하여 권한을 부여합니다. 
![AalyticRedshiftCatalogSecurity03](/images/analytic_redshift_catalog_security_03.png)

5. 추가 권한을 부여하기 위해 Data permissions에서 **Grant** 버튼을 클릭합니다. 
![AalyticRedshiftCatalogSecurity02](/images/analytic_redshift_catalog_security_02.png)

6. Grant Permissions 팝업 창이 뜨면, IAM users and roles drop-down 옵션에서 `RedshiftLakeFormationRole`을 선택합니다. Database는 `bank_db`를 선택하고, Table은 `dim_customers`을 선택하되 개인정보 (PII)가 포함된 컬럼은 Exclude(제외)합니다. 그리고 Table permissions은 **Select** 권한만 선택하고 **Grant**를 클릭하여 권한을 부여합니다.
![AalyticRedshiftCatalogSecurity04](/images/analytic_redshift_catalog_security_04.png)
![AalyticRedshiftCatalogSecurity05](/images/analytic_redshift_catalog_security_05.png)


7. **dim_customers** 테이블의 경우 **RedshiftLakeFormationRole** 역할은 개인 식별 정보(PII)가 포함되어 있지 않는 컬럼만 조회 할 수 있습니다.