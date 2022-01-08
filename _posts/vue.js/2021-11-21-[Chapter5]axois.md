---
title: "[Vue][Chapter5] axois"
last_modified_at: "2021-11-21 22:13"
categories:
    - vue
tags:
    - vue
    - javascript
    - front-end
---

## 1. axois
* Vue 에서 권고하는 HTTP 통신 라이브러리.
* promise 기반의 HTTP 통신 라이브러리이며, 상대적으로 다른 라이브러리에 비해 문서화가 잘 되어이고 API가 다양하다.
* axois.get(URL) << Promise 객체를 return >> then, catch 사용 가능.
* https://github.com/axios/axios

    >javascript는 싱글스레드 방식이라 특정 로직의 처리를 기다려 주지를 않는다. 따라서 데이터를 동기적으로 처리할 때는 주로 Promise를 사용한다. 그리고 데이터 수신시 데이터를 화면에 표시하거나 연산을 수행하는 등 특정 로직을 수행한다.

### 1.1 axois 설치

* CDN 방식

    ```html
    <script src="https://unpkg.com/axois/dist/axois.min.js"></script>
    ```

* NPM 방식

    ```text
    npm install axios
    ```
    
*   <strong>resolve</strong>는 <strong>then</strong>, <strong>reject</strong>는 <strong>catch</strong>를 실행한다.

### 1.2 axois 적용

* 기존 방식

    ```html
    <head>
        <meta charset="UTF-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>

        <script>
            const todosURL = "https://jsonplaceholder.typicode.com/todos/1";

            const xhr = new XMLHttpRequest();

            xhr.open("GET", todosURL);
            xhr.send();

            // const response = xhr.response;
            // console.log(response);
            xhr.onload = function () {
                if (xhr.status == 200) {
                    const response = xhr.response;
                    console.log(response);
                }
            };
        </script>
    </head>
    ```

* axios

    ```html
           <script src="https://unpkg.com/axios/dist/axios.min.js"></script>
    <script>
        const todosURL = "https://jsonplaceholder.typicode.com/todos";
        const promise = axios.get(todosURL);
        console.log(promise);
        promise
            .then((response)=>{
                console.log(response.data[0].id);
                return response.data[0].id;
            })
            .then((id)=>{
                console.log(id);
                return axios.get(`${todosURL}/${id}`);
            })
            .then((todo)=>{
                console.log(todo);
            });
    </script>
    ```


## 2. axois API

* axois 대표 API

|API 유형|처리 결과
|---|---
|axois.get('주소').then().catch()|해당 주소에 GET요청을 보냄. 정상인경우 then(), 오류일 경우 catch()
|axois.post('주소').then().catch()|해당 주소에 POST요청을 보냄. 나머지는 위와 동일
|axois(\{옵션 속성\})|HTTP 요청에 대한 자세한 속성들을 직접 정의해서 전송. URL, HTTP 방식, 전송데이터 타입 etc..

