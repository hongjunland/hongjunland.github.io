---
title: "[Vue][Chapter4] Vue method(2)"
last_modified_at: "2021-11-20 00:45"
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

## 1. class binding 
* element의 class 와 style 변경
* v-bind:class는 조건에 따른 class를 적용.
```html
    <style>
      .active {
        background: red;
        color: white;
      }

      div {
        width: 200px;
        height: 150;
        border: 1px solid #111;
      }
    </style>
  </head>
  <body>
    <div id="app">
      <div v-bind:class="{ active: isActive }">VueCSS적용</div>
      <button v-on:click="toggle">VueCSS</button>
    </div>
    <script>
      new Vue({
        el: '#app',
        data: {
          isActive: false,
        },
        methods: {
          toggle: function (todo) {
            this.isActive = !this.isActive;
          },
        },
      });
    </script>
```

## 2. 폼입력 바인딩(Form Input Bindings)
* v-model 를 사용해서 폼 input과 textarea에 양방향 데이터 바인딩 사용
    * text 와 textarea태그는 value 속성과 input 이벤트 사용.
    * checkbox, radiobutton들은 checked속성과 change이벤트 사용.
    * select 캐그는 value를 prop으로, change를 이벤트로 사용.


* input:text와 textarea

```html
<input v-model="message" placeholder="여기를 수정해보세요">
<p>메시지: { { message } }</p>

<span>여러 줄을 가지는 메시지:</span>
<p style="white-space: pre-line">{ { message } }</p>
<br>
<textarea v-model="message" placeholder="여러줄을 입력해보세요"></textarea>

```

* 체크박스 

```html
<!-- 단일은 boolean형 -->
<input type="checkbox" id="checkbox" v-model="checked">
<label for="checkbox">{ { checked } }</label>
<!-- 다수는 같은 배열끼리 바인딩 -->
<div id='example-3'>
  <input type="checkbox" id="jack" value="Jack" v-model="checkedNames">
  <label for="jack">Jack</label>
  <input type="checkbox" id="john" value="John" v-model="checkedNames">
  <label for="john">John</label>
  <input type="checkbox" id="mike" value="Mike" v-model="checkedNames">
  <label for="mike">Mike</label>
  <br>
  <span>체크한 이름: {{ checkedNames }}</span>
</div>
new Vue({
  el: '#example-3',
  data: {
    checkedNames: []
  }
})
```

* 라디오 버튼

```html
<input type="radio" id="one" value="One" v-model="picked">
<label for="one">One</label>
<br>
<input type="radio" id="two" value="Two" v-model="picked">
<label for="two">Two</label>
<br>
<span>선택: { { picked } }</span>
```

* 셀렉트

```html
<!-- 단일 -->
<select v-model="selected">
  <option disabled value="">Please select one</option>
  <option>A</option>
  <option>B</option>
  <option>C</option>
</select>
<span>선택함: { { selected } }</span>
new Vue({
  el: '...',
  data: {
    selected: ''
  }
})
<!-- 다중 -->
<select v-model="selected" multiple>
  <option>A</option>
  <option>B</option>
  <option>C</option>
</select>
<br>
<span>Selected: { { selected } }</span>
```

* from-수식어(Modifiers).
  * .lazy
    * change 이벤트 이후에 동기화
  * .number
    * 사용자 입력이 자동으로 숫자로 형 변환되기를 원할때 사용
  * .trim
    * v-model이 관리하는 input을 자동으로 trim하기 원할떄 사용.