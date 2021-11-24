---
title: "[Vue.js][Chapter7] vue-cli"
last_modified_at: "2021-11-23 21:14"
categories:
    - vue.js
tags:
    - vue.js
    - javascript
    - front-end
---

## 1. Vue/cli 설치

* Node.js 설치, npm설치 (설명 생략)
* https://nodejs.org/ko/

### 1.1 npm 명령어

* npm init : 새로운 프로젝트나 패키지를 만들 때 사용 (package.son이 생성)
* npm install package : 생성되는 위치에서만 사용 가능한 패키지로 설치.
* npm install -g package : 글로벌 패키지 설치.
* npm install yarn : 프로젝트생성때 필요한 패키지 매니저

## 2. @Vue/cli

* Vue.js 개발을 위한 시스템으로 공식 제공 CLI
* 개발의 편리성을 위해 필수처럼 사용.
* Vue 프로젝트를 빠르게 구성할 수 있는 스캐폴딩 제공.
* Vue와 관련된 오픈 소스들의 대부분이 CLI를 통해 구성이 가능하도록 구현되어있음.
* https://cli.vuejs.org/

### 2.1 vue/cli 생성
1. vue craate 프로젝트명

    ![vue-create-1](/assets/img/vue/vue-create-1.jpg)

2. Manually select features 선택(enter)

    ![vue-create-2](/assets/img/vue/vue-create-2.jpg)

3. Router 선택 (space bar -> enter)

    ![vue-create-3](/assets/img/vue/vue-create-3.jpg)

4. 2.x 선택

    ![vue-create-4](/assets/img/vue/vue-create-4.jpg)

5. Y 입력 또는 enter

    ![vue-create-5](/assets/img/vue/vue-create-5.jpg)

6. formatter 선택(여기서는 Prettier)

    ![vue-create-6](/assets/img/vue/vue-create-6.jpg)

7. 저장 할떄 마다 Lint 설정(1번)

    ![vue-create-7](/assets/img/vue/vue-create-7.jpg)

8. 라이브러리 저장 파일(여기서는 package.json)

    ![vue-create-8](/assets/img/vue/vue-create-8.jpg)

9. 설정 재사용 여부

    ![vue-create-9](/assets/img/vue/vue-create-9.jpg)

## 3. SFC(Single File Component)

* 확장자가 .vue 인 파일
* .vue = template + script + style
* 구문 강조 가능.
* 컴포넌트에만 CSS의 범위를 제한할 수 있다.
* 전처리기를 사용해 기능 확장 가능.

### 3.1 template

* \<template\> 태그 1개만 포함가능
* HTML
* 각 *.vue 파일은 한번에 최대 하나의 템플릿 블록을 포함 할수 있음.
* 내용은 문자열로 추출되어 컴파일 된 Vue Component의 template 옵션으로 사용.

### 3.2 script

* \<script\>
* javascript(ES6 지원가능)
* 각 *.vue 파일은 한번에 최대 하나의 스크립트 블록을 포함.

### 3.3 style

* \<style\>
* CSS
* scoped 속성을 이용하여 현재 컴포넌트에서만 사용 가능한 css를 지정가능.