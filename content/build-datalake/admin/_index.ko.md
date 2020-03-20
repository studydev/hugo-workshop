---
title: "관리자 설정"
weight: 41
pre: "<b>4-1. </b>"

---

데이터 레이크 관리자는 데이터 카탈로그 리소스에 대한 모든 권한을 IAM 사용자 또는 IAM 역할에 부여할 수 있습니다. 데이터 레이크를 구축하기 위한 가장 처음 단계는 데이터 레이크 관리자를 데이터 카탈로그의 첫 번째 사용자로 설정하는 것 입니다.

1.	현재 사용 중인 AWS 콘솔에서 **로그아웃**하고 CloudFormation 스택의 Output 탭 화면에 출력 된 로그인 링크를 사용하여 **lf-admin** 사용자 (기본 비밀번호: `Welcome1`)로 로그인 합니다.
{{% notice warning %}}
중요: 반드시 **lf-admin** 으로 로그인을 해야 합니다. 이후 실습에서 권한 문제로 진행이 되지 않을 수 있습니다.
{{% /notice %}}

2.	[Lake Formation 콘솔](https://console.aws.amazon.com/lakeformation)로 이동합니다.

3.	Lake Formation의 모든 리소스에 액세스 할 수 있도록 데이터 레이크 관리자를 설정합니다. **Add Administrators** 버튼을 클릭하여 데이터 레이크 관리자를 설정합니다.
![DatalakeAdmin01](/images/build_datalake_admin_01.png)

4.	IAM users and roles 드롭 다운 목록에서 **lf-admin** IAM 사용자를 선택하고 **Save** 버튼을 클릭합니다. 이 사용자는 데이터 레이크 관리자로 설정 되었기 때문에, 앞으로 남은 실습 동안 데이터 레이크에 대한 모든 액세스 권한을 갖습니다. 
![DatalakeAdmin02](/images/build_datalake_admin_02.png)

