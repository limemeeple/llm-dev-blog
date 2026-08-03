---
title: "5. JavaScript 문법 2"
date: 2026-07-28
draft: false
tags: ["javascript"]
categories: ["STUDY"]
summary: "이어드림2026 서비스 개발 수업 DAY5"
weight: 5
---
#### 객체
- 프로퍼티, 메서드로 구성. 
- 키-값 형태로 데이터 저장. 
- 여러종류의 데이터 타입 삽입 가능.
[실습코드 1]
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

#### undefinde, null
- undefined : 변수 안에 데이터를 저장하지 않은 상태
- null : 개발자가 의도적으로 변수가 비어 있음을 나타낸 상태

#### 문자열 프로퍼티와 메서드
- 자세한 내용은 아래 소스 주석을 참조.
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
#### 배열 프로퍼티와 메서드
- 자세한 내용은 아래 소스 주석을 참조.
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
        console.log(fruits.join(','));

        // x번 인덱스 포함, n개 삭제, 넣을 값,....
        // 수정 : 2번 인덱스 부터 2개를 지우고, orange, peach를 추가한다.
        fruits.splice(2,2,'orange','peach');
        console.log(fruits.join(','));

        // 추가 : 2번 인덱스에 수박, 참외 추가.
        fruits.splice(2 , 0, '수박', '참외');
        console.log(fruits.join(','));
    </script>
</html>
```
#### Math의 수학 연산 매서드와 문자를 숫자로 변환하는 메서드
- 자세한 내용은 아래 소스 주석을 참조.
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
        // 메서드의 종류는 상위 객체나 클래스를 보면 대략 알 수 있다.
        console.log(Math);
        console.log(window);

        // Math 하위의 메서드들
        console.log('절대값 : ', Math.abs(-3));
        console.log('올림 : ', Math.ceil(0.3));
        console.log('내림 : ', Math.floor(10.3));
        console.log('임의의 수 : ', Math.random()); // 0 ~ 0.999999
        // 주사위 : 1, 2, 3, 4, 5, 6
        // 0*6 = 0 + 1
        // 0.9*6 =5.4 + 1 -> 6.4
        var num = Math.floor(Math.random() * 6) + 1;
        console.log('주사위 수 : ', num); 

        // window 하위 객체(너무 당연한 최상위 이므로 생략 가능)
        console.log(parseInt('20.6'));
        console.log(parseFloat('20.6'));

    </script>
</html>
```
#### 산술 연산자와 증감 연산자
- 자세한 내용은 아래 소스 주석을 참조.
```
<html>
    <head>
        <meta charset="UTF-8">
        <title>Java Script</title>
        <style></style>
    </head>
    <body>
        <p>단항 연산자 : a++</p>
        <p>이항 연산자 : a + b</p>
        <p>3항 연산자 : a+b == 3 ? true : false</p>
        
    </body>
    <script>
       /*이항 연산자 - 문자열 연산*/
       console.log('10'+'20');// 문자열 더하기는 append
       // 그 외는 연산이 가능(strict mode 에서는 안된다.)
       console.log('10'-'20');
       console.log('10'*'20');
       console.log('10'/'20');

       //단항 연산자
       var num = 10;
       console.log(++num); // 11
       console.log(--num); // 10

       console.log(num++); // 10?
       console.log(num--); // 11?

       //console.log(++num); 증가를 먼저하고 출력
       //한바퀴 라고 외치고 운동장 돌기
       //console.log(num++); 출력을 먼저하고 나중에 증가
       //운동장을 돌고 나서 한바퀴 라고 외치기

       /*3항 연산자*/
       // var val = 조건 ? 참일경우의 값 : 거짓일 경우의 값
       // val = "값" if 조건 else "아닐경우 값"
       var score = 80;
       var grade = score >= 90 ? 'A' : 'B';
       console.log(score + '의 등급 : ' + grade);
       score = 70;
       var grade = score >= 90 ? 'A' : score >= 80 ? 'B' : 'C';
       console.log(score + '의 등급 : ' + grade);
    </script>
</html>
```

