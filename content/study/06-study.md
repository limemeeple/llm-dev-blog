---
title: "6. DOM과 이벤트"
date: 2026-07-28
draft: false
tags: ["DOM", "EVENT"]
categories: ["STUDY"]
summary: "이어드림2026 서비스 개발 수업 DAY6"
weight: 6
---
## DOM과 이벤트
### DOM이란?
- 문서 객체 모델(Document Object Model). 구조화된 문서(XML, HTML)를 객체 형태로 표현하는 방식.
- DOM은 자바스크립트 엔진이 HTML 코드를 해석한 결과. 각각의 태그가 자바스크립트의 node 객체로 변환한다.

### node 객체
- 노드 : HTML DOM에서 정보를 저장하는 계층적 단위.
- 노드 트리 : 노드들의 집합으로, 노드 간의 관계를 트리 구조로 나타낸 것.
- node 객체에는 해당 요소를 조작할 수 있는 수많은 프로퍼티와 메서드가 존재한다.

### document 객체
- document 객체는 웹페이지를 의미한다. javaScript에서 DOM에 접근하고자 할 때에는 document 객체를 사용해야 한다.

### document, node 객체
- document와 node 객체는 아래와 같이 HTML 요소와 관련된 작업을 도와주는 다양한 메서드를 제공한다.
  - HTML 요소의 선택과 조작
  - HTML 요소의 생성 및 추가
  - HTML 이벤트 핸들러 추가

[HTML 요소의 선택]
|메서드 사용 예시|설명|
|---|---|
|document.getElementById("box")|해당 아이디의 요소를 선택|
|document.getElementsByClassName("list-item")|해당 클래스에 속한 요소를 모두 선택|
|document.getElementsByName("email")|해당 name 속성값을 가지는 요소를 모두 선택|
|document.querySelectorAll("ul > li")| 해당 CSS 선택자로 선택되는 요소를 모두 선택|
|document.querySelector("header > #title")|해당 CSS 선택자로 선택되는 요소 중 첫 번째 요소 선택|
[실습코드]
```
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>JavaScript 문법</title>
  <link rel="icon" href="favicon.png">
</head>
	<body>
		<h1>H1 Tag1</h1>
        <h1>H1 Tag3</h1>
        <h1>H1 Tag4</h1>
        <h1>H1 Tag5</h1>
		<h2 class="head2">head2 class</h2>
        <h2 class="head2">head2 class</h2>
        <h2 class="head2">head2 class</h2>
        <h2 class="head2">head2 class</h2>
		<h3 id="head3">head3 id</h3>
	</body>
  <script>
      var elemTag = document.getElementsByTagName('h1');
      console.log(elemTag);
      var elemClass = document.getElementsByClassName('head2');
      console.log(elemClass);
      var elemId = document.getElementById('head3');
      console.log(elemId);
      console.log([elemId]);
      console.log("---------------------------------------------------------");
      // 가장 먼저 걸리는 것 하나(css 선택자 사용)
      console.log(document.querySelector('.head2'));
      // 전체(css 선택자 사용)
      console.log(document.querySelectorAll('.head2'))
  </script>
</html>
```

### HTML 요소의 조작
|메서드 사용 예시|설명|
|---|---|
|요소.style.스타일속성 = "200px"| 요소의 스타일 조회 및 변경|
|요소.innerHTML = "<li><sapn>hello</span></li>"| 요소 하위의 HTML 조회 및 변경|
|요소.innerText = "Hello Elice"| 요소 하위의 텍스트 조회 및 변경|
|요소.getAttrubute("placeholder") &#10; 요소.setAttribute("min", "20")| 요소의 HTML 속성 조회 및 변경|
|요소.classList = "button button--red"|요소의 class 수정|

[실습 코드]
```
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>JavaScript 문법</title>
  <link rel="icon" href="favicon.png">
</head>
<body>
    <ul id="list">

    </ul>
</body>
<script>
    var li = document.createElement('li');
    //새로운 아이템 이라는 문구를 li 사이에 넣기
    var ul = document.getElementById('list')
    ul.appendChild(li);
    li.innerHTML = "새로운 아이템";

    //<input type="submit" value="로그인"/>
    //<input>
    var input = document.createElement('input');
    input.setAttribute('type', 'submit');
    input.setAttribute('value', '로그인');

    var body = document.getElementsByTagName('body');
    body[0].appendChild(input);
</script>
</html>
```
- 'createElement'로 생성된 노드는 처음엔 자바스크립트 상에서만 존재한다.
- 화면에 보여지기 위해 'appendChild'등을 통해 DOM 추가 되어야 한다.

