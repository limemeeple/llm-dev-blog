---
title: "9. 비동기1 - 이벤트 루프와 Promise"
date: 2026-08-12
draft: false
tags: ["Promise", "마이크로태스크 큐", "async/await"]
categories: ["STUDY"]
summary: "이어드림2026 서비스 개발 수업 DAY9"
weight: 9
---
### 타이머 함수
- 자바스크립트에는 특수한 역할을 하는 함수가 두 개 존재한다.
- 즉시 실행되는 일반 함수 호출과는 달리, 위 두 함수를 사용하면 함수 호출을 예약할 수 있다.

#### setTimeout
- 'setTimeout' 함수를 사용하면 특정 시간 후에 콜백 함수가 호출되도록 예약할 수 있다.
- 두 번째 인수로 시간을 전달하며, 단위는 미리초(ms)이다.
- 세 번째 이후의 인수에는 콜백 함수에 전달될 인수를 전달할 수 있다.

#### clearTimeout
- 'setTimeout' 함수는 숫자 형태의 timeout id를 반환한다.
- 'clearTimeout' 함수에 이 id를 인수로 넘겨 호출하면 해당 timeout을 제거할 수 있다.

#### setInterval
- 'setInterval' 함수를 사용하면 특정 시간마다 콜백 함수가 호출되도록 예약할 수 있다.
- 두 번째 인수로 시간을 전달하며, 단위는 밀리초(ms)이다.

#### clearInterval
- 'setInterval' 함수는 숫자 형태의 interval id를 반환한다.
- 'clearInterval' 함수에 이 id를 인수로 넘겨 호출하면 해당 interval을 제거할 수 있다.

### 비동기 처리방식
- 작업이 시작된 후 완료 여부와 상관 없이 다음 작업을 즉시 실행하며, 작업 완료 시 콜백 함수나 프로미스 등을 통해 결과를 처리하는 방식

<비동기 동작 원리 코드>
```
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF8">
        <title>비동기</title>
        <style>

        </style>
    </head>
    <body>
        
    </body>
    <script>
        console.log('===== 동기방식 ======');
        console.log('1. 프로그램 시작');
        console.log('2. 작업 시작');
        console.log('3. 프로그램 종료');

        console.log('===== 비동기방식 ======')
        console.log('1. 프로그램 시작');
        //작업이 1시간 걸린다고 가정.
        setTimeout(()=> console.log('2. 작업 시작'),1000);
        console.log('3. 프로그램 종료');
        /*
            call stack : 해야할 일을 넣는다.
            비동기 처리될 내용은 일단 task queue에 넣어둔다.
            그리고 이후 call stack 으로 불러와 처리.
        */
    </script>
</html>
```

<콜백 헬에 대한 예제 코드>
```
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF8">
        <title>비동기</title>
        <style>

        </style>
    </head>
    <body>
        <p>콜백 지옥은 처리 단계가 많아 지면서 콜백에 콜백을 호출할때 발생 한다.</p>
        <p>가독성이 굉장히 좋지 않다.</p>
    </body>
    <script>
        //콜백 함수로 동작되는 plus, multi, divide 함수를 이용해 보자.
        function plus(a, b, onResult){
            setTimeout(() => onResult(a+b), 100)
        }

        function multi(a, b, onResult){
            setTimeout(() => onResult(a*b), 100)
        }

        function divide(a, b, onResult){
            setTimeout(() => onResult(a/b), 100)
        }

        // (a+b)*c/d
        // 1단계 : a 와 b 를 더한 값 구하기
        plus(3,4,function(r1){
            console.log("a+b =", r1);
        });

        // 2 단계 : 여기에 c 를 곱한다.
        plus(3,4,function(r1){
           multi(r1, 4, function(r2){
                console.log("(a+b)*c =",r2);
           });
        });

        // 3 단계 : 여기에 d 를 나눈다.
        plus(3,4,function(r1){
           multi(r1, 4, function(r2){
                divide(r2, 2, function(r3){
                    console.log("(a+b)*c/d =", r3)
                });
           });
        });

        // 콜백은 최대 2단계를 넘지 않는걸로 하자.
        

    </script>
</html>
```
#### Promise
- 기본적인 비동기 처리는 처리 결과를 활용할 수 없다.(콜 스택이 빈 후에 콜백 함수가 호출되기 때문)
- 또한 에러 처리를 정상적으로 할 수도 없다.(당장은 에러가 발생하지 않기 때문)
- Promise를 이용하면 앞선 문제를 모두 해결하며 비동기 처리를 진행할 수 있다.
- 'new Promise(콜백 함수)'와 같이 new 키워드를 통해 Promise 객체를 생성한다. 이때 콜백 함수가 무조건 인수로 넘겨져야 한다.

