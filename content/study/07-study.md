---
title: "7. ES6와 배열 메서드"
date: 2026-08-04
draft: false
tags: ["스코프", "ES6"]
categories: ["STUDY"]
summary: "이어드림2026 서비스 개발 수업 DAY7"
weight: 7
---
### let, const
- 'var'이 아닌 다른 키워드로도 변수 선언이 가능한데, 'let', 'const'를 사용하면 된다.
- 'let'키워드는 값이 변경 가능한 변수를 선엄하는 키워드이다. 따라서 선언과 초기화를 동시에 하지 않아도 된다.
- 'const'키워드는 값의 변경이 불간으한 변수를 선언하는 키워드이다. 따라서 선언과 초기화를 모조건 동시에 해야한다.

### var VS let, const
- 변수 선언 시에는 무조건 'let' 혹은 'const'를 사용하는것을 추천.
- 이유는 'var' 키워드로는 변수 중복 선언이 허용된다. 반면 'let', 'const'는 변수 중복 선언 시 에러가 발생한다.


### 스코프(Scope)
- 스코프란 변수를 참조할 수 있는 유효 범위를 의미한다.
- 스코프는 크게 전역 스코프와 지역 스코프로 나뉘다.
|종류|설명|
|---|---|
|전역 스코프|가장 바깥의 코드 영역|
|지역 스코프|함수 및 코드 블록 내부|

### 지역 스코프(LocalScope)
|종류|설명|
|---|---|
|함수 레벨 스코프|함수 내부만 지역 스코프로 인정|
|블록 레벨 스코프|함수 내부 및 코드 블록 내부 둘 다 지역 스코프로 인정|

### 함수 레벨 스코프 - var
- 'var' 키워드는 함수 레벨 스코프를 지원하므로, if문ㄴ 내부의 변수 'b'는 전역 변수가 된다.

### 블록 레벨 스코프 - let, const
- 반면 'let', 'const' 키워드는 블록 레벨 스코프를 지원하므로, if문 내부의 변수 'b'는 지역 변수가 된다.
[함수/블록 레벨 스코프 실습 코드] 
```
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>ECMA SCRIPT 6</title>
        <style>

        </style>
    </head>
    <body>
        <p>let : 블록단위 변수</p>
        <p>var : 함수단위 변수</p>
        <p>const : 변경 불가한 값(상수), 배열이나 오브젝트의 일부 변경은 가능</p>
    </body>
    <script>
        
        let global = "전역함수";

        function test(){
            let global = "지역변수";
            // let은 철저히 영역에 제한을 받는다.
            console.log(global);
        }
        test();
        console.log(global);

        function test2(){
            if(true){
                var scope = '함수 영역'; //블록영역 영향을 받긴 하는데, 함수 안에서는 안받는다.
                let local = '블록 영역'; //철저히 블록 영역을 벗어나지 못한다.
            }
            console.log(scope);
            //console.log(local);
        }
        test2();

        const x = 10;
        x = 30;
    </script>
</html>
```
### 스코프 체인
- 스코프 체인이란 모든 스코프가 계층적 구조로 연결된 것을 의미한다.
- 모든 지역 스코프의 최상위 스코프는 전역 스코프이다.
- 변수 및 함수를 참조할 때 해당 스코프에서 시작해 상위 스코프 방향으로 순차적으로 이동 하면서 변수 및 함수를 검색한다.
[스코프 체인 시각화]
![alt text](../img/scop_chain_img.png)
[스코프 체인 실습코드]
```
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>ECMA SCRIPT 6</title>
        <style>

        </style>
    </head>
    <body>
        <h3>Scope Chain</h3>
        <p>scope chain 은 변수를 찾는 순서에 대한 이야기</p>
        <p>우선 내 주변부터 찾고, 없으면 점차 밖으로 나가면서 찾는다.</p>
        <p>global 이 각각 누구를 찾는지 확인해 보자</p>
    </body>
    <script>
        const global = 10;
        console.log('=== 함수 밖 ===');
        console.log(global); //10
        outerFunc();

        function outerFunc(){
            const global = 20;
            const local1 = 10;
            console.log('=== OUTER FUNCTION ===');
            console.log(global); //20
            console.log(local1); //10
            innerFunc();

            function innerFunc(){
                const local1 = 40;
                const local2 = 50;
                console.log('=== INNER FUNCTION ===');
                console.log(global); //20
                console.log(local1); //40
                console.log(local2); //50
            }
        }
    </script>
</html>
```
### 동적 스코프, 렉시컬 스코프
- 함수를 호출한 위치에서 상위 스코프를 결정하는 방식을 동적 스코프라고 한다.
- 반면 함수를 정의한 위치에서 상위 스코프를 결정하는 방식을 렉시컬 스코프라고 한다.