### HTML 이벤트 핸들러 추가
[요소의 여러 이벤트에 대해 이벤트 핸들러 연결]
```
<요소 onclick="handler()"/>

function handler(){...}
```
```
요소.onclick = function(){...}
요소.onsubmit = function(){...}
     ...
```
```
요소.addEventListener("click", function(){...})
```
#### 이벤트란?
- 웹 브라우저가 알려주는 HTML 요소에 대한 사건의 발생.
- 자바스크립트는 발생한 이벤트에 반응하여 특정 동작을 수행할 수 있다.

#### 이벤트 핸드러
- 이벤트가 발생했을 때 처리를 담당하는 함수.
- 지정된 이벤트가 발생하면, 그 요소에 등록된 이벤트 핸들러를 실행한다.

#### 이벤트 핸들러 등록 - HTML 속성
- HTML 요소의 'on이벤트명' 속성에 동작 추가하는 방법
- HTML 코드에 JS 코드가 포함되는 방식으로, 권장 되지는 않는다.
```
<button onclick="handleClick()">Click Event</button>

<script>
  function handleClick() { console.log("button clicked");}
</scirpt>  
```
#### 이벤트 핸들러 등록 - node 객체 프로퍼티
- node 객체의 'on이벤트명' 프로퍼티 핸들러 함수를 등록하는 방법.
```
var button = document.getElementById("btn");
button.onclick= function(){ console.log("button clicked");}
```
[실습 코드]
```
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>JavaScript 문법</title>
    <link rel="icon" href="favicon.png">
    <style>
        div{
            width: 250px;
            height: 250px;
            background-color: yellowgreen;
        }
    </style>
	</head>
    <!--onload : html 문서가 다 읽히면 일어나는 이벤트-->
    <body onload="alert('loading 끝')">
        <!-- 한 요소에 여러 이벤트를 추가할 수도 있다. -->
        <div onmouseover="mouseEvt('over')" onmouseout="mouseEvt('out')"></div>
        <br/>
        <!-- inline 속성을 갖은 div : 특정 문자열을 지정할때 사용-->
        <h3>오늘의 날짜 : <span id="demo"></span></h3>
        <input type="button" onclick="getDay()" value="날짜 추출"/>
        <br/>
        <input type="text" onkeydown="going(event)" value=""/>
        <input type="text" onkeyup="typing(event)"/>
        <h4 id="typing"></h4>
        <!-- this == event.target -->
        <select onchange="selectOne(this)">
            <option value="Audi">Audi</option>
            <option value="BMW">BMW</option>
            <option value="Benz">Benz</option>
            <option value="Volvo">Volvo</option>
        </select>
	</body>
    <script>
        function selectOne(elem){
            console.log(elem.value);
            document.getElementById('typing').innerHTML = elem.value;
        }

        function mouseEvt(arg){
            console.log(arg);
            var div = document.getElementsByTagName('div')[0];
            console.log([div]);
            div.innerHTML = 'MOUSE' +arg;
            if(arg == 'over'){
                div.style.backgroundColor = 'yellow';
            }else{
                div.style.backgroundColor = 'green';
            }
        }

        function getDay(){
            var date = new Date()
            console.log(date);
            document.getElementById('demo').innerHTML = date;
        }

        //evt: 일어난 사건에 대한 모든 정보가 들어가 있음.
        function going(evt){ // keydown : 눌렸는지에 대한 여부
            // evt.target : 이벤트가 일어난(당한) 당사자
            console.log(evt.target.value);
            document.querySelector('h3').innerHTML = evt.target.value;
        }

        function typing(evt){ // keyup : 입력한 내용
            // evt.target : 이벤트가 일어난(당한) 당사자
            //console.log(evt.target.value);
            document.querySelector('h3').innerHTML = evt.target.value;
        }
    </script>
</html>
```

#### 이벤트 핸들러 등록 - addEventListener
- 첫 번째 인수로 '이벤트명' 전달, (on을 붙이지 않음) 두 번재 인수로 핸들러 함수 전달.
```
var button = document.getElementById("btn");

button.addEventListener("click", function(){
  console.log("button clicked");
});
```
### event 객체
- 이벤트 핸드러 함수에 첫 번째 인수로 전달되는 객체 -> 해당 이벤트에 대한 정보와 기능 담겨 있다.
```
var button = document.getElementById("btn");

button.addEventListener("click", function(event){
  console.log(event);
});
```

