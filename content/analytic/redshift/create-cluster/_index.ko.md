---
title: "Redshift 클러스터 생성"
weight: 56
---

{{% notice note %}}
이번 실습에서는 Redshift Spectrum 쿼리를 실행하기 위해 필요한 기본적인 Redshift 클러스터 환경을 구성합니다.
{{% /notice %}}

{{% notice warning %}}
Redshift 클러스터는 **AdministratorAccess** 권한을 가진 관리자로 **생성**합니다.
{{% /notice %}}


1. [Redshift 콘솔](https://console.aws.amazon.com/redshift/)로 이동하여, **AdministratorAccess 권한**이 있는 사용자로 로그인을 합니다. 

2. Redshift 메인 페이지가 보이면 **Create cluster**(클러스터 생성)를 클릭합니다.
![AnalyticRedshiftCreate01](/images/analytic_redshift_create_01.png)

3. **Cluster configuration** (클러스터 구성) 팝업 화면에서 다음과 같이 설정합니다.

| Key | Value |
|:---|:---|
| Node type | `d2.large` |
| Nodes | `1` |
| Cluster identifier | `redshift-lakeformation-demo` |
| Database name | `dev` |
| Database port | `5439` |
| Master user name | `awsuser` |
| Master user password | `Welcome1!` |
| Available IAM roles	| `RedshiftLakeFormationRole` |

![AnalyticRedshiftCreate02](/images/analytic_redshift_create_02.png)
![AnalyticRedshiftCreate03](/images/analytic_redshift_create_03.png)

{{% notice warning %}}
**RedshiftLakeFormationRole**을 선택한 후에 **Add IAM role** 버튼을 **반드시 클릭** 해야 합니다. **Add IAM role을 클릭**하면 다음과 같이 **RedshiftLakeFormationRole**이 클러스터와 연동됩니다.
{{% /notice %}}
![AnalyticRedshiftCreate04](/images/analytic_redshift_create_04.png)

4. 설정을 완료했으면 Redshift 클러스터를 생성합니다. 
![AnalyticRedshiftCreate05](/images/analytic_redshift_create_05.png)

5. 클러스터의 상태가 **Available**이 될 때까지 잠시 대기합니다. 상태가 변경될 때까지 새로 고침 아이콘을 주기적으로 클릭합니다. 