## ES6
### 화살표 함수
- 화살표 함수는 ES6 이후에 도입된 새로운 함수 선언 형태로, 일반 함수보다 간략하게 함수를 선언할 수 있다.
[실습 코드]
```
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>ECMA SCRIPT 6</title>
        <style>
            
        </style>
    </head>
    <body>
        <p>화살표 함수(fat arrow) =></p>
        <p>익명함수 표현시 간단하게 해준다.</p>
        <p>화살표 함수만의 특징도 존재 한다.</p>
        <button id="btn">click</button>
    </body>
    <script>
        const func = function(x,y){
            console.log(x+y);
        }
        func(3,4);

        // 규칙 1. function 은 생략 가능
        const x1 = (x,y)=>{
            console.log(x+y);
        }
        x1(6,9);
        // 규칙 2. 실행문이 console 이나 return 문만 존재할 경우 {} 생략이 가능
        const x2 = (x,y)=>console.log(x+y);        
        x2(6,9);
        // 규칙 3. 매개변수가 1개라면 () 도 생략 가능
        const x3 = x=>console.log(x*x);        
        x3(4);

        // 차이점1 : this 가 이벤트를 당한 당사자가 아닌 window
        document.getElementById('btn').addEventListener('click',evt=>console.log(this));

        // 차이점2 : hositing 지원이 되지 않는다.
        hositing();
        function hositing(){
            console.log('hoist : 끌어 올려지다.');
        }

        x4(3);
        const x4 = x=>console.log(x*x); 

    </script>
</html>
```

### 옵셔널 체이닝
- 객체에서 존재하지 않는 프로퍼티에 접근하거나, 배열에서 범위를 벗어난 인덱스를 조회하려고 하면 에러가 발생한다.
- '?.' 연산자를 이용하면 존재하지 않는 프로퍼티/인덱스에 대해 에러 없이 undefined를 반환한다.
[실습 코드]
```
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>ECMA SCRIPT 6</title>
        <style>
            
        </style>
    </head>
    <body>
        <p>Null 이나 None, undefined 의 값에 무언가를 하려고 하면 에러 발생</p>
        <p>Null Point Exception</p>
    </body>
    <script>

        const user = {
            profile:{
                name:'Alice'
            },
            numbers:[1,2,3]
        };
        // oo.oo 라고 했을때 undefined 값이 나오면 뭘 하지 마라!
        console.log(user.profile.name);     //Alice
        console.log(user.profile.age);      //undefined
        //console.log(user.contact.email);  //error
        console.log(user.contact?.email);   // undefined

        console.log(user.numbers[1]);    //2
        console.log(user.numbers[4]);
        //console.log(user.numbers[4].toString());// error
        console.log(user.number[4]?.toString());//undefined

    </script>
</html>
```

### 구조 분해 할당
- ES6 이전에는 객체의 프로퍼티 값을 새로운 변수에 할당하려면, 각 변수를 선언할 때마다 프로퍼티를 하나씩 참조해야 했다.
- ES6부터는 간단하게 객체의 프로퍼티 값을 변수에 할당할 수 있다.
[ES6 이전]
```
const person = {
    name: 'John',
    age: 30,
    city: 'New York'
};

const name = person.name;
const age = person.age;
const city = person.city;
```

