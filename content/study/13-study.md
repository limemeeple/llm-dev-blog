---
title: "13. next.js 2"
date: 2026-08-24
draft: false
tags: ["axios", "board 만들기", "jwt"]
categories: ["STUDY"]
summary: "이어드림2026 서비스 개발 수업 정리(코드 리뷰)"
weight: 13
---
***코드에 주석으로 내용 첨부**
### axios
### <src/app/page.js>
```
// SendList 컴포넌트를 불러온다.
// "@/" 는 프로젝트 루트(src/)를 가리키는 별칭(alias)으로, 
// jsconfig.json 또는 tsconfig.json의 paths 설정에서 지정된다.
import SendList from "@/app/sendList";

export default function Page(){

    // 데이터를 가져올 API 주소.
    const url = 'https://jsonplaceholder.typicode.com/posts/';

    return(
        <div>
            {/* 서버 컴포넌트가 클라이언트 컴포넌트에 'props'로 값을 내려누다.
                이때 넘기는 값은 직렬화(serialize) 가능해야 하는데 url은
                문자열이라 문제없이 전달된다. */}
            <SendList url={url}/>
        </div>
    )
}
```

### <src/app/sendList.jsx>
```
// 이 파일의 컴포넌트들을 클라이언트 컴포넌트로 지정한다.
// 브라우저에서 실행되므로 useState 같은 훅과 onClick 같은 이벤트를 쓸 수 있다.
// 이 지시어는 반드시 파일 최 상단에 있어야 한다.
'use client'

import {useState} from "react";
import axios from "axios";
//useState는 client 에서만 사용 가능한다.
//next.js server와 client를 모두 다룬다.
//그래서 컴포넌트 사용에 따라 어느 포지션인지 명시해줘야 한다.

// page.js에서 내려준 url을 props로 구조 분해하여 받는다.
export default function SendList({url}){
    console.log(url)

    const [list, setList] = useState([]);

    // 버튼을 눌렀을 때 실행될 비동기 함수.
    const send = async function(){

        // axios.get()은 Promise를 반환하므로 await로 응답이 올 때까지 기다려야한다.
        // axios의 응답은 { data, status, headers, ...} 형태의 객체이고,
        // 실제 서버가 보낸 본문은 그중 data 안에 들어 있다.
        // 여기서는 구조 분해로 data만 꺼내 씁니다. 
        let {data} = await axios.get(url);
        console.log(data); 

        // SendList가 재렌더링 되고, 새 list가 Post로 전달된다.
        setList(data);
    }

    return(
      <div>
          <button onClick={send}>전송</button>
          <Post list={list}/>
      </div>
    );
}

function Post({list}){

     // 배열의 각 게시글 객체를 <li> 엘리먼트로 변환.
     // map은 원본을 바꾸지 않고 새 배열을 만들어 반환하므로,
     // posts는 <li> 엘리먼트들이 담긴 배열이 된다.
     let posts = list.map(function(item, idx){

         return(<li key={item.id}>{item.title}</li>);

     });


    if(posts.length === 0 ){
       posts = <li>아이템이 없습니다.</li>;
    }

    console.log(posts.length);

    return(
        <ul>
            {/* JSX에서 중괄호 안에 배열을 넣으면 각 요소가 순서대로 랜더링 된다.
                단일 엘리먼트를 넣어도 그대로 렌더링되므로 두 경우 모두 동작 한다. */}
            {posts}
        </ul>
    );
}
```
#### axios.get()은 Promise를 반환하므로 await로 응답이 올 때까지 기다려야한다. 
- 네트워크 요청은 경과가 오기까지 시간이 걸린다. 그런데 JavaScript는 싱글 스레드이기 때문에 응답이 올때까지 기다리게 되면 화면이 얼어 붙게 된다.
- 그래서 JS는 이렇게 동작한다.
  - 요청은 보내놓고, 즉시 다음 줄로 넘어간다. 대신 결과를 받을 빈상자를 하나 줄고 간다.
  - 여기서 빈 상자가 'Promise'다.
- await를 안쓰면 Promise 객체에는 data라는 속성 자체가 없으므로 undefined가 나온다.
- awiat는 Promise 상자에 결과가 채워질 떄까지 함수의 진행을 멈추라는 뜻이다. 결과가 채워지면 상자를 열어서 안에 든 실제 응답 객체를 꺼내준다.
- 여기서 중요한 점은 멈추는 건 send 함수 뿐이다.
- 정리하자면 await는 비동기로 도착할 값을 동기 코드처럼 꺼내쓰기 위한 문법이다.

