---
title: "Bank Analyst"
weight: 49
---

{{% notice note %}}
Bank Analyst는 은행 거래와 관련된 테이블만 액세스 할 수 있으며 고객 테이블의 개인 식별 정보는 조회 할 수 없습니다. 
{{% /notice %}}

이번 태스크를 수행하기 위해 **lf-bank-analyst** IAM 사용자로 AWS 계정에 로그인 합니다 (기본 비밀번호: `Welcome1`).

1. AWS 콘솔에서 **Amazon Athena**를 검색하여 Athena 콘솔로 이동합니다.
![BankAnalyst01](/images/bank_analyst_01.png)

2. **lf-bank-analyst**로 로그인 되었는지 한 번 더 확인합니다. 다른 사용자로 로그인 되었다면 로그아웃을 하신 이후 **lf-bank-analyst**로 로그인 하십시오.
![BankAnalyst02](/images/bank_analyst_02.png)

3. **bank_db** 데이터베이스의 테이블 목록을 보시면 **lf-bank-analyst** 사용자는 **dim_accounts**, **dim_branches**, **dim_customers**, **dim_date**와 **fact_bank_transactions** 테이블만 표시 되어 있습니다. 그리고 **dim_customer** 의 컬럼 목록에는 개인 정보 컬럼을 제외한 나머지 컬럼만 표시 되어 있습니다.
4. **lf-bank-analyst** 사용자가 Athena를 처음 사용하기 때문에 쿼리 결과를 저장할 S3 위치를 설정하라는 메시지가 표시됩니다. 링크를 클릭합니다.
![BankAnalyst03](/images/bank_analyst_03.png)

5. CloudFormation 스택의 Outputs 탭 화면에 출력 된 Athena 쿼리 결과 S3 버킷 경로를 입력하고, 버킷 이름 맨 끝에 “/”를 입력하고 저장합니다. (예시: `s3://lf-athena-query-results-xxxxxxxx/`)
![BankAnalyst04](/images/bank_analyst_04.png)

6. 화면 왼쪽 상단에서 **Saved Queries** (저장된 쿼리)를 선택하고 **LF-BankAnalyst-Query**를 선택하면 쿼리 편집기로 쿼리가 복사됩니다.
![BankAnalyst05](/images/bank_analyst_05.png)
![BankAnalyst06](/images/bank_analyst_06.png)

7. 각 쿼리를 마우스로 드래그 앤 드랍으로 **하이라이트 영역으로 선택**하여 한 번에 하나씩 실행합니다. 처음 세 개의 쿼리를 실행하면 몇 초 내에 결과가 나타납니다.
![BankAnalyst07](/images/bank_analyst_07.png)

8. 네 번째 쿼리를 실행하면 **bank analyst**가 고객의 개인 정보 컬럼에 접근 권한이 없기 때문에 다음과 같은 오류 메시지가 나타납니다.
![BankAnalyst08](/images/bank_analyst_08.png)

9. 마지막 쿼리를 실행하면 **bank analyst**가 신용 카드 거래와 관련 된 테이블에 접근 권한이 없기 때문에 다음과 같은 오류 메시지가 나타납니다.
![BankAnalyst09](/images/bank_analyst_09.png)
