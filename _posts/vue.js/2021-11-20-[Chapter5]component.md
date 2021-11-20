---
title: "[Vue.js][Chapter5] Vue component"
last_modified_at: "2021-11-20 22:45"
categories:
    - vue.js
tags:
    - vue.js
    - javascript
    - front-end
---

## 1. 컴포넌트

* 가장 강력한 기능중 하나.
* 기본 HTML엘리먼트를 확장해여 재사용 가능한 코드를 캡슐화.
* Vue Component는 Vue Instance이기도 하기 때문에 모든 옵션 객체 사용.
* Life Cycle 훅 사용가능.
* 전역 컴포넌트와 지역 컴포넌트.
* 주의: 컴포넌트는 항상 data가 <strong>함수</strong>형식이어야함.

## 2. 전역 컴포넌트 등록
* 전역 컴포넌트를 등록하려면 Vue.component(tagName, options)를 사용.
* 컴포넌트 명명규칙: 소문자 마다 하이픈(-) ex) MyClassName => my-class-name

```html
<div id="example">
  <my-component></my-component>
</div>
```

```js
// 등록
Vue.component('my-component', {
  template: '<div>사용자 정의 컴포넌트 입니다!</div>'
})

// 루트 인스턴스 생성
new Vue({
  el: '#example'
})
```

## 3. 지역 컴포넌트 등록
* 컴포넌트를 components 인스턴스 옵션으로 등록함으로써 다른 인스턴스/컴포넌트의 범위에서만 사용할 수 있는 컴포넌트를 만들 수 있음.

```js
var Child = {
  template: '<div>사용자 정의 컴포넌트 입니다!</div>'
}

new Vue({
  // ...
  components: {
    // <my-component> 는 상위 템플릿에서만 사용할 수 있습니다.
    'my-component': Child
  }
})
```

## 4. Component 작성 관련
* 컴포넌트는 부모-자식 관계에서 가장 일반적으로 함께 사용하기 위한 것. 
* 명확하게 정의된 인터페이스를 통해 가능한한 분리된 상태로 유지해야 관리가 편해짐.
* Vue.js에서 부모-자식 컴포넌트 관계는 props는 아래로, events 는 위로.
* 부모는 props를 통해 자식에게 데이터를 전달(Pass Props)하고, 자식은 events를 통해 부모에게 메시지를 전송(Emit Event).
![componentdataproblem](https://kr.vuejs.org/images/props-events.png)