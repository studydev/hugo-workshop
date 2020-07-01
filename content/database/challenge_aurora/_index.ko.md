---
date: 2020-06-01
title: "도전 과제 - RDS Aurora 연결"
weight: 360
pre: "<b>3-6. </b>"
---

{{% notice note %}}
일반적인 Database 관리/운영에 사용되는 MySQL CLI를 통한 RDS 연결을 수행해 보도록 하겠습니다. 
{{% /notice %}}

이를 위해서는,
1. VPC-Lab내의 Public Subnet에 만들어 둔 AMI로 EC2 인스턴스를 생성합니다. 이 때, 네트워킹 옵션에서 Public IP를 허용해 주어야 합니다.
2. RDS Aurora의 보안 그룹 설정을 변경해 줍니다. 새로 생성한 EC2 인스턴스의 보안 그룹을 소스로 허용하도록 구성합니다.
3. 방금 생성한 EC2 인스턴스에 SSH로 로그인한 후, MySQL Client를 통하여 RDS Aurora에 접속합니다. EC2 Web Server에는 EC2 배포과정에서 이미 MySQL Client가 설치되어 있습니다.

위 항목을 직접 구성해 보시는 것이 바로 도전 과제가 되겠습니다. 이후 가이드는 아래를 참조해 주세요.

```ssh
$ ssh -i AWS-ImmersionDay-Lab.pem ec2-user@”EC2 Host FQDN or IP”
Last login: Sun Feb 18 14:41:59 2018 from 112.148.83.236

       __|  __|_  )
       _|  (     /   Amazon Linux AMI
      ___|\___|___|

https://aws.amazon.com/amazon-linux-ami/2017.09-release-notes/


$ mysql -u awsuser -pawspassword -h awsdb.ccjlcjlrtga1.ap-northeast-2.rds.amazonaws.com

Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 34
Server version: 5.6.10 MySQL Community Server (GPL)


Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| immersionday       |
| mysql              |
| performance_schema |
+--------------------+
4 rows in set (0.01 sec)

mysql> use immersionday;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql> show tables;
+------------------------+
| Tables_in_immersionday |
+------------------------+
| address                |
+------------------------+
1 row in set (0.01 sec)

mysql> select * from address;
+----+-------+--------------+---------------------+
| id | name  | phone        | email               |
+----+-------+--------------+---------------------+
|  1 | Bob   | 630-555-1254 | bob@fakeaddress.com |
|  2 | Alice | 571-555-4875 | alice@address2.us   |
+----+-------+--------------+---------------------+
2 rows in set (0.00 sec)

mysql>
```