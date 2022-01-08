---
title: "[Vue][Chapter5] Vue component"
last_modified_at: "2021-11-20 22:45"
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
* Vue는 <strong>단방향 통신</strong>을 지향한다.

![componentdataproblem](https://kr.vuejs.org/images/props-events.png)

## 5. Props로 데이터 전달하기
* 하위 컴포넌트는 상위 컴포넌트의 값을 직접 참조할 수 없음.
* data와 마찬가지로 props 속성의 값을 template에서 사용 가능.

```js
Vue.component('child', {
  // props 정의
  props: ['message'],
  // 데이터와 마찬가지로 prop은 템플릿 내부에서 사용할 수 있으며
  // vm의 this.message로 사용할 수 있습니다.
  template: '<span>{ { message } }</span>'
});
new Vue({
  el:'#app',
  // 이 부분은 상위라서 하위 컴포넌트에서 데이터 참조 불가
  // data{
  //   return{
  //     message: 'parent message!'
  //   }
  // }
})
```

```html
<child message="안녕하세요!"></child>
```

## 6. 랜더링 과정

1. 부모 컴포넌트인 Vue Instance 하나생성.
2. Vue.component()로 자식 컴포넌트 child-component 생성.
3. \<div id ="app" \> 내부에 \<child-component\> 가 있기 때문에, 자식 컴포넌트가 된다.<br> 처음 생성한 인스턴스 객체가 #app 요소를 가지기 때문에 부모와 자식관계가 성립된다.
4. 자식 컴포넌트에 props속성을 정의한다. ['propsdata']
5. html에 컴포넌트 태그 (child-component)를 추가한다.
6. 자식 컴포넌트에 v-bind 속성을 사용하면, 부모 컴포넌트의 data의 key에 접근이 가능.(message)
7. 부모 컴포넌트의 message 속성 값인 String 값이 자식 컴포넌트의 propsdata로 전달된다.
8. 자식 컴포넌트의 template 속성에 정의된 \<span\>\{\{propsdata\}\}\<span\>에게 전달된다.

## 7. 동적 props
* v-bind로 부모의 데이터에 props를 동적으로 바인딩.
* 데이터가 상위에서 업데이터 될 때마다 하위 데이터로도 전달 됨.

```html
<div>
  <input v-model="parentMsg">
  <br>
  <child v-bind:my-message="parentMsg"></child>
</div>
<!-- inline 형 -->
<child :my-message="parentMsg"></child>
```

## 8. 객체의 속성 전달

## 9. 사용자 정의 이벤트(Custom Events)

* 이벤트 이름
  * 컴포넌트 및 props와 달리, 이벤트는 자동 대소문자 변환을 제공하지 않는다.
  * 대소문자를 혼용하는 대신, emit 할 정확한 이벤트 이름을 작성하는 것을 권장.
  * v-on은 항상 소문자로 변환됨 


```html
// 이벤트 발생
  <!-- vm.$emit("이벤트 명", [... 파라미터]); -->
    ex) vm.$emit("speed",100);
  
// 이벤트 수신
  <!-- vm.$on("이벤트명", 콜백함수(){}); -->
    ex) <child v-on:이벤트 명="상위 컴포넌트 메소드명"></child>
```

## 10. 비 상하위간 통신.
* 비어있는 Vue Instance 객체를 Event Bus로 사용
* 복잡해질 경우 상태관리 라이브러리인 Vuex 사용 권장.

```js
var bus = new Vue();
// 컴포넌트 A
bus.$emit('id-selected',1);

// 컴포넌트 B
bus.$on('id-selected', function(id){});
```