[ES6 부터]
```
const person = {
    name: 'John',
    age: 30,
    city: 'New York'
};

// 모든 프로퍼티를 할당할 필요는 없음.
const { name, age } = person;
```
- 각 변수의 이름을 다른 이름으로 변경할 수도 있다.
- 기본값 설정, 객체 안의 객체 구조 분해 할당 또한 가능하다.
[예시 코드]
```
const person = {
    name: 'John',
    age: 30,
    city: 'New York'
    contact: {
        email: 'john@elice.io',
        phone: '000-0000-0000'
    }
};

const { name, age, grade = 1, contact: { email } } = person;

console.log(name);  // 'John'
console.log(age);   // 30
console.log(grade); // 1
console.log(email); // 'john@elice.io'
```
- '...'을 이용해 할당되지 않은 나머지 프로퍼티는 객체 형태로 할당할 수 있으며, 이 객체는 보통 'rest'라는 이름으로 설정한다. -> Rest Syntax
[예시 코드]
```
const person = {
    name: 'John',
    age: 30,
    city: 'New York'
    contact: {
        email: 'john@elice.io',
        phone: '000-0000-0000'
    }
};

const { name, age, ...rest } = person;

console.log(name);  // John 
console.log(age);   // 30
console.log(rest);  // { city: 'New York', contact: {email: 'john@elice.io', phone: '000-0000-0000'}}
```
- 함수의 인수로 전달되는 객체를 바로 구조 분해 할당할 수 있다.
[실습 코드]
```
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>ECMA SCRIPT 6</title>
        <style>
            
        </style>
    </head>
    <body>
        <p>비 구조할당 == 구조분해 할당</p>
    </body>
    <script>
        const user = {
            name:'홍길동',
            age:'25',
            city:'서울'
        };

        // 전통적방식 으로 이름과 나이 출력
        const name = user.name;
        const age = user.age;
        console.log(name,age);

        // 객체서 필요한것만 분해해서 가져다 쓴다.
        // 규칙: 속성명(키) 가 변수명이 되어야 한다.
        //const {name,city} = user;
        // 이름이 겹칠 경우 임시 이름을 지어줄 수 있다.
        const {name:userName,city} = user;
        console.log(userName,city);

        // 새로운속성을 추가하거나 객체안의 객체도 꺼낼 수 있다.
        const person = {
            name:'Jhone',
            age:30,
            city:'NewYork',
            contact:{
                email:'Jhone@elice.io',
                phone:'02-6954-1105'
            }
        };

        const {name:user_name,contact:{email},grade='A'} = person;
        console.log(user_name);
        console.log(email); //객체안의 객체
        console.log(grade); // 추가된 내용

        // name, email, grade;
        // 모든 값을 다 출력해 보기
        // const{name:n,age:a,city:c,contact:{email:e, phone:p}} = person;
        // console.log('name:',n);
        // console.log('age:',a);
        // console.log('city:',c);
        // console.log('email:',e);
        // console.log('phone:',p);
        const {name:n, age:a,...etc} = person;
        console.log('name:',n);
        console.log('age:',a);
        // 선택된 요소 외 나머지(rest) 를 사용한다고 해서
        // rest 문법 이라고 부른다.
        console.log('etc:',etc); 



    </script>
</html>
```
- 배열 또한 구조 분핼 할당을 할 수 있다. 이때 변수의 이름은 원하는 대로 설정할 수 있다.
- 배열에서도'...'를 사용해 나머지 값을 배열 형태로 할당할 수 있다. -> Rest Syntax
[실습코드]
```
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>ECMA SCRIPT 6</title>
        <style></style>
    </head>
    <body>
        <p class="txt">구조분해 할당은 함수의 인자값, 배열 등에도 활용이 가능하다.</p>
    </body>
    <script>
        const user = {
            name:'홍길동',
            age: 25,
            city:'서울'
        };
        //getUser(user.name,user.age);
        getUser(user); // 함수 호출시 오브젝트 전체를 던져주면

        // 그 중에서 특정 속성만 뽑아쓴다.(필요한 것만)
        function getUser({name,age}){
            console.log(name,age);
        }

        // p 태그를 클릭했을때 해당 태그의 innerHTML 과 className 을 출력하세요.
        p_tag = document.querySelector('.txt');
        p_tag.addEventListener('click',({target:{innerHTML,className}})=>{
            console.log(innerHTML+' / '+className);
        });

        const numbers = [1,2,3,4,5];
        const [first,second,...rest] = numbers;
        console.log(first);
        console.log(second);
        console.log(rest);

    </script>
</html>
```
### Spread 문법
- 이터러블(배열이나 문자열처럼 순서대로 하나씩 접근할 수 있는 객체)에 해당하는 여러값의 집합을 펼쳐서 개별 값의 목록으로 만드는 문법이다.
- 여러 배열을 결합할 때 간편하게 사용할 수 있다.
- 또한 함수의 인수로 여러 값을 전달할 때 사용할 수 있다.
- 객체 또한 Spread 문법을 사용할 수 있다. 단, 객체 리터럴 내부에서만 사용 가능하다.
[실습코드]
```
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>ECMA SCRIPT 6</title>
        <style></style>
    </head>
    <body>
        <p>spread 연산자(...) 는 뭉쳐있는 것들을 펼쳐줍니다.</p>
    </body>
    <script>
        console.log([1,2,3],'->',...[1,2,3]);
        console.log('elice','->',...'elice');

        const arr1 = [1,2,3];
        const arr2 = [10,20,30];
        // result = arr1 + arr2
        let result = [arr1,arr2];
        console.log(result);
        result = arr1.concat(arr2);
        console.log(result);
        //1,2,3,10,20,30
        result = [...arr1,...arr2]

        // shallow copy 문제 해결
        let x = [1,2,3,4,5];
        let y = [...x];
        x[4] = 'X';
        y[0] = 'Y';
        console.log([...x]);
        console.log([...y]);

        function total(...params){ // def total(*params):
            console.log(params); // array
            //params 에서 받은 값을 합하여 반환
            let result = 0; 
            for (const num of params) {
                result += num;
            }
            return result; // params 의 합 반환
        }
        console.log('인자값 2개: ',total(10,20));
        console.log('인자값 3개: ',total(1,2,3));
        console.log('인자값 5개: ',total(1,2,3,4,5));

        // spread copy
        const user = {name:'홍길동',age:25};
        // key:value 형태여야 하는데 user 는?
        let person = {user,city:'서울'};
        console.log(person);
        // spread 를 사용하면 객체 안의 객체가 아닌
        // 동일선상의 속성으로 추가가 된다.
        person = {...user,city:'서울'};
        console.log(person);

    </script>
</html>
```
### 구조 분해 할당 + Rest 문법 + Spread 문법 
- 구조 분핼 할당과 Rest 문법, Spread 문법을 활용하면 특정 프로퍼티의 값만 바꾼 복사된 객체를 생성하는 로직을 구현할 수 있다.
```
const person = {
    name: 'John',
    age: 30,
    city: 'New York'
};

function copyPersonWithNewName(person, newName) {
    const { name, ...rest } = person; //Rest 문법
    return {
        name: newName,
        ...rest //Spread 문법
    }
}

copyPersonWithNewName(person, "Hellobit"); // { name: 'Hellobit', age: 30, city: 'New Yotk'}
```
### 템플릿 리터럴
- 문자열 내부에 변수나 표현식을 포함해야 할 때, ``백틱 기호와 '${}'를 이용하여 편리하게 문자열을 작성할 수 있다.

