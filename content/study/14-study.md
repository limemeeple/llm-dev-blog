---
title: "14. next.js 3"
date: 2026-08-26
draft: false
tags: ["kakaoMap", "chart", "data_flow"]
categories: ["STUDY"]
summary: "이어드림2026 서비스 개발 수업 정리(코드 리뷰)"
weight: 14
---
***코드에 주석으로 내용 첨부**
### kakaoMap 연동
### <src/app/layout.js>
```
export default function Layout({children}){

    const API_KEY = "카카오맵 API 키";

    return(
      <html lang={"ko"}>
        <head>
            <meta charSet={"UTF-8"}/>
            <title>KAKAO MAP API</title>
            <script type="text/javascript" src={`https://dapi.kakao.com/v2/maps/sdk.js?appkey=${API_KEY}`}></script>
        </head>
        <body>
            {children}
        </body>
      </html>
    );
}
```
### <src/app/page.js>
```
'use client';
import {useEffect, useRef} from "react";

export default function MainPage(){

    let container = useRef(null);

    useEffect(() => {
        // 맵 옵션 설정
        const mapOption = {
            center: new kakao.maps.LatLng(37.53542719679273, 126.8978317969594), // 지도의 중심좌표
            level: 3 // 지도의 확대 레벨
        };

        // 지도를 표시할 div와  지도 옵션으로  지도를 생성합니다
        let map = new kakao.maps.Map(container.current, mapOption);

        // 맵 중앙에 마커 표시
        let marker = new kakao.maps.Marker({
            position: map.getCenter()
        });

        marker.setMap(map);

        //이벤트 추가
        kakao.maps.event.addListener(map, 'click', function (evt){
           let pos = evt.latLng;
           marker.setPosition(pos);
           console.log(`위도: ${pos.getLat()}/ 경도: ${pos.getLng()}`);
        });

    }, []);

    return(
        <>
            <div id={"map"} style={{width: '100%', height: '350px'}} ref={container}>

            </div>
        </>
    );
}
```
---
### bootstrap
### <src/app/layout.jsx>
```
export default function Layout({children}){
    return(
        <html>
            <head>
                <meta charSet={"UTF-8"}/>
                <title>BOOT STRAP</title>
            </head>
            <body>
                {children}
            </body>
        </html>
    )
}
```
### <src/app/page.jsx>
```
// npm install react-bootstrap bootstrap
// 패키지를 두 개 설치한 이유는
// bootstrap -> css 파일 (실제 색상, 여백, 모양을 담당)
// react-bootstrap -> 그 css를 쓴ㄴ React  컴포넌트들

// react-bootstrap은 css를 자동으로 넣어주지 않음으로 추가.
import "bootstrap/dist/css/bootstrap.min.css"
import {Button, ButtonToolbar} from "react-bootstrap";

export default function MainPage(){
    return(
        <div>
            {/* ButtonToolbar: 버튼들을 가로로 묶어주는 컨테이너이다. */}
            <ButtonToolbar>
                </* variant: Bootstrap이 정해둔 색상 테마 이름이다.*/>
                <Button variant={"primary"}>Primary</Button>
                <Button variant={"outline-secondary"}>outline-secondary</Button>
                <Button variant={"success"}>success</Button>
                <Button variant={"warning"}>warning</Button>
                <Button variant={"danger"}>danger</Button>
                <Button variant={"info"}>info</Button>
                <Button variant={"light"}>light</Button>
                <Button variant={"dark"}>dark</Button>
                <Button variant={"link"}>link</Button>
            </ButtonToolbar>
        </div>
    )
}
```
#### 이전에 사용한 MUI와 Bootstrap의 차이(그냥 참고용)
| |React Bootstrap|MUI|
|---|---|---|
|기반|Bootstrap CSS|자체|
|CSS 파일|직접 import 필요|불필요|
|서버 컴포넌트|대부분 가능|'use client'필요|
|커스터마이징|CSS 변수/클래스|theme 객체|
- 한 프로젝트에서 둘이 같이 쓰지 않도록 주의 하자. 스타일 충돌과 번들 크기 상승이 일어난다.
---
### chart
### <src/app/bar/page.jsx>
```
// MUI X Charts는 브라우저에서 SVG(벡터)를 그리고 크기를 측정하므로 클라이언트 컴포넌트여야 한다.
'use client';
import {BarChart} from "@mui/x-charts";

