---
title: "4. JavaScript 문법"
date: 2026-07-28
draft: false
tags: ["javascript"]
categories: ["STUDY"]
summary: "이어드림2026 서비스 개발 수업 DAY4"
weight: 4
---
#### CSS 속성 상속
- 미디어 쿼리 외부 영역에 있는 CSS 속성을 상속 받음.
- 만약 상속을 받지 않고자 하면 속성값으로 none 입력
[실습코드]
```
<!DOCTYPE html>
<html>
<head>

  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  
  <title>미디어쿼리</title>
  
  <style>
    .media {
      width: 500px;
      height: 500px;
      background-color: yellow;
      border: 10px, solid, red;
    }

  @media (min-width: 320px) and (max-width: 800px) {
      .media {
        width: 300px;
        height: 300px;
        /*배경색과 경계선은 기본클래스 것을 그대로 상속받아서 적용 된다.*/
        /*겹치는 속성이 발생시 덮어쓰게 된다.*/
        background-color: yellowgreen;
        border: 10px, dashed, blue;
      }
  }
  </style>
  
</head>
<body>

  <div class="media"></div>

</body>
</html>
```
### JavaScript 문법

```
var fruit;         //변수 선업
fruit = "apple";   //변수 초기화
var fruit = "apple"; // 변수 선언 및 초기화
```
- 변수 선언 : 데이터를 담을 공간을 생성하는 것
- 변수 초기화 : 생성된 변수에 데이터를 전달하는 것
- 주석으로 된 변수 초기화 부분에서 'fruit'는 변수명, 'apple'은 데이터 다.

```
var fruit = "apple";
fruit = "banana";
```
- 변수 'fruit'의 데이터는 변경할대 변수는 이미 선언 되었으므로 var 키워드를 다시 작성할 필요가 없다.

```
console.log();
```
- 변수 안에 데이터를 확인할 때 사용하는 명령어.

#### 데이터 타입
- String : 문자열.
- Number : 숫자.
- Function : 함수.
- Array : 배열.
- Object : 객체.
- Boolean : 불린.
- undefined : 정의되지 않음.
- null : 널.

[실습 코드]
```
<html>
    <head>
        <meta charset="UTF-8">
        <title>Java Script</title>
        <style>/</style>
    </head>
    <body>
        
    </body>
    <script>
        //char -> string
        var str1 = "Hello world!"; //더블쿼터 - 문자열을 의미
        var str2 = 'Nice to meet you'; //싱글쿼터 - 문자열을 의미(특정 언어에서는 인정 안함.)
        console.log(str1);
        console.log(str2);
        debugger;// F12 모드에서만 동작, 개발 후 반드시 지워줄것
        var num ="20";
        console.log(num);
        console.log(num+1);

    </script>
</html>
```
#### 함수
```
var func1 = function(){
    console.log("func1");
}// 함수 생성

func1(); //함수 호출
```
- 함수 생성 : function 키워드를 사용하여 생성.
- 함수 호출 : 함수 안에 있는 코드를 실행히시키고, 그 결과를 반환

```
var area = function(width, height)//매개변수{
    return width * height; //반환값
}

var result = area(10,20); //(10, 20) 매개변수 값 전달
console.log(result); //매개변수 값 전달.
console.log(area(10, 20));
```
- 매개변수 : 함수 호출 시 함수에 전달 되는 데이터
- return(반환 값) : 함수 호출 이후 반환하는 값

[실습 코드]
```
<html>
    <head>
        <meta charset="UTF-8">
        <title>Java Script</title>
        <style></style>
    </head>
    <body>
        
    </body>
    <script>
        //함수 구성 -> 함수명, 매개변수(인자값, 레퍼런스변수), 반환문
        
        function 토스트기(빵){
            return "구워진 " + 빵;
        }

        var 접시 = 토스트기("호밀빵");
        console.log(접시);

        function 번호표기기(){
            return "번호표"
        }

        function 저금통(동전){
            console.log(동전 + '원 저금');
        }

        function 호출벨(){
            console.log('띵동');
        }

        var func_var = function(){
            console.log('변수에 담긴 함수');
        }

        func_var();
    </script>
</html>
```
#### 배열
- 비슷한 성격을 갖고 있는 데이터를 하나의 변수 안에서 관리.
- 인덱스를 활용하여 데이터 조회(0부터 시작)