#### 구조 분해란?
- 구조 분해(destructuring)는 객체나 배열안에서 필요한 값만 꺼내 변수로 만드는 문법이다.

---
### 게시판 만들기
### <src/app/page.jsx>
```
//npm install axios
import Link from "next/link";
// next/link의 Link는 <a> 태그와 비슷하지만, 페이지 전체를 새로 로드하지 않고 필요한 부분만
// 교체하는 클라이언트 사이드 이동(SPA 방식)을 한다.
// 또 화면에 보이는 순간 해당 페이지를 미리 받아두는 최적화도 해준다.

import PostList from "@/app/PostList";

export default function Home(){
    return(
        <div>
            <h3>리스트 가져오기</h3>
            <Link href="/write"><button>글쓰기</button></Link>
            <PostList page={1}/>
        </div>
    );
}
```
### <src/app/PostList.jsx>
```
'use client'
import {useEffect, useState} from "react";
import axios from "axios";
import Link from "next/link";

// CSS 파일을 import하면 Next.js가 빌드 시 스타일을 주입해준다.
import './List.css';

export default  function PostList({page}){

    const[posts, setPosts] = useState();

    // useEffect : 컴포넌트가 화면에 그려진 '직후' 실행되는 코드를 등록.
    // 랜더링 도중이 아니라 랜더링이 끝난 뒤 실행되므로 여기서 setState를 호출하면 다시 렌더링이 일어남.
    // useEffect 에는 await async 사용 안됨.
    useEffect(function(){

        // 두 번째 인자가 빈 배열 '[]' 이므로 이 함수는 최초 1회만 실행.
        // 배열에 값을 넣으면 그 값이 바뀔 때마다 다시 실행. 
        axios.get('http://localhost/list/' + page).then(({data}) => {
            console.log(data); // 데이터를 받아온 다음
            //html 태그로 만들어서 -> stat에 저장.
            makeHtml(data);
        });
    },[]);

    const makeHtml = function makeHtml({list}){
        console.log(list);

        let content = list.map(item =>
            (
                <div key={item.idx} className="post">
                    <Link href={`/detail/${item.idx}`}>
                        <div className="title">
                            {item.idx} : {item.subject}
                            <span className="cnt">[{item.bHit}]</span>
                        </div>
                    </Link>
                    <div className="sub">{item.user_name}</div>
                </div>
            )
        );
        setPosts(content);
    }

    return(
        <div>
            {posts}
        </div>
    );
}
```
### <src/app/src/write/page.jsx>
```
'use client';
import Link from "next/link";
import "./Write.css";
import {useState} from "react";
import axios from "axios";

export default function Write(){

    // 입력값 3개를 각가의 useState로 나누지 않고 객체 하나로 묶어서 관리.
    // 필드가 많아질수록 이 방식이 관리하기 편함.
    const [info, setInfo] = useState({user_name:"", subject:"", content:""});

    const inputVal = function(e){
        setInfo({

            // ...info : 스프레드 문법. 기존 객체의 모든 속성을 그대로 복사.
            ...info,
            [e.target.name]: e.target.value
        })
    }

    const save = async function(){
        //axios.post(url, {params});
        //obj.data == {data};
        let {data} = await axios.post('http://localhost/write', info);

        console.log(data);

        if(data.success){
            alert("글쓰기에 성공했습니다.");

            // location.href는 브라우저를 통째로 새로 로드한다.
            location.href = 'detail/' + data.idx;
        }else{
            alert("글쓰기에 실패했습니다.");
        }
    }

    return(
        <div className="write">
            <div className="header">

                {/* value와 onChange가 한 쌍인 입력칸을 '제어 컴포넌트' 라고 한다.
                    타이핑하면 onChage -> 상태변경 -> 재렌더링 -> 새 값 표시 순으로 진행된다. */}
                <input type="text" name="user_name" value={info.user_name} placeholder="작성자" onChange={inputVal}/>
            </div>
            <div className="title">
                <input type="text" name="subject" value={info.subject} placeholder="글 제목" onChange={inputVal}/>
            </div>
            <div>
                <textarea value={info.content} name="content" onChange={inputVal}></textarea>
            </div>
            <div className="btn_area">
                <Link href="/">리스트</Link>
                <button onClick={save}>저장</button>
            </div>
        </div>
    );
}
```
### <src/app/detail/[slug]/page.jsx>
```
//params : 경로 뒤에 오는 파라매터
//searchParams : ? 뒤에 오는 파라매터
//await async는 서버에서 사용 가능

// [slug] 처럼 대괄호로 감싼 폴더명은 '동적 라우트'다.
// 어떤 값이든 이 페이지가 처리하고 그 값은 params.slug로 전달된다.
// slug라는 이름은 폴더명에서 온 거다.
import Post from "@/app/detail/[slug]/Post";

// 서버 컴포넌트는 함수 자체를 async로 만들 수 있다.
// 클라이언트 컴포넌트는 이게 불가능해서 useEffect를 쓴다.
export default async function Detail(props){

    //let {slug} = await props.params;
    let idx = (await props.params).slug;
    console.log(idx);

    return(
        <div>
            <h3>{idx}번 글 상세보기</h3>
            <hr/>
            <Post idx={idx}/>
        </div>
    );
}
```
### <src/app/detail/[slug]/Post.jsx>
```
'use client'; // await async 사용 불가(Component, useEffect)
import {useEffect, useState} from "react";
import axios from "axios";
import Link from "next/link";
import "./Post.css";

export default function Post({idx}){

    console.log(idx+"번 글 확인.");

    const[post, setPost] = useState(null);

    const del = async function del(){
        console.log(idx + '번 글 삭제!');
        let {data} = await axios.get("http://localhost/delete/" + idx);
        console.log(data);
        /*
        let msg = "이미 삭제된 게시글 입니다.";

        if(data.success){
            msg = "삭제에 성공했습니다.";
        }
        */

        let msg = data.success === true ? '삭제에 성공했습니다.'  : '이미 삭제된 게시글 입니다.';

        alert(msg);

        location.href='/';

    }

    const makeHtml = function makeHtml({post}){

        let content = <div>게시물이 존재하지 않습니다.<p><Link href="/">돌아가기</Link></p></div>;

        if(post != null){
            content = <div>
                <div className="header">
                    <div>작성자 : {post.user_name}</div>
                    <div>조회수 : {post.bHit}</div>
                </div>
                <div className="title">제목 : {post.subject}</div>
                <hr/>
                <div>{post.content}</div>
                <hr/>
                <div className="btn_area">
                    <Link href="/">리스트</Link>
                    <button onClick={del}>삭제</button>
                </div>
            </div>
        }

        setPost(content);
    }

    let content = useEffect(function(){
        axios.get("http://localhost/detail/"+idx).then(function({data}){
            console.log(data);

            makeHtml(data);
        });
    },[]);

    return(
      <div>
          {post}
      </div>
    );
}
```
---
### jwt
### <src/app/layout.jsx>
```
export default function Layout({children}){
    return(
        <html>
            <head>
                <meta charSet="UTF-8"/>
                <title>JWT 서비스</title>
            </head>
            <body>
            {/* 'children'에는 현재 URL에 해당하는 page.jsx가 표시됨.
                 '/' 면 로그인 페이지, '/write'면 글쓰기 페이지가 표시됨. 페이지가 바뀌어도 layout은
                 다시 그려지지 않고 유지 된다.*/}
                {children}
            </body>
        </html>
    )
}
```

