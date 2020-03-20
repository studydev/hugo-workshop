---
title: "Redshift Spectrum 쿼리 실행"
weight: 58
---

{{% notice note %}}
**Amazon Redshift Spectrum**은 Data Lake (S3)에 저장된 데이터에 대한 복잡한 쿼리를 로드하거나 다른 데이터를 준비할 필요 없이 바로 실행할 수 있습니다.
{{% /notice %}}

1. IAM 콘솔에서 **로그아웃**을 하고, [Redshift 콘솔](https://console.aws.amazon.com/redshift/)로 이동하여 **lf-bank-analyst** 사용자로 **로그인**을 합니다.

2. Redshift 콘솔 메뉴에서 **Editor** (편집기)를 클릭합니다. 그 다음, **Connect to database** (데이터베이스 연결) 팝업 창에서 Redshift 클러스터의 연결 정보로 클러스터에 연결 합니다.

| Key | Value |
|:---|:---|
| Cluster identifier | `redshift-lakeformation-demo` |
| Database name | `dev` |
| Master user name | `awsuser` |
| Master user password | `Welcome1!` |

![AalyticRedshiftSpectrum01](/images/analytic_redshift_spectrum_01.png)

3. **Query 1** 탭의 텍스트 상자 안에 **External Schema** (외부 스키마)를 생성하기 위해 다음 DDL 명령문을 실행하여 Lake Formation의 **bank_db** 데이터베이스를 Amazon Redshift의 **lf_schema** 외부 스키마와 매핑 합니다.
![AalyticRedshiftSpectrum02](/images/analytic_redshift_spectrum_02.png)

{{% notice warning %}}
다음 DDL 명령문을 실행할 때 `<account-id>`를 현재 사용하고 있는 AWS 계정 번호로 변경하고, region을 현재 사용하고 있는 AWS Region의 API 값으로 변경해야 합니다. 현재 사용하고 있는 리전이 버지니아 북부라면 **us-east-1** 으로 교체하십시오.
{{% /notice %}}

```sql
create external schema if not exists lf_schema
    from DATA CATALOG database 'bank_db'
    iam_role 'arn:aws:iam::<account-id>:role/RedshiftLakeFormationRole'
    region 'us-east-1';
```

4. 다음 SELECT 명령문을 실행하면, 쿼리 결과에 외부 스키마에 속한 Lake Formation 테이블을 확인할 수 있습니다. 
```sql
select * from svv_external_tables;
```
![AalyticRedshiftSpectrum03](/images/analytic_redshift_spectrum_03.png)

5. **Select Schema**(스키마 목록)에서 **lf_schema**를 선택하면, 이전에 Lake Formation에서 RedshiftLakeFormationRole에게 Select 권한을 부여 한 테이블만 목록에 표시됩니다.
![AalyticRedshiftSpectrum04](/images/analytic_redshift_spectrum_04.png)

6. 이전 Athena 콘솔에서 실행했던 Bank Analyst (lf-bank-analyst)의 쿼리를 실행하여 Lake Formation에서 정의한 권한이 적용되는지 결과를 확인합니다. 

다음 쿼리를 **Query 1** 탭에서 텍스트 상자에 붙여 놓고 각 쿼리를 **마우스로 하이라이트 선택**하여 한 번에 하나씩 실행합니다. 처음 세 개의 쿼리를 실행하면 성공적으로 쿼리 결과가 하단에 나타납니다.
```sql
/* 
 * 1. 가장 많은 자산을 보유하고 있는 Top 10 은행 지점?
 */
SELECT br.branch_name, br.state, sum(f.trans_amount) as total_amount
  FROM lf_schema.fact_bank_transactions f, lf_schema.dim_branches br
 WHERE f.branch_id = br.branch_id
GROUP BY br.branch_name, br.state
ORDER BY total_amount desc
LIMIT 10;

/* 
 * 2. 가장 많은 고객을 유치하고 있는 Top 10 은행 지점은?
 */
SELECT br.branch_name, br.state, count(distinct f.customer_id) as NumberOfCustomers
  FROM lf_schema.fact_bank_transactions f, lf_schema.dim_branches br
 WHERE f.branch_id = br.branch_id
GROUP BY br.branch_name, br.state
ORDER BY NumberOfCustomers desc;  

/* 
 * 3. 저축 예금 계좌의 잔고가 10만불 이상인 고객은? 
 */
SELECT distinct fc.customer_id, cu.full_name as customer_name, 
       fc.account_id, ac.account_type, fc.account_balance
FROM lf_schema.fact_bank_transactions fc, 
     lf_schema.dim_accounts ac, 
     lf_schema.dim_customers cu
 WHERE fc.account_id = ac.account_id
   AND fc.customer_id = cu.customer_id
   AND ac.account_type = 'savings'
   AND fc.account_balance > 100000
ORDER BY fc.account_balance desc;
          
/*
 * 4. 권한이 없는 개인 정보 데이터에 액세스 시도를 했을때 오류 발생
 */
SELECT cu.customer_id, cu.full_name, cu.date_of_birth, 
       cu.email, cu.gender, fb.trans_amount, fb.account_balance
  FROM lf_schema.fact_bank_transactions fb, lf_schema.dim_customers cu
 WHERE fb.customer_id = cu.customer_id

/*
 * 5. 권한이 없는 테이블에 액세스 시도를 했을때 오류 발생
 */
SELECT * FROM lf_schema.fact_card_transactions LIMIT 10;
```
![AalyticRedshiftSpectrum05](/images/analytic_redshift_spectrum_05.png)
![AalyticRedshiftSpectrum06](/images/analytic_redshift_spectrum_06.png)

7. 네 번째 쿼리를 실행하면 **bank analyst**가 고객의 개인 정보 컬럼에 접근 권한이 없기 때문에 다음과 같은 오류 메시지가 나타납니다.
![AalyticRedshiftSpectrum07](/images/analytic_redshift_spectrum_07.png)

8. 마지막 쿼리를 실행하면 **bank analyst**는 신용 카드 거래와 관련된 테이블에 접근 권한이 없기 때문에 다음과 같은 오류 메시지가 나타납니다.
![AalyticRedshiftSpectrum08](/images/analytic_redshift_spectrum_08.png)

