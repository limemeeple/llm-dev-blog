---
title: "3. 웹사이트의 정보와 디자인 2"
date: 2026-07-28
draft: false
tags: ["CSS", "애니메이션", "미디어쿼리"]
categories: ["STUDY"]
summary: "이어드림2026 서비스 개발 수업 정리"
weight: 3
---
### 레이아웃에 영향을 미치는 속성
#### display
- Block과 Inline 요소의 성격을 바꿀 때 사용
- inline-block을 사용하면 두 요소의 성격을 모두 가짐

[실습]
```
<!DOCTYPE html>
<html>
<head>

  <meta charset="UTF-8">
  <title>레이아웃에 영향을 미치는 속성 : display</title>
  
  <style>
    p {
      width: 300px;
      height: 300px;
      background-color: pink;
      display:  inline;/*기본 속성이 block -> inline 으로 변경*/
    }
    
    a {
      width: 300px;
      height: 300px;
      background-color: yellow;
      display: block; /*기본 inline -> block 으로 변경*/
      margin-top: 10px;
    }
  </style>
  
</head>
<body>

   <p>Block Element</p>
   <p>Block Element</p>
   <p>Block Element</p>
   
   <a href="#">Inline Element</a>
   <a href="#">Inline Element</a>
   <a href="#">Inline Element</a>

</body>
</html>

```
[결과]
![alt text](../img/display_img.png)
block 속성을 가진 &lt;p&gt; 태그가 inline 으로 변경
inline 속성을 가진 &lt;a&gt; 태그가 block 으로 변경

#### float 과 clear
[float]
- 선택된 요소를 왼쪽 끝 혹승ㄴ 오른쪽 끝에 정렬 시키고자 할때 사용.
- 레이어가 겹치지 않는 상태로 왼쪽에서 부터 정렬시키고 싶은 경우 float:left;를 연속적으로 입력

[clear]
- float에 대한 속성을 제어하고자 할때 사용.

[실습]
```
<!DOCTYPE html>
<html>
<head>

  <meta charset="UTF-8">
  <title>레이아웃에 영향을 미치는 속성 - float</title>
  
  <style>
    /*
    .left {  
      width: 300px;
      height: 300px;
      background-color: yellow;
      float: left;
    }
    
    .right {
      width: 300px;
      height: 400px;
      background-color: blue;
      float: right;
    }
    */
    
    
    header {
      width: 100%;
      height: 100px;
      background-color: yellow;
    }
    
    #left {
      width: 200px;
      height: 200px;
      background-color: red;
      float: left;
    }
    
    main {
      width: 300px;
      height: 200px;
      background-color: green;
      float: left;
    }
    
    #right {
      width: 200px;
      height: 200px;
      background-color: pink;
      float: right;
    }
    
    footer {
      width: 100%;
      height: 100px;
      background-color: black;
      /*float 양쪽으로 부터 자유로워 질래*/
      clear: both;
    }
    
    
  </style>
  
</head>
<body>


  <!--2026.07.28 불필요한 내용이므로 주석 처리(황순현)-->
  <!--<div class="left"></div>-->
  <!--<div class="right"></div>-->
  
  

  <!-- 전통적인 웹사이트 레이아웃 구조 만들기 -->

  <header></header>
  
  <article id="left"></article>
  <main></main>
  <article id="right"></article>
  
  <footer></footer>

  

</body>
</html>
```
[결과]
![alt text](../img/float_img.png)

#### 브라우저와 공간 사이의 공백 제거하기
- &lt;html&gt;과 &lt;body&gt;태그는margin과 padding 값을 가지므로 초기화를 해줘야 함.
- 혹은 *로 모든 html에 적용.

