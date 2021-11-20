---
title: "[Vue.js][Chapter3] Template"
last_modified_at: "2021-11-18 23:01"
categories:
    - vue.js
tags:
    - vue.js
    - javascript
    - front-end
---
## 1. 보간법(Interpolation)
### 1.1 문자열
* 데이터 바인딩의 기본형은 <strong>이중 중괄호</strong>를 사용한 택스트 보간.
    * \{\{속성명\}\}
* v-once 디렉티브를 사용하여 데이터 변경 시 업데이트 되지 않는 일회성 보간을 수행
    * v-once
    ```html
        <span v-once>다시는 변경X : { {msg} }</span>
    ```

### 1.2 원시 HTML
* 이중 중괄호는 HTML이 아닌 일반 테스트로 데이터를 해석
* 실제 HTML을 출력하기위해서는 v-html 디렉티브를 사용

    ```html
    <p>이중 중괄호사용: { {rawHtml} }</p>
    <p>Using v-html directive: <span v-html="rawHtml"></span><p>
    ```

### 1.3 JavaScript 표현식을 사용.
* VUe.js는 모든 데이터 바인딩 내에서 js표현식의 모든 기능을 지원
    ```html
        { {number+1} }
        { {ok ? 'YES' : 'NO'} }
        <div v-bind:id="'list-'+id"></div>
    ```

## 2. 디렉티브(Directives).
* v-접두사가 있는 특수속성
* 값은 단일 js표현식이 된다. (v-for는 예외).
* 표현식의 값이 변경될 떄 사이드 이펙트를 반응적으로 DOM에 적용하는 역할.
    * v-text  
    * v-bind 
    * v-else
    * v-html
    * v-show
    * v-for
    * v-once
    * v-if
    * v-cloak
    * v-model
    * v-else-if
    * v-on
    
### 2.1 v-model
* 양방향 바인딩 처리를 위해서 사용한다(form의 input, textarea등)

```html
    <div id="app">
        <input type="text" v-model="message"><br>
        입력 메세지: { { message } }
    </div>
    <script>
        new Vue({
            el: "#app",
            data(){
                return {
                    message : "Hello Vue!!",
                }
            }
        });
    </script>
```

### 2.2 v-bind

* 엘리먼트의 속성과 바인딩 처리를 위해 사용.
* v-bind는 약어로 ":"로 사용가능
* \[속성이름\] 값 그대로 사용. ex) \<div \[key\]="zxc" \> => \<div id="zxc" \>
```html
    <div id="app">
        <div :id="idValue">v-bind....</div>
        <button :[key]="idValue">눌러</button><br>
        <a v-bind:href="link1" @click="handle">구글로 가기</a>
        <a :href="link2">네이버로 가기</a>
    </div>
    <script>
        new Vue({
            el:"#app",
            data(){
                return {
                    idValue: "test-id",
                    key: "id",
                    link1: "http://google.com",
                    link2: "http://naver.com"
                };
            }
        })
    </script>
```

### 2.3 v-show

* 조건에 따라 엘리먼트를 화면에 랜더링
* style의 display를 변경
```html
    <div id="app">
       <div v-show="isShow">{ { msg } }</div>
    </div>
    <script>
        new Vue({
            el:"#app",
            data(){
                return {
                   msg: "내가 보이는가?", 
                   isShow: true,
                };
            }
        })
    </script>
```

### 2.4 v-if, v-else-if, v-else.
* 조건에 따라 엘리먼트를 화면에 렌더링
```html
    <div id="app">
        <div>나이 : <input type="text" v-model="age"></div>
        <div v-if="age < 10">요금 : 3000원</div>
        <div v-else-if="age < 20">요금 : 7000원</div>
        <div v-else-if="age < 60">요금 : 10000원</div>
        <div v-else>요금 : 무료</div>
    </div>
    <script>
        new Vue({
            el:"#app",
            data(){
                return {
                   age: 0,
                };
            }
        })
    </script>
```

### 2.5 v-show와 v-if의 차이점

| |v-if|v-show
|--|--|--
|렌더링|false일경우 x|항상 O
|false|엘리먼트 삭제|display:none 적용
|template 지원| O | X 
|v-else지원| O | X

### 2.6 v-for

* 배열이나 객체의 반복에 사용.
* v-for="element 변수명 in 배열명" v-for="(요소변수이름, 인덱스) in 배열"
* es)v-for="item in items"
* key(단순배열은 인덱스), idx, value(객체배열은 객체 반환)
* v-for는 key와 항상 함께 사용한다. (객체 불변성)

```html
<div id="app">
        <h2>범위 지정 -4</h2>
        <div v-for="i in 4">{ { i } }</div>
        <h2>객체의 값</h2>
        <ul>
            <li v-for="value in student">{ { value } }</li>
        </ul>
        <h2>객체의 속성명,값</h2>
        <ul>
            <li v-for="(value, key, idx) in student">{ {idx} }. { { key } }: { { value } }</li>
        </ul>
        <h2>배열의 값</h2>
        <ul>
            <li v-for="(value, key, idx) in regions">{ { key } }. { { value } }</li>
        </ul>
    </div>
    <script>
        new Vue({
            el:"#app",
            data(){
                return{
                    student: {
                        name: "홍길동", age:25,
                    },
                    regions: ["서울","부산","대구","인천","광주"],
                };
            },
        })
    </script>
```
### 2.7 v-for 와 v-if 
* v-for와 v-if를 같이쓰는 것은(같은 태그에 선언) 권장하지 않음.
* computed와 같이 다른곳에서 함수를 선언해서 필터링하기를 권장.(성능문제)

```html
    <div id="app">
        <h2>v-for와 v-if 같이 사용.</h2>
        <input type="number" v-model="age">
        <!-- <ul>
            같이 쓴 경우: 권장X
            <li v-for="person in persons" :key="person.name" v-if="person.age===33">
                 { { person.name } } : { { person.age } }살</li>
        </ul> -->
        <ul>
            <!-- 필터함수와같이 바깥에서 걸러내기 -->
            <li v-for="person in matchAge" :key="person.name">
                { { person.name } } : { { person.age } }살</li>
        </ul>
    </div>
    <script>
        new Vue({
            el: "#app",
            data() {
                return {
                    age: 0,
                    persons: [
                        { name: "홍길동", age: 22 },
                        { name: "김철수", age: 33 },
                        { name: "김삼성", age: 66 },
                        { name: "이사과", age: 33 },
                        { name: "박상어", age: 35 },
                    ]
                };
            },
            computed: {
                matchAge: function () {
                    return this.persons.filter(person => person.age == this.age);
                }
            },
        })
    </script>
```

### 2.8 v-cloak

* Vue Instance가 준비될 때까지 \{\{\}\} 바인딩을 숨기는데 사용
* \[v-cloak\]\{display:none\}과 같은 CSS와 같이 사용.
* Vue Instance가 준비 되면 v-cloak는 제거.

## 3. template

* 여러 태그들을 묶어서 처리해야 할 경우 사용.
* v-if, v-for, component등과 함께 많이 사용한다.
