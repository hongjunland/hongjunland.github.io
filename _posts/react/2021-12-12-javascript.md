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
* let/const
* 화살표 기능
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
            return 'Hello, I\'m Student, My name is '+ this.name + ' !!';
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
                \<h1\>Hello React!!\</h1\>
            }
        }
    }

    ```
## 참조
* https://dev.to/nsebhastian/javascript-basics-before-you-learn-react-38en#es6-classes