### <src/app/page.jsx>
```
'use client';
import "./common.css";
import {useEffect, useState} from "react";
import axios from "axios";

export default function LoginPage(){

    const[info, setInfo] = useState({id:"", pw:""});

    const inputVal = function (e){
        setInfo({
            ...info,
            [e.target.name]:e.target.value
        });
    }

    // 로그인 화면에 진입하면 기존 인증 정보를 삭제한다.
    // sessionStorage는 브라우저에만 존재하므로 useEffect 안에 있어야 한다.
    // 랜더링 본문에 두면 서버 빌드 시 sessionStorage is not definde 에러 발생한다.
    useEffect(() => {
        sessionStorage.removeItem('id');
        sessionStorage.removeItem('token');
    },[])

    const login = async function(){
        console.log(info); //admin / pass

        // 아이디/비밀번호를 본문에 담아 서버에 전달한다.
        let {data} = await axios.post("http://localhost/login", info);

        console.log(data);

        if(data.success){
            // token 값 저장.
            // JWT 인증의 핵심 부분
            // 서버는 세션을 들고 있지 않고, 대신 검증됐다는 증명서(token)을 발급한다.
            // 클라이언트가 이걸 보관했다가 매 요청마다 제시하면 서버가 검증해서 확인.
            // sessionStorage는 탭을 닫으면 사라진다.(localStorage는 계속 남음.)
            sessionStorage.setItem('id', info.id);
            sessionStorage.setItem('token', data.token);

            alert("로그인에 성공하였습니다.");
            location.href = '/list/1';
        }else{
            alert("아이디 또는 비밀번호를 확인해 주세요.");
        }
    }

    return(
        <div>
            <h3>로그인</h3>
            <hr/>
            <table>
                <tbody>
                    <tr>
                        <th>ID</th>
                        <td>
                            <input type="text" name="id" value={info.id} onChange={inputVal}/>
                        </td>
                    </tr>
                    <tr>
                        <th>PW</th>
                        <td>
                            <input type="password" name="pw" value={info.pw} onChange={inputVal}/>
                        </td>
                    </tr>
                    <tr>
                        <th colSpan={2}>
                            <button onClick={login}>로그인</button>
                        </th>
                    </tr>
                </tbody>
            </table>
        </div>
    );
}
```
### <src/app/list/[slug]/page.jsx>
```
'use client';
import {useEffect, useState} from "react";
import axios from "axios";
import '@/app/common.css';
import Link from "next/link";

// next/image의 Image는 <img>를 대체한다.
// 크기 지정을 강제해서 이미지 로딩 중 화면이 밀리는 현상을 막아줌.
import Image from "next/image";

// Pagination 컴포넌트가 페이지 번호 버튼들을 알아서 그련준다.
import {Pagination, Stack} from "@mui/material";

//[slug] 동적 라우트, /list/1, /list/2 ... 등을 처리한다.
export default function ListPage({params}){

    const[list, setList] = useState([]);
    let[pages, setPages] = useState();

    // useEffect 콜백에는 async를 붙일 수 없어서 await 대신 .then()으로 값을 꺼낸다.
    useEffect(() => {
        params.then(({slug}) => {
            console.log(slug);
            callList(slug);
        });
    }, []);

    const callList = async function(page){
        // 서버에 요청하기
        // http://localhost/{id}/{page}
        // header: {Authorization:{jwt 토큰}}

        // 로그인 할때 저장해둔 인증 정보를 꺼낸다.
        const id = sessionStorage.getItem('id');
        const token = sessionStorage.getItem('token');

        // axios.get(주소, 옵션) - 두 번째 인자가 옵션이다.
        // post는 (주소, 본문, 옵션) 순서라 헷갈릴 수 있다.
        // headers의 Authorization에 토큰을 실어 보낸다.
        // JWT 인증은 요청할 때마다 증명서를 제시해야 한다.
        let {data} = await axios.get(`http://localhost/list/${id}/${page}`
                                        ,{headers:{Authorization:token}})
        console.log(data);

        // 서버가 토큰을 검증한 결과 유효하지 않은 토큰일 경우 로그인 페이지로 이동 시킨다.
        if(!data.loginYN){
            alert("로그인이 필요한 서비스 입니다.");
            location.href = "/";
        }

        setPages(data.pages);

        let content = data.list.length === 0 ?
                      <tr><th colSpan={6}>작성된 글이 없습니다.</th></tr>
                      : data.list.map((item) =>
                           (

                               <tr key={item.idx}>
                                   <td>{item.idx}</td>
                                   <td>
                                       {item.cnt === 0?<Image src="/noimage.png" width={25} height={25} alt={"이미지 없음"}/>
                                                     :<Image src="/image.png" width={25} height={25} alt={"이미지 있음"}/>}
                                   </td>
                                   <td><Link href={`/detail/${item.idx}`}>{item.subject}</Link></td>
                                   <td>{item.user_name}</td>
                                   <td>{item.bHit}</td>
                                   <td>{item.reg_date}</td>
                               </tr>

                           )
                      );
        setList(content);
    }

    return(
        <>
            <Link href="/write">글쓰기</Link>
            <table className="list">
                <thead>
                    <tr>
                        <th>번호</th>
                        <th>이미지</th>
                        <th>제목</th>
                        <th>작성자</th>
                        <th>조회수</th>
                        <th>작성일</th>
                    </tr>
                </thead>
                <tbody>
                    {list}
                    <tr>
                        <th colSpan={6}>
                            <div style={{justifyContent:'center', display:'flex'}}>
                                <Stack>
                                    <Pagination
                                        count={pages} //전체 페이지 수
                                        color={'primary'} //선택한 색
                                        variant={'outlined'} //외각 선

                                        shape={'rounded'} //모양변경
                                        siblingCount={1} //중간정도 왔을때 양쪽에 표시할 갯수
                                        onChange={function(evt, page){
                                            console.log(evt, page);
                                            //location.href = `/list/${page}`
                                            callList(page);
                                        }}
                                    />
                                </Stack>
                            </div>
                        </th>
                    </tr>
                </tbody>
            </table>
        </>
    );
}
```
### <src/app/write/page.jsx>
```
'use client';
import '@/app/common.css';
import {useState} from "react";
import Image from "next/image";
import axios from "axios";
import Link from "next/link";

