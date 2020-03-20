---
date: 2020-03-20
title: "AWS 설정"
weight: -20
---

## S3 환경 만들기

### S3 버킷 만들기
![bucket](/images/aws/create_bucket.png)

### 정적 웹 호스팅 설정하기
![Hosting](/images/aws/static_web_hosting.png)
> 에러 페이지는 404.html 파일로 가도록 설정합니다. (에러 나더라도 문제 없도록, 또는 index.html로 리다이렉트 하게 하거나)

## Route53 설정

### 서브 도메인 설정
![Route53](/images/aws/route53.png)

## 배포
Hugo CLI나 AWS CLI로 진행하거나, 컴파일된 public 폴더 안에 있는 컨텐츠를 업로드 함.
`hugo deploy --target=s3`
`aws s3 sync ./public s3://hugo.awsdemo.kr --acl public-read`

## CICD
간단한 방법들