[지금까지 배운걸 가지고 실습]
```
<!DOCTYPE html>
<html>

<head>

    <meta charset="UTF-8">
    <title>쇼핑몰 상단 만들기</title>

    <link rel="stylesheet" href="index.css">
    <link rel="icon" href="img/favicon.png">
</head>

<body>

    <div class="container">

        <header id="intro">

            <h1>
                <a href="https://academy.elice.io/">엘리스 사전</a>
            </h1>

            <nav>
                <ul>
                    <li class="one"><a href="#">캐릭터 소개</a></li>
                    <li class="two"><a href="#">줄거리</a></li>
                    <li class="three"><a href="#">부록</a></li>
                </ul>
            </nav>

        </header>
        <main role="main" id="main">

            <article class="one">
                <a href="#">
                    <img src="img/image1.png" alt="White Rabbit">
                    <h2>하얀토끼</h2>
                </a>
            </article>

            <article class="two">
                <a href="#">
                    <img src="img/image2.png" alt="Cheshire Cat">
                    <h2>체셔 고양이</h2>
                </a>
            </article>

            <article class="three">
                <a href="#">
                    <img src="img/image3.png" alt="Queen of Hearts">
                    <h2>하트여왕</h2>
                </a>
            </article>

            <article class="one">
                <a href="#">
                    <img src="img/image4.png" alt="Mad Hatter">
                    <h2>모자장수</h2>
                </a>
            </article>

            <article class="text">
                <p>엘리스에 오신 여러분 환영합니다! :)</p>
            </article>

            <article class="two">
                <a href="#">
                    <img src="img/image5.png" alt="Dodo">
                    <h2>도도새</h2>
                </a>
            </article>

            <article class="three">
                <a href="#">
                    <img src="img/image6.png" alt="Caterpillar">
                    <h2>애벌레</h2>
                </a>
            </article>

            <article class="one">
                <a href="#">
                    <img src="img/image7.png" alt="Card Soldier">
                    <h2>카드병정</h2>
                </a>
            </article>

            <article class="two">
                <a href="#">
                    <img src="img/image8.png" alt="Golden Key">
                    <h2>황금열쇠</h2>
                </a>
            </article>

        </main>
        <!--art + shift + f로 코드 정렬 가능-->
        <footer id="footer">

            <div class="copyright">
                <p>Copyright © 2016-2018 elice. All rights reserved.</p>
            </div>

            <div class="address">
                <p>대전광역시 유성구 문지로 193 KAIST 문지캠퍼스 진리관 T326호</p>
            </div>

        </footer>
    </div>

</body>

</html>
```
[css]
```
*{/*모든 요소에 여백을 없애겠다.*/
    margin: 0;
    padding: 0;
}

div.container{
    /*넓이를 고정*/
    width:960px;
}

/*list의 스타일을 제거*/
ol,ul{
    list-style-type: none;
}

/*a 태그의 스타일을 제거*/
a{
    text-decoration-line: none;
    color: black;
}

#intro{/*header의 넓이와 높이 지정*/
    width: 100%;
    height: 80%;
}

#intro h1{
    float: left;
    width: 50%;
    height: 80px;
    background-color: #fbfbfb;
}
/*a 태그 내 글자를 공간형성을 위해 block으로 변환*/
#intro h1 a{
    display: block;
    padding: 15px 0 0 30px
}
#intro nav{
    float: right;
    width: 50%;
    height: 80px;
    background-color: pink;   
}
#intro nav ul li{
    float:left;
    height: 80px;
    width: 33.3%;
    text-align: center;
    line-height: 80px;
}
#intro nav ul li.one{
    background-color: lightgray;
}
#intro nav ul li.two{
    background-color: darkgray;
}
#intro nav ul li.three{
    background-color: grey;
}
#intro nav ul li a{
    font-size: 20px;
    font-weight: bold;
}

/*실습8*/
/*main 영역 정리 - 크기 지정, 정렬 지정*/
#main article{
    width : 50%;
    height: 320px;
    float: left;
}
/*a 태그 영역을 article 영역과 동등하게*/
#main article a{
    display: block;
    width: 100%;
    height: 100%
}

#main article.one{
    background-color: blueviolet;
}

#main article.two{
    background-color: purple;
}

#main article.three{
    background-color: #3ab6bc;
}
/*상위 요소 크기 변화에 이미지도 맞추도록...*/
#main article img{
    width: 100%;
}

#main article h2{
    color: white;
    font-size: 25px;
    padding: 3px 0;
    text-align: center;
}
#main article.text{
    width: 100%;
    height: 60px;
}
#main article.text p{
    text-align: center;
    font-size: 25px;
    font-weight: 600;
    padding: 17px 0;
    border: 1px solid black;
    height: 24px;
}

#footer{
    clear: both;/*이전에 사용한 float은 무시*/
    color: gray;
    text-align: center;
    background-color: lightgray;
    font-size: 12px;
    padding: 20px 0;
    height:  30px;
}
#footer div.copyright{
    width: 50%;
    float: left;
}

#footer div.address{
    width: 50%;
    float: right;
}
```
[결과]
![alt text](../img/final_css_img.png)

### Transform
#### rotate, scale
- rotate(45deg) : 입력한 각도만큼 회전. 음수도 입력 가능.
- scale(2,3) : 숫자는 비율을 의미. width를 2배, height를 3배 확대

