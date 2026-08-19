---
title: "8. 내장 객체, 실행 컨텍스트와 this"
date: 2026-08-09
draft: false
tags: ["내장 객체", "this"]
categories: ["STUDY"]
summary: "이어드림2026 서비스 개발 수업 DAY8"
weight: 8
---
## 내장 객체
- 자바스크립트는 여러 용도에 활용하는 객체를 내장하고 있다.
- 숫자 다루기, 문자 다루기, 날짜 다루기, JSON 객체 다루기 등에 유용한 객체를 제공한다.
- 핵심 내장 객체들의 기능을 이해하면, 실제 프로젝트에서 유용하게 활용할 수 있다.

### window
- DOM document를 포함하는 브라우저 창을 나타내는 개체.
- 현재 창의 정보를 얻거나, 창을 조작할 수 있다.
[실습 코드]
```
<html>
    <head>
        <meta charset="UTF-8">
        <title>내장객체</title>
        <style>

        </style>
    </head>
    <body>
        <button onclick="myWinOpen()">창열기</button>
        <button onclick="myWinClose()">창닫기</button>
    </body>
    <script>
        //console.log(window);
        console.log(window.innerWidth+' / '+window.innerHeight);
        let win;
        function myWinOpen(){
            //window.open(url,창제목,옵션);
            //window.open('','',''); // 크기를 정해주지 않으면 새 탭으로 생성
            // width=100, height=100 : 창 크기
            // top=100,left=100 : 창 위치(1번모니터)
            win = window.open('https://www.daum.net/','',
                'width=400,height=400,top=100,left=500,resizable=no,scrollbar=no');
        } 

        function myWinClose(){
            win.close();
        }
    </script>
</html>
```
### document
- 브라우저에 로드된 웹 페이지에 대한 객체.
- 문서의 title, URL 등의 정보를 얻을 수 있고, 요소 생성, 검색 등의 기능을 제공한다.
```
function pritDocumentInfo(){
    console.log("문서 URL : ", document.URL);
    console.log("문서 타이틀 : ", document.title);
    console.log("모든 노드 : ", document.querySelectorAll("*"));
}
```

### Number
- 자바 스크립트의 number 자료형을 감싸는 객체로, 유의미한 상수값, 숫자를 변환하는 메서드 등을 제공한다.
- 'Number.toFixed' 메서드는 숫자의 소숫점 자릿수를 제어한다.

### Math
- 기본적인 수학 연산 메서드, 상수를 다루는 객체.
- 'Math.max', 'Math.min'은 개별 숫자를 인자로 받아 각각 최대, 최솟값을 리턴한다.
- 'Math.random'은 0에서 1사이의 float number을 랜덤으로 구한다.
- 'Math.floor'는 소숫점 이하 숫자를 버린다.
[실습 코드]
```
<html>
    <head>
        <meta charset="UTF-8">
        <title>내장객체</title>
        <style>

        </style>
    </head>
    <body>

    </body>
    <script>
        console.log(Math);// min,max,ceil,floor,round
        let num = 314.56789;
        console.log(num,'->',num.toFixed(2)); // 문자열로 처리 된다.
    </script>
</html>
```

