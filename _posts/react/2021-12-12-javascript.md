---
title: "[React]JavaScript Basic"
last_modified_at: "2021-12-12 21:10"
categories:
    - react
tags:
    - javascript
    - front-end
    - react
    
toc: true
toc_sticky: true
toc_label: "Contents"
---

## 1. React 공부 하기 전, 알아야 할 JavaScript 개념

* ES6 Classes
* let & const
* 화살표 함수
* Destructuring assignment(구조 분해 할당)
* Map & Filter
* ES6 Module System

## 2. ES6 Classes

* ES6 class의 구문은 객체지향 언어(Java)와 유사. 
* constructor 메소드로 생성자 역할 수행.
* 기본형

    ```js
    //정의
    class Person{
        constructor(name){  // 생성자
            this.name = name;
        }
        hello(){
            return 'Hello, My name is '+ this.name + ' !!';
        }
    }
    ```

    ```js
    //적용
    var minsoo = new Person('Minsoo');
    minsoo.hello()  // Hello, My name is Minsoo !!
    ```

* 상속

    ```js
    class Student extends Person{
        studyMath(){
            return 'study Math .. Done.';
        }
    }
    var minsoo = new Student('Minsoo');
    minsoo.hello(); // Hello, My name is Minsoo !!
    minsoo.studyMath(); // study Math .. Done.
    ```

* 오버라이딩

    ```js
    class Student extends Person{
        studyMath(){
            return 'study Math .. Done.';
        }
        hello(){
            return "Hello, I 'm Student, My name is "+ this.name + ' !!';
        }
    }
    var minsoo = new Student('Minsoo');
    minsoo.hello(); // Hello, I 'm Student, My name is Minsoo !!
    ```

* React 사용시

    ```js
    import React, {Component} from 'react';
    class App extends Component{
        render(){
            return{
                <h1>Hello React!!</h1>
            }
        }
    }

    ```

## 3. let & const

* var 키워드는 전역변수 이므로 ES6에서 const 와 let이 탄생
* const 와 let 둘다 지역적인 scope 기능
* const는 값 변경 불가, let은 변경 가능
* React 적용시

    ```js
    import React, {Component} from 'react'

    class App extends Component{
        render(){
            const greeting = 'Welcome to React';
            return (
                <h1>{greeting}</h1>
            )
        }
    }
    ```

## 4. 화살표 함수

* ES6 부터 요즘 코드 스타일을 적용해서 가독성과 간결함을 향상.
* "function" 키워드 생략
* () 기호 뒤에 => 를 사용

    ```js
    // 일반
    const ex1Function = function(){
        //내용
    }
    // 화살표
    const ex1Function = () => {
        // 내용
    }
    ```

* parameter가 1개면 더 짧게 가능

    ```js
    const exFunction = firstName => { return firstName;}
    ```

* 인라인 함수로 선언시 return 키워드를 생략해여, 암시적 반환 가능

    ```js
    const exFunction = () => 'Hello World!' ;
    exFunction();
    ```

* React 적용시

    ```js
    const HelloWorld = (props) => {
        return <h1> {props.hello}</h1>;
    }
    ```

* ES6 + React

    ```js
    class HelloWorld extends Component{
        render(){
            return (
                <h1>{props.hello}</h1>;
            );
        }
    }
    ```

## 4. Destructuring assignment(구조 분해 할당)

* ES6에서 새로운 구문중 가장 유용한 것 중에 하나인 기능.
* object 또는 배열의 한 부분을 간단하게 복사해서 명명된 변수에 기입.

* 사용 예
    ```js
    const developer = {
        firstName: 'John',
        lastName: 'Terry',
        developer: true,
        age: 25,
    }

    // destructure developer object
    const {firstName, lastName} = developer;
    console.log(firstName);
    console.log(lastName);
    console.log(developer);
    
    ```

* 새로운 변수에 기입
    ```js
    const {firstName:name} = developer;
    console.log(name);
    ```