export default function BarChartPage(){
    return(
        <>
            <div style={{width: "30%"}}>
                <BarChart
                    // series: 그릴 데이터 묶음.
                    series={[
                        {data:[4,2,3,5], label:"성장률"}
                    ]} /* 막대 그래프 하나하나의 내용 */

                    // xAxis: 가로축 설정.
                    // scaleType:'band' - '구간형 축', 없으면 축을 숫자로 해석해서 막대가 겹치거나 이상하게 나옴.
                    xAxis={[
                        {data:['1분기', '2분기', '3분기', '4분기'], scaleType:'band'}
                    ]}/* x 축의 내용 */

                    // MUI X Chars는 크기를 직접 줘야 그려진다.
                    width={500}
                    height={300}

                    barLabel={'value'}          // bar에 표시될 내용.
                    borderRadius={10}           // bar 모서리 둥글기
                    grid={{horizontal:true}}    // 가로 눈금선만 표시
                />
            </div>
            <div style={{width: "30%"}}>
                <BarChart

                    // series를 3개 주면 분기마다 막대 3개가 나란히 표시된다.
                    series={[
                        {data:[4,2,3,5], label:"매출"},
                        {data:[3,1,3,4], label:"순이익"},
                        {data:[2,2,5,6], label:"방문객"}
                    ]} /* 막대 그래프 하나하나의 내용 */
                    xAxis={[
                        {data:['1분기', '2분기', '3분기', '4분기'], scaleType:'band'}
                    ]}/* x 축의 내용 */
                    width={500}
                    height={300}
                    barLabel={'value'} // bar에 표시될 내용.
                    borderRadius={10}
                    grid={{horizontal:true}}
                />
            </div>
            <div style={{width: "30%"}}>
                <BarChart series={[
                    /* stack의 이름이 같은 그래프 끼리 쌓이게 된다. */
                    // 위에 있는 다중 시리즈와의 차이는 합계도 함께 보인다는 점이다.
                    // 서로 다른 stack 이름을 주면 그룹이 나뉜다.
                    {data:[4000, 3000, 2000, 2780], label:'pv', stack:'stack1'},
                    {data:[2400, 1390, 9800, 3908], label:'uv', stack:'stack1'}
                ]}
                  xAxis={[
                      {data:['1분기', '2분기', '3분기', '4분기'], scaleType:'band'}
                  ]}
                  width={500}
                  height={300}
                />
            </div>
        </>
    )
}
```
### <src/app/line/page.jsx>
```
'use client';
import {LineChart} from "@mui/x-charts";

