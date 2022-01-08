---
title: "[Vue][Chapter4] Vue method"
last_modified_at: "2021-11-19 23:45"
categories:
    - vue
tags:
    - vue
    - javascript
    - front-end
---
## 1. Vue method

* Vue Instance는 생성과 관련된 데이터및 메소드 정의 가능.
* 메소드 안에서 데이터들을 this선택자로 접근가능 ex)this.data

```js
let vm  = new Vue({
    data:{
        count: 1,
    },
    methods:{
        fun1(){
            console.log('fun1!');
            this.count++;
        }
    }
})
vm.fun1();
console.log(cm.count);  //2
```

## 2. Vue filter

* 화면에 표시되는 텍스트 형식을 쉽게 변환해줌.
* filter를 이용해서 표현식에 새로운 결과 형식 적용.
* 중괄호 보간법 \[\{\{\}\}\] 또는 v-bind에서 사용 가능.
* '\|' 기호로 체이닝 사용.

```html
<div id="app">
        금액 : <input type="text" v-model="msg">
        전화번호 : <input type="text" v-model="msg">
        <h2>결과 : </h2>
        <h3>{ { msg|count1 }}</h3>
        <h3>{ { msg|count2('문자를 넣어보세요')}}</h3>
    </div>
    <script>
        Vue.filter("count1",(val)=>{
            if(val.length==0){
                return;
            }
            return `${val} : ${val.length}자`;
        });
        new Vue({
            el: '#app',
            data:{
                msg:"",
            },
            filters:{
                count2(val, alternative){
                    if(val.length==0){
                        return alternative;
                    }
                    return `${val} : ${val.length}자`;
                },
            }
        })
    </script>
```

## 3. Vue computed

* 특정 데이터의 변경사항을  실시간처리
* 캐싱을 이용해서 데이터의 변경이 없을시, 캐싱된 데이터 반환.
* getter/setter를 직접 지정.
* 작성은 method형태로 작성하지만, Vue에서 proxy처리해서, property처럼 사용.

### 3.1 computed와 일반 methods와 차이점

* computed
  * getter 형식으로 참조한다.
  * 캐싱해놓기 때문에 여러번 선언해도 호출을 1번한다.
  * 해당 데이터가 또 변할때 까지 중복호출하지않음.(재활용)

      -> `<p>함수명: ` "\{\{ funcEx \}\}"`</p>`
* methods
  * methods는 함수형식으로 참조한다.
  * 실행할때 마다 매번 호출

      -> `<p>함수명: ` "\{\{ funcEx() \}\}"`</p>`

## 4. Vue watch

* Vue Instance의 특정 property가 변경 될 때(computed) 실행할 콜백 함수 설정.

```html
<div id="app">
      <div>
        <input type="text" v-model="a" />   //변수명 같아야함
      </div>
    </div>
    <script>
      var vm = new Vue({
        el: '#app',
        data: {
          a: 1, //변수명 같아야함
        },
        watch: {
          a: function (val, oldVal) {   //변수명 같아야함
            console.log('new: %s, old: %s', val, oldVal);
          },
        },
      });
      console.log(vm.a);
      vm.a = 2; // => new: 2, old: 1
      console.log(vm.a);
    </script>
```

### 4.1 watch가 computed와의 차이점
* computed는 종속된 data가 변경되었을 경우 그 data를 다시 계산해서 캐싱.
* watch는 data가 변경됬을 경우 다른 data를 변경하는 작업을 한다.

## 5. Vue event
* DOM Event를 청취하기 위해 v-on 디렉티브 사용.
* Inline event handling
* method를 이용한 event handling

### 5.1 method event handler

```html
div id="app">
      <button v-on:click="greet">Greet</button>
    </div>
    <script>
      var vm = new Vue({
        el: '#app',
        data: {
          name: 'SSAFY',
        },
        methods: {
          greet: function (event) {
            alert('Hello ' + this.name + '!');
            console.dir(event.target);
          },
        },
      });

```

### 5.2 inline method handler

* 메소드 이름을 직접 바인딩 하는 대신에 인라인 js 구문에 메소드를 사용할수도있음.
* 원본 DOM 이벤트에 접근해야 하는 경우 특별한 $event 변수를 사용해 메소드에 전달할 수 있다.

```html
    <div id="app">
      <form action="http://www.naver.com">
        <button v-on:click="greet1('Myhome')">Greet</button>
        <button v-on:click="greet2($event, 'Myhome')">Greet</button>
      </form>
    </div>
    <script>
      new Vue({
        el: '#app',
        methods: {
          greet1: function (msg) {
            alert('Hello ' + msg + '!');
            console.dir(event.target);
          },
          greet2: function (e, msg) {
            if (e) e.preventDefault();
            alert('Hello ' + msg + '!');
            console.dir(e.target);
          },
        },
      });
    </script>
```
### 5.3 이벤트 수식어(Event Modifier)

* event.preventDefault()와 같이 method내에서 작업할수도 있지만, method는 DOM의 이벤트를 처리하는 것 보다 data 처리를 위한 로직만 작업하는 것을 권장.
* 이를 위해 Vue는 v-on 이벤트에 이벤트 수식어를 제공
* 수식어는 점(.)으로 표시된 접미사

```html
    <div id="app">
      <h2>페이지 이동</h2>

      <a href="test.html" @click="sendMsg1">페이지 이동 막기1</a><br />
      <a href="test.html" @click="sendMsg2">페이지 이동 막기2</a><br />
      <a href="test.html" @click.prevent="sendMsg1">페이지 이동 막기3</a><br />
    </div>
    <script>
      new Vue({
        el: '#app',
        methods: {
          sendMsg1() {
            alert('이동할까요?');
          },
          sendMsg2(e) {
            e.preventDefault();
            alert('이동막기');
          },
        },
      });
    </script>
```

### 5.4 키 수식어(Key Modifier)
* 키보드 이벤트를 청취할 때, 종종 공통 키 코드를 확인해야 함. 
* Vue는 키 이벤트를 수신할 때 v-on에 대한 키 수식어를 추가할 수 있음.
```html
<!-- only call `vm.submit()` when the `key` is `Enter` -->
<input v-on:keyup.enter="submit">
```

* key code
```text
.enter
.tab
.delete (“Delete” 와 “Backspace” 키 모두를 캡처합니다)
.esc
.space
.up
.down
.left
.right
```

## 6.ref $ref

* \$refs 속성으로 DOM에 접근 가능
* Vue의 목적은 DOM를 직접 다루지않는다는 것에 있으므로, 되도록 ref사용을 지양함.

```html
아이디 : <input type="text" v-model="id" ref="id" @keyup="idCheck" />
...
console.log(this.$refs.id.value);
```
* 위 경우에는 "this.id"로 사용하도록 하자.