### Date
- 특정 시점의 날짜를 표시하기 위한 객체.
- 날짜와 관련된 작업을 하기 위한 여러 메서드를 포함한다.
- 'Date.getDay'는 요일을 0(일요일)부터 6(토요일)로 반환한다.
- 이 외에 년도, 월, 일, 시, 분, 초, 밀리초 등을 구할 수 있다.
- 'setDate'등의 메서드로 시간을 설정한다.
- 'toDateString' 메서드는 특정 포맷의 날짜로 문자열을 반환한다.
- 'getTime' 메서드는 1970.01.01 시점부터 흐른 시간을 밀리초 단위로 반환한다.
[실습 코드]
```
<html>
    <head>
        <meta charset="UTF-8">
        <title>내장객체</title>
        <style>

        </style>
    </head>
    <body>

    </body>
    <script>
        // js 에서는 변수선언시 선언문을 안 붙이면? 전역변수로 인식        
        let time = setInterval(()=>{
            const date = new Date();
            
            console.log(date); // Wed Aug 05 2026 13:34:20 GMT+0900 (한국 표준시)
            let year = date.getFullYear(); // 연도(YYYY)
            let month = date.getMonth()+1; //월(0~11) : 인덱스 개념
            let day = date.getDate();       //일(1~31)
            let hours = date.getHours();    //시(0~23)
            let minutes = date.getMinutes(); //분(0~59)
            let seconds = date.getSeconds(); //초(0~59)
    
            let str_month = month.toString().padStart(2,'0');//만들고싶은자릿수,붙일내용
            let str_day = day.toString().padStart(2,'0');
            let str_hours = hours.toString().padStart(2,'0');
            let str_min = minutes.toString().padStart(2,'0');
            let str_sec = seconds.toString().padStart(2,'0');
    
            let print_date = `${year}-${str_month}-${str_day} ${str_hours}:${str_min}:${str_sec}`;
            document.getElementsByTagName('body')[0].innerHTML= '<h1>'+print_date+'</h1>';
        },1000);

    </script>
</html>
```
### String 
- 문자열을 조작하기 위한 여러 메서드를 포함한다.
```
"hello".toUpperCase();           // 'HELLO'
"Daiel,Kim,SW".split(',');       // [ 'Daniel, 'Kim', 'SW' ]
"Daiel,Kim,SW".replace(',', ' ') // 'Daniel Kim,SW'
"Daiel,Kim,SW".includes("Kim");  // true
"  Daiel,Kim,SW  ".trim();       // 'Daniel,Kim,SW'
"Daiel,Kim,SW".indexOf("Kim");   // 7
```

### JSON
- 자바스크립트 객체를 직렬화 / 역직렬화 하는데 사용되는 객체.
  - 직렬화 : 객체 데이터를 다른 시스템에서도 사용할 수 있도록 바이트 스트림 형태의 연속적인 데이터로 변환하는 포맷 기술.
  - 역직렬화 : 직렬화 된 데이터를 다시 원래의 객체 형태로 되돌린는 것.
- 데이터 통신 과정에서 자주 사용된다.
[실습 코드]
```
<html>
    <head>
        <meta charset="UTF-8">
        <title>내장객체</title>
        <style>

        </style>
    </head>
    <body>
        항목 :  <input type="text" name="key" value=""/>
        <br/> 
        값 :    <input type="text" name="val" value=""/> 
        <br/>
        <button onclick="add()">추가</button>
    </body>
    <script>
        // Java Script Object Notion
        let json = {
            name:'Daniel',
            age:12,
            message:'Hello, My name is Daniel,Lee'
        };
        console.log(json);

        // 직렬화(통신을 할수 있는 형태로 변경(object -> string))
        let str_json = JSON.stringify(json);
        console.log(str_json);
        // 문자열 다루기
        //console.log(str_json.toLocaleUpperCase());
        //console.log(str_json.split(','));
        //console.log(str_json.includes('Lee'));
        //console.log(str_json.indexOf('age'));

        // {"name":"Daniel","age":12,"message":"Hello, My name is Daniel,Lee"
        // ,"gender":"male","married":false}
        let values = str_json.replace('{','').replace('}','');
        console.log(values);
        function add(){
            let key = document.querySelector('input[name="key"]').value;
            let val = document.querySelector('input[name="val"]').value;
            console.log(key+':'+val);
            values = values.replace('{','').replace('}','');
            values += `,"${key}":"${val}"`;
            values = `{${values}}`;
            console.log(values);
            let result = JSON.parse(values); // 역직렬화
            console.log(result);
        }

    </script>
</html>
```

## 실행 컨텍스트
- 자바스크립트 엔진은 코드 실행 전 실행 컨텍스트라는 것을 생성한다.
- 실행 컨텍스트는 생성 단계(변수 선언문, 먼저 저장) => 실행 단계(나머지 코드실행, 변수값 할당) 두 단계를 통해 생성 및 사용된다.
- 그리고 실행 컨텍스트는 각 스코프가 실행되기 직전에 생성 된다.
```
// 1. 전역 실행 컨텍스트 생성.
const x = 10;

function func1(){
    const x = 20;

    func2();
}

function func2(){
    console.log(x);
}
// 2. func1 실행 컨텍스트 생성.
func1(); 
// 3. func2 실행 컨텍스트 생성.
func2(); 
```

