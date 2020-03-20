---
title: "Supervisor"
weight: 50
---

{{% notice note %}}
Supervior는 은행 및 신용카드 거래와 관련된 모든 테이블에 액세스할 수 있으며 고객 테이블의 개인 식별 정보 또한 조회할 수 있는 권한이 있습니다. 
{{% /notice %}}

이번 태스크를 수행하기 위해 **lf-supervisor** IAM 사용자로 AWS 계정에 로그인 합니다 (기본 비밀번호: `Welcome1`).

1. Athena 콘솔에 **lf-supervisor** 로 로그인 되었는지 한 번 더 확인합니다. 다른 사용자로 로그인 되어 있다면 로그아웃 한 이후에 **lf-supervisor**로 로그인 하십시오.
![Supervisor01](/images/supervisor_01.png)

2. **bank_db** 데이터베이스의 테이블 목록을 보면 모든 fact 테이블 및 dimension 테이블이 표시됩니다. 그리고 dim_customer 테이블을 확장하면 모든 개인 정보 컬럼 또한 표시 되어 있습니다.
![Supervisor02](/images/supervisor_02.png)

3. **lf-supervisor** 사용자가 Athena를 처음 사용하기 때문에 쿼리 결과를 저장할 S3 위치를 설정하라는 메시지가 표시됩니다. 링크를 클릭합니다.
![Supervisor03](/images/supervisor_03.png)

4. CloudFormation 스택의 Outputs 탭 화면에 출력된 Athena 쿼리 결과 S3 버킷 경로를 입력하고, 버킷 이름 맨 끝에 “/”를 입력하고 저장합니다. (예시: `s3://lf-athena-query-results-xxxxxxxx/`)
![Supervisor04](/images/supervisor_04.png)

5. 화면 왼쪽 상단에서 **Saved Queries** (저장된 쿼리)를 선택하고 LF-Supervisor-Query를 선택하면 쿼리 편집기로 쿼리가 복사됩니다.
![Supervisor05](/images/supervisor_05.png)
![Supervisor06](/images/supervisor_06.png)

6. 각 쿼리를 **마우스로 하이라이트 선택**하여 한 번에 하나씩 실행합니다. 쿼리를 실행하면 몇 초 내에 결과가 나타납니다.
![Supervisor07](/images/supervisor_07.png)
![Supervisor08](/images/supervisor_08.png)
![Supervisor09](/images/supervisor_09.png)

7. 예상대로 **Supervisor**의 쿼리는 모든 테이블에 데이터를 조회할 수 있습니다. 이렇게 해서 Lake Formation의 액세스 권한 모델이 잘 동작하는 것을 확인 하였습니다.
