---
title: "2. 웹사이트의 정보와 디자인"
date: 2026-07-27
draft: false
tags: ["HTML 태그", "CSS"]
categories: ["STUDY"]
summary: "이어드림2026 서비스 개발 수업 DAY2"
weight: 2
---
### HTML 기본 태그
[예제]
![alt text](../img/tag_basic_img.png)
- 태그명 : HTML이 갖고있는 고유의 기능 <열린태그><\/닫힌태그> 형태로 입력
- 컨텐츠 : 열린 태그와 닫힌 태그 사이에 있는 내용물
- 속성 : HTML 태그가 갖고 있는 추가 정보
- 속성 값 : 어떤 역할을 수행할지 구체적인 명령을 진행하는것

#### &lt;a&gt;, &lt;img&gt; 태그 실습
```
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>HTML 기본태그</title>
        <link rel="icon" href="chrome.png">
    </head>
    <body>
        <!-- href="이동할 링크" target="열리는 창" -->
        <!-- _blank:새창 / _self:자기창(기본이기 때문에 생략 가능) -->       
        <a href="https://www.naver.com">네이버</a>
        <!-- a태그로 감싸면  이미지를 눌러도 해당 링크로 이동 가능 하다.-->
        <a href="https://yeardream2026.elice.io/" target="_blank">
            <img alt="엘리스 회사 로고" src="elice_logo.png"/>
        </a>
    </body>
</html>
```
[tip]
- VS code에서 'alt + 위아래 화살표' 하면 줄 이동 함.

#### &lt;h&gt; 태그
```
<h1>Hello World</h1>
<h2>Hello World</h2>
<h3>Hello World</h3>
```
- 제목이나 부제목을 표현
- 숫자 값이 클수록 폰트 사이즈가 작음.

#### &lt;p&gt; 태그
```
<p>Nice to meet you</p>
```
- 본문 내용을 표현
- 웹 사이트의 중요 정보를 담는 태그

#### &lt;ol&gt;, &lt;li&gt; 태그
```
<ol>
  <li>메뉴 1</li>
  <li>메뉴 2</li>
  <li>메뉴 3</li>
</ol>
```
- &lt;ol&gt; 태그 : 순서가 있는 리스트 생성.
- &lt;li&gt; 태그 : <\ol>과<\Ul>의 각 항목을 나열할 때 사용.

#### &lt;ul&gt; 태그
```
<ul>
  <li>메뉴 1</li>
  <li>메뉴 2</li>
  <li>메뉴 3</li>
</ul>
```
<\ul> 태그 : 순서가 없는 리스트 생성.

[tip]
VSCODE 에서 alt + 태그 클릭 하면 클릭한 애들 끼리 바로 바꿀 수 있다.

#### &lt;h&gt;, &lt;p&gt;, &lt;ol&gt;, &lt;li&gt;, &lt;ul&gt; 태그 실습
```
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title> 실습3 </title>
        <link rel="icon" href="chrome.png">
    </head>
    <body>
        <h1>Hello World</h1>
        <h2>Hello World</h2>
        <h3>Hello World</h3>
        <h4>Hello World</h4>
        <h5>Hello World</h5>
        <h6>Hello World</h6>

        <!-- 그냥 엔터 누른 듯한 줄 바꿈 -->
        <strong>[br태그]</strong><br/>
        첫번째 문단<br/> 
        두번째 문단<br/><br/> 

        <!-- shift + 엔터 누른 듯한 간격이 넓은 줄 바꿈 -->
        <strong>[p태그]</strong><br/>
        <p>첫번째 문단</p>
        <p>두번째 문단</p>

        <ol>
            <li>menu 1</li>
            <li>menu 2</li>
            <li>menu 3</li>
        </ol>

        <ul>
            <li>menu 1</li>
            <li>menu 2</li>
            <li>menu 3</li>
        </ul>

        <ul>
            <li><a href="">홈</a></li>
            <li><a href="">회사 소개</a></li>
            <li><a href="">연락처</a></li>
        </ul>
    </body>
</html>
```

### 구조를 잡을 때 사용하는 태그
#### &lt;header&gt;, &lt;nav&gt; 태그
```
<header>  <!-- 상단영역 -->
  <nav>   <!-- 메뉴열역 -->
    ....
  </nav>
</header>
```
- &lt;header&gt; : 웹사이트의 머리글을 담는 공간
- &lt;nav&gt; : 메뉴 버튼을 담는 공간(navigation)