*  배열
    ```js
    const numbers = [1,2,3,4,5];
    const [one, two] = numbers; // one = 1, two = 2
    ```

* 부분 인덱스 생략
    ```js
    const numbers = [1,2,3,4,5];
    const [one, two, , four] = numbers; // one = 1, two = 2, four = 4
    ```

* React 적용 시

    ```js
    reactFunction = () =>{
        const {name, email} = this.state;
    }
    ```
* React 적용 시 - stateless 컴포넌트

    ```js
    const HelloWorld = (props) => {
        return <h1>{props.hello}</h1>;
    }
    ```

* 매개변수 즉각 구조화

    ```js
    const HelloWorld = ({hello}) => {
        return <h1>{hello}</h1>;
    }
    ````

* React의 useState hook 기반의 배열

    ```js
    const [user, setUser] = useState('');
    ```

## 5. Map & Filter

* map, filter 메소드들은 React App을 빌드할 때, 가장 많이 사용되는 ES5기능.

* 기본

    ```js
    const users = [
    { name: 'Nathan', age: 25 },
    { name: 'Jack', age: 30 },
    { name: 'Joe', age: 28 },
    ];
    ```

* React 적용 시

    ```js
    import React, {Component} from 'react';

    class App extends Component {
        render(){
            { name: 'Nathan', age: 25 },
            { name: 'Jack', age: 30 },
            { name: 'Joe', age: 28 },
        };

        return (
            <ul>
            {users
            .map(user=> <li>{user.name}</li>)
            }
            </ul>
            
        )
    }
    ```

* filter 적용 시

    ```js
    <ul>
        {users
        .filter(user => user.age > 26)
        .map(user => <li>{user.name}</li>)
        }
    </ul>
    ```

## ES6 모듈 시스팀

* ES6 모듈 시스템은 자바스크립트 파일을 import 와 export를 할수 있다.

* React 적용시
    ```js
    import React, {Component} from 'react';
    import logo from './logo.svg';
    import './App.css';

    class App extends Component{
        render(){
            return (
            <div className="App">
                <header className="App-header">
                    <img src={logo} className="App-logo" alt="logo" />
                    <p>
                        Edit <code>src/App.js</code> and save to reload.
                    </p>
                    <a
                        className="App-link"
                        href="https://reactjs.org"
                        target="_blank"
                        rel="noopener noreferrer"
                    >
                        Learn React
                    </a>
                </header>
            </div>
            )
        }
    }
    
    export default App;
    ```
* import 기본형

    ```js
    import React, {Component} from 'react';
    ```

* export 기본형

    ```js
    export default App;
    ```

* default export 함수

    ```js
    export default function times(x){
        return x * x;
    }
    ```

* 또는 다중 선언된 export들

    ```js
    export function times(x){
        return x * x;
    }

    export function plusTwo(number){
        return number + 2;
    }
    ```

* src/App.js 파일 import

    ```js
    import {times, plusTow} from './utils.js';
    console.log(times(2));
    console.log(plusTwo(3));
    ```

* 단일 함수 & 다수 함수 예시

    ```js
    // in util.js
    export default function times(x) {
        return x * x;
    }

    // in app.js
    import k from './util.js';

    console.log(k(4)); // returns 16
    ```

    ```js
    // in util.js
    export function times(x) {
        return x * x;
    }

    export function plusTwo(number) {
        return number + 2;
    }

    // in app.js
    import { times as multiplication, plusTwo as plus2 } from './util.js';
    ```

* React 적용 예시

    ```js
    //index.js file
    import React from 'react';
    import ReactDOM from 'react-dom';
    import './index.css';
    import App from './App';
    import * as serviceWorker from './serviceWorker';

    ReactDOM.render(<App />, document.getElementById('root'));

    serviceWorker.unregister();
    ```


## 참조
* https://dev.to/nsebhastian/javascript-basics-before-you-learn-react-38en#es6-classes