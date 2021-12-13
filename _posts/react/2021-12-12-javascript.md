---
title: "[React] JavaScript Basic"
last_modified_at: "2021-12-12 21:10"
categories:
    - react
tags:
    - javascript
    - front-end
    - react
---

## 1. 리액트를 하기 전에 알아야 할 JS

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

## 참조
* https://dev.to/nsebhastian/javascript-basics-before-you-learn-react-38en#es6-classes