#### &lt;main&gt;, &lt;article&gt; 태그
```
<main role="main"> <!--본문 영역-->
  <article> <!--정보 영역-->
    .....
  </article>
```
- &lt;main&gt; : 문서의 주요 내용을 담는 태그
- &lt;article&gt; : 문서의 주요 이미지나 텍스트 등의 정보를 담고 구역을 설정하는 태그

#### &lt;footer&gt;, &lt;div&gt; 태그
```
<footer>
  <div>
    <p>주소 : 대정광역시 유성구 문지로 193 KAIST</p>
    <p>이메일 : contact@elice.io</p>
  </div>
</footer>
```
- &lt;footer&gt; : 가장 하단에 들어가는 정보를 표기할 때 사용. 
- &lt;div&gt; : 임의의 공간을 만들 때 사용.

[실습]
```
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title> 실습04 </title>
        <link rel="icon" href="chrome.png">
    </head>
    <body>
        <header>
            <h1><img src="elice_logo.png"></h1>
            <!--항상 어딜가나 붙어있는 네비게이션 : GNB-->
            <nav>
                <ul>
                    <li><a href="#">홈</a></li>
                    <li><a href="#">회사 소개</a></li>
                    <li><a href="#">연락처</a></li>
                </ul>   
            </nav>
        </header>
        <!--사이트의 주요 내용, 컨테츠가 있는 부분-->
        <main role="main">
            <!--1개 이상 아티클이 main을 이룬다.-->
            <article>
                <h2>회사 소개</h2>
                <p>우리회사는 최고로 좋은 회사에요.</p>                    
            </article>
        </main>
        <footer>
            <!--영역지정/스타일적용-->
            <div>
                <p>서울시 영등포구 여의도동 어딘가</p>
            </div>
            <div>
                <p>010-1111-1234</p>
            </div>
        </footer>
    </body>
</html>
```

### HTML 태그의 두가지 성격
#### Block 요소와 Inline 요소
- 요소 - 태그 - 엘리먼트 동일한 말이다.
- 속성 - 프로퍼티 동일한 말이다.
- Block 요소 - 입력하면 줄바꿈 현상. 공간을 만들 수 있고, 상하 배치 작업 가능.
- Inline 요소 - 가로 - 세로 배치. 공간을 만들 수 없고, 상하 배치 작업 불가능.

[실습]
```
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title> 실습05 </title>
        <link rel="icon" href="chrome.png">
    </head>
    <body>
        <!--block : 블록처럼 쌓이는 속성-->
        <h3>Hello</h3>
        <h3>Hello</h3>
        <h3>Hello</h3>
        <!--inline : 줄 긋듯이 좌에서 우로 쌓이는 속성-->
        <!--HTML 에서는 1개 이상의 공백을 인정하지 않음.-->
        <!--그래서 공백문자를 활용 한다.-->
        <a href="#">Bye</a>&nbsp; &nbsp; &nbsp; &nbsp;
        <a href="#">Bye</a>&nbsp;&nbsp;&nbsp;&nbsp;
        <a href="#">Bye</a>
        <!--div-->
        <div>block</div>
        <div>block</div>
        <div>block</div>
        <!--span-->
        <span>inline</span>
        <span>inline</span>
        <span>inline</span>
    </body>
</html>
```
[결과]
![alt text](../img/block_inline_img.png)

### CSS
#### CSS란?
- Cascading Style Sheet
- 정보(HTML)와 디자인(CSS)의 분리
- 문서의 레이아웃과 스타일 정의
- HTML로 작성된 정보를 꾸며주는 언어

#### CSS 구성 요소
```
선택자 { 속성 : 속성값; }
```
- 선택자 : 디자인을 적용할 HTML 영역
- 속성 : 어떤 디자인을 적용할지 정의
- 속성값 : 어떤 역할을 수행할지 구체적으로 명령. 세미콜론(;)필수 입력.

[실습]
```
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title> 실습06 </title>
        <link rel="icon" href="chrome.png">
        <style>
            /*css 주석*/
            h2{/*선택자(selector)*/
                /*속성:값*/
                color : cadetblue;
            }
        </style>
    </head>
    <body>
        <!--HTML 주석-->
        <h1 style="color:blue">Hello World</h1>
        <h2>Hello World</h2>
    </body>
</html>
```

