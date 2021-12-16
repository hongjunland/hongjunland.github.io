---
title: "[React][Chapter2] React적인 사고"
last_modified_at: "2021-12-17 01:00"
categories:
    - react
tags:
    - javascript
    - front-end
    - react
---

## 학습 목표

* React로 앱을 설계하는 방식을 설명할 수 있다.
* React적인 사고에 대해 간단히 설명 할 수 있다.

## 목업 시작

* 디자이너에게 받은 목업

    ![start](https://ko.reactjs.org/static/1071fbcc9eed01fddc115b41e193ec11/d4770/thinking-in-react-mock.png)

* JSON

    ```text
    [
        {category: "Sporting Goods", price: "$49.99", stocked: true, name: "Football"},
        {category: "Sporting Goods", price: "$9.99", stocked: true, name: "Baseball"},
        {category: "Sporting Goods", price: "$29.99", stocked: false, name: "Basketball"},
        {category: "Electronics", price: "$99.99", stocked: true, name: "iPod Touch"},
        {category: "Electronics", price: "$399.99", stocked: false, name: "iPhone 5"},
        {category: "Electronics", price: "$199.99", stocked: true, name: "Nexus 7"}
    ];

    ```
## 1. UI를 컴포넌트 계층으로 나누기

* 처음에 할 일은 모든 컴포넌트의 주변의 박스를 그리고 이름을 붙이는 것.
* 컴포넌트는 단일 책임 원칙을 기본으로 하는 것이 이상적임. 
* UI와 데이터 모델이 정보 아키텍처(Information Architecture)를 가지는 경향이 있어서 JSON과 UI가 잘 연결 됨.
* 예시

    ![fitst](https://ko.reactjs.org/static/9381f09e609723a8bb6e4ba1a7713b46/90cbd/thinking-in-react-components.png)

    1. FilterableProductTable(노랑): 전체 포괄.
    2. SearchBar(파랑): 모든 유저의 입력을 받음.
    3. ProductTable(연두) : 유저의 입력기반으로 데이터 컬렉션을 필터링해서 보여줌.
    4. ProductCategoryRow(하늘): 각 카테고리의 헤더를 보여줌.
    5. ProductRow(빨강): 각 제품에 해당하는 행을 보여줌.

    *  ProductTable을 보면 "Name"과 "Price" 레이블을 포함한 테이블 헤더만을 가진 컴포넌트는 없음(독립된 컴포넌트를 생성할지는 선택사항)
    * 헤더가 복잡해지면(정렬기능과 같은 추가) ProductTableHeader라는 컴포넌트를 따로 만들면 됨.

* 결과

    * FilterableProductTable

        * SearchBar

        * ProductTable

            * ProductCategoryRow
            * ProductRow


