---
title: "데이터 마트 생성"
weight: 46
pre: "<b>4-6. </b>"

---

정규화 구성이 되어있는 소스 데이터 모델을 비 정규화 데이터 모델로 재구성하여 분석 쿼리 실행의 성능을 최적화 합니다. 그리고 Athena, Redshift Spectrum, EMR Spark 등 데이터 레이크의 데이터를 활용하는 분석 서비스의 성능 및 비용을 최적화 하기 위해 csv 파일로 수집된 데이터를 parquet 형식으로 변환합니다.

데이터 모델을 재구성하고 변환하기 위해 AWS Glue의 ETL job을 작성하여 처리할 수도 있지만, 본 실습에서는 실습 데이터의 사이즈가 크지 않고 익숙한 SQL 기반으로 Athena 쿼리 명령문을 실행하여 데이터 마트를 생성합니다.

이번 태스크를 완료하면 다음과 같이 2개의 fact와 5개의 dimension으로 구성되는 star schema 데이터 모델이 생성 됩니다.
![DataLakeMart01](/images/data_lake_mart_01.png)
          
1. 다음 DDL 명령문을 Athena 쿼리 편집기로 복사하여 각 쿼리 명령문을 마우스로 하이라이트 선택하여 하나씩 실행합니다. 모든 쿼리 실행이 성공적으로 완료되면 lf-bank-db-bucket의 parquet 폴더 안에 fact 및 dimension 테이블의 데이터가 생성되고, Lake Formation의 **bank_db** 데이터베이스에 연관된 테이블이 생성됩니다.
![DataLakeMart02](/images/data_lake_mart_02.png)

{{% notice warning %}}
다음 DDL 명령문을 실행하기 전에 **`<your AWS account id>`** 문구는 현재 사용하고 있는 **AWS 계정 번호로 변경**해야 합니다. 
{{% /notice %}}

```sql
/*
 * Fact table for bank transactions
 */
CREATE TABLE fact_bank_transactions
WITH (
      external_location = 's3://lf-bank-db-bucket-<your AWS account id>/parquet/fact_bank_transactions/',
      format = 'Parquet',
      parquet_compression = 'SNAPPY')
AS 
SELECT 
bt.Transaction_id as trans_id, 
bt.Description as trans_desc, 
date_format(bt.Date, '%Y%m%d') as trans_date_key, 
cu.customer_id, acc.account_id, br.Branch_id,
(CASE WHEN bt.Transaction_Type='credit' THEN bt.Amount else bt.Amount*-1 END) as trans_amount, 
acc.Account_Balance as account_balance
FROM 
csv_bank_db_banking_transactions bt, 
csv_bank_db_customers cu, 
csv_bank_db_account_customers ac, 
csv_bank_db_accounts acc, 
csv_bank_db_branches br
WHERE 
bt.Customer_id = cu.Customer_id AND
cu.Customer_id = ac.Customer_id AND
ac.Account_id = acc.Account_id AND
acc.Branch_id = br.Branch_id;


/*
 * Fact table for credit card transactions
 */

CREATE TABLE fact_card_transactions
WITH (
      external_location = 's3://lf-bank-db-bucket-<your AWS account id>/parquet/fact_card_transactions/',
      format = 'Parquet',
      parquet_compression = 'SNAPPY')
AS 
SELECT 
cctr.Transaction_id as trans_id, 
cctr.CC_Number as credit_card_num,
cc.Customer_id as customer_id,
date_format(cctr.Transaction_Date, '%Y%m%d') as trans_date_key, 
cctr.Merchant_Details as merchant_details,
cctr.Amount as trans_amount
FROM
csv_bank_db_customers cu, csv_bank_db_credit_cards cc, csv_bank_db_cc_transactions cctr
WHERE
cu.Customer_id = cc.Customer_id AND
cc.CC_number = cctr.CC_Number;


/*
 * Dimension table for branches
 */
CREATE TABLE dim_branches
WITH (
      external_location = 's3://lf-bank-db-bucket-<your AWS account id>/parquet/dim_branches/',
      format = 'Parquet',
      parquet_compression = 'SNAPPY')
AS 
SELECT
br.Branch_id as branch_id,
br.Branch_Name as branch_name,
br.Street_Address as street_addr,
br.City as city,
br.State as state,
br.Zipcode as postcode,
br.Phone_Number as phone_number
FROM csv_bank_db_branches br;


/*
 * Dimension table for accounts
 */
CREATE TABLE dim_accounts
WITH (
      external_location = 's3://lf-bank-db-bucket-<your AWS account id>/parquet/dim_accounts/',
      format = 'Parquet',
      parquet_compression = 'SNAPPY')
AS 
SELECT
acc.Account_id as account_id, 
acc.Account_Type as account_type, 
acc.Date_Opened as date_opened
FROM csv_bank_db_accounts acc, csv_bank_db_account_type acct
WHERE acc.Account_Type = acct.Account_Type;


/*
 * Dimension table for customers
 */
CREATE TABLE dim_customers
WITH (
      external_location = 's3://lf-bank-db-bucket-<your AWS account id>/parquet/dim_customers/',
      format = 'Parquet',
      parquet_compression = 'SNAPPY')
AS 
SELECT 
cu.customer_id, 
concat(cu.First_Name, ' ', cu.Last_Name) as full_name, 
cu.Date_of_Birth as date_of_birth, 
cu.Street_Address as street_addr, 
cu.City as city, 
cu.Zipcode as postcode, 
cu.Email as email, 
cu.Sex as gender
FROM csv_bank_db_customers cu


/*
 * Dimension table for credit cards
 */
CREATE TABLE dim_credit_cards
WITH (
      external_location = 's3://lf-bank-db-bucket-<your AWS account id>/parquet/dim_credit_cards/',
      format = 'Parquet',
      parquet_compression = 'SNAPPY')
AS 
SELECT 
CC_number as credit_card_num, 
Maximum_Limit as max_limit,
Expiry_Date as expiry_date,
Credit_Score as credit_score
FROM csv_bank_db_credit_cards cc;
```