### 프로퍼티/메서드 축약
- 객체 리터럴에서 프로퍼티의 이름과 변수 이름이 동일할 때, 해당 프로퍼티 이름을 생략할 수 있다.
[실습 코드]
```
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>ECMA SCRIPT 6</title>
        <style></style>
    </head>
    <body>

    </body>
    <script>
        const name = 'Elice';
        const age = 20;
        let person = {name:name,age:age};
        console.log('person',person);

        let person2 = {name,age};
        console.log('person2 :',person2);
    </script>
</html>
```
### 고차 함수
- 고차 함수는 함수를 인자로 받거나, 함수를 반환하는 함수이다.
```
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>ECMA SCRIPT 6</title>
        <style></style>
    </head>
    <body>
        <button>click</button>
    </body>
    <script>
        // 고차함수 - 함수를 인자값으로 받는 함수를 지칭
        function repeat(n,action){
            for (let i = 0; i < n; i++) {
                // action 은 실행할 함수(이름은 임의로 지어준 것)   
                // action 은 repeat 함수 실행시 마다 달라질 수 있다.
                action(i);              
            }
        }

        //repeat(3,function(n){console.log(`Hello - ${n}`);});
        repeat(3,n => console.log(`Hello - ${n}`));

        // button 클릭시 '버튼 클릭' 이라는 콘솔이 출력 되도록 해 보자
        let btn = document.getElementsByTagName('button')[0];
        btn.addEventListener('click',evt => console.log('버튼클릭'));
    </script>
</html>
```
### 콜백 함수
- 함수를 인자로 받는 고차 함수에서, 인자로 넘겨주는 함수를 콜백 함수라고 부른다.
- 'addEventListener'에서 등록하는 이벤트 핸들러 함수 또한 콜백 함수의 일종이다.
- 콜백 함수를 사용하는 또 다른 예시는 배열 메서드를 사용하는 것이다.

