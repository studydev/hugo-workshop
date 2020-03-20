# hugo-workshop 템플릿 설명

## 사전 할 일
1. HUGO 환경 [설치 방법](https://gohugo.io/getting-started/quick-start/)  
    - Hugo 설치
    ```sh
    brew install hugo
    ```
    - 버전 확인
    ```sh
    hugo version
    ```
    - git 설치
    ```sh
    brew install git
    ```

1. HUGO 템플릿 다운로드 받기 (하단의 `workshop-name`은 수정해 주세요.)
    ```sh
    git clone https://github.com/studydev/hugo-workshop.git workshop-name
    ```

1. 설정 변경
    - 설치 디렉토리 이동
    ```sh
    cd hugo-workshop
    ```
    - config.toml 파일 수정
    ```toml
        # 변경 필요
        author = "Hyounsoo Kim"

    # 변경 필요
    title = "Hugo 템플릿 활용하기"
    googleAnalytics = "UA-160433107-1"

    [[menu.shortcuts]] 
    name = "<i class='fab fa-aws'></i> AWS Homepage"
    url = "https://aws.amazon.com/"
    weight = 1000

    [[menu.shortcuts]]
    name = "<i class='fas fa-blog'></i> AWS Blog"
    url = "https://aws.amazon.com/blogs/aws/"
    weight = 1020

    [[menu.shortcuts]]
    name = "<i class='fab fa-facebook-square'></i> Facebook"
    url = "https://www.facebook.com/groups/awskrug/"
    weight = 1030

    [Languages]
    [Languages.ko]
    weight = 1
    languageName = "한국어"

    [Languages.en]
    weight = 2
    languageName = "English"

    [deployment]
    [[deployment.targets]]
        name = "s3"
        URL = "s3://hugo.awsdemo.kr?region=ap-northeast-2"
    ```

## 설치