<Promise 예제 코드>
```
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF8">
        <title>비동기</title>
        <style>

        </style>
    </head>
    <body>
        
    </body>
    <script>
        // 비동기 방식의 문제점.
        function plus(a){
            let result = 0;
            setTimeout(()=>{
                result = a+10
                //return result; //undefined
            },100);
            console.log('비동기 방식 : ', result);
        }
        plus(5);

        // 콜백 함수 : 특정 내용이 끝나면 실행할 함수를 전달
        function plus_call(a, onResult){
            let result = 0;
            setTimeout(()=>{
                onResult(a+10);
            },100);
        }
        plus_call(5,(sum)=>console.log('콜백 함수에서 내놓은 값 : '+sum));

        // 콜백지옥을 해결하기 위한 첫 단계 - Promise 객체
        // promise 객체 사용시 콜백함수를 기본으로 사용 한다.
        function prom(a){
            // resolve : 성공시 실행할 함수.
            // reject : 실패시 실행할 함수.
            return new Promise(function(reslove,reject){
                setTimeout(() => reslove (a+10), 100);
            }); 
        }

        // PromiseResult : 결과값
        // PromiseState  : 상태값
        // pending       : 비동기 처리 전, 대기
        // fullfilled    : 비동기 처리 완료, 성공
        // rejected      : 비동기 처리 완료, 실패
        console.log(prom(10)); // resolve 에서 보내주는 값을 어떻게 뽑아내느냐
    </script>
</html>
```

#### Promise - resolve, reject
- 콜백 함수에는 'resolve'와 'reject' 함수가 인수로 전달된다.
- 'resolve' : 비동기 처리의 성공을 나타냄
- 'reject' : 비동기 처리의 실패를 나타냄

<resolve와 reject 예시 코드>
```
const promise = new Promise((resolve, reject) => {
    const fetcheData = fetchData();

    if (fetchedData >= 5) resolve(retchedData);
    else reject("fetch failed");
});
```

#### Proimise의 상태
|상태|설명|
|---|---|
|pending|비동기 처리가 완료되기 전, 대기|
|fulfilled|비동기 처리가 완료되었으며, 성공|
|rejected|비동기 처리가 완료되었으며, 실패|


#### Promise - then, catch, finally
|메서드|설명|
|---|---|
|.then(r => {...}, e=> {...})|비동기 성공 및 실패 후 처리|
|.catch(e => {...})|비동기 실패 후 처리|
|.finally(() => {...})|비동기 성공/실패 여부 관계 없이 이후 처리|

- Promise 객체는 위 세 가지 메서드를 가지며, 이 메서드를 사용해서 비동기 처리 완료 이후의 처리를 진행한다.
- 각 메서드는 모두 Promise 객체를 반환한다.

#### Promise - then
- 'then' 메서드에는 두 가지 콜백 함수가 인수로 전달된다.
- 1: 비동기 성공 시 처리(인수: resolve 함수 호출 시 넘긴 값)
- 2: 비동기 실패 시 처리(인수: reject 함수 호출 시 넘긴 값)


#### Promise - catch
- 'catch' 메서드에는 비동기 실패 시 처리를 위한 콜백 함수가 인수로 전달된다.(인수: reject 함수 호출 시 넘긴 값)