#### CSS 선택자
- HTML의 어떤 요소에 CSS를 적용할 것인가.
- Type : 특정 태그에 스타일을 적용 (h2{ .... })
- Class : 클래스 이름으로 특정 위치에 스타일을 적용(.coding{ .... })
- ID : ID를 이용하여 스타일을 적용(#coding{ .... })

[실습]
```
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title> CSS selecotr </title>
        <link rel="icon" href="chrome.png">
        <style>
            /*
            # 아이디
            . 클래스
            태그
            태그[속성]
            */
            h1{
                background-color: dodgerblue;
            }
            /*h1 태그 중에서 id가 coding인 요소*/
            h1#coding{
                color: red;
            }
            input[type="text"]{
                background-color: pink;
            }
            .codes{
                color: orange;
            }
        </style>
    </head>
    <body>
       <h1>Type Elice</h1>
       <h1 class="codes">Class Elice</h1>
       <!--id 는 페이지내 오직 1개만 존재-->
       <h1 id="coding">ID Elice</h1>
       <input type="text" value="입력값">

       <h2>Type Hello World</h2>
       <h2 class="codes">Type Hello World</h2>
       <h2 id="coding">Type Hello World</h2>
    </body>
</html>
```

### 부모 자식 관계
- 띄워 쓰기를 하면 부모 자식 관계가 형성 된다.
- 해당 부모 아래의 태그에만 적용 된다.
```
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title> CSS selecotr </title>
        <link rel="icon" href="chrome.png">
        <style>
            /*부모를 지정하면 자식까지 적용 된다.*/
            header{
                color: red;
            }
            header p{
                color: blue;
            }
            /*부모자식 사이는 공백 또는 > 를 사용*/
            header>p{
                background-color: aqua;
            }
            /*클래스 footer의 자식 중 h2 태그의 글자색을 오렌지로 변경*/
            .footer h2{
                color: orange;
            } 
        </style>
    </head>
    <body>
        <header>
            <h2>header h2</h2>
            <p>header p</p>
        </header>
        <footer class="footer">
            <h2>footer h2</h2>
            <p>footer p</p>
        </footer>
    </body>
</html>
```

### 캐스케이딩
CSS의 우선순위를 결정 하는 세가지 요소는 아래와 같다.
- 순서 : 나중에 적용한 속성 값의 우선순위가 높다.
- 디테일 : 더 구체적으로 작성된 선택자의 우선순위가 높다.
```
header p{ color: red;} /*해당 스타일이 적용 된다.*/
p{ color: blue;}
```
- 선택자 : **style > id> class > type** 순으로 우선순위가 높다.

### CSS 주요 속성
#### width, height
- width : 선택한 요소의 넓이를 설정. 
- height : 선택한 요소의 높이를 설정.
#### font
- font-family : 브라우저마다 지원하는 폰트가 다름.
- font-weight : 100 ~ 900 사이의 숫자를 입력할 수 있음.
#### border
- border-style : 
  - solid(실선) 
  - dotted(점선) 
  - 주석과 같이 한줄에 이어 쓸 수 있음.
#### background
- background-repeat : 
  - repeat-x(x축으로 반복) 
  - repeat-y(y축으로 반복) 
  - no-repeat(반복하지 않음)
- background-position : 공간 안에서 이미지의 좌표를 변경할 때. top, bottom, center, left, right 등.

### 박스모델
[박스모델의 구조]
![alt text](../img/box_img.png)

#### margin과 padding의 차이
- margin : border 바깥쪽에서 왼쪽에 여백을 만듦
- padding : border 안쪽에서 왼쪽에 여백을 만듦

### Block 요소와 Inline 요소
- Block 요소의 특징 
  - 줄바꿈 현상이 나타남
  - width height 값 사용 가능 -> 공간 만들기 가능
  - margin과 padding값 사용 가능 -> 상하 배치 작업 가능
- Inline 요소의 특징
  - 줄바꿈 현상 없음.
  - width/height 값 적용 불가
  - margin/padding/bottom 값 적용 불가

### 마진 병합 현상.
margin-bottom과 margin-top 중 숫자가 큰 값으로 적용 된다.
```
.box1{margin-bottom: 150px;}
.box2{margin-top: 100px;}
```
[결과]
![alt text](../img/margin_collapsing_img.png)
150px만 적용 된다.