#### forEach
- 'forEach'는 배열의 각 요소에 대해 콜백 함수를 한번씩 순서대로 실행한다.
- 콜백 함수에는 각 차례의 배열요소와 해당 요소의 인덱스 값이 인수로 전달된다.
- 반환 값은 없다.
```
const numbers = [1, 2, 3, 4, 5];

numbers.forEach((num, index) => {
    console.log(`Index ${index}: ${num}`);
});

//출력 :
// index 0 : 1;
// index 1 : 2;
// index 2 : 3;
// index 3 : 4;
// index 4 : 5;
```
#### map
- 'map'은 배열의 각 요소에 대해 콜백 함수를 실행한 반환 값을 모아 새로운 배열을 반환한다.
- 콜백 함수에는 각 차례의 배열 요소와 해당 요소의 인덱스 값이 인수로 전달된다.
- 원래 배열의 값은 변경되지 않는다.
```
const numbers = [1, 2, 3, 4, 5];

const doubled = numbers.map(num => num *2);

console.log(doubled); // [2, 4, 6, 8, 10]
```

#### filter
- 'filter'는 배열의 각 요소에 대해 특정 조건을 만족하는 요소를 모아 새로운 배열을 반환한다.
- 콜백 함수에는 각 차례의 배열요소와 해당 요소의 인덱스 값이 인수로 전달 된다.
- 콜백 함수는 조건의 결과를 boolean 값으로 반환 해야 한다.
- 원 래 배열의 값은 변경되지 않는다.
```
const numbers = [1, 2, 3, 4, 5];

const evenNumbers = numbers.filter(num => num % 2 === 0);

console.log(evenNumbers); // [2, 4]
```

#### reduce
- 'reduce'는 배열의 각 요소에 대해 콜백 함수를 호출하여 누적된 결과를 반환한다.
- 두 번째 인수로 초긱값을 지정할 수 있다.
- 콜백 함수에는 이전 단계까지의 누적 값과 현재 요소 값이 인수로 전달된다.
- 콜백 함수는 누적 값과 현재 요소 값으로 계산한 결과를 반환 해야 한다.
```
const numbers = [1, 2, 3, 4, 5];

const sum = numbers.reduce((accumulator, currentValue) => accumulator + currentValue, 0);

console.log(sum); // 15
```
[실습 코드]
```
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>ECMA SCRIPT 6</title>
        <style></style>
    </head>
    <body>
        <p>forEach: 배열의 값을 하나씩 꺼내는 기능</p>
        <p>map: 배열의 값을 하나씩 꺼내 특정 처리를 해서 새로운 배열로 내보내는 기능</p>
        <p>filter: 특정 조건을 만족하는 값만 새로운 배열로 내보내는 기능</p>
        <p>reduce: 배열의 안의 값을 하나씩 꺼내 누적 연산 하는 기능</p>
    </body>
    <script>
        const info_list = [
            {id:1,name:'김지훈',grade:'A',score:95},
            {id:2,name:'이지훈',grade:'C',score:70},
            {id:3,name:'박지훈',grade:'B',score:82},
            {id:4,name:'나지훈',grade:'A',score:93},
            {id:5,name:'라지훈',grade:'C',score:71},
            {id:6,name:'마지훈',grade:'C',score:70}
        ];
        /*
        let new_arr = info_list.map(function(item,index,array){
            console.log(index,item);
            console.log(array); // 가져온 원본 배열
            return item;
        });
        */
        // info_list 안의 name 을 000 학생 으로 변경해서 반환
        let students = info_list.map(item => {
            item.name += ' 학생';
            return item;
        });
        console.log(students);

        // 특정 요소만 뽑아서 배열로 반환 할 수 있다.
        let ids = info_list.map(item => item.id);
        console.log(ids);

        let supple = students.filter(function(info){
            return info.score <80; // condition  에 맞는 값만 반환
        }).map(item => item.name);

        console.log('보충반:',supple);

        let scores = [1,2,3,4,5,6,7,8,9,10];
        // reduce((누적된값,현재값)=>{},초기값)
        let sum = scores.reduce(function(acc,curr){
            // acc : 초기엔 0번인덱스 값, 이후로는 누적계산된 값
            // curr : 초기엔 acc 옆의 값, 이후로는 현재 인덱스의 값
            console.log(acc,curr);
            return acc+curr;
        });
        console.log(sum);
        

    </script>
</html>
```