#### skew, translate
- skew(10deg,20deg) : x축 y축을 기준으로 입력한 각도만큼 비틂.
- translate(100px, 200px) : 선택한 오브젝트의 좌표 변경.

[실습 코드]
```
<!DOCTYPE html>
<html>
<head>

  <meta charset="UTF-8">
  <title>CSS Transform</title>
  
  <style>
    .transform {
       width: 100px;
       height: 100px;
       background-color: red;
       margin: 100px 0 0 100px;
       /*같은 속성에 값을 변화하면 덮어쓴다.*/
       /*00 다음 00을 적용하도록 하면 된다.*/
       /*transform: rotate(45deg) scale(2, 3);*/  
       /*transform: translate(100px, 150px);*/
       transform: skew(0deg,0deg);/*x축 기준, y축 기준 회전*/

       /*구형 특정 브라우저에서 동작하게 하려면?*/
       -moz-transform: skew(0deg, 0deg);    /*firefox*/
       -webkit-transform: skew(0deg, 0deg); /*Chrome,Safari*/
       -o-transform: skew(0deg, 0deg);      /*Opera*/
       -ms-transform: skew(0deg, 0deg);     /*I.E*/
    }
  </style>
  
</head>
<body>

  <div class="transform"></div>

</body>
</html>
```

