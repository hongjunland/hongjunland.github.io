---
title: "[Vue.js][Chapter6] router"
last_modified_at: "2021-11-22 20:14"
categories:
    - vue.js
tags:
    - vue.js
    - javascript
    - front-end
---

## 1. vue-router

* 라우팅: 웹 페이지 간의 이동 방법.
* Vue.js의 공식 라우터
* 라우터는 컴포넌트와 매핑됨.
* Single Page Application 제작할때 유용.
* URL에 따라 컴포넌트를 연결하고, 설정된 컴포넌트를 보여준다.

### 1.1 설치
* CDN

    ```html
        <script src="https://unpkg.com/vue/dist/vue.js"></script>
        <script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>
    ```
*  NPM

    ```text
    npm install vue-router
    ```

* 모듈 시스템에서 사용시 Vue.use()를 통해 명시적으로 라우터를 추가해야 함.

    ```javascript
    import Vue from 'vue'
    import VueRouter from 'vue-router'

    Vue.use(VueRouter)
    ```

## 1.2 vue-router 연결

1. 컴포넌트 정의

    ```js
    const Foo = { template: '<div>foo</div>' }
    const Bar = { template: '<div>bar</div>' }
    ```

2. 라우트 정의: 각 라우트는 반드시 컴포넌트와 매핑 되어야 함.

    ```js
    const routes = [
        { path: '/foo', component: Foo },
        { path: '/bar', component: Bar }
    ]
    ```

3. routes 옵션과 함께 router 인스턴스 생성.

    ```js
    const router = new VueRouter({
        routes // `routes: routes`의 줄임
    })
    ```

4. 루트 인스턴스를 생성한뒤 mount: router와 router 옵션을 전체 App에 주입.

    ```js
    const app = new Vue({
        router
    }).$mount('#app')
    ```    

## 2. vue-router 이동 및 랜더링

* 네비게이션을 위해 router-link 컴포넌트 사용
* 속성은 'to'을 사용
* 기본적으로 \<router-link\> 는  \<a\> 태그로 랜더링

    ```html
    <router-link to="/">Home</router-link>
    <router-link to="/board">Board</router-link>
    ```

    ```html
    <!-- 현재 라우트에 맞는 컴포넌트가 렌더링 -->
    <router-view></router-view>
    ```


## 3. $router, $route

* router 설정

    ```js
    const router = new VueRouter({
        routes:[
        ...,
        {
            path: '/board/:no',
            component: BoardView,
        },
        ...
        ],
    });
    ```

* router link

    ```html
    <router-link to="/board/23">23번 게시물</router-link>
    ```

* router 정보

    ```js
    // 전체 라우터정보
    this.$router
    // 현재 호출된 해당 라우터 정보
    this.$route
    this.$route.params.no
    this.$route.path
    ```

## 4. 중첩된 라우트
* App UI는 일반적으로 여러 단계로 중첩 된 컴포넌트로 이루어짐.
* URL의 세그먼트가 중첩 된 컴포넌트의 특정 구조와 일치함.

    ```html
    <!-- 최상위 router-view => 최상위 경로와 일치하는 컴포넌트를 렌더링-->
    <div id="app">
    <router-view></router-view>
    </div>
    ```

    ```js
    // User컴포넌트의 템플릿 안에 컴포넌트 추가
    const User = {
    template: `
    <div class="user">
      <h2>User {{ $route.params.id }}</h2>
      <router-view></router-view>
    </div>
    `
    }
    ```
* 중첩 outlet에 컴포넌트를 랜더링 하기 위해서는 children을 사용해야 함.

    ```js
    const router = new VueRouter({
  routes: [
    { path: '/user/:id', component: User,
      children: [
        {
          // /user/:id/profile 과 일치 할 때
          // UserProfile은 User의 <router-view> 내에 렌더링 됩니다.
          path: 'profile',
          component: UserProfile
        },
        {
          // /user/:id/posts 과 일치 할 때
          // UserPosts가 User의 <router-view> 내에 렌더링 됩니다.
          path: 'posts',
          component: UserPosts
        }
    ]}
    ]
    })
    ```

## 5. 프로그래밍 방식 라우터
* \<router-link\>를 사용하여 선언적 네이게이션용 anchor 태그를 만드는 것 외에도 라우터의 instance method를 사용하여 프로그래밍으로 수행

    ```js
    // 리터럴 string
    router.push('home')

    // object
    router.push({ path: 'home' })

    // 이름을 가지는 라우트
    router.push({ name: 'user', params: { userId: 123 }})

    // 쿼리와 함께 사용, 결과는 /register?plan=private 입니다.
    router.push({ path: 'register', query: { plan: 'private' }})
    ```