### 자바스크립트 코드 실행 단계
- 1. 전역 코드 평가 -> 전역 실행 컨텍스트 & 렉시컬 환경 생성 
  - 변수 및 함수의 선언문을 먼저 실행시켜 렉시컬 환경에 기록
  - 추가로 this 바인딩을 진행

- 2. 전역 코드 실행 -> 전역 렉시컬 환경 업데이트
  - 선언된 변수의 초기화 코드를 만나면 해당 값으로 업데이트

- 3. 블록코드 평가 -> 블록 렉시컬 환경 생성 및 연결
  - 블록 실행 직전 블록 렉시컬 환경 생성, 현재 실행중인 실행 컨텍스트와 연결

- 4. 블록 코드 실행 -> 블록 렉시컬 환경 업데이트
  - 블록 내 선언된 변수의 초기화 코드를 만나면 해당 값으로 업데이트
  - 블록 실행 이후에는 전역 실행 컨텍스트가 다시 전역 렉시컬 환경에 연결된다.

- 5. 함수 코드 평가 -> 함수 실행 컨텍스트 & 렉시컬 환경 생성
  - 함수 내의 선언문을 먼저 실행 시켜 렉시컬 환경에 기록(배개변수와 전달된 인수 포함)
  - 추가로 this 바인딩(추후 학습) 및 상위 스코프 연결을 진행

- 6. 함수 코드 실행 -> 함수 렉시컬 환경 업데이트
  - 매개변수 업데이트 및 함수 내 선언된 변수의 초기화 코드를 만나면 해당 값으로 업데이트

- 함수 실행 이후에는 해당 함수의 실행 컨텍스트가 실행 컨텍스트 스택에서 빠져나온다.
- 생성되는 실행 컨텍스트는 모두 실행 컨텍스트 스택(콜 스택)에서 관리된다.
[시각화 자료]
![alt text](../img/script_run_img.png)

## this
- this는 자신이 속한 객체를 참조할 수 있는, 자기 참조 변수이다.
- 다른 객체 지향 프로그래밍 언어에서도 this를 비슷한 의미로 사용한다.
- 다른 언어에서 this는 보통 클래스 내부에서만 사용할 수 있다.
- 반면 JavaScript에서는 this를 어디에서나 사용할 수 있다. -> 대신 각 this가 무엇과 바인딩되어 있는지 파악해야 한다.

### this 바인딩 - 전역 및 일반 함수
- 기본적으로 this는 전역 객체와 바인딩 되어 있다.
- 일반적인 상황에서 전역 객체는 window 객체를 가리킨다.

### this 바인딩 - 메서드
- 객체 내의 메서드에서는 this는 메서드를 호출한 객체와 바인딩 된다.
- 호출한 객체가 중요하기 때문에, 다른 객체에서 해당 메서드를 호출하면 해당 객체와 바인딩 된다.

### this 바인딩 - 간접 호출(apply, call, bind)
- 'apply', 'call', 'bind' 메서드를 사용하면 함수를 간접적으로 호출할 수 있다.
- 이 방법을 통해 특정 객체와 함수를 직접 바인딩할 수 있다.
[실습 코드]
```
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>this</title>
        <style>
           
        </style>
    </head>
    <body>
        
    </body> 
    <script>
        const obj = {
            name:'Elice',
            age:20,
            introduce:function(){
                //this 는 자신이 소속된 상위 객체를 의미
                //python의 self와 유사.
                console.log(`My name is ${this.name} and I am ${this.age}`);
                console.log('Object this : ', this);
            }
        };
        //obj.introduce();

        console.log('window', this); //외부에서는 최상위 객체인 window 객체가 this가 된다.
    
        //함수 에서 this 는?
        //함수가 소속된 오브젝트가 없다면 this는 window를 가르키게 된다.
        function outerfunction(){
            console.log('outer function : ', this);//window
            innerfunc();
            function innerfunc(){
                console.log('inner function : ', this);//window
            }
        }
        //outerfunction();

        const hellobit = {
            name : "hellobit",
            age : 20
        }

        const caterlillar = {
            name : "caterlillar",
            age : 23
        }

        const cheshire = {
            name : "cheshire",
            age : 26
        }

        function introduce(){
            console.log(`My name is ${this.name} and I am ${this.age} years old`);
            console.log('Object this : ', this);
        }

        // 강제로 함수에 특정 객체를 바인딩 시켜줌.
        introduce.apply(hellobit);    // 바로실행, Apply -> Array : 매개변수를 담아 보내느 형식  
        introduce.call(caterlillar);  // 바로실행, Call -> Comma : 매개변수를 담아 보내는 형식
        // 바로 실행하지 않고 실행 가능한 함수를 반환.
        const func = introduce.bind(cheshire);
        func(); // 이후에 실행하면 된다.(매개변수는 실행 시 전달하면 된다.)

        function addArgs(a,b){
            console.log(this, a, b);
        }

        addArgs.apply(hellobit, [1,2]);
        addArgs.call(caterlillar, 3,4);
        //const args = addArgs.bind(cheshire);
        //args(5,6);
        addArgs.bind(caterlillar)(5,6);
        
    </script>
</html>
```