### Transition
#### property, duration, timing-fuction, delay 
- property : 효과를 적용하고자 하는 css 속성.
- duration : 효과가 나타나는데 걸리는 시간.
- timing-function : 효과의 속도 linear은 '일정하게'라는 의미
- delay : 특정 조건 하에서 효과 발동 1s는 '1초 후'라는 의미
#### 가상선택자:hover
- css에서 미리 만들어 놓은 클래스. **마우스를 올렸을때**라는 조건
[실습 코드]
```
<!DOCTYPE html>
<html>
<head>

  <meta charset="UTF-8">
  <title>CSS Transtion</title>
  
  <style>

    .transition {
        width: 100px;
        height: 100px;
        background-color: red;
        /*어떤 속성에 효과를 줄래?*/
        transition-property: width background-color;
        /*얼마동안에 진행할까? 1s == 1000ms*/
        transition-duration: 1s;
        /*어떤 움직임?*/
        transition-timing-function: cubic-bezier(.12,.54,1,.1);
        /*
          ease : 기본값 - 천천히 -> 빠르게 -> 천천히
          ease-in : 천천히 -> 빠르게(가속)
          ease-out : 빠르게 -> 천천히(감속)
          ease-in-out : 천천히 -> 빠르게 -> 천천히(출발 도착에 더 강조)
          linear : 일정한속도
          cubic-bezier : 배치에 공선을 이용한 애니메이션
          cubic-bezier(p1, p2, p3, p4)
        */
        transition-delay: 10ms;
        /*transition: property duration timing-function delay;*/
    }
    .transition:hover {
        width: 1000px;
        background-color: orange;
    }
  </style>
  
</head>
<body>

  <div class="transition"></div>

</body>
</html>
```
### Animation
- animation-iteration-count : 애니메이션 반복 횟수.
- animation-direction : 애니메이션 진행 방향.
[실습코드]
```
<!DOCTYPE html>
<html>
<head>

  <meta charset="UTF-8">
  <title>CSS Animation</title>
  
  <style>

    .animation {
        width: 300px;
        height: 300px;
        background-color: yellow;
        animation-name: changewidth;
        animation-duration: 1000ms;
        animation-delay: 1000ms;
        animation-iteration-count: 1;
        animation-direction: alternate;
        /*
        alternate:from->to->from; count를 2씩 소모
        alternate-reverse:to->from->to;
        normal: from -> to
        reverse: to -> from
        */
        animation-fill-mode: forwards;
        /*
        fill-mode를 사용하면 애니메이션이 짝이 안맞아 툭 끊어지는 상황을 방지
        forwards : 마지막상태에서 정지
        backwards : from 상태로 다시 되돌림
        */
    }

    @keyframes changewidth {
        from{
            width: 300px;
        }
        to{
            width: 100px
        }
    }
  </style>
  
</head>
<body>

  <div class="animation"></div>

</body>
</html>
```
#### Transform & Animation
[실습 코드]
```
<!DOCTYPE html>
<html>
<head>

  <meta charset="UTF-8">
  <title>CSS Transform, Animation</title>
  
  <style>

    .box1 {
        width: 300px;
        height: 300px;
        background-color: red;
        animation: rotaion 1500ms linear 500ms infinite alternate none;
    }

    @keyframes rotaion {
        from{
            transform: rotate(45deg) scale(0.1); 
        }
        to{
            transform: rotate(-45deg) scale(0.5);
        }
    }
  </style>
  
</head>
<body>

  <div class="box1"></div>

</body>
</html>
```
### 본문과 메뉴 영역 애니메이션
- 메뉴 영역에 마우스를 올렸을 때 색상이 변하도록 하기
- 본문 영역에 마우스 를 올렸을 때 색상이 변하도록 하기
[실습코드-html]
```
<!DOCTYPE html>
<html>

<head>

    <meta charset="UTF-8">
    <title>쇼핑몰 상단 만들기</title>

    <link rel="stylesheet" href="index.css">
    <link rel="icon" href="img/favicon.png">
</head>

<body>
    <div class="container">

        <header id="intro">

            <h1>
                <a href="https://academy.elice.io/">엘리스 사전</a>
            </h1>

            <nav>
                <ul>
                    <li class="one"><a href="#">캐릭터 소개</a></li>
                    <li class="two"><a href="#">줄거리</a></li>
                    <li class="three"><a href="#">부록</a></li>
                </ul>
            </nav>

        </header>
        <main role="main" id="main">

            <article class="one">
                <a href="#">
                    <img src="img/image1.png" alt="White Rabbit">
                    <h2>하얀토끼</h2>
                </a>
            </article>

            <article class="two">
                <a href="#">
                    <img src="img/image2.png" alt="Cheshire Cat">
                    <h2>체셔 고양이</h2>
                </a>
            </article>

            <article class="three">
                <a href="#">
                    <img src="img/image3.png" alt="Queen of Hearts">
                    <h2>하트여왕</h2>
                </a>
            </article>

            <article class="one">
                <a href="#">
                    <img src="img/image4.png" alt="Mad Hatter">
                    <h2>모자장수</h2>
                </a>
            </article>

            <article class="text">
                <p>엘리스에 오신 여러분 환영합니다! :)</p>
            </article>

            <article class="two">
                <a href="#">
                    <img src="img/image5.png" alt="Dodo">
                    <h2>도도새</h2>
                </a>
            </article>

            <article class="three">
                <a href="#">
                    <img src="img/image6.png" alt="Caterpillar">
                    <h2>애벌레</h2>
                </a>
            </article>

            <article class="one">
                <a href="#">
                    <img src="img/image7.png" alt="Card Soldier">
                    <h2>카드병정</h2>
                </a>
            </article>

            <article class="two">
                <a href="#">
                    <img src="img/image8.png" alt="Golden Key">
                    <h2>황금열쇠</h2>
                </a>
            </article>

        </main>
        <!--art + shift + f로 코드 정렬 가능-->
        <footer id="footer">

            <div class="copyright">
                <p>Copyright © 2016-2018 elice. All rights reserved.</p>
            </div>

            <div class="address">
                <p>대전광역시 유성구 문지로 193 KAIST 문지캠퍼스 진리관 T326호</p>
            </div>

        </footer>
    </div>

</body>

</html>
```
[실습코드-css]
```
*{/*모든 요소에 여백을 없애겠다.*/
    margin: 0;
    padding: 0;
}

div.container{
    /*넓이를 고정*/
    width:960px;
}

/*list의 스타일을 제거*/
ol,ul{
    list-style-type: none;
}

/*a 태그의 스타일을 제거*/
a{
    text-decoration-line: none;
    color: black;
}

#intro{/*header의 넓이와 높이 지정*/
    width: 100%;
    height: 80%;
}

#intro h1{
    float: left;
    width: 50%;
    height: 80px;
    background-color: #fbfbfb;
}
/*a 태그 내 글자를 공간형성을 위해 block으로 변환*/
#intro h1 a{
    display: block;
    padding: 15px 0 0 30px
}
#intro nav{
    float: right;
    width: 50%;
    height: 80px;
    background-color: pink;   
}
#intro nav ul li{
    float:left;
    height: 80px;
    width: 33.3%;
    text-align: center;
    line-height: 80px;
}
/*메뉴 배경색*/
#intro nav ul li.one{
    background-color: lightgray;
}
#intro nav ul li.two{
    background-color: darkgray;
}
#intro nav ul li.three{
    background-color: grey;
}
#intro nav ul li a{
    font-size: 20px;
    font-weight: bold;
}
/*움직이는 웹 - 실습5*/
#intro nav ul li:hover{  
  background-color: red;
}
#intro nav ul li{
   transition-property: background-color;
   transition-duration: 1000ms;
}

/*실습8*/
/*main 영역 정리 - 크기 지정, 정렬 지정*/
#main article{
    width : 50%;
    height: 320px;
    float: left;
}
/*a 태그 영역을 article 영역과 동등하게*/
#main article a{
    display: block;
    width: 100%;
    height: 100%
}

#main article.one{
    background-color: blueviolet;
}

#main article.two{
    background-color: purple;
}

#main article.three{
    background-color: #3ab6bc;
}
/*상위 요소 크기 변화에 이미지도 맞추도록...*/
#main article img{
    width: 100%;
}

#main article h2{
    color: white;
    font-size: 25px;
    padding: 3px 0;
    text-align: center;
}
#main article.text{
    width: 100%;
    height: 60px;
}
#main article.text p{
    text-align: center;
    font-size: 25px;
    font-weight: 600;
    padding: 17px 0;
    border: 1px solid black;
    height: 24px;
}

/*움직이는 웹 - 실습6*/
#main article.one:hover{
    background-color: #8683bd;
}
#main article.one{
    transition-property: background-color;
    transition-duration: 3000ms;
}
#main article.two:hover{
    background-color: #bf7eac;
    
}
#main article.two{
    transition-property: background-color;
    transition-duration: 3000ms;
}
#main article.three:hover{
    background-color: #75ccd0;
    
}
#main article.three{
    transition-property: background-color;
    transition-duration: 3000ms;
}
/*
#main article:hover{
    opacity: 0.6;
}
#main article{
    transition-property: opacity;
    transition-duration: 3000ms;
}
*/
#main article img:hover{
    /*
      이미지 주변에 영향 생김.
      0,0 좌표를 기준으로 커짐.
      CPU 연산이므로 부드럽지 않다.
      width: 110%;
      height: 110%;
    */
    transform: scale(1.1);
}
#main article img{
    transition:all 500ms;
}

#footer{
    clear: both;/*이전에 사용한 float은 무시*/
    color: gray;
    text-align: center;
    background-color: lightgray;
    font-size: 12px;
    padding: 20px 0;
    height:  30px;
}
#footer div.copyright{
    width: 50%;
    float: left;
}

#footer div.address{
    width: 50%;
    float: right;
}
```
*변경 사항 및 설명은 css 실습 코드에 주석 참조.

