---
date: 2020-06-01
title: "정적 웹 사이트 호스팅 사용"
weight: 440
pre: "<b>4-4. </b>"
---

{{% notice note %}}
Amazon S3를 이용하여 정적 웹 사이트를 호스팅할 수 있습니다. 
{{% /notice %}}

{{% notice info %}}
정적 웹 사이트란, 웹 페이지에 정적 콘텐츠(HTML, 이미지, 비디오) 또는 클라이언트 측 스크립트(Javascript)만을 포함하는 웹 사이트를 가리킵니다.
이와는 대조적으로 동적 웹 사이트의 경우 PHP, JSP 또는 ASP.NET과 같은 서버 측 스크립트를 포함한 서버 측 처리를 필요로 합니다. Amazon S3에서는 서버 측 스크립팅을 지원하지 않습니다. 동적 웹 사이트를 호스팅하고자 하는 경우, AWS의 EC2와 같은 다른 서비스를 사용하면 됩니다. 
{{% /notice %}}

1. S3 콘솔에서 아까 생성한 버킷을 선택하고, **Properties** 탭을 클릭합니다.

2. **Static website hosting** 항목을 클릭하고, **index document** 란에 `index.html`을 입력한 뒤, **Save** 버튼을 누릅니다.

3. 다시 **Static website hosting** 항목을 클릭해 봅니다. 아래 **Bucket hosting** 항목이 켜져 있는 것을 볼 수 있습니다. 위의 **Endpoint URL**을 클릭해 정적 웹 사이트로 접속해 봅니다.
 
접속이 잘 됩니다. 이렇게 Amazon S3를 이용하여 정적 웹 사이트를 호스팅할 수 있습니다. 이를 통해 최종적으로 구성하게 된 서비스 아키텍쳐는 아래와 같습니다:


