---
title: "10. Ajax와 RestAPI"
date: 2026-08-14
draft: false
tags: ["", ""]
categories: ["STUDY"]
summary: "이어드림2026 서비스 개발 수업 DAY10"
weight: 10
---
### API란?
- 소프트웨어 애프리케이션 간의 상호작용을 가능하게 하는 인터페이스
- 함수/메서드, 데이터 구조, 프로토콜, 라이브러리 등이 API가 될 수 있다.

#### 웹에서의 API란?
- 웹이라는 분야 안에서는 API는 주로 (HTTP를 기반으로) 웹 애플리케이션 간의 상호작용을 가능하게 하는 인터페이스를 의미한다.

### REST API
- HTTP의 장점을 최대한 활용할 수 있는 아키텍처

#### REST API의 특징
1. 클라이언트-서버(Client-Server)구조
  - 사용자 인터페이스와 데이터 스토리지의 분리

2. 무상태(Stateless)
  - 각 요청은 독립적, 서버에 상태 정보 유지 X

3. 캐시 처리 가능(Cacheable)
  - 응답은 캐싱 가능해야함

4. 계층화 시스템(Layered System)
  - 클라이언트는 서버를 직접 호출하지 않음(보안, 로드밸런싱)

5. 인터페이스 일관성(Uniform Interface)
  - 효율적인 아키텍처를 위한 일관된 인터페이스

6. 코드 온 디맨드(Code-On-Demand)
  - 서버는 실행 가능 코드 형태로 기능을 전송할 수 있음

#### RESTful API가 되려면
- REST 원칙을 잘 지킨 API 아키텍처를 RESTful하다고 이야기 한다.
- RESTful한 API를 설계하려면,
  1. 자원을 중심으로 설계해야 한다. 따라서 URI는 명사를 위주로 사용해야 한다.
  2. 자원에 대한 행위는 HTTP 메서드로만 표현한다.
  |HTTP 메서드|역할|
  |---|---|
  |GET|데이터 조회|
  |POST|데이터 생성|
  |PUT|데이터 전체 교체|
  |PATCH|데이터 일부 수정|
  |DELETE|데이터 삭제|

### Ajax
- 웹 페이지를 새로고침하지 않고도 서버와 비동기적으로 데이터를 주고 받을 수 있게 하는 기술 현대의 브라우저에는 Ajax 기능이 모두 포함되어 있다.

#### XMLHttpRequest
- Ajax 기능을 활용하는 가장 기본적인 API는 'XMLHttpRequest' 객체이다. 
- 그러나 사용이 매우 불편 한다.

#### Fetch API
- Fetch API를 사용하면 Promise를 기반으로 훨씬 간단하게 Ajax 기능을 사용할 수 있다.
- 'fetch' 함수의 첫 번재 인수로는 요청을 보낼 URL이 전달 된다.
- 두 번째 인수로는 'option' 객체가 전달되며, 없는 경우 기본으로 GET 요청을 보낸다.
- 'option' 객체에는 해당 요청의 Header, Body 등을 설정할 수 있는 여러 프로퍼티가 들어간다.
```
fetch('https://api.example.com/data', {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json'
    },
    body: JSON.stringify({
        name: 'Elice',
        email: 'elice@example.com'
    })  
});
```
- Body를 JSON 형태로 전달할 때에는 무조건 'JSON.stringify'를 통해 직렬화하여 전달해야 한다.
- 'fetch'는 Promise를 반환한다. 따라서 응답에 대한 이후 처리는 'then', 'catch', 'finally'의 메서드 체인을 통해 수행한다.
- 'fetch' 호출 이후 반환하는 'Response' 객체는 Body의 내용을 가공할 수 있는 여러 메서드를 제공한다.
- '.json()' 메서드를 사용하면 JSON 형태의 Body 내용을 간편하게 역직렬화해 준다.(해당 메서드 또한 Promise를 반환)
<REST API 실습 예제 코드 1>
```
<!--axios.rest에 가서 CDN 방식 복사-->
<!--https://unpkg.com/axios@<x.x.x>/dist/axios.min.js 에서 버전을 모르면-->
<!--https://unpkg.com/axios/dist/axios.min.js 이렇게 주소창에 입력-->
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>REST API</title>
        <!--CDN 방식-->
        <script src="https://unpkg.com/axios@1.19.0/dist/axios.min.js"></script>
        <style>
        </style>
    </head>
    <body>

    </body>
    <script>
        // axios.method(url,{config}).then.catch().finally();
        let url = "https://catfact.ninja/breeds";
        //axios.get(url).then(function({data}){
        //    console.log(data);
        //});

        //구조분해 할당 이용시에는 매개변수가 하나라도, () 생략 안됨.
        axios.get(url).then(({data}) => console.log(data));
    </script>
</html>
```

<REST API 실습 예제 코드 2>
```
<!--axios.rest에 가서 CDN 방식 복사-->
<!--https://unpkg.com/axios@<x.x.x>/dist/axios.min.js 에서 버전을 모르면-->
<!--https://unpkg.com/axios/dist/axios.min.js 이렇게 주소창에 입력-->
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>REST API</title>
        <!--CDN 방식-->
        <script src="https://unpkg.com/axios@1.19.0/dist/axios.min.js"></script>
        <style>
        </style>
    </head>
    <body>
        <button>데이터 요청</button>
    </body>
    <script>
        let url = "https://catfact.ninja/breeds";

        // 버튼 클릭시 서버에 요청하여 데이터 console로 출력 하기;
        var btn = document.getElementsByTagName("button")[0];

        btn.addEventListener('click', function(evt){
            axios.get(url).then(async({evt}) => { 
                let {data} = await axios.get(url)
                console.log(data)
            });
        });

        
    </script>
</html>
```