export default function LineChartPage(){
    return(
        <>
            <div style={{width: "50%"}}>
                <LineChart
                    series={[
                        
                        // curve: 점과 점을 잇는 방식.
                        // 'linear' - 직선
                        // 'setp' - 계단 모양 
                        // 'natural'/'monotoneX' - 부드러운 곡선
                        {data:[2, 5, 2, 8, 3, 1, 3], curve:"step"}
                    ]}
                    width={500}
                    height={300}
                    xAxis={[
                        {data:['1월','2월','3월','4월','5월','6월','7월'], scaleType:'band'}
                    ]}
                    grid={{vertical: true, horizontal: true}} // 격자 전체 표시
                />
            </div>

            <div style={{width: "50%"}}>
                <LineChart
                    series={[

                        // area:true - 선 아래를 색으로 채움.
                        // color로 선 색을 직접 지정하면 채움색도 그 색의 반투명이 된다.
                        {data:[2, 5, 2, 8, 3, 1, 3], area:true, color:'red'}
                    ]}
                    width={500}
                    height={300}
                    xAxis={[
                        {data:[1, 2, 3, 4, 5, 6, 7]}
                    ]}
                    grid={{vertical: true, horizontal: true}}
                />
            </div>

            <div style={{width: "50%"}}>
                <LineChart
                    series={[

                        // stack이 같으므로 세 영역이 위로 쌓임.
                        // highlightScope: 마우스를 올렸을 때 강조 범위 지정.
                        // 'item' - 해당 지점.
                        // 'series' - 그 시리즈 전체
                        {data:[2, 5, 2, 8, 3, 1, 3], area:true, stack:'stack1', label:'A그룹', highlightScope:{highlight:'item'}},
                        {data:[1, 2, 7, 2, 1, 8, 2], area:true, stack:'stack1', label:'B그룹', highlightScope:{highlight:'item'}},
                        {data:[6, 8, 4, 4, 8, 4, 1], area:true, stack:'stack1', label:'C그룹', highlightScope:{highlight:'item'}}
                    ]}
                    width={500}
                    height={300}
                    xAxis={[
                        {data:['1월','2월','3월','4월','5월','6월','7월'], scaleType:'band'}
                    ]}
                    grid={{vertical: true, horizontal: true}}
                    onAreaClick={(evt, data) => console.log(evt, data)}
                    onMarkClick={(evt, data) => console.log(evt, data)}
                    onLineClick={(evt, data) => console.log(evt, data)}
                />
            </div>
        </>
    )
}
```
### <src/app/pie/page.jsx>
```
'use client';
import {PieChart} from "@mui/x-charts";

export default function PieChartPage(){
    return(
      <>
        <div>
            <PieChart
                series={[
                    {data:[
                        // 파이 차트는 data가 숫자 배열이 아닌 {value, label} 객체 배열이다.
                        {value:10, label:'A영역'},
                        {value:15, label:'B영역'},
                        {value:20, label:'C영역'}
                       ],
                        innerRadius:20,     // 차트 안쪽 구멍
                        outerRadius:100,    //외각 크기(outer-inner = 보여지는 파이)
                        paddingAngle:5,     //파이 영역별 간격
                        cornerRadius:10,    //파이 모서리 둥글기 정도
                        startAngle:30,      //시작 각도
                        endAngle:390,       //종료 각도

                        // arcLabel: 각 조각에 표시할 문자열을 만드는 함수
                        // 문자열('value', 'label')로 줄 수도 있고, 함수로 직접 조합할 수도 있다.
                        arcLabel:function(item){
                            console.log(item);
                            return `${item.label} : ${item.value}`;
                        }
                    }
                ]}
                width={400}
                height={200}
            />
        </div>

          <div>
              <PieChart
                  series={[
                      {data:[
                              {value:10, label:'A영역'},
                              {value:15, label:'B영역'},
                              {value:20, label:'C영역'},
                              {value:55, label:'D영역'}
                          ],
                          arcLabel:item =>  `${item.value}%`,

                          // highlight:'item' - 올린 조각을 강조
                          // fade:'global' - 나머지 전부를 흐리게
                          highlightScope:{highlight:'item', fade:'global'},

                          // faded: 흐려진 조각들의 모양을 따로 지정한다.
                          faded:{
                                color:'gray',
                                innerRadius:50,
                                additionalRadius:-30
                          }
                      }
                  ]}
                  width={400}
                  height={200}
                  onItemClick={(evt, data)=>console.log(evt, data)

                  }
              />
          </div>
      </>
    );
}
```
### <src/app/scatter/page.jsx>
```
'use client';
import {ScatterChart} from "@mui/x-charts";

