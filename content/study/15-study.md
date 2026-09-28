---
title: "15. next.js 4"
date: 2026-08-31
draft: false
tags: ["redux", "jest", "rtl"]
categories: ["STUDY"]
summary: "이어드림2026 서비스 개발 수업 정리(코드 리뷰)"
weight: 15
---
***코드에 주석으로 내용 첨부**
### Redux
#### [src > redux > counterSlicer.jsx]
```
import {createSlice} from "@reduxjs/toolkit";

//slice? state + reducer가 쪼개져서 들어가 있다는 의미
//1. 실행할 reducer와 state를 선언해 slicer를 만든다.
const counterSlicer = createSlice({
    name: 'counter', //슬라이서 이름
    initialState:{ //사용 스테이트
        value:0
    },
    reducers:{ // 리듀서 등록(state를 변화시키는 함수)
        increment:(state,action)=>{
            console.log('state', state);
            console.log('action', action);
            state.value = state.value + 1; // state 안의 value 속성을 변경 후
            return state; // 밖으로 던진다.
        },
        decrement:(state,action)=>{
            console.log('state', state);
            console.log('action', action);
            state.value = state.value - 1; // state 안의 value 속성을 변경 후
            return state; // 밖으로 던진다.
        }
    }
});

export default counterSlicer.reducer;
```
#### [src > redux > stroe.jsx]
```
// 2. 완성된 slicer를 store에 등록
import {configureStore} from "@reduxjs/toolkit";
import counterSlicer from "@/redux/counterSlicer";

// store : 앱 전체의 상태를 담는 단일 저장소.
// Context가 여러 개일 수 있는 것과 달리 Redux는 store 하나에 모두 모은다.
export const store = configureStore({
    reducer:{
        // 여기에 적은 키가 state에서의 경로가 된다.
        // counter로 등록했기 때문에 state.counter.value로 접근한다.
        // slice를 여러 개 만들면 이렇게 늘어난다 ( reducer : { counter: counterSlicer, user : userSlicer })
        counter:counterSlicer
    }
});
```
#### [src > app > layout.js]
```
'use client';
import {Provider} from "react-redux";
import {store} from "@/redux/store";

export default function Layout({children}){

    //3. Provider를 통해 store를 공유

    return(
        <html lang={"ko"}>
            <head>
                <meta charSet={"UTF-8"}/>
                <title>REDUX</title>
            </head>
            <body>
                {/*
                    Context의 Privider와 같은 원리다.
                    실제로 react-redux는 내부적으로 Context를 씁니다.
                    layout에 두었으므로 모든 페이지에서 store를 쓸 수 있습니다.
                */}
                <Provider store={store}>
                    {children}
                </Provider>
            </body>
        </html>
    );
}
```
#### [src > app > page.js]
```
'use client';
import {store} from "@/redux/store";
import {useSelector} from "react-redux";

export default function App(){

    // action을 통해 reducer 호출 -> dispatch
    // dispatch : '이런 일이 일어났다' 고 store에 알라닌 것.
    // store가 type을 보고 어떤 reducer를 실행할지 찾아간다.
    const upHit = function (){
        store.dispatch({type:'counter/increment'});
    }

    const downHit = function (){
        store.dispatch({type:'counter/decrement'});
    }

    // 구독
    // useSelector : store에 등록된 모든 slice의 state 정보를 가져온다.
    // 전달한 함수가 반환하는 값이 바뀔 때만 이 컴포넌트가 재렌더링 된다.
    // 그래서 state 전체가 아니라 필요한 부분만 골라 반환하는 게 중요한다.
    let count = useSelector((state) => {
        console.log(state);
        return state.counter.value;
    });

    return(
        <div>
            <h3>Count : {count}</h3>
            <button onClick={upHit}>증가</button>
            <button onClick={downHit}>감소</button>
        </div>
    );
}
```
#### [해당 소스의 전체 흐름]
```
        [버튼 클릭]
             ↓
        dispatch({type:'counter/increment'})
             ↓
        store가 type을 보고 해당 reducer 찾음
             ↓
        counterSlicer의 increment 실행 → state.value + 1
             ↓
        store의 상태 갱신
             ↓
        useSelector가 변화 감지 → 컴포넌트 재렌더링
             ↓
        화면에 새 숫자 표시
```
- 데이터가 한 방향으로만 흐른다.
- 상태가 바뀌는 통로가 dispatch 하나뿐이라 추척하기 쉽다.