### 콜백 함수
- 콜백 함수란 매개변수를 통해 다른 함수로 전달되는 함수를 의미한다.
```
function plus(a, b) { return a + b; }
function minus(a, b) { return a - b; }

function calculateAndSquare(a, b, calFunc) {

    const result = calFunc(a, b);
    return result * result;
    
}

calculateAndSquare(3, 5, plus);   // 64
calculateAndSquare(3, 5, minus);  // 4
```

#### this 바인딩과 콜백 함수
- 해당 콜백 함수는 결국 특정 객체와 바인딩된 적이 없는 일반 함수 이므로, 전역 객체와 바인딩 된다.
- 이를 해결하기 위해 'bind' 배서드를 사용하여 this와 직접 바인딩한다.
- 혹은 직접 바인딩 없이 화살표 함수를 활용하는 방법도 있다.
[실습 코드]
```
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>DOM과 이벤트 2</title>
        <style>
           
        </style>
    </head>
    <body>
        
    </body> 
    <script>
        function square(callback){
            const result = callback();
            return result * result;
        }
        /*
        const calc = {
            a:3,
            b:5,
            calculate:function(){

                return square(function(){
                    return this.a + this.b;
                }); // 콜백 함수는 소속이 없기에 this는 window를 본다.
            
            }
        };

        console.log(calc.calculate()); //NaN
        */

        // 해결방법 1. 강제로 소속 객체를 연결해줘야 한다.(apply, call, bind);  
        /*
        const calc = {
            a:3,
            b:5,
            calculate:function(){

                return square((function(){
                    console.log(this);
                    return this.a + this.b;
                }).bind(this));
            }
        };

        console.log(calc.calculate()); //NaN
        */

        //해결방법 2. 화살표 함수
        const calc = {
            a:3,
            b:5,
            calculate:function(){
                return square(() => this.a + this.b); 
            } 
        };

        console.log(calc.calculate()); //64
    </script>
</html>
```

### 화살표 함수
- 일반 함수와 단순히 선언 방식만 다른 것이 아닌, this 바인딩 방식이 다르다.

#### 화살표 함수의 this
- 화살표 함수는 this 바인딩 자체가 존재하지 않는다. 따라서 화살표 함수 내부의 this는 그대로 상위 스코프의 this를 참조한다.
- 결국 화살표 함수가 정의된 위치에 의해 결정된다고 볼 수 있다. -> 렉시컬 this

#### 화살표 함수와 this 바인딩 문제
- 앞선 바인딩 문제를 화살표 함수가 해결하는 이유는 상위 스코프인 calculate의 this를 그대로 참조하기 때문이다.
[실습 코드]
```
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>DOM과 이벤트 2</title>
        <style>
           
        </style>
    </head>
    <body>
        <button>click</button>
    </body> 
    <script>
        let btn = document.getElementsByTagName('button')[0];
        //console.log(btn);
        /*
        btn.addEventListener('click', function(){
            console.log(this); //this는 내부적으로 event.currentTarget과 연결.
        });
        */
        
        // 화살표 함수는 정직하게 외부의 객체를 this로 삼는다.
        btn.addEventListener('click',evt =>
            console.log(this) //this는 내부적으로 event.currentTarget과 연결.
        );
    </script>
</html>
```