export default function ScatterPage(){
    // 한 행에 두 그룹의 좌표가 같이 들어 있는 데이터
    // (x1, y1)이 A그룹, (x2, y2)가 B그룹의 한 점이다.
    const data = [
        {x1: 329.39, y1: 443.28, x2: 391.29, y2: 153.9},
        {x1: 96.94, y1: 110.5, x2: 139.6, y2: 217.8},
        {x1: 336.35,x2: 282.34, y1: 175.23,y2: 286.32},
        {x1: 159.44,x2: 384.85,y1: 195.97,y2: 325.12},
        {x1: 188.86,x2: 182.27,y1: 351.77,y2: 144.58},
        {x1: 143.86, x2: 360.22,y1: 43.253,y2: 146.51},
        {x1: 202.02,x2: 209.5,y1: 376.34, y2: 309.69},
        {x1: 384.41,x2: 258.93,y1: 31.514,y2: 236.38},
        {x1: 256.76,x2: 70.571,y1: 231.31,y2: 440.72},
        {x1: 143.79,x2: 419.02,y1: 108.04,y2: 20.29},
        {x1: 103.48,x2: 15.886,y1: 321.77,y2: 484.17},
        {x1: 272.39,x2: 189.03,y1: 120.18,y2: 54.962},
        {x1: 23.57,x2: 456.4,y1: 366.2,y2: 418.5},
        {x1: 219.73,x2: 235.96,y1: 451.45,y2: 181.32},
        {x1: 54.99,x2: 434.5,y1: 294.8,y2: 440.9},
        {x1: 134.13,x2: 383.8,y1: 121.83,y2: 273.52},
        {x1: 12.7,x2: 270.8,y1: 287.7,y2: 346.7},
        {x1: 176.51, x2: 119.17,y1: 134.06,y2: 74.528},
        {x1: 65.05,x2: 78.93,y1: 104.5,y2: 150.9},
        {x1: 162.25,x2: 63.707,y1: 413.07,y2: 26.483},
        {x1: 68.88,x2: 150.8,y1: 74.68,y2: 333.2},
        {x1: 95.29,x2: 329.1,y1: 360.6,y2: 422.0},
        {x1: 390.62,x2: 10.01,y1: 330.72,y2: 488.06},
    ];

    return(
        <>
            <div>
                <ScatterChart
                    series={[

                        // 산점도는 {x, y} 객체 배열이 필요하다.
                        // 원본이 x1/y1, x2/y2로 섞여 있으므로 map으로 형태를 변환한다.
                        {label:'A', data:data.map((d) => ({x:d.x1, y:d.y1}))},
                        {label:'B', data:data.map((d) => ({x:d.x2, y:d.y2}))}
                    ]}
                    width={400}
                    height={300}
                />
            </div>

            <div>
                <ScatterChart
                    series={[
                        {data:data.map((d) => ({x:d.x1, y:d.y1}))}
                    ]}
                    width={400}
                    height={300}
                    xAxis={[
                        /*

                        // colorMap: 축의 값에 따라 점 색을 자동으로 정한다.
                        // piecewise: 구간을 나눠 색을 딱딱 끊어서 지정.
                        // thresholds가 3개면 구간이 4개가 되므로 colors도 4개여야 한다.
                        {colorMap:{
                            type:'piecewise',
                            thresholds:[100, 200, 300],
                            colors:['yellow', 'orange', 'red', 'blue']
                        }}
                        */
                        {colorMap:{
                            type:'continuous',
                            min:0,
                            max:350,
                            color:['yellow', 'red']
                        }}
                    ]}
                />
            </div>
        </>
    );
}
```
---
### data_flow
### <src/app/layout.jsx>
```
export default function Layout({children}){
    return(
        <html lang={"ko"}>
            <head>
                <meta charSet={"UTF-8"}/>
                <title>DATA FLOW</title>
            </head>
            <body>
                {children}
            </body>
        </html>
    );
}
```
### <src/app/page.jsx>
```
export default function App(){
    return(
        <>
            {/* props를 넘기는 문법은 HTML 속성과 같은 모양이지만, 실제로는 함수 인자를 전달하는 것이다. 
                Fisrt({item: "Third에 보내는 데이터"}) 를 호출하는 것이다.*/}
            <First item={"Third에 보내는 데이터"}/>
            <Island item={"Island에 보내는 데이터"}/>
        </>
    );
}

// 비(분배)구조 할당 안썼음.
function First(props){
    return(
        <>
            <div>
                First Componet / 거처가는 곳 1 : {props.item}
            </div>
            {/* First는 이값을 쓰지도 않고 필요하지도 않지만 Third에게 전달하려면 반드시 거쳐야 한다. */}
            <Second item={props.item}/>
        </>
    );
}

