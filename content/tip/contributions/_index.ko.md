---
date: 2020-03-20
title: "HUGO"
weight: -150
---

## HUGO는 몰라도 되요.
{{% notice note %}}
유일하게 다루는건 문서(content), 그리고 이미지(/static/images) 입니다.
{{% /notice %}}
> 굳이 알고 싶다면 [클릭](https://gohugo.io/getting-started/directory-structure/)

## 원래 디렉토리 구조
```
.
├── archetypes
├── config.toml
├── content
├── data
├── layouts
├── static
└── themes
```

## 이것만 알면 됩니다.
폴더 만들고 파일 만드는거 어렵지 않아요.
```
.
├── content
|   ├── tip
|   └── cleanup
└── static
    └── images
        └── vscode
```

## 그래도 조금 더 보시겠다면,
```
.
└── layouts
    ├── _default
    |   └── _markup
    ├── partials
    └── shortcodes
```
> layouts 폴더는 theme가 가지고 있는 layouts 폴더를 Overwrite 하는 구조입니다. (굳이 넣지 않아도 됩니다.)
- 가능하면 Hugo learn 테마 폴더에서는 수정하지 않습니다. 필요할 경우 `/layouts` 폴더로 복사하여 수정하여 사용합니다.
> 몇 가지만 수정 기능이 들어가 있습니다.
- Google Analytics를 활성화 하였습니다.
- Hyperlink를 클릭하면 새 탭으로 열리게 수정했습니다. (워크샵 페이지에서 페이지 이동하는건 좋지 않습니다.)
- AWS Logo 이미지를 SVG로 대체하였습니다.

## 기타 변경사항
- Hugo learn 테마는 소스코드에 완전히 넣은게 아니라 Sub Module로 등록되어 있습니다. 만약, 업데이트가 있을 경우, 반영할 수 있습니다. (추천하고 있습니다.)
- [Learn 테마의 CSS 스타일](https://learn.netlify.com/en/basics/style-customization/)을 최근 워크샵 스타일에 맞게 수정하였습니다. `/static/css/thene-aws.css`
```css
:root{

    --MAIN-TEXT-color:#232F3E; /* Color of text by default */
    --MAIN-TITLES-TEXT-color: #161E2D; /* Color of titles h2-h3-h4-h5 */
    --MAIN-LINK-color:#95b0ff; /* Color of links */
    --MAIN-LINK-HOVER-color:#527FFF; /* Color of hovered links */
    --MAIN-ANCHOR-color: #95b0ff; /* color of anchors on titles */

    --MENU-HEADER-BG-color:#161E2D; /* Background color of menu header */
    --MENU-HEADER-BORDER-color:#161E2D; /*Color of menu header border */

    --MENU-SEARCH-BG-color:#202c3c; /* Search field background color (by default borders + icons) */
    --MENU-SEARCH-BOX-color: #4d6584; /* Override search field border color */
    --MENU-SEARCH-BOX-ICONS-color: #4d6584; /* Override search field icons color */

    --MENU-SECTIONS-ACTIVE-BG-color:#232F3E; /* Background color of the active section and its childs */
    --MENU-SECTIONS-BG-color:#161E2D; /* Background color of other sections */
    --MENU-SECTIONS-TEXT-color: #FFFFFF; /*Color of pre text */
    --MENU-SECTIONS-LINK-color: #ccc; /* Color of links in menu */
    --MENU-SECTIONS-LINK-HOVER-color: #e6e6e6;  /* Color of links in menu, when hovered */
    --MENU-SECTION-ACTIVE-CATEGORY-color: #232F3E; /* Color of active category text */
    --MENU-SECTION-ACTIVE-CATEGORY-BG-color: #FF9900; /* Color of background for the active category (only) */
    --MENU-SECTION-ACTIVE-CATEGORY-TEXT-color: #fff; /* Color of pre text when selected */

    --MENU-VISITED-color: #527FFF; /* Color of 'page visited' icons in menu */
    --MENU-SECTION-HR-color: #20272b; /* Color of <hr> separator in menu */

}

@font-face {
    font-family: 'Roboto';
    src: url('https://fonts.googleapis.com/css?family=Roboto&display=swap');
    /* src: url("../webfonts/AmazonEmber_W_Lt.eot"); */
    /* src: url("../webfonts/AmazonEmber_W_Lt.eot?#iefix") format("embedded-opentype"), url("../webfonts/AmazonEmber_W_Lt.woff2") format("woff2"), url("../webfonts/AmazonEmber_W_Lt.woff") format("woff"); */
    font-style: normal;
    font-weight: 200;
}

@font-face {
    font-family: 'Roboto';
    src: url('https://fonts.googleapis.com/css?family=Roboto&display=swap');
    /* src: url("../webfonts/AmazonEmber_W_Bd.eot"); */
    /* src: url("../webfonts/AmazonEmber_W_Bd.eot?#iefix") format("embedded-opentype"), url("../webfonts/AmazonEmber_W_Bd.woff2") format("woff2"), url("../webfonts/AmazonEmber_Bd_Lt.woff") format("woff"); */
    font-style: bold;
    font-weight: 600;
}

body {
    font-family: "Roboto", "Work Sans", "Helvetica", "Tahoma", "Geneva", "Arial", sans-serif;
    color: var(--MAIN-TEXT-color) !important;
}

textarea:focus, input[type="email"]:focus, input[type="number"]:focus, input[type="password"]:focus, input[type="search"]:focus, input[type="tel"]:focus, input[type="text"]:focus, input[type="url"]:focus, input[type="color"]:focus, input[type="date"]:focus, input[type="datetime"]:focus, input[type="datetime-local"]:focus, input[type="month"]:focus, input[type="time"]:focus, input[type="week"]:focus, select[multiple=multiple]:focus {
    border-color: none;
    box-shadow: none;
}

h2, h3, h4, h5, h6 {
    font-family: "Roboto", "Work Sans", "Helvetica", "Tahoma", "Geneva", "Arial", sans-serif;
    color: var(--MAIN-TITLES-TEXT-color) !important;
    font-weight: 300;
}

h1 {
    font-family: "Roboto", "Work Sans", "Helvetica", "Tahoma", "Geneva", "Arial", sans-serif;
}

#chapter h1 {
    margin-top: 0;   
}

#chapter h3 {
    margin-top: 0;
}

a {
    color: var(--MAIN-LINK-color);
}

.anchor {
    color: var(--MAIN-ANCHOR-color);
}

a:hover {
    color: var(--MAIN-LINK-HOVER-color);
}

#sidebar ul li.visited > a .read-icon {
	color: var(--MENU-VISITED-color);
}

#sidebar #footer {
  padding-top: 20px !important;
}

#sidebar #footer h2.github-title {
  font-size: 20px;
  color: #fd9827 !important;
  margin: 10px 0px 5px;
  padding: 0px;
  font-weight: normal !important;
  margin-top: 10px;
  padding-top: 30px;
  border-top: 1px dotted #384657;
}

#sidebar #footer h3.github-title {
  font-size: 14px;
  margin: 10px 0px 5px;
  padding: 0px;
  text-transform: uppercase;
  letter-spacing: .15px;
}

#sidebar #footer h5.copyright, #sidebar #footer p.build-number {
  color: var(--MENU-SECTIONS-LINK-color) !important;
  font-size: 10px;
  letter-spacing: .15px;
  line-height: 150% !important;
  font-weight: 300;
}

#body a.highlight:after {
    display: block;
    content: "";
    height: 1px;
    width: 0%;
    -webkit-transition: width 0.5s ease;
    -moz-transition: width 0.5s ease;
    -ms-transition: width 0.5s ease;
    transition: width 0.5s ease;
    background-color: var(--MAIN-LINK-HOVER-color);
}
#sidebar {
	background-color: var(--MENU-SECTIONS-BG-color);
}
#sidebar #header-wrapper {
    background: var(--MENU-HEADER-BG-color);
    color: var(--MENU-SEARCH-BOX-color);
    border-color: var(--MENU-HEADER-BORDER-color);
}
#sidebar .searchbox {
	border-color: var(--MENU-SEARCH-BOX-color);
    background: var(--MENU-SEARCH-BG-color);
}
#sidebar ul.topics > li.parent, #sidebar ul.topics > li.active {
    background: var(--MENU-SECTIONS-ACTIVE-BG-color);
}
#sidebar .searchbox * {
    color: var(--MENU-SEARCH-BOX-ICONS-color);
}

#sidebar a {
    color: var(--MENU-SECTIONS-LINK-color);
}

#sidebar a b {
    color: var(--MENU-SECTIONS-TEXT-color);
}

#sidebar a:hover {
    color: var(--MENU-SECTIONS-LINK-HOVER-color);
}

#sidebar ul li.active > a {
    background: var(--MENU-SECTION-ACTIVE-CATEGORY-BG-color);
    color: var(--MENU-SECTION-ACTIVE-CATEGORY-color) !important;
}

#sidebar ul.topics > li > a b {
    color: var(--MENU-SECTION-ACTIVE-CATEGORY-TEXT-color) !important;
    opacity: 1;
}

#sidebar hr {
    border-color: var(--MENU-SECTION-HR-color);
}

#sidebar #shortcuts h3 {
    font-family: "Amazon Eber", "Novacento Sans Wide", "Helvetica", "Tahoma", "Geneva", "Arial", sans-serif;
    color: white !important;
    margin-top:1rem;
    padding-left: 1rem;
}

#navigation a.nav-prev, #navigation a.nav-next {
    color: #f19e39 !important;
}

#navigation a.nav-prev:hover, #navigation a.nav-next:hover  {
    color: #e07d04 !important;
}

div.notices p:first-child:before {
    position: absolute;
    top: 2px;
    color: #fff;
    font-family: 'Font Awesome\ 5 Free';
    content: #F06A;
    font-weight: 900; /* Fix version 5.0.9 */
    left: 10px;
}

.ui-state-default, .ui-widget-content .ui-state-default, .ui-widget-header .ui-state-default, .ui-button, html .ui-button.ui-state-disabled:hover, html .ui-button.ui-state-disabled:active {
    border: 1px solid #dddddd;
    font-weight: normal;
    color: #454545;
}

.ui-state-active, .ui-widget-content .ui-state-active, .ui-widget-header .ui-state-active, a.ui-button:active, .ui-button:active, .ui-button.ui-state-active:hover {
    border: 1px solid var(--MENU-HEADER-BG-color);
    background: var(--MENU-HEADER-BG-color);
    font-weight: normal;
    color: #fff;
}

.ui-widget.ui-widget-content {
    border: 1px solid #eeeeee;
}

.ui-widget-header {
  border: 1px solid #eeeeee;
}

.hljs {
  background-color: none; 
}

pre {
  background-color: var(--MENU-SECTIONS-BG-color) !important;
}

div.notices.info p {
    border-top: 30px solid #fd9827;
    background: #FFF2DB;
}

.btn {
    display: inline-block !important;
    padding: 6px 12px !important;
    margin-bottom: 0 !important;
    font-size: 14px !important;
    font-weight: normal !important;
    line-height: 1.42857143 !important;
    text-align: center !important;
    white-space: nowrap !important;
    vertical-align: middle !important;
    -ms-touch-action: manipulation !important;
        touch-action: manipulation !important;
    cursor: pointer !important;
    -webkit-user-select: none !important;
       -moz-user-select: none !important;
        -ms-user-select: none !important;
            user-select: none !important;
    background-image: none !important;
    border: 1px solid transparent !important;
    border-radius: 4px !important;
    -webkit-transition: all 0.15s !important;
       -moz-transition: all 0.15s !important;
            transition: all 0.15s !important;
  }
  .btn:focus {
    /*outline: thin dotted;
    outline: 5px auto -webkit-focus-ring-color;
    outline-offset: -2px;*/
    outline: none !important;
  }
  .btn:hover,
  .btn:focus {
    color: #2b2b2b !important;
    text-decoration: none !important;
  }
  
  .btn-default {
    color: #fff !important;
    background-color: #527FFF !important;
    border-color: #527FFF !important;
  }
  .btn-default:hover,
  .btn-default:focus,
  .btn-default:active {
    color: #fff !important;
    background-color: #95b0ff !important;
    border-color: #95b0ff !important;
  }
  .btn-default:active {
    background-image: none !important;
  }

  #aws-logo {
      margin-top: 15px;
      margin-bottom: 25px;
  }
```