#### Promise - finally
- 'finally' 메서드에는 비동기 성공/실패 여부와 관계없이 실행될 콜백 함수가 인수로 전달된다.

#### Promise Chaining
- 보통 'then', 'catch', 'finally'는 메서드 체이닝을 통해 같이 사용된다.(각 메서드는 Promise를 반환하기 때문에 체이닝이 가능하다.)
  - 메서드 체이닝 : 객체의 메서드를 연속적으로 호출하여 각 메서드가 자신을 반환하도록 함으로써 여러 작업을 한 줄의 코드로 수행할 수 있게 하는 프로그래밍 기법

<Promise Chaining 예제 코드 1>
```
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF8">
        <title>비동기</title>
        <style>

        </style>
    </head>
    <body>
        
    </body>
    <script>
        let num = prompt('숫자를 입력 하세요');
        console.log(num);
        //prom(parseInt(num)).then((data)=>console.log(data) ,(error)=>console.error(error));
        prom(parseInt(num))
            .then((data)=>console.log(data))
            .catch((error)=>console.error(error))
            .finally(() => console.log('성공을 하던, 실패를 하던 실행함.'));

        function prom(a){
            return new Promise(function(resolve, reject){
                setTimeout(() => {
                    if(a >= 10){
                        resolve(a+10);
                    }else{
                        reject('인자값은 10보다 크거나 같아야 합니다.');
                    }
                });
            });
        }
    </script>
</html>
```

<Promise Chaining 예제 코드 2>
```
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF8">
        <title>비동기</title>
        <style>

        </style>
    </head>
    <body>
        
    </body>
    <script>
        //47_callback_hell.html의 plus, multi, divide 함수를 promise 객체를 활용하도록 변경.
        function plus(a, b){
            return new Promise(function(resolve, reject){
                setTimeout(() => {
                    if(a != '' && b != ''){
                        console.log(a+b);
                        resolve(a + b);
                    }else{
                        reject('plus 계산에 실패하였습니다.');
                    }
                
                }, 100);
            });
        }

        function multi(a, b){
            return new Promise(function(resolve, reject){
                setTimeout(() => {
                    if(a != '' && b != ''){
                        console.log(a*b);
                        resolve(a * b);
                    }else{
                        reject('multi 계산에 실패하였습니다.');
                    }
                
                }, 100);
            });
        }

        function divide(a, b){
             return new Promise(function(resolve, reject){
                setTimeout(() => {
                    if(a != '' && b != ''){
                        console.log(a/b);
                        resolve(a / b);
                    }else{
                        reject('divide 계산에 실패하였습니다.');
                    }
                
                }, 100);
            });
        }

        let num1 = 10;
        let num2 = 3;

        plus(num1, num2).then(r1 => multi(r1,5))
                        .then(r2 => divide(r2, 2))
                        .then(r3 => console.log("(a+b)*c/d =", r3))
                        .catch(error => console.log('계산에 실패 하였습니다.'));

        // Promise 객체를 반환하는 함수는 반드시 await을 붙여야 한다.
        async function getResult(){

            try{
                let r1 = await plus(20,30);
                let r2 = await multi(r1, 4);
                let r3 = await divide(r2,2);
                console.log('(a+b)*c/d=',r3)
            }catch{ // try 실행 중 reject가 발생되면 이쪽으로 튕긴다.
                console.error(error);
            }
        }

        getResult();

        /*
        // 감싸줄수 있는 함수가 없을 경우?
        let r2 = await multi(r1, 4);
        // 1. then 을 활용
        multi(20,20).then(data=>{});
        // 2. 강제로 함수를 만들어 사용
        async function multi_func(){
            let r = await multi(r1, 4);
        }
        multi_func();
        */
    </script>
</html>
```

#### Promise.resolve
- 'Promise.resolve' 메서드를 사용하면 인수로 전달받은 값을 resolve하는 프로미스를 반환 받을 수 있다.
```
const resolvedPromise = Promise.resolve("Hello Elice");
resolvedPromise.then((result) => { console.log(result); }); // 'Hello Elice'
```