### 미디어 쿼리 소개
- 미디어 쿼리란 PC뿐만 아니라 모바일과 태블릿에도 대응 되는 웹사이틑 만들기 위해 사용.
- 모바일에 대응되는 반응형 또는 적응형 웹사이트를 만들 때 사용되는 CSS 구문

```
@media (min-width:320px) and (max-width:800px) {
        .media{
            width: 300px;
            height: 300px;
            background-color: yellow;
            border: 10px solid blue;
        }
        
    }
```
- min-width와 max-width로 브라우저 가록폭 설정.
- 위 코드는 가로폭이 최소 320px, 최대 800px 되었을 경우, 중괄호 안의 css 속성으로 대체하겠다는 의미

#### viewport
```
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```
- **미디어 쿼리가 제대로 작동하지 않는 문제**가 발생할 수 있으므로 **viewport**로 너비와 배율을 설정해야 모바일 디바이스에서 **의도한**화면을 볼 수 있음.
- 다양한 디지털 기기의 화면 상에 표시되는 영역을 의미 너비와 배율을 설정할 때 사용하는 메타 태그의 속성 중 하나.
- width=device-width : viewport의 가로폭 = 디바이스의 가로폭
- initial-scale=1.0 : 비율은 항살 1.0

[실습코드]
```
<!DOCTYPE html>
<html>
<head>

  <meta charset="UTF-8">
  <!--
    모바일모드는 실제 크기와 상관없이 기본적으로 900px로 인지하도록 되어있음.
    meta 태그로 실제 기기(브라우저)화면을 폭으로 그대로 사용하도록 설정해 주어야 한다.
  -->
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>미디어쿼리 기초</title>
  
  <style>
  
    .media {
        width: 500px;
        height: 500px;
        background-color: red;
        transition: width 0.5s, height 0.5s,background-color 0.5s;
    }
  
    @media (min-width:320px) and (max-width:800px) {
        .media{
            width: 300px;
            height: 300px;
            background-color: yellow;
            border: 10px solid blue;
        }
        
    }
  </style>
  
</head>
<body>

  <div class="media"></div>

</body>
</html>
```