#### [Context와 Redux의 비교]
|  | Context | Redux |
|---|---|---|
|상태보관|useState(컴포넌트)|store(앱 바깥)|
|변경방법|setter 직접 호출| dispatch -> reducer|
|구독단위|Context 전체|selector가 고른 값만|
|추적|어려움|DevTools로 전부 기록|
|코드량|적음|많음|
- Redux가 번거로워 보이지만, 상태가 언제 왜 바뀌었는지 전부 기록에 남는다는게 큰 장점이다.

--------
### jest
#### [src > app > calcModule.js]
```
import {error} from "next/dist/build/output/log";

export function plus(a,b){
    return a + b;
}

export function minus(a,b){

    // 유효성 검사를 통과 못 하면 throw로 실행을 중단시킨다.
    // throw가 실행되면 아래 return 까지 가지 않는다.
    if(b > a){
        throw new Error("뺄셈의 값은 0보다 커야 합니다.");
    }

    return a - b;
}

export function multiply(a,b){
    return a * b;
}

export function divide(a,b){

    if(b===0){
        throw new error("0으로 나눌수 없습니다.");
    }

    return a / b;
}
```

#### [src > __tests__ > calcModule.test.js]
```
//test()        : 특정한 테스트 단위
//expect()      : 테스트 실행
//describe()    : test() 의 group, describe는 describe를 담을 수 있다.

import {divide, minus, multiply, plus} from "@/app/calcModule";

// describe : 관련된 테스트를 묶는 그룹이다.
// 중첩할 수 있어서 결과 출력이 계층적으로 보인다.
describe("사칙연산 통합 테스트(정상, 에러)", function (){
    describe("사직연산테스트", function (){
        test("더하기 모듈 테스트",function(){

            // expect(실제값).matcher(기대값)
            expect(plus(10,30)).toBe(40);
        });
        test("빼기 모듈 테스트",function(){
            expect(minus(40,10)).toBe(30);
        });
        test("곱하기 모듈 테스트",function(){
            expect(multiply(5,3)).toBe(15);
        });
        test("나누기 모듈 테스트",function(){
            expect(divide(100,10)).toBe(10);
        });
    });

    describe("사칙연산 에러 테스트",function(){
        test("A보다 B값이 클 경우",function (){
            // throws 사용시 실험 함수를 한번 더 감싸 준다.
            expect(() => minus(10, 30)).toThrow();
        })

        test("0으로 나누기 시도", function(){
            expect(() => divide(20, 0)).toThrow();
        })
    });
})

/*
    toBe() : 숫자, 문자, 블리언 타입의 값이 일치.
    toEqual() : 객체나 배열의 일치
    toContain() : 배열이나 문자열 내에 특정 값 포함 여부
    toMatch() : 문자열이 지정된 정규표현식 매턴에 일치하는지
    toThrow() : 특정 에러가 발생하는지 여부
 */
```
#### [src > app > page.jsx]
```
/*
    1. JEST 설치 : npm install -D jest jest-environment-jsdom
    2. jext.config.js 설정
    3. package.json 에 test script 추가
    4. 모듈작성(테스터블 하게)
    5. 테스트코드 작성
    6. npm run test
 */

'use client';

import {useState} from "react";
import {divide, minus, multiply, plus} from "@/app/calcModule";

export default function App(){

    // 입력값 두 개, 연산자, 결과를 객체 하나로 관리한다.
    const [result, setResult] = useState({su1:0, su2:0, oper:"+", result:0});

    const setVal = function(e){
        setResult({
            ...result,
            [e.target.name]:e.target.value
        })
    }

    const calculate = function (){
        const {su1,su2,oper} = result;

        // input의 값은 type="number" 여도 항상 문자열이다.
        // 변환 없이 더하면 "10" + "30" = "1030"이 된다.
        let num1 = parseInt(su1);
        let num2 = parseInt(su2);

        if(oper === '+'){
            /*
            setResult({
                ...result,
                result:num1 + num2
            });
             */
            setResult({
                ...result,
                result:plus(num1, num2)
            })
        }
        if(oper === '-'){
            /*
            setResult({
                ...result,
                result:num1 - num2
            });
             */
            setResult({
                ...result,
                result:minus(num1, num2)
            })
        }
        if(oper === '*'){
            /*
            setResult({
                ...result,
                result:num1 * num2
            });
             */
            setResult({
                ...result,
                result:multiply(num1, num2)
            })
        }
        if(oper === '/'){
            /*
            setResult({
                ...result,
                result:num1 / num2
            });
             */
            setResult({
                ...result,
                result:divide(num1, num2)
            })
        }


    }

    return(
        <div>
            <input type={"number"} name={"su1"} value={result.su1} onChange={setVal}/>
            {/* select는 value 없이 onChange만 있어서 비제어 컴포넌트 이다. */}
            <select name={"oper"} onChange={setVal}>
                <option value={"+"}>+</option>
                <option value={"-"}>-</option>
                <option value={"*"}>*</option>
                <option value={"/"}>/</option>
            </select>
            <input type={"number"} name={"su2"} value={result.su2} onChange={setVal}/>
            <p><button onClick={calculate}>계산</button></p>
            <h3>답 :{result.result}</h3>
        </div>
    )
}
```
--------
### rtl
#### [ src > __test__ > calcModule.test.js ]
```
// calcModule의 함수들
import {divide, minus, multiply, plus} from "@/app/calcModule";

// render : React 컴포넌트를 가상 DOM(jsdom)에 실제로 그려줌.
//          반환값에 container(그려진 DOM의 최상위 요소)가 들어 있음.
// screen : 화면 전체(document.body)에서 요소를 찾는 도구 모음.
import {render, screen} from "@testing-library/react";
import App from "@/app/page";

// 사용자의 실제 조작(타이핑, 클릭, 선택)을 흉내 내는 도구.
import {userEvent} from "@testing-library/user-event/dist/cjs/setup/index.js";

describe('사칙연산 UI 테스트', function(){

    // 1. UI 가져옴
    const {container} = render(<App/>);

    // 2. 원하는 요소 확보
    const su1 = container.querySelector("input[name='su1']");
    const su2 = container.querySelector("input[name='su2']");
    const oper = container.querySelector("select[name='oper']");
    const btn = container.querySelector("button");
    const result = screen.getByTestId('result');

    test("더하기 테스트",  async function (){

        // 3. 특정 이벤트 발생시
        await userEvent.type(su1,'10');
        await userEvent.type(su2,'20');
        await userEvent.selectOptions(oper,"+")
        await userEvent.click(btn);

        // 4. 특정한 결과 확인
        // toHaveTextContent : 요소의 텍스트에 이 문자열이 포함되는지 검사.
        expect(result).toHaveTextContent('답 : 30');
    });

    test("빼기 테스트", async function(){

        // 3. 특정 이벤트 발생시
        await userEvent.type(su1,'30');
        await userEvent.type(su2,'20');
        await userEvent.selectOptions(oper,"-")
        await userEvent.click(btn);

        expect(result).toHaveTextContent('답 : 10');
    });

    test("곱셈 테스트", function(){
       userEvent.type(su1, '20');
       userEvent.type(su2, '2');
       userEvent.selectOptions(oper, '*');
       userEvent.click(btn);

       expect(result).toHaveTextContent('답 : 40');
    });

    test("나누기 테스트", function(){
       userEvent.type(su1, '100');
       userEvent.type(su2, '10');
       userEvent.selectOptions(oper, '/');
       userEvent.click(btn);

       expect(result).toHaveTextContent('답 : 10');
    });
});
```
#### [ src > app > calcModule.js ]
```
import {error} from "next/dist/build/output/log";

// 순수 함수 : 같은 입력에 항상 같은 출력, 바깥에 영향 없음.
// 컴포넌트에서 분리해둔 덕분에 화면 없이 함수만 테스트 할 수 있다.
export function plus(a,b){
    return a + b;
}

export function minus(a,b){

    if(b > a){
        throw new Error("뺄셈의 값은 0보다 커야 합니다.");
    }

    return a - b;
}

export function multiply(a,b){
    return a * b;
}

export function divide(a,b){

    if(b===0){
        throw new error("0으로 나눌수 없습니다.");
    }

    return a / b;
}
```
#### [ src > app > page.jsx ]
```
/*
    1. JEST 설치 : npm install -D jest jest-environment-jsdom
    2. React-Test-Library 설치
    npm install -D @testing-library/react @testing-library/dom @testing-library/jest-dom @testing-library/user-event
    3. jest.confing.js 설정
    4. jest.setup.js설정(test에서만 쓸 환경설정)
    5. package.json에 test script 추가
    6. 모듈 작성(테스트를 하게)
    7. 테스트 코드 작성
    8. npm run test
 */

'use client';

import {useState} from "react";
import {divide, minus, multiply, plus} from "@/app/calcModule";

export default function App(){

    const [result, setResult] = useState({su1:0, su2:0, oper:"+", result:0});

    // 세 개의 입력 요소가 이 함수 하나를 공유한다.
    const setVal = function(e){
        setResult({
            ...result,
            [e.target.name]:e.target.value
        })
    }

    const calculate = function (){
        const {su1,su2,oper} = result;

        let num1 = parseInt(su1);
        let num2 = parseInt(su2);

        if(oper === '+'){
            /*
            setResult({
                ...result,
                result:num1 + num2
            });
             */
            setResult({
                ...result,
                result:plus(num1, num2)
            })
        }
        if(oper === '-'){
            /*
            setResult({
                ...result,
                result:num1 - num2
            });
             */
            setResult({
                ...result,
                result:minus(num1, num2)
            })
        }
        if(oper === '*'){
            /*
            setResult({
                ...result,
                result:num1 * num2
            });
             */
            setResult({
                ...result,
                result:multiply(num1, num2)
            })
        }
        if(oper === '/'){
            /*
            setResult({
                ...result,
                result:num1 / num2
            });
             */
            setResult({
                ...result,
                result:divide(num1, num2)
            })
        }


    }

    return(
        <div>
            <input type={"number"} name={"su1"} value={result.su1} onChange={setVal}/>
            <select name={"oper"} onChange={setVal}>
                <option value={"+"}>+</option>
                <option value={"-"}>-</option>
                <option value={"*"}>*</option>
                <option value={"/"}>/</option>
            </select>
            <input type={"number"} name={"su2"} value={result.su2} onChange={setVal}/>
            <p><button onClick={calculate}>계산</button></p>
            {/*
                data-testid : 테스트 전용 식별자.
                화면에는 아무 영향이 없고 getByTestId로만 쓰인다.
                결과 영역을 명시적으로 잡기 위해 추가한 것이다.
            */}
            <h3 data-testid="result">답 : {result.result}</h3>
        </div>
    )
}
```
- RTL = React Testing Library.
- Jest와 RTL의 역할 분담
  - 둘은 경쟁 관계가 아니라 한 세트이다.
  |  |담당|이 코드에서|
  |---|---|---|
  |Jest|테스트를 찾아 실행하고 결과를 판정|descrive, test, expect|
  |RTL|React를 화면에 그리고 요소를 찾아줌|render, screen, userEvent|
  - Jest만으로 화면을 띄울 수 없다. Jest는 그냥 함수를 실행하는 도구라서 React가 뭔지 모르다. 이때 다리를 연결해 주는게 RTL이다.
  ```
    test("더하기 테스트", async () => { // <- Jest가 이 테스트를 실행
        render(<App/>);               // <- RTL이 컴포넌트를 DOM에 그림
        await userEvent.click(btn);   // <- RTL이 클릭을 흉내 냄.
        expect(result).toHaveTextContent('답 : 30'); // Jest, jest-dom이 추가한 matcher
    })
  ```