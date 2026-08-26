---
title: "12. next.js"
date: 2026-08-20
draft: false
tags: ["route", "Image"]
categories: ["STUDY"]
summary: "이어드림2026 서비스 개발 수업 DAY12"
weight: 12
---
### <index.html>
```
<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <link rel="icon" type="image/svg+xml" href="/favicon.svg" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>01_start</title>
  </head>
  <body>
    <div id="root"></div>
    <script type="module" src="/src/main.jsx"></script>
  </body>
</html>
```
- React 앱은 HTML 파일이 딱 하나이며 나머지는 전부 JavaScript 가 만들어서 그 안에 채워 넣는다.(SPA = Single Page Apllication)


### <src/main.jsx>
```
import { StrictMode } from 'react'
import { createRoot } from 'react-dom/client'
import './index.css'
import App from './App.jsx'
// id="root" 인 요소를 가져와 뭘 그린다?
createRoot(document.getElementById('root')).render(
  <StrictMode> {/*엄격 모드 : 문법 검사를 정석적으로 함*/}
    <App />     {/*<App/> 은 App 함수(클래스) 컴포넌트를 가져온것*/}
  </StrictMode>,
)
```
- import의 중괄호 차이는 가져오는 쪽이 아니라 내보내는 쪽에서 결정한다.
- App.jsx에는 'export default App'이라고 되어 있고 default는 파일당 하나이다. 가져오는게 명확하기 때문에 중괄호를 쓰지 않는다.
- react-dom/client 같이 안에 여러 기능이 들어있을 경우 여러기능 중 하나를 찍어서 가져오기 때문에 중괄호가 필요하다.
```
import { createRoot } from 'react-dom/client'   // 중괄호 O
import App from './App.jsx'                     // 중괄호 X
```
- CSS를 import 해서 사용한다(import './index.css'). 가져온 값을 쓰지 않으니 'from'없이 경로만 적는다.
<createRoot(document.getElementById('root')).render(...)>
```
document.getElementById('root')   // 1. 아까 그 빈 div를 찾아온다
createRoot( ... )                 // 2. 그 div를 React 관리 구역으로 지정
.render( ... )                    // 3. 그 안에 실제로 그린다

root라는 이름표 붙은 자리를 찾은 후 -> React 담당 구역으로 만들고 -> 해당 구역에 App을 그린다.
```
- App은 그냥 함수이다. 그런데 App()이 아닌 &lt;App /&gt;이라고 태그처럼 사용한다. 대문자가 중요한데 소문자로 쓸 경우 태그로 인식할 수 있다.(그래서 컴포넌트의 이름은 항상 대문자로 시작해야 한다.)

- JSX안에서 주석을 쓰려면 '{/* 주석 */}' 같이 중괄호로 감싸야 한다.

### <src/App.jsx>
```
import './App.css'

function App() {

  let html = <h3>Hello, React.js</h3>;
  // return 은 render() 를 품고 있다.
  // return() 안에서는 태그 하나로 끝나야 한다.
  return (<>{html}</>);
}

// App.jsx 에서는 기본으로  App 함수를 내보낸다.
export default App
```
- 컴포넌트는 JSX를 반환하는 함수이다.
- 컴포넌트는 이름이 대문자로 시작해야하고 JSX를 반환해야 한다.
```
 let html = <h3>Hello, React.js</h3>;
```
- 위 코드를 보면 HTML을 변수에 넣는다고 볼 수 있으나 HTML이 아닌 화면에 붙이기 전까지는 JavaScript 객체이다.
- 객체이기 때문에 변수에 담을 수 있으며 배열에도 넣을 수 있고 함수 인자로 넘길수도 있다.

```
function App() {
  const isLoggedIn = false
  
  const message = isLoggedIn ? <h3>환영합니다!</h3> : <h3>로그인해주세요</h3>

  return message
}
```
- 조건에 따라 다른 화면ㅇ르 보여주는 것도 자연스럽게 가능하다.
- JSX 안에서 '{}' 중괄호는 "여기서부터는 JavaScript다"라는 신호이다.
- 중괄호 안에는 값이 되는 것은 다 들어갈 수 있다.
```
<h3>{1 + 2}</h3>              // 3
<h3>{"안녕" + "하세요"}</h3>    // 안녕하세요
<h3>{user.name}</h3>          // 객체 속성
<h3>{getName()}</h3>          // 함수 호출 결과
```
- &lt;&gt;...&lt;/&gt;의 경우 여러 개를 반환할 수 없기 떄문에 화면에 남지 않는 해당 태그로 감싼다.
```
// 두 개를 반환하려고 해 에러 발생.
return(
  <h3>제목</h3>
  <p>내용<p>
) 


// <>...</> 태그로 감싸서 해결 
return(
  <>
    <h3>제목</h3>
    <p>내용<p>
  </>
)
```

---

### <app/page.js>
```
// 폴더의 대표는 page 라는 이름을 가지고 있어야 한다.
import Link from "next/link";

export default function Page(){
    return (
        <div>
            <h1>Main Page</h1>
            <p><Link href="/blog">페이지 이동</Link></p> // 1번
            <p><Link href="/blog?idx=11&method=detail">쿼리 파라메터</Link></p> // 2번
            <p><Link href="/blog/11">path variable</Link></p> // 3번
        </div>
    );
}
```
- [위 코드 1번] : /blog/page.jsx 로 이동
- [위 코드 2번] : /blog/page.jsx 이동 + searchParams 전달.
- [위 코드 3번] : /blog/[...slug]/page.js로 이동(slug = ['11'])

