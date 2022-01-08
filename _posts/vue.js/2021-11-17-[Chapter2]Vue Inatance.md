---
title: "[Vue][Chapter2] Vue Instance"
last_modified_at: "2021-11-17 23:45"
categories:
    - vue
tags:
    - vue
    - javascript
    - front-end
toc: true
toc_sticky: true
toc_label: "Contents"
---
## 1. Vue Instance
* 모든 Vue app은 Vue 함수로 새로운 <strong>Vue Instace</strong>를 만드는 것부터 시작한다.
* 엄격하게 MVVM패턴과 관련 없지만 Vue의 설계는 부분적으로 영감을 받아서, 종종 참조하기위한 변수를 vm을 사용한다.
```javascript
<script>
    let vm = new Vue({
        el: '#app',
        // 객체형식
        data:{
            message: 'Hello first Vue.js app!',
        }
        // 함수형식-> 주로 이것을 권장
        /*
        data() {
            return{
                message: 'Hello first Vue.js app!'
            }
        }
        */
    });
</script>
```

### 2. Vue Instance 주요 속성
* el : Vue가 적용될 요소를 지정한다. ex) '#app'
* data : Vue에서 사용되는 데이터 저장한다. 객체 또는 함수형태로 저장한다.
* template : 화면에 표시할 HTML, CSS 등의 마크업 요소를 정의한다. Vue의 데이터 및 기타 속성들도 함께 화면에 그릴수 있다.
* methods: 화면 로직 제어와 관련된 메소드를 정의한다. ex)이벤트처리, 화면동작과 관련된 로직
* created : Vue Instance가 생성되자마자 실행할 로직 정의한다. ex) 리스트조회등

### 3. 인스턴스 생명주기

#### 3.1 Vue Instance가 화면에 적용되는 과정
    1.Vue 라이브러리 로딩
        ↓
    2.인스턴스 객체 생성(옵션 속성 포함)
        ↓
    3.특정 화면 요소에 인스턴스를 붙임
        ↓ 
    4.인스턴스 내용이 화면 요소로 변환
        ↓
    5.변환된 화면 요소를 사용자가 최종 확인

* Vue Instance의 유효범위: el 속성으로 지정한 div태그 하위 요소들 ex) \<div id="app">

#### 3.2 생명주기

![vueinstancelifecycle](https://kr.vuejs.org/images/lifecycle.png)

* 크게 나누면 생성(created), 부착(mounted), 갱신(updated), 소멸(destroyed)의 4단계로 나뉜다.

|속성|설명
|---|---
|beforeCreate|Vue Instance가 생성되고 각 정보의 설정전에 호출. DOM과 같은 <strong>화면요소에 접근 불가</strong>
|created|Vue Instance가 생성된 후에 데이터들의 설정이 완료된 후 호출. Instance가 화면에 부착되기 전이므로 template속성에 정의된 DOM요소는 접근 불가. <br>ex) http 통신을 통한 서버에 데이터를 받아오는 로직을 수행하기 좋다.
|beforeMount|마운트가 시작되기 전에 호출
|mounted|지정된 element에 Vue Instance 데이터가 마운트 된 후에 호출. <br> template 속성에 정의한 화면 요소에 접근할 수 있어 화면 요소를 제어하는 로직 수행.
|beforeUpdate|256M데이터가 변경될 떄 vitural DOM이 랜더링, 패치되기 전에 호출.
|updated|Vue에서 관리되는 데이터가 변경되서, DOM이 업데이트 된 상태. 데이터 변경 후 화면 요소 제어와 관련된 로직을 추가.
|beforeDestroy|Vue Instance가 제거되기 전에 호출.
|destroyed|Vue Instance가 제거된 후에 호출.

#### 3.3 생명주기 예제

```javascript
<div id="app">
    <h3> { {message} } </h3>
</div>
<script>
    new Vue({
        el: '#app',
        data:{
            message : 'Life Cycle Hooks.'
        },
        beforeCreate() {
            console.log('before created!');
        },
        created() {
            console.log('created!');
        },
        mounted() {
            console.log('mounted!!');
        },
        updated() {
            console.log('updated!!');
        },
    })
</script>
```