#### 논리 연산자
- 자세한 내용은 아래 소스 주석을 참조.
```
<html>
    <head>
        <meta charset="UTF-8">
        <title>Java Script</title>
        <style></style>
    </head>
    <body>
        <p> == (느슨한 동등) : 필요하면 타입을 자동으로 변환해서 비교</p>
        <p> === (엄격한 동등) : 타입과 값을 모두 체크</p>
    </body>
    <script>
        // 1) 숫자와 문자열
        console.log(2 == '2');
        console.log(2 === '2');
        // 2) 불린과 숫자
        console.log(false == 0);
        console.log(false === 0);
        // 3) 빈문자열과 불린
        console.log('' == false);
        console.log('' === false);
        // 4) null 과 undefined
        console.log(null == undefined);
        console.log(null === undefined);

        //not 연산자 - 실행시 마다 뒤집히는 toggle 개념
        var flag = true;
        console.log('flag', flag);
        flag = !flag;
        console.log('flag', flag)
    </script>
</html>
```

#### 조건문
- 자세한 내용은 아래 소스 주석을 참조.
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
        var a = 20
        var b = 40

        //if(조건이 참일 경우) {실행}
        if (a < b) {
            console.log('a 는 b보다 작습니다.');
        }

        if (a > b) {
            console.log('a 는 b보다 큽니다.');
        } else {
            console.log('a 는 b보다 작거나 같습니다.');
        }
        
        var item = prompt('원하는 음료를 고르세요');
        console.log(item);
        var menu = ['콜라', '식혜', '생수', '아이스커피', '에너지음료'];

        var idx = menu.indexOf(item)
        
        if(0 > idx){
            console.log('1.목록에 있는 메뉴를 선택하세요 : '+ menu.join(','));
            alert('1.목록에 있는 메뉴를 선택하세요 : '+ menu.join(','))
        }else{
            //back tic 문자 '`' : f-string과 같은 효과
            console.log(`${menu[idx]}가 나왔습니다.`);
            alert(`${menu[idx]}가 나왔습니다.`);
        }


        /*
        if (item == '콜라') {
            console.log( item + '나왔습니다.');
        }else if(item == '식혜'){
            console.log( item + '나왔습니다.');
        }else if(item == '생수'){
            console.log( item + '나왔습니다.');
        }else if(item == '아이스커피'){
            console.log( item + '나왔습니다.');
        }else if(item == '에너지음료'){
            console.log( item + '나왔습니다.');
        }else{
            console.log('1.목록에 있는 메뉴를 선택하세요 : '+ menu.join(','));
        }
        */

    </script>
</html>
```
#### for문 
- 자세한 내용은 아래 소스 주석을 참조.
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
        var arr = [1, 2, 3, 4, 5];
        var str = "forEach와 for of의 차이";

        // 1. for - 세밀한 조정 가능, 문자열도 가능
        for (let i = 0; i < arr.length; i++) {   
            console.log(arr[i]);
        }
  
        // 2. for each - 문자열에도 사용할 수 없다.
        //arr.forEach(function(item, idx){
        //    console.log(item, idx)
        //    if(idx==3){ // 중간에 break로 멈출수가 없다.
        //        break;
        //    }
        //});

        // 3. for of - index가 없으며, 시작점 지정이 안된다.
        var idx = 0; //인텍스를 사용하고 싶다면 직접 만들어 사용.
        for (var item of arr) {
            console.log(item);
            if(item == 3){ //break 가능
                break;
            }
            idx++;
        }

        for (var ch of str) { // 문자열도 가능
            console.log(ch);
        }
        
    </script>
</html>
```
#### while 문
- 자세한 내용은 아래 소스 주석을 참조.
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
        /*
        // 조건이 참이면 {} 안을 실행
        var num = 0;
        while (true) { // 무한루프
            num++;
            console.log(num);
            if (num >= 10){ // 조건이 변경될 수 있거나 강제 정지 할 수 있는 코드 필요.
                break;
            }
        }
        console.log('반복 종료'); //unreachable code
        */
        
        var cnt = 1;
        while(cnt <= 10){ // 조건을 만족하면 실행
            console.log(`${cnt}회 사용`);
            cnt++;
        }

        // do while : 일단 실행하고 반복 여부를 체크.
        // do while 은 조건이 만족하지 않아도 한번은 실행하게 하고 싶을 경우.
        // 버스 내릴대 버스 태그
        cnt = 11
        do {
            console.log(`${cnt}회 사용`);
            cnt ++;
        } while (cnt <= 10);
    </script>
</html>
```