<[...slug]>
- 폴더 이름에 붙이는 점 3개(...)는 해당 경로 뒤에 오는 경로를 몇 개든 받겠다는 뜻이다.
```
app/blog/[slug]/page.js     =>  한 칸만 받음
app/blog/[...slug]/page.js  =>  뒤에 오는 거 전부 받음
```

- 받아지는 모양
|접속 주소|[slug]|[...slug]|
|---|---|---|
|/blog|매칭안됨|매칭안됨|
|/blog/11|"11"(문자열)| ["11"] (배열)|
|/blog/food/11|매칭안됨|["food", "11"]|
|/blog/food/korea/11|매칭안됨|["food", "korea", "11"]|

- 핵심 차이 두가지
  1. [slug]는 값이 문자열, [...slug]는 항상 배열이라 '.map'을 쓸 수 있다.
  2. [slug]는 칸이 정확히 1개일 때만 매칭, [...slug]는 1개 이상이면 다 매칭 가능.

- 대괄호 두겹은 어떨까?
```
app/blog/[[...slug]]/page.js  =>  /blog 까지 포함해서 전부 받음
```


### <app/blog/page.jsx>
```
//요청시 파라메터는 props를 통해서 받아낼 수 있다.
export default async function Page(props){
    console.log(props);

    /*
    props.params.then(function(res){
        console.log('params',res);
    });
     */

    let search = await props.searchParams; 
    console.log(search); //Object

    // 키를 하나씩 추출
    let items = Object.keys(search).map(function(key){
        console.log(key,search[key]);// 키에 해당하는 값 추출
        return(<li>{key}:{search[key]}</li>);// html로 보여주기 위해 조립
    });

    return(
        <div>
            <h1>blog/page.jsx</h1>
            <ul>{items}</ul>
        </div>
    );
}
```

- Next.js App Router에서 page.jsx의 함수는 직접 호출하는게 아닌 Next가 대신 호출 해준다. 그때 Next가 넣어주는 인자가 **props다.
- props에 들어오는건 두개다.
```
props = {
  params:       Promise<{ ... }>, // 경로에서 뽑은 값 => /blog/[...slug]
  searchParams: Promise<{ ... }>  // ? 뒤에 붙은 값  => /blog?idx=11
}
```

< searchParams >

```
let search = await props.searchParams;  // Promise 풀기 => {idx:"11", method:"detail"}

let items = Object.keys(search).map(function(key){  // 키만 뽑기 => ["idx", "method"]
    console.log(key,search[key]);
    return(<li>{key}:{search[key]}</li>); // 키로 값을 꺼내서 태그 조립
});
```
- 값은 문자열이다.
- 같은 키가 반복 되면 값이 배열로 바뀐다.(ex> '?tag=a&tag=b' => ["a","b"])
- 굳이 props로 통째로 받지 않고 필요한 것만 꺼내 쓰는 방식이 더 흔하다. 
```
export default async function Page({ searchParams }){   // 필요한 것만 꺼냄
    const search = await searchParams;
    ...
}
```

### <app/blog/[...slug]/page.js>
```
export default async function Page(props){
    console.log("props",props);

    let params = await props.params;
    //[slug]일 경우 slug:11
    console.log("params", params.slug);
    //[...slug]일 경우 blog/11 -> slug:['11']
    //[...slug]일 경우 blog/food/11 -> slug:['food', '11']
    //[...slug]일 경우 blog/food/korea/11 -> slug:['food', 'korea', '11']

    let list = params.slug.map((item, idx) => (<li key={idx}>{idx}.{item}</li>))

    return(
        <>
            <p>경로로 받은 파라메터</p>
            <ul>
                {list}
            </ul>
        </>
    );
}
```
----
### <app/page.js>
```
import Image from "next/image";

export default function Page(){
    return(
        /*
         *  절대경로 : /src -> 서버의 메인을 중심으로
         *  상대경로 : ./src, src -> 현재위치를 기준으로
         *  ./는 현재 위치, ../ 현재 위치에서 한단계 위
         */
        <div>
            <img src="/incheon.jpg" width={1027} height={768} alt="incheon image"/>
            <hr/>
            <Image
                src="/incheon.jpg"
                width={1027}
                height={768}
                alt="incheon image"
                placeholder="blur"
                blurDataURL="/incheon_small.jpg"
            />
        </div>
    );
}
```
- 'public'폴더는 URL상 '/' 다.
```
프로젝트 구조              브라우저가 보는 주소
public/incheon.jpg    =>    /incheon.jpg
public/img/a.png      =>    /img/a.png
```
- 상대 경로를 쓰면 안되는 이유
```
/blog 에서   ./incheon.jpg  =>  /incheon.jpg         // 우연히 맞음
/blog/food 에서 ./incheon.jpg => /blog/incheon.jpg   // 깨짐
```
상대 경로는 지금 보고 있는 URL 기준이기 때문에 페이지 마다 결과가 달라진다.

- 'img'는 서버가 파일을 그냥 내보내지만 'Image'는 아래와 같은 작업이 진행 된다.
```
브라우저 요청 => Next 서버가 가로챔
=> 1. 화면 크기에 맞게 리사이즈 
   2. webp로 변환
   3. 압축 (q=75)
   4. 캐시에 저장
=> 브라우저에 전달.
```

< 차이표 >
||< img >|< Image >|
|---|---|---|
|파일크기|원본 그대로|자동 축소·변환|
|화면별 다른 이미지| 직접 srcset 작성|자동|
|지연로딩|loading="lazy"수동|기본 적용|
|width의 의미|표시 크기|원본 비율|
- Image의 width는 자리 미리 잡아두기용 정보이다.
