---
date: 2020-06-01
title: "버킷에 오브젝트 추가하기"
weight: 420
pre: "<b>4-2. </b>"
---

{{% notice note %}}
버킷이 정상적으로 생성 되었다면 오브젝트를 추가할 준비가 되었습니다. 오브젝트는 텍스트 파일, 이미지 파일 및 비디오 파일 등 모든 종류의 파일이 될 수 있습니다. Amazon S3에 파일을 추가할 때, 해당 파일에 대한 권한 및 접근 설정 등에 대한 정보를 메타데이터에 포함시킬 수 있습니다.
{{% /notice %}}

1. 본 Lab에서는 S3를 통해, 정적 웹사이트를 호스팅합니다. 해당 정적 웹사이트는 특정 이미지를 클릭하면 VPC Lab에서 생성한 인스턴스로 리다이렉팅하는 역할을 합니다. 따라서 **이미지( [이미지 다운로드](https://github-connection.s3.ap-northeast-2.amazonaws.com/immersion-day/aws.png))** 와 생성한 로드밸런서의 **DNS 이름**을 준비합니다.  

2. 메모장이나 코드 편집기에 아래의 코드를 입력한 후, 파일 이름을 ***index.html*** 로 저장하십시오.  
   [이 링크](https://github-connection.s3.ap-northeast-2.amazonaws.com/immersion-day/index.txt)를 클릭하여 실제 구성하는 방법을 참조하세요.
   ```html
    <html>
        <head>
            <meta charset="utf-8">
            <title> AWS General Immersion Day S3 HoL </title>
        </head>
        <body>
            <center>
            <br>
            <h2> Click image to be redirected to the EC2 instance that you created </h2>
            <img src="S3에 업로드될 이미지 접근 URL" onclick="window.location='DNS 이름'"/>
            </center>
        </body>
    </html>
    ```

1. Amazon S3 Console에서 오브젝트를 업로드하고자 하는 버킷을 선택하십시오(버킷 아이콘이 아닌 버킷 이름의 링크 클릭). 해당 버킷에 파일(오브젝트)를 업로드 하거나, 하위 폴더(디렉토리)를 만들 수 있습니다. **Upload** 버튼을 선택하면 업로드 대화상자가 열립니다.

2. **Add files**를 선택하여 업로드할 파일을 파일 탐색기에서 선택하십시오. 또는 파일을 드래그하여 추가 할 수도 있습니다.

3. HTML 파일을 업로드한 후, 다음 챕터에서 S3 버킷을 웹 사이트 호스팅용으로 구성하겠습니다. 2번에서 생성한 HTML 파일과 이미지를 업로드합니다.

4. **Upload** 버튼을 누릅니다. 콘솔 화면 아래쪽에 업로드 과정과 결과가 보여집니다.
