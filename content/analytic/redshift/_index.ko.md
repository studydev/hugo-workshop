---
title: "Redshift를 이용한 분석"
weight: 55
pre: "<b>5-2. </b>"
---

**Amazon Redshift**는 MPP 아키텍처 기반의 데이터 웨어하우스 (DW) 서비스 입니다. Redshift를 사용하여 Petabyte 급의 전형적인 DW 환경을 구축하여 운영할 수 있습니다. Redshift는 또한 Redshift Spectrum이라는 기능을 통해 잠재적으로 Exabyte 급의 데이터가 저장된 Amazon S3 기반의 데이터 레이크로 쿼리를 확장하여 집계할 수도 있습니다. 이번 실습에서는 Redshift Spectrum을 설정하여 데이터 레이크를 쿼리하기 위해 다음 단계를 수행합니다.
- [Redshift 클러스터 IAM 역할 설정](/analytic/redshift/create-cluster-iam)
- [Redshift 클러스터 생성](/analytic/redshift/create-cluster)
- [Lake Formation 카탈로그 보안 설정](/analytic/redshift/lake-formation-catalog)
- [Redshift Spectrum 쿼리 실행](/analytic/redshift/spectrum-query)

{{% notice warning %}}
Redshift 클러스터는 **Administrator** (관리자)로 **생성**하고 Redshift 쿼리 편집기에는 **Bank Analyst**로 로그인하여 **쿼리**를 실행 합니다.
{{% /notice %}}