#### event 객체 - 마우스 정보
- 마우스 관련 이벤트의 경우 event 객체를 통해 마우스에 대한 정보를 얻을 수 있다.(위치 클릭 정보 등)
[실습 코드]
```
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>JavaScript 문법</title>
    <link rel="icon" href="favicon.png">
    <style>
        #eventZone{
            width: 250px;
            height: 250px;
            background-color: yellowgreen;
            text-align: center;
        }
    </style>
	</head>
	<body>
        <h4 id="msg">FOCUS NULL</h4>
		<div id="eventZone"></div>
        <h4>mouse position : <span id="pos"></span></h4>
	</body>
    <script>
        var zone = document.getElementById('eventZone');
        //이벤트걸요소. addEventListener('이벤트명', 실행함수명)
        // 함수 등록시() 은 뺀다. ()을 넣을 경우 바로 실행하라는 명령이기 때문.
        zone.addEventListener('click',callBack);

        function callBack(){
            zone.style.backgroundColor = 'orange';
            zone.innerHTML = 'click';
        }

        // 익명함수 -> 여기서만 딱 사용 된다.(이름이 필요 없다.)
        zone.addEventListener('dblclick', function(evt){
            evt.target.style.backgroundColor = 'blue';
            // evt.target == this
            evt.target.innerHTML = 'double click';
        });

        //mouseenter
        zone.addEventListener('mouseenter', function(evt){
            evt.target.style.backgroundColor = 'white';
            evt.target.innerHTML = "enter";
        });

        //mouseout
         zone.addEventListener('mouseout', function(evt){
            this.style.backgroundColor = 'pink';
            this.innerHTML = "";       
        });

        document.addEventListener('mousemove', function(evt){
            var pos = document.getElementById('pos');
            pos.innerHTML = evt.clientX + '/' + evt.clientY; 
            //console.log(evt.clientX + '/' + evt.clientY);
        });
    </script>
</html>
```

#### event 객체 - 키보드 정보
- 키보드 관련 이벤트의 경우 event 객체를 통해 키보드에 대한 정보를 얻을 수 있다.(누른 키 정보 등)
- 이처럼 각 이벤트 발생에 대한 정보를 event 객체를 통해 얻을 수 있다.

#### event 객체 - target
- 'event.target'프로퍼티를 통해 이벤트가 발생한 타겟 요소의 node 객체에 접근할 수 있다.

#### event 객체 - preventDefault
- form 태그에서 submit 버튼의 기본 도작은 form 제출 -> 새로고침
- 이 기본 동작을 막기 위해 event객체의 'event.preventDefault()'메서드 사용한다.

[실습 코드]
```
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>JavaScript 문법</title>
  <link rel="icon" href="favicon.png">
</head>
<body>
  <!--
    action="보낼 서버 주소"
    method="전송 방식(POST|GET)"
    GET : 보내는 내용이 주소에 나타남(기본)
    POST : 보내는 내용이 주소에 나타나지 않음.
  -->
  <form id="login-form" action="" method="get">
    <input type="text" name="id" placeholder="아이디"/>
    <input type="password" name="pw" placeholder="비밀번호"/>
    <!-- form에 있는 내용을 서버로 전송 -->
    <input type="submit">
  </form>
</body>
<script>
      //id나 pw가 공백이면 서버로 전송하지 않는다.
      var id = document.querySelector('input[name="id"]');
      var pw = document.querySelector('input[name="pw"]');
      var form = document.getElementById('login-form');

      form.addEventListener('submit',function(evt){         
          if(id.value == '' || pw.value == ''){
            alert("아이디 또는 비밀번호를 입력해주세요.");
            // 이벤트의 기본동작을 사전에 막는다.
            evt.preventDefault();
          }
      });
</script>
</html>
```

#### 이벤트 핸들러 제거 - removeEventListener
- 'removeEventListener'메서드로 등록된 이벤트 핸들러를 제거할 수 있다.
[실습 코드]
```
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>JavaScript 문법</title>
  <link rel="icon" href="favicon.png">
  <style>
    div{
      width: 500px;
      height: 500px;
      border: 1px solid black;
    }
  </style>
</head>
<body>
  <div></div>
  <h3 id="pos"></h3>
  <button onclick="evtAdd()">이벤트 추가</button>
  <button onclick="evtDel()">이벤트 제거</button>
</body>
<script>
  function printPos(evt){
     var pos = document.getElementById('pos');
     pos.innerHTML = evt.clientX + '/' + evt.clientY;
  }

  var div = document.querySelector('div');

  function evtAdd(){
    //요소.addEventListener('이벤트', 할일)
    div.addEventListener('mousemove',printPos);
  }

  function evtDel(){
    div.removeEventListener('mousemove',printPos);
  }
</script>
</html>
```