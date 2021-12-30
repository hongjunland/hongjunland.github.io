---
title: "[React] React Router"
last_modified_at: "2021-12-31 00:42"
categories:
    - react
tags:
    - javascript
    - front-end
    - react
    - router
---
## 학습목표

* React Router에 대해서 설명할 수 있다.
* v6가 다른 버전과의 차이점에 대해서 간단히 설명할 수 있다.
* 라우팅을 통한 페이지 전환을 할 수 있다.

## 1. 개요

* url 분리에 있어서 React Router를 사용하는데 필수적임

## 2. 설치

~~~js
npm install react-router-dom@6
~~~

## 3. 라우트 설정

* v6 이전 버전은 여러 경로가 애매한 URL과 일치할 때, 특정 방식으로 경로를 정렬해야 했었음.
* v6버전부터 신경쓸 필요가 없어짐.

```js
import {render} from "react-dom";
import {
    BrowserRouter,
    Routes,
    Route
} from "react-router-dom";

render(
    <BrowserRouter>
        <Routes>
            <Route path="/" element={<App />}>
            <Route index element={<Home />} />
            <Route path="teams" element={<Teams />}>
            <Route path=":teamId" element={<Team />} />
            <Route path="new" element={<NewTeamForm />} />
            <Route index element={<LeagueStandings />} />
            </Route>
        </Routes>
    </BrowserRouter>,
    document.getElementById("root")
);

```
* 기본 구성

    * BrowserRouter
        * Routes
            * Route
                * index : 초기
                * path : URL 경로
                * element : 삽입할 컴포넌트


## 4. Navigation

* <code>Link</code> 컴포넌트 또는 <code>useNavigate</code>를 사용
* Link

    ```js
    import {Link} from "react-router-dom";

    function() Home(){
        return (
            <div>
                <h1>Home</h1>
                <nav>
                    <Link to="/">Home</Link> | {" "}
                    <Link to="about">About</Link>
                </nav>
            </div>
        );
    }
    ```

* useNavigate

    ```js
    import {useNavigate} from "react-router-dom";

    function Invoices(){
        let navigate = useNavigate();
        return (
            <div>
                <NewInvoiceForm
                    onSubmit={async event => {
                        let newInvoice = await createInvoice(event.target);
                    }};
                    navigate(`/invoices/${newInvoice.id}`);
                />
            </div>
        );
    }
    ```

## 5. URL Pattern 적용

* <code>useParams()</code> 과 라우터 경로를 <code>:style</code> 구문사용

    ~~~js
    import {Routes, Route, useParams} from "react-router-dom";

    function App(){
        return (
            <Routes>
                <Route
                    path="invoices/:invoiceId"
                    element={<Invoice/>}
                />
            </Routes>
        );
    }

    function Invoice(){
        let params = useParams();
        return <h1>Invoice {params.invoiceId}</h1>;
    }
    ~~~

* 일반적인 예시

    ```js
    function Invoice(){
        let {invoiceId} = useParams();
        let invoice = useFakeFetch(`/api/invoices/${invoiceId}`);
        return invoice ? (
            <div>
                <h1>{invoice.customerName}</h1>
            </div>
        ) : (
            <Loading />
        )
        ;
    }
    ```

## 6. 중첩된 라우트

* React Router의 가장 강력한 기능
* 복잡한 레이아웃 코드를 간단하게 해줌
* 대부분의 레이아웃은 URL의 세그먼트에 연결됨
* <code>\<Outlet\></code>로 중첩영역 렌더링 요소를 간결하게 해줌

* App.js

    ~~~js
    function App() {
    return (
        <Routes>
        <Route path="invoices" element={<Invoices />}>        // /invoices
            <Route path=":invoiceId" element={<Invoice />} />   // /invoices/sent
            <Route path="sent" element={<SentInvoices />} />    // /invoices/:invoiceId
        </Route>
        </Routes>
    );
    }
    ~~~

* Invoices.js

    ```js
    import { Outlet } from "react-router-dom";
    function Invoices() {
        return (
            <div>
            <h1>Invoices</h1>
            <Outlet />
            </div>
        );
    }
    ```

* Invoice.js

    ```js
        function Invoice(){
            let {invoiceId} = useParams();
            return <h1>Invoice {invoiceId}</h1>
        }
    ```

* SentInvocies.js

    ```js
    SentInvocies(){
        return <h1>Sent Invoices</h1>;
    }
    ```