#### Promise.reject
- 'Promise.reject' 메서드를 사용하면 인수로 전달받은 값을 reject 하는 프로미스를 반환 받을 수 있다.
```
const rejectedPromise = Promise.reject("Elice Rejected");
rejectedPromise.catch((error) => { console.log(`error: ${error}`); }); // 'error: Elice Rejected'
```

#### Promise.all
- 'Promise.all' 메서드를 사용하면 여러 Promise를 병렬적으로 처리할 수 있다.(인수로 여러 Promise 객체가 담긴 배열 전달)
- 인수로 전달된 모든 Promise의 처리가 완료된 후에 then으로 넘어간다. 이때 각 Promise의 결과는 배열의 형태로 전달된다.

#### 마이크로태스크 큐 (Microtask Queue)
- 브라우저에는 마이크로태스크 큐라는 공간이 하나 더 존재한다. 마이크로태스크 큐는 태스크 큐보다 우선순위가 높다!
- 즉, 마이크로태스크 큐에서 대기중인 작업이 먼저 콜 스택으로 들어가고, 마이크로태스크 큐가 비면 태스크 큐의 작업이 콜 스택으로 들어간다.
|종류|저장하느 비동기 처리 작업|
|---|---|
|마이크로태스크 큐|Promise, Ajax|
|태스크 큐|타이머 함수, 이벤트 핸들러 함수|
<마이크로태스크 큐 실습 코드>
```
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF8">
        <title>비동기</title>
        <style>

        </style>
    </head>
    <body>
        <h3>Task Queue 우선순위</h3>
        <p>1. Micro Task Queue : Promise, Ajax</p>
        <p>2. Task Queue : Timer, Event</p>
    </body>
    <script>
        console.log('1. 프로그램 시작');

        setTimeout(()=>console.log('2. 작업시작(Timer)'),0);

        new Promise((resolve, reject) => resolve('3. 작업시작(promise)'))
            .then(data => console.log(data));

        console.log('4. 프로그램 종료');
    </script>
</html>
```
### Promise의 단점
- Promise를 이용하여 비동기를 처리하는 경우, 체이닝이 계속되면 가독성이 좋지 않은 코드가 될 수 있다.
- ES8부터 도입된 asynx/await 문법을 사용하면, 비동기 처리를 훨씬 깔끔하게 작성할 수 있다.

### async/await
- async/await을 이용해 비동기 처리를 동기 처리하듯이 작성할 수 있다.
```
async function fetchData(){
    try{
        const data = await fetchData('https://api.example.com/deta1');
        console.log(data);
    }catch(error){
        console.error('Error:', error);
    }
}

fetchData();
```
- 'await' 키워드는 무조건 'async' 함수 내부에서만 사용할 수 있다.
- async 함수는 항상 반환값을 resolve하는 Promise를 반환한다.
- 'await' 키워드를 사용하면, 해당 비동기 처리가 완료될 때까지 기다린 후, resolve 된 값을 반환한다.
- 동기 처리 방식과 달리, await 키워드를 통해 진행되는 비동기 처리가 끝난 후에야 다음 코드가 실행 된다.
- async/await 사용 시에는 try ... catch ... finally 문을 사용하여 에러 처리를 진행한다.

### 예외처리
- 모든 프로그램은 에러가 발생할 가능성이 존재하며, 발생시 프로그램은 종료된다.
- 종료를 방지하기 위해 예외 상황에 최대한 대응하는 코드를 작성해야 한다.

#### try ... catch 예외 처리
- 기본적으로 try ... catch 문을 이용해 예외를 처리할 수 있다.(try만 단독으로 사용할 수 는 없다.)
- finally 문을 추가해 에러 여부와 상관없이 실행되는 코드를 작성할 수 있다.
```
const jsonString = '{"name": "John", "age": 30'; // JSON 형식에 맞지 않음.

try{
    JSON.parse(jsonString); //SyntaxError
}catch(err){
    console.log(err);
}finally{
    console.log("JSON 처리 완료");
}
```