[실습 코드]
```
<html>
    <head>
        <meta charset="UTF-8">
        <title>Java Script</title>
        <style></style>
    </head>
    <body>
        
    </body>
    <script>
        //array 선언
        var arrNum = []; // 리터럴
        var arrStr = new Array(); // 객체화 방식
        
        //데이터 추가
        arrNum = [1, 2, 3, 4];
        arrNum[4] = 5; //방번호 지정 후 추가.
        arrNum.push(6);
        console.log(arrNum);
        console.log(arrNum.length); //배열의 길이를 알 수 있다.

        // 배열에 아무거나 다 넣어보자
        arrNum.push(1);
        arrNum.push("A");
        arrNum.push(false);
        arrNum.push([10,20,30]);//9
        arrNum.push(function(){console.log('함수 실행')});
        console.log(arrNum);
        console.log(arrNum[9]);
        arrNum[10](); //함수를 실행할때는 매개변수를 넣을 주머니가 필요.
        
    </script>
</html>
```
[실습 코드2]
```
<html>
    <head>
        <meta charset="UTF-8">
        <title>Java Script</title>
        <style></style>
    </head>
    <body>
        
    </body>
    <script>
        var x = 10;
        var y = x;

        x = 20;

        console.log(x,y);
        
        var a = [1,2,3,4,5];
        var b = a;

        console.log("a : " + a.valueOf())
        console.log("b : " + b.valueOf())

        a[0] = 50;     //50,2,3,4,5
        b[4] = 100;    //1,2,3,4,100
        console.log("a : "+a.valueOf());
        console.log("b : "+b.valueOf());
        /*
            a의 값을 b에 대입할때 값을 복사해서 그대로 넣어줘야 한다.(휴대폰을 하나 더 사서 주는 개념)
            배열의 경우 덩치가 커질것을 우려해 대입시 위치 정보만 공유한다.(비싸니깐 휴대폰 위치만 공유한 개념)
            그래서 a 와 b에서 변경한 내용이 모두 반영되는것(shallow copy)
        */

        /* 진짜로 독립되게 사용하고 싶다면? -> 노가다로 값을 하나하나 전달 */
        var src = [1,2,3,4,5];
        var dest = [];
        
        for(let i=0; i < src.length; i++){
            dest[i] = src[i]
        }

        console.log("src : " + src);
        console.log("dest : " + dest);
        
        src[0] = 'true';
        dest[5] = 'false';
        console.log('src : ' + src.valueOf());
        console.log('desc : ' + dest.valueOf());

        

    </script>
</html>
```

#### 객체
- 프로퍼티, 메서드로 구성.
- 키-값 형태로 데이터 저장.
여러 종류의 데이터 타입 삽입 가능.
```
<html>
    <head>
        <meta charset="UTF-8">
        <title>Java Script</title>
        <style></style>
    </head>
    <body>
        
    </body>
    <script>
        // Object == Dict
        // 오브젝트 선언
        var car = {};
        var computer = new Object();

        // 값 넣는 방법 1
        car.type = 'Fiat';
        car.model = 500;
        car.color = 'white';

        console.log(car);

        // 값 넣는 방법 2
        computer['cpu'] = 'Octa core';
        computer['ram'] = '16GB';
        computer['game'] = false;
        computer['price'] = 1400000;

        console.log(computer);

        computer.work = function(){
            console.log('입력된 작업 실행');
        }

        console.log(computer.cpu);
        console.log(computer.ram);
        computer.work();
    </script>
</html>
```

#### undefined, null
- undefined : 변수 안에 데이터를 저장하지 않은 상태.
- null : 개발자가 의도적으로 변수가 비어 있음을 나타낸 상태.

#### 내장 프로퍼티와 메서드
[문자열]
- 자주 사용하는 프로퍼티와 메서드를 아래 실습 코드에 작성해 보았다.
```
<html>
    <head>
        <meta charset="UTF-8">
        <title>Java Script</title>
        <style></style>
    </head>
    <body>
        
    </body>
    <script>
        // 문자열 -> 문자 + 배열
        // 문자 배열을 다루는 오브젝트
        // 소속된 프로퍼티(속성)과 메서드가 존재 한다.
        var str = 'Hello World';
        console.log(str.length);
        console.log(str[0]);//배열 개념이므로 인덱스가 존재한다.
        console.log(str.charAt(1));
        console.log(str.split(' '));
        // 0 번부터 보여주고 , 5번 부터 버린다.
        console.log(str.substring(0,5));//slicing str[0:5]
    </script>
</html>
```
[배열]
- 자주 사용하는 프로퍼티와 메서드를 아래 실습 코드에 작성해 보았다.
```
<html>
    <head>
        <meta charset="UTF-8">
        <title>Java Script</title>
        <style></style>
    </head>
    <body>
        
    </body>
    <script>
        var fruits = ['사과', '바나나', '포도', '배', '오렌지', '복숭아'];
        var result; // undefined == null point Exception

        console.log(fruits);
        console.log(result);
        
        //push [0][1][2] <- [3]
        fruits.push('new 1');
        console.log(fruits.join(','));

        //unshift [3] -> [0][1][2]
        fruits.unshift('new 2');
        console.log(fruits.join(','));

        //pop() [0][1][2] -> [3]
        result = fruits.pop();
        console.log('result : ' + result);
        console.log(fruits.join(','));

        //shift() [0] <- [1][2][3]
        result = fruits.shift();
        console.log('result : ' + result);
        //join() : 배열안에 내용을 특정한 구분자로 연결해서 문자열로 보여줌.
        console.log(fruits.join(','));

        //slice(a인덱스 부터 보여주고, b인덱스 부터 버려라)
        //원본은 건드리지 않고 새로운 배열에 적용 시킨다.
        var newArr = fruits.slice(1,4);
        console.log(newArr);

        //indexOf() 특정 내용을 앞에서부터 찾아 인덱스 번호를 반환(1개)
        var idx = fruits.indexOf('수박'); //바나나
        console.log('수박이 있는 위치 : ', idx); // -1 <- 더이상 없다.

        // splice() - 원복을 건드린다. 추가/수정/삭제가 모두 가능.
        // x번 인덱스 포함, n개 삭제;
        fruits.splice(2, 2); //2번 인덱스인 포로를 포함 2개 삭제. 
        console.log(fruits);
    </script>
</html>
```