2. 추가적으로 데이터 마트에 필요한 Date dimension 테이블을 생성하기 위해 다음 [date.csv](https://aws-sample-data-repo.s3.amazonaws.com/lake-formation-workshop/sampledata/date.csv) 파일을 다운로드 받습니다.  
{{% button href="https://aws-sample-data-repo.s3.amazonaws.com/lake-formation-workshop/sampledata/date.csv" icon="fas fa-download" icon-position="left" %}}&nbsp;date.csv{{% /button %}}

3. Amazon S3 콘솔로 이동하여 **lf-bank-db-bucket**의 **csv** 폴더 안에 `csv_bank_db_date` 폴더를 만들고 다운로드 받은 csv 파일을 업로드 합니다.
![DataLakeMart03](/images/data_lake_mart_03.png)
![DataLakeMart04](/images/data_lake_mart_04.png)

4. Athena 콘솔로 돌아와서 다음 DDL 명령문을 실행하여 업로드 한 csv 데이터에 대한 External (외부) 테이블을 만들고 parquet 형식의 Date dimension 테이블을 생성합니다.
![DataLakeMart05](/images/data_lake_mart_05.png)

{{% notice warning %}}
중요: 다음 DDL 명령문을 실행하기 전에 `<your AWS account id>` 문구는 현재 사용하고 있는 AWS 계정 번호로 변경해야 합니다. 
{{% /notice %}}

```sql
 /*
 * Date table in CSV format
 */
CREATE EXTERNAL TABLE IF NOT EXISTS csv_bank_db_date (
  date_key string,
  year string,
  month string,
  month_name string,
  day string
)
ROW FORMAT SERDE 'org.apache.hadoop.hive.serde2.lazy.LazySimpleSerDe'
WITH SERDEPROPERTIES (
  'serialization.format' = ',',
  'field.delim' = ','
) LOCATION 's3://lf-bank-db-bucket-<your AWS account id>/csv/csv_bank_db_date/'
TBLPROPERTIES ('has_encrypted_data'='false', 'skip.header.line.count'='1');


/*
 * Dimension table for Date in parquet format
 */
CREATE TABLE dim_date
WITH (
      external_location = 's3://lf-bank-db-bucket-<your AWS account id>/parquet/dim_date/',
      format = 'Parquet',
      parquet_compression = 'SNAPPY')
AS 
SELECT * FROM csv_bank_db_date;
```
5. 모든 DDL 명령문이 성공적으로 실행 되었으면 2개의 fact 및 5개의 dimension 테이블이 목록에 표시됩니다. 이제 데이터 마트가 생성 되었습니다. 이제 다음 단계로 넘어가 데이터 보안 설정을 시작합니다.
![DataLakeMart06](/images/data_lake_mart_06.png)