# hugo-workshop 템플릿 설명

## 사전 설치
1. HUGO 환경 [설치 방법](https://gohugo.io/getting-started/quick-start/)  
    - HUGO 설치
    ```sh
    brew install hugo
    ```
    - git 설치
    ```sh
    brew install git
    ```

1. 템플릿 다운로드 및 설정(하단의 `workshop-name`은 수정해 주세요.) - 테마 업데이트까지 진행
    ```sh
    git clone https://github.com/studydev/hugo-workshop.git workshop-name
    cd workshop-name
    git submodule init 
    git submodule update
    ```

1. 한글 환경 추가 `/themes/hugo-theme-learn/i18n/` 폴더에 `ko.toml` 설정 파일 추가  
    ```toml
    [Search-placeholder]
    other = "검색..."

    [Clear-History]
    other = "기록삭제"

    [Attachments-label]
    other = "첨부"

    [title-404]
    other = "에러"

    [message-404]
    other = "죄송합니다. 페이지가 존재하지 않습니다. ㅜ_ㅜ."

    [Go-to-homepage]
    other = "홈으로"

    [Edit-this-page]
    other = "이 페이지를 편집"

    [Shortcuts-Title]
    other = "More"

    [Expand-title]
    other = "펼치기..."
    ```

1. HUGO Template 설정
    - config.toml 파일 열어서 아래 내용 수정
    ```toml
        # 제작작 변경
        author = "Hyounsoo Kim"

    # 제목 변경
    title = "HUGO 템플릿 활용하기"

    # Google Analytics 추적 ID를 발급 받아서 아래 변경
    googleAnalytics = "UA-160433107-1"

    # 관련된 바로가기 링크들을 수정
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

    # 언어 설정을 추가함
    [Languages]
    [Languages.ko]
    weight = 1
    languageName = "한국어"

    [Languages.en]
    weight = 2
    languageName = "English"

    # 배포 옵션 정의, public 폴더에 컴파일 된 정적 컨테츠를 S3에 업로드 가능
    [deployment]
    [[deployment.targets]]
        name = "s3"
        URL = "s3://hugo.awsdemo.kr?region=ap-northeast-2"
    ```

## HUGO 로컬 테스트
1. HUGO 서버 올리기 (port는 1313이며 -p 옵션으로 변경 가능)
```sh
hugo server -D
```
1. HUGO 테스트
```sh
http://localhost:1313/
```
1. HUGO 서버 컴파일 하기
```sh
hugo -d public
```

## HUGO 배포
public 폴더를 S3에 배포한다. (사전에 AWS CLI가 설치되어 있고 설정이 되어 있어야 합니다.)
```sh
hugo deploy --target=s3
```
