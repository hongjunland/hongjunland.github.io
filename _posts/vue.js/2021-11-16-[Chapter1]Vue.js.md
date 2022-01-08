---
title: "[Vue][Chapter1] Vue.js"
last_modified_at: "2021-11-16 23:55"
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
## 1. Vue.js
* Vue.js는 웹 개발을 단순화하고 정리하기 위해 개발된 대중적인 자바스크립트 프론트엔드 프레임워크이다.
* Vue는 수많은 프로젝트에서 AngularJS를 사용하여 구글을 위해 작업하던 Evan You에 의해 개발되었다.
* 일반적으로 깃허브의 가장 대중적인 오픈 소스 프로젝트들에 속하게 되었으며 리액트에 이어 2번째로 대중적인 자바스크립트 프레임워크/라이브러리로 되었다.

### 1.1 Vue.js 특징
* Approachable(접근성)
* Versatile(유연성)
* Performant(고성능)
* 기본적으로 MVVM 패턴구조.

### 1.2 MVVM 패턴
    기존에는 js로 view에 해당하는 DOM에 접근하거나 수정하기위해 JQuery와 같은 것을 이용했지만, Vue.js와 같은 프레임워크는 view와 Model을 연결하고 자동으로 바인딩하므로 양방향 통신을 가능케 한다.

![mvvmimage](https://upload.wikimedia.org/wikipedia/commons/thumb/8/87/MVVMPattern.png/500px-MVVMPattern.png)
* Model+View+ViewModel
* Model: 순수 js 객체.
* View : DOM
* ViewModel : Vue의 역할. 

## 2. Vue.js 시작하기
### 2.1 생산성 향상 도구
#### 2.1.1 Vue.js devtools
* 크롬 웹스토어에 Vue.js devtools 추가
* 브라우저에서 디버깅하는데 도와주는 Tool

    <a>"https://chrome.google.com/webstore/detail/vuejs-devtools/nhdogjmejiglipccpnnnanhbledajbpd?hl=ko"</a>

#### 2.1.2 Material Theme
* 테마 변경 툴

![Material Theme](/assets/img/vue/materialIconTheme.jpg){: width="40%"}

#### 2.1.3 Live Server
* 로컬 서버 변경 자동 반영

![Live Server](/assets/img/vue/liveserver.jpg){: width="40%"}

#### 2.1.4 Prettier
* 코드 정렬도구

![Prettier](/assets/img/vue/prettier.jpg){: width="40%"}

#### 2.1.5 Bracket Pair Colorizer
* 괄호에 색상 지정

![Bracket Pair Colorizer](/assets/img/vue/brackepaircolorizer.jpg){: width="40%"}

#### 2.1.6 indent rainbow
* 들여쓰기 별 색상 지정

![indent rainbow](/assets/img/vue/indentrainbow.jpg){: width="40%"}

#### 2.1.7 Auto Rename Tag
* 시작 태그의 이름변경시 닫는 태그이름 자동완성

![Auto Rename Tag](/assets/img/vue/autorenametag.jpg){: width="40%"}

#### 2.1.8 CSS Peek
* html에서 css선택시 찾기 기능

![CSS Peek](/assets/img/vue/csspeek.jpg){: width="40%"}

#### 2.1.9 HTML CSS Support
* html에서 css 자동완성

![HTML CSS Support](/assets/img/vue/htmlcsssupport.jpg){: width="40%"}

#### 2.1.10 Vetur

* .vue파일에 코드생성, 문법 강조, 자동완성, 디버깅등.
* vue 입력 시 template, script, style 자동완성 기능.

![Vetur](/assets/img/vue/vetur.jpg){: width="40%"}

#### 2.1.11 Vue3 Snippets
* vue에 관련된 자동완성기능

![vue3snippets](/assets/img/vue/vue3snippets.jpg){: width="40%"}


### 2.2 Hello Vue.js 출력하기
* 스크립트에 추가
```html
<!-- 개발버전, 도움되는 콘솔 경고를 포함. -->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
```
또는
```html
<!-- 상용버전, 속도와 용량이 최적화됨. -->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
</head>
<body>
    <div id="app">
        <h2>{{message}}</h2><!-- == ${message} -->
    </div>
    <script>
        new Vue({
            el: "#app",
            data:{
                message: "Hello Vue!!!"
            }
        });
    </script>
</body>
</html>
```
<br>

![hellovuejs](/assets/img/vue/hellovuejs.jpg){: width="40%"}