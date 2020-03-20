---
title: "Redshift 클러스터 IAM 역할 설정"
weight: 55
---

{{% notice note %}}
이번 실습에서는 Redshift에서 Lake Formation 기반의 데이터 레이크를 액세스하기 위해 필요한 IAM 권한을 설정합니다.
{{% /notice %}}

1. 관리자 권한(**AdministratorAccess**)이 있는 사용자로 [IAM 콘솔](https://console.aws.amazon.com/iam/)로 로그인을 합니다.  

2. 탐색 창에서 **Policies** (정책)을 선택합니다. 만약 정책을 처음 선택하는 경우 **Welcome to Managed Policies**(관리형 정책 시작) 페이지가 나타납니다. 그럴 경우 **Get Started**(시작)을 선택하십시오.   

3. **Create policy** (정책 생성)을 선택합니다.  

4. **JSON 탭**을 선택합니다.  

5. 다음 JSON 정책 문서를 붙여 넣습니다.
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "lakeformation:GetDataAccess",
                "glue:GetTable",
                "glue:GetTables",
                "glue:SearchTables",
                "glue:GetDatabase",
                "glue:GetDatabases",
                "glue:GetPartitions"
            ],
            "Resource": "*"
        }
    ]
}
```
6. 완료되면 **Review**(정책 검토)를 선택하여 정책을 검토합니다. 

7. **Review policy**(정책 검토) 페이지에서 정책의 이름을 `RedshiftLakeFormationPolicy`로 입력하고, 옵션으로 간단한 설명을 Description 필드안에 입력합니다. 그런 다음, Summary (요약)에서 해당 정책이 부여한 권한을 확인하고 **Create policy**(정책 생성)을 선택하여 정책을 생성합니다.
![AnalyticRedshiftIam01](/images/analytic_redshift_iam_01.png)

8. IAM 콘솔의 탐색 창에서 **Roles**(역할)을 선택한 다음 **Create role**(역할 생성)을 선택합니다.

9. Select type of trusted entity(신뢰할 수 있는 유형의 개체) 선택에서 **AWS Service**를 선택합니다. Choose the service that will use this role (이 역할을 사용할 서비스 선택)에서 **Redshift**를 선택합니다. Select your use case(사용 사례 선택)에서 **Redshift Customizable**를 선택한 다음 **Next: Permission**(다음: 권한)을 클릭합니다.
![AnalyticRedshiftIam02](/images/analytic_redshift_iam_02.png)

10. 이전에 생성한 **RedshiftLakeFormationPolicy** 정책을 검색하고 목록에서 정책 이름 옆에 있는 선택란을 선택 합니다. 그리고 **Next: Tags**(다음:태그)를 클릭합니다.
![AnalyticRedshiftIam03](/images/analytic_redshift_iam_03.png)

11. **Next: Review** (다음: 검토)를 선택합니다.

12. Role name (역할 이름)에 `RedshiftLakeFormationRole`을 입력하고 **Create role**(역할 생성)을 클릭합니다.

13. 다음은 Bank Analyst (lf-bank-analyst) 사용자가 Redshift에서 쿼리를 실행할 수 있도록 필요한 권한을 부여합니다. 탐색 창에서 **Users**(사용자)를 선택하고 **lf-bank-analyst** 사용자를 클릭합니다. 
![AnalyticRedshiftIam04](/images/analytic_redshift_iam_04.png)

14. 다음 **Managed policy**(관리형 정책)을 정책에 추가합니다.
- `AmazonRedshiftReadOnlyAccess`
- `AmazonRedshiftQueryEditor`
![AnalyticRedshiftIam05](/images/analytic_redshift_iam_05.png)
![AnalyticRedshiftIam06](/images/analytic_redshift_iam_06.png)
![AnalyticRedshiftIam07](/images/analytic_redshift_iam_07.png)
![AnalyticRedshiftIam08](/images/analytic_redshift_iam_08.png)