export default function WritePage(){

    const[info, setInfo] = useState({subject:'',content:''});

    // 로그인한 사용자 정보를 꺼내옵니다.
    const id = window.sessionStorage.getItem('id');
    const token = window.sessionStorage.getItem('token');

    const[upload, setUpload] = useState([]); //업로드할 사진 저장소
    const[prev, setPrev] = useState([]); //사진미리보기

    const inputVal = function (e){
        // dict[name] = val
        setInfo({
            ...info,
            [e.target.name]: e.target.value
        });

        console.log(info);
    }

    const fileSelect = function(e){
        console.log(e);
        // 파일 정보를 추출
        // multiple이 없으면 항상 1개이므로 [0]만 쓴다.
        let file = e.target.files[0];

        // 업로드시킬 정보에 등록
        // upload.push(file) 처럼 원본을 직접 수정하면 React가 변경을 감지 하지 못한다.
        setUpload([...upload, file]);

        //미리보기
        // 1. 파일을 읽을 리더 준비
        // 파일은 브라우저 메모리에 있는 바이너리라 그대로는 <img src>에 넣지 못한다.
        let reader = new FileReader();
        reader.readAsDataURL(file); // 2.데이터를 base64 형식으로 읽어온다.(바니너리를 16진수 형태 문자로 읽음)
        
        reader.onloadend = function(e){ // 3.파일을 다 읽었을 때
            console.log(e);// e.target.result를 Image 태그에 넣으면 된다.
            setPrev([...prev, <Image key={e.timeStamp} src={e.target.result} alt={file.name} width={100} height={100}/> ]);
        }
    }

    //file upload 시 지켜야할 법칙
    //1. POST 방식으로 보낼것
    //2. enctype=multipart/form-data 지정할 것
    const save = async function(){
       let formData =  new FormData();
       formData.append('user_name', id);
       formData.append('subject', info.subject);
       formData.append('content', info.content);

       for(const file of upload){
           formData.append('files', file);
       }

       console.log("저장 시작");
       // post(url, param, option)
       // axios는 본문이 FormData면 Content-Type: multipart/form-data와 boundary 값을 자동으로 붙여준다.
       let {data} = await axios.post("http://localhost/write", formData,{headers:{Authorization:token}})

       console.log(data);

       if(data.success === true){
           alert("글쓰기에 성공하였습니다.");
           location.href='/detail/' + data.idx;
       }else{
           alert("글쓰기에 실패하였습니다.");
       }

    }

    return(
        <>
            <h3>글 쓰 기</h3>
            <hr/>
            <table className={"form"}>
                <tbody>
                    <tr>
                        <th>제목</th>
                        <td>
                            <input type={"text"} name={"subject"} onChange={inputVal} value={info.subject}/>
                        </td>
                    </tr>
                    <tr>
                        <th>작성자</th>
                        <td>
                            <input type={"text"} name={"name"} value={id} readOnly={true}/>
                        </td>
                    </tr>
                    <tr>
                        <th>내용</th>
                        <td>
                            <textarea name={"content"} onChange={inputVal} value={info.content}>

                            </textarea>
                        </td>
                    </tr>
                    <tr>
                        <th>사진</th>
                        <td>
                            <input type={"file"} name={"files"} onChange={fileSelect}/>
                            <div>{prev}</div>
                        </td>
                    </tr>
                    <tr>
                        <th colSpan={2}>
                            <button onClick={save}>저장</button>
                            <Link href={"list/1"}>리스트</Link>
                        </th>
                    </tr>
                </tbody>
            </table>
        </>
    )

}
```
### <src/app/detail/[slug]/page.jsx>
```
'use client';
import '@/app/common.css';
import {useEffect, useState} from "react";
import axios from "axios";
import Link from "next/link";
export default function DetailPage({params}){

    const[info, setInfo] = useState({}); // 글에 대한 정보
    const[photos, setPhotos] = useState([]); // 사진 정보 들

    useEffect(function(){
        //slug로 부터 받아온 idx를 이용해 게시판글 가져오기
        params.then(({slug}) => {
            console.log(slug);
            getDetail(slug);
        });
    },[])

    const getDetail = async function(idx){

        let id = sessionStorage.getItem('id');;
        let token = sessionStorage.getItem('token');
        const ip = 'http://localhost';

        // http://{server IP}/detail/{id}/{idx}
        // 글 번호와 사용자 id를 함꼐 보내고, 토큰으로 인증합니다.
        // 서버는 이 글을 볼 권한이 있는지 확인한 뒤 응답한다.
        let {data} = await axios.get(`${ip}/detail/${id}/${idx}`, {headers:{Authorization:token}});

        console.log(data);

        if(data.loginYN === false){
            alert("로그인이 필요한 서비스 입니다.");
            location.href = '/';
        }else{
            setInfo(data.detail);

            // 사진은 파일 자체를 응답에 담지 않고, 파일 번호만 받아온다.
            // 실제 이미지는 <img src>가 그 번호로 서버에 다시 요청해서 가져온다.
            let photoList = data.photos.map(function(photo){
                return(
                    <div key={photo.file_idx}>
                        <p>
                            <img src={`${ip}/photo/${photo.file_idx}`} width={300} alt={photo.ori_filename}/>
                        </p>
                        <br/>
                        <a href={`${ip}/download/${photo.file_idx}`}>다운로드</a>
                    </div>
                )
            });
            setPhotos(photoList);
        }
    }

    const del = async function(){

        let id = sessionStorage.getItem('id');;
        let token = sessionStorage.getItem('token');
        const ip = 'http://localhost';

        //method : DELETE
        console.log()
        let {data} = await axios.delete(`${ip}/del/${id}/${info.idx}`,{headers:{Authorization:token}});
        //{ip}/del/{id}/{idx}

        console.log(data);
        alert("삭제되었습니다.");

        location.href='/list/1'

    }

    return(
        <>
            <h3>{info.idx} 상세보기</h3>
            <hr/>
            <table className={"form"}>
                <tbody>
                <tr>
                    <th>제목</th>
                    <td>
                        {info.subject}
                    </td>
                </tr>
                <tr>
                    <th>작성자</th>
                    <td>
                        {info.user_name}
                    </td>
                </tr>
                <tr>
                    <th>내용</th>
                    <td>
                        {info.content}
                    </td>
                </tr>
                <tr>
                    <th>조회수</th>
                    <td>
                        {info.bHit}
                    </td>
                </tr>
                <tr>
                    <th>사진</th>
                    <td>
                        {photos}
                    </td>
                </tr>
                <tr>
                    <th colSpan={2}>
                        <button onClick={del}>삭제</button>
                        <Link href={"/list/1"}>리스트</Link>
                    </th>
                </tr>
                </tbody>
            </table>
        </>
    )
}
```
- 인증은 **로그인 시 토큰 발급 -> sessionStorage 보관 -> 매 요청 Authorization 헤더에 첨부 -> 서버가 검증** 구조이다.
#### JSX와 JS를 나눠서 쓰는 이유는 뭘까?
- Next.js에서는 기능상 차이가 없다.
- 차이는 문법이 아닌 관례와 도구 설정에 있다.
- 이전에는 빌드 시 어떤 파일에 JSX가 들어 있으니 변환해달라고 알려줘야 했고 그 표시가 '.jsx' 확장자 였지만 지금은 Next.js가 전부 통일하게 처리하도록 미리 설정해두고 있다. 그래서 확장자를 가리지 않는다.
- 그럼 왜 여전히 나눠 쓸까?
  1. 파일 성격을 이름만 보고 구분하기 위해
  2. 에디터/도구가 더 잘 알아본다.
  3. 일부 도구에서는 아직 구분해서 변환 함.