// 비(분해)구조 할당 사용
function Second({item}){
    // 위 First와 표기만 다른 동일한 동작이다.
    return(
        <>
            <div>
                Second Component / 거쳐가는 곳 2: {item}
            </div>
            <Third item={item}/>
        </>
    );
}

function Third({item}){
    // 실제로 값을 쓰는 곳.
    return(
        <>
            <div>
                Third Component / 도착지 : {item}
            </div>
        </>
    )
}

function Island(props){
    // App의 직속 자식이기 때문에 한 번에 도착한다.
    return(
        <div>
            Island Componet / 도착지 : {props.item}
        </div>
    );
}
```
#### 데이터 흐름
```
App
 ├─ First   ← "Third에 보내는 데이터"    (안 씀. 그냥 통과)
 │   └─ Second                         (안 씀. 그냥 통과)
 │       └─ Third                      (여기서 사용)
 │
 └─ Island  ← "Island에 보내는 데이터"   (바로 사용)
```
- React 데이터는 '위에서 아래로만' 흐른다.(단방향)
- 그래서 Third가 App의 값을 쓰려면 중간 컴포넌트들이 전부 이어줘야 한다.(직접 요청할 방법 없음.)
- 여기서 알 수 있는 문제 점은?
  - 중간 컴포넌트가 자기와 무관한 props를 알고 있어야 한다.
  - 값 이름을 바꾸면 경로상의 모든 파일을 고쳐야 한다.
  - 5~6 단계 깊어지면 어디서 온 값인지 추적하기 어려워진다.
  - 중간 컴포넌트를 다른 곳에 재사용하기 어려워진다.

---

### context
### <src/app/page.jsx>
```
// Context와 훅은 클라이언트에서만 동작하므로 'use client'가 필요하다.
'use client';

import {createContext, useContext} from "react";

// 공용 저장소를 만든다.
// 컴포넌트 안에 두면 랜더링마다 새 Context가 만들어져서 연결이 끊길 수 있으므로 최상단에 둔다.
// createContext('')의 인자는 '기본값'이다.
// Provider로 감싸지 않은 컴포넌트가 useContext를 호출하면 이 값을 받는다.
const DataContext = createContext(''); // 공용으로 사용할 저장소 생성.

export default function App(){
    return(
        // 공용으로 사용할 값 지정.
        // Provider : 이 안쪽 컴포넌트들은 이 값을 꺼내 쓸 수 있다는 범위 지정.
        // value에 담은 값이 아래 트리 전체에 공유.
        // data_flow에서 본 것처럼 First나 Second를 거치지 않아도 된다.
        // props로 전달되는 게 아니라, 필요한 컴포넌트가 직접 꺼내간다.
        <DataContext.Provider value={"공용으로 사용할 데이터"}>
            <First/>
            <Island/>
        </DataContext.Provider>
    );
}

// 비(분배)구조 할당 안썼음.

const First = () =>(<><div>First Componet</div><Second/></>);

const Island = () => {
    // useContext: 가장 가까운 상위 Provider의 value를 꺼내온다.
    // 몇 단계 위에 있든 상관 없이 바로 접근 가능하다.
    const data = useContext(DataContext);
    return(
        <div>Island Componet / 도착지: {data}</div>
    );
}

// 비(분해)구조 할당 사용
const Second = () => (<><div>Second Component</div><Third/></>);


const Third = () => {
    // First -> Second를 거쳐 3단계 아래인데도 한줄로 값을 꺼낸다.
    // data_flow에서 했던 것 처럼 중계하던 부분이 다 사라졌다.
    const data = useContext(DataContext);
    return(<><div>Third Component / 도착지:{data}</div></>)
};
```
### data_flow 코드와의 비교
```
[data_flow]                         [Context]

App ─ item ─→ First                 App (Provider)
              ↓ item                 ├─ First      (props 없음)
            Second                   │   └─ Second (props 없음)
              ↓ item                 │       └─ Third  ← useContext
            Third ✓                  └─ Island        ← useContext
```