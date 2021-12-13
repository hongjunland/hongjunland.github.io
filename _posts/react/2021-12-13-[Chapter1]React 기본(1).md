---
title: "[React][Chapter1] React 기본(1)"
last_modified_at: "2021-12-13 21:17"
categories:
    - react
tags:
    - javascript
    - front-end
    - react
---

## 학습 목표

* 리액트가 무엇인지 설명할 수 있다. 
* JSX가 무엇인지 설명하고 사용할 수 있다.
* props와 state가 무엇인지 설명하고 사용 할 수 있다.
* 컴포넌트의 생명주기에 대해서 설명하고 사용할 수 있다.
* 이벤트처리에 대해서 설명하고 사용할 수 있다.

## 1. React란 무엇인가

* 사용자인터페이스를 만들기 위한 JavaScript 라이브러리.
* 페이스북(현:Meta)에서 개발.
* SPA(Single Page Application)을 전제로 함.
* 선언형 : 상호작용이 많은 UI를 만들 떄 생기는 어려움을 줄여줌.
* 컴포넌트 기반: 다양한 형식의 데이터를 앱 내에서 쉽게 전달할 수 있고, DOM과 별개로 상태를 관리.

### 1.1 개발 환경 만들기

* Node.js 설치 후 

    ```text
    // 자바스크립트
    npx create-react-app [프로젝트 이름]

    // 타입스크립트
    npx create-react-app . --template typescript
    ```
* 실행

    ```text
    npm start (또는 yarn start)
    ```

* 빌드 

    ```text
    npm run build
    ```

### 1.2 Hello World

* import 선언
    ```js
    import React from 'react'; 
    import ReactDOM from 'react-dom';  
    ```
* 예시
    ```js
    ReactDOM.render(
    <h1>Hello, world!</h1>,
    document.getElementById('root')
    );

    ```

* 출력
    ```text
    Hello, world!
    ```

## 2. JSX

### 2.1 JSX 란?

* JavaScript XML이며, JavaScript를 확장한 문법.
* React "element"를 생성.
* React는 별도의 파일에 마크업과 로직을 넣어 인위적으로 분리하는 대신에, 둘다 포함하는 컴포넌트라고 부르는 느슨히 연결된 유닛으로 관심사를 분리.
* JSX 사용이 필수가 아니지만, UI 작업시 시각적으로 도움되서 많이 사용.

### 2.2 JSX에 표현식 포함시키기

* HTML보단 JavaScript에 가까워서 React DOM은 camelCase 사용.
* JSX에 EL(표현식) 포함 - 변수

    ```js
    const name = "Josh Perez";
    const element = <h1>Hello, {name}</h1>; // 중괄호 안에 유효한 모든 js표현식을 넣을수 있음.

    ReactDOM.render(
        element,
        document.getElementById('root')
    );
    ```
    
    ```text
    Hello, Josh Perez
    ```


* JSX에 EL(표현식) 포함 - 함수

    ```js
    function formatName(user) {
        return user.firstName + ' ' + user.lastName;
    }

    const user = {
        firstName: 'Harper',
        lastName: 'Perez'
    };

    const element = (
    <h1>
        Hello, {formatName(user)}!
    </h1>
    );

    ReactDOM.render(
    element,
    document.getElementById('root')
    );
    ```

    ```text
    Hello, Harper Perez
    ```

* 컴파일이 끝날시, JSX 표현식이 정규 JavaScript 함수 호출이 되고 JavaScript 객체로 인식됨.

* if문 및 for loop 안에 사용, 변수 할당, 인자, 함수 반환 가능.

    ```js
    function getGreeting(user) {
        if (user) {
            return <h1>Hello, {formatName(user)}!</h1>;
        }
        return <h1>Hello, Stranger.</h1>;
    }

    ```
* JSX 속성 정의: 속성에 따옴표로 문자열 리터럴 정의

    ```js
    const element = <div tabIndex="0"></div>
    ```

* 중괄호를 사용해 속성에 JS표현식 삽입 가능.

    ```js
    const element = <img src={user.avatarUrl}></img>;
    ```
    -> 어트리뷰트에 js표현식을 삽일할 때 중괄호 주변에 따옴표 입력 금지.

### 2.3 JSX로 자식 정의하기

* JSX 태그는 자식을 포함 가능.
    
    ```js
    const element = (
    <div>
        <h1>Hello!</h1>
        <h2>Good to see you here.</h2>
    </div>
    );
    ```

* 주입 공격 방지(보안)

    ```js
    const title = response.potentiallyMaliciousInput;
    // 이것은 안전합니다.
    const element = <h1>{title}</h1>;
    ```

    기본적으로 React DOM은 JSX에 삽입된 모든 값을 렌더링하기 전에, 이스케이프 하기 때문에 App에 명시적으로 작성되지 않은 내용은 주입되지 않음.(XSS:cross-site-scripting 방지) 

* 객체 표현 : React.createElement() 호출

    ```js
    const element = (
    <h1 className="greeting">
        Hello, world!
    </h1>
    );

    // 동일한 결과
    const element = React.createElement(
        'h1',
        {className: 'greeting'},
        'Hello, world!'
    );
    ```

    다음과 같은 객체 생성

    ```js
    // 주의: 다음 구조는 단순화되었습니다
    const element = {
        type: 'h1',
        props: {
            className: 'greeting',
            children: 'Hello, world!'
        }
    };
    ```

## 3. props & state

## 4. 생명주기

## 5. 이벤트 처리


