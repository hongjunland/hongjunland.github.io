---
title: "[React][Chapter4]Thinking in React"
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

    ``
## Step1. UI를 컴포넌트 계층으로 나누기

* 처음에 할 일은 모든 컴포넌트의 주변의 박스를 그리고 이름을 붙이는 것.
* 컴포넌트는 단일 책임 원칙을 기본으로 하는 것이 이상적임. 
* UI와 데이터 모델이 정보 아키텍처(Information Architecture)를 가지는 경향이 있어서 JSON과 UI가 잘 연결 됨.
* 예시

    ![first](https://ko.reactjs.org/static/9381f09e609723a8bb6e4ba1a7713b46/90cbd/thinking-in-react-components.png)

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


## Step2. 정적인 버전 만들기

* 데이터 모델을 가지고 렌더링만 되는 "UI 껍데기 버전" 만드는 단계
* 다른 컴포넌트를 재사용하는 컴포넌트를 만들고 props를 이용해 데이터를 전달.
* <strong>state</strong> 사용 금지(데이터 변경이 필요 없어서)
* 컴포넌트들은 render() 만 가져야 함.
* 앱을 만드는 방향

    * 하향식(Bottom-up) : 보통 구현하기 쉬움(간단한 프로토타입)
    * 상향식(Top-down) : 프로젝트가 크고, 테스트를 작성하면서 개발하기 편함.

* 데이터 모델 변경시, props을 통해 전달 받고, ReactDOM.render()를 다시 호출해 UI 업데이트.
* 단방향 데이터흐름을 통해 모든 것을 모듈화하고 빠르게 해줌.


## Step3. UI state에 대한 최소한(완전하면서)의 표현 찾아내기

* 최소한의 state를 찾아야 하는 단계
* UI를 상호작용하게 하기 위해서는 기반 데이터 모델을 변경할 수 있는 방법이 있어야 함. (state)
* 필요로 하는 변경가능한 state의 최소 집합을 생각해 보아야 함.
* 예를 들어, 리스트의 길이를 구하고자 할때, 아이템의 개수를 표현하는 state를 별도로 만들 필요까진 없음.

### 3.1 예시 적용

* 예시 데이터

    * 제품의 원본 목록
    * 유저가 입력한 검색어
    * CheckBox의 값
    * 필터링 된 제품들의 목록

* state가 될수 없는 요소

    1. 부모로부터 props를 통해 전달이 되는가? -> False
    2. 시간이 지나도 변하지 않는가? -> False
    3. 컴포넌트 안의 다른 state나 props를 가지고 계산 가능한가? -> False

* state가 될 요소

    * 유저가 검색한 검색어
    * 체크박스의 값


## Step 4. State가 어디에 있어야 할지 찾기

* 어떤 컴포너트가 state를 변경하거나 소유할지 찾아야 할 단계.
* React는 항상 컴포넌트 계층구조를 따라 아래로 내려가는 단방향 데이터 흐름을 따라야 한다는 규칙을 유념해야함.
* 결정 요소

    * state를 기반으로 렌더링하는 모든 컴포넌트 찾기
    * 공통 소유 컴포넌트를 찾기(상위에 있는 하나의 컴포넌트)
    * 공통 혹은 더 상위에 있는 컴포넌트가 state를 가져야 함.
    * state를 소유할 적절한 컴포넌트를 찾지 못했다면, state를 소유하는 컴포넌트를 하나 만들어서 상위계층에 추가한다.

* 예시

    * 'ProductTable'은 state에 의존한 상품 리스트의 필터링해야 하고 'SearchBar'는 검색어와 체크박스의 상태를 표시해주어야 함.
    * 공통 소유 컴포넌트는 'FilterableProductTable'
    * 의미상으로도 'FilterableProductTable'이 검색어와 체크박스의 체크 여부를 가지는 것이 타당함.

    ~~~js
    class FilterableProductTable extends React.Component{
        constructor(props){
            super(props);
            this.state = {filterText: '', inStockOnly: false} 
            // 생략
        }
        // 생략
    }
    ~~~

## Step 5. 역방향 데이터 흐름 추가하기

* 하위 컴포넌트에서 상위 컴포넌트의 state를 업데이트 할 수 있게 만드는 단계
* 상위 컴포넌트는 하위 컴포넌트에 콜백을 넘겨서 state가 업데이트되어야 할 때마다 호출되도록 함.

## 최종 참조 코드

* 미리 보기 : [https://codepen.io/gaearon/pen/LzWZvb](https://codepen.io/gaearon/pen/LzWZvb)

~~~js
class ProductCategoryRow extends React.Component {
  render() {
    const category = this.props.category;
    return (
      <tr>
        <th colSpan="2">
          {category}
        </th>
      </tr>
    );
  }
}

class ProductRow extends React.Component {
  render() {
    const product = this.props.product;
    const name = product.stocked ?
      product.name :
      <span style={{color: 'red'}}>
        {product.name}
      </span>;

    return (
      <tr>
        <td>{name}</td>
        <td>{product.price}</td>
      </tr>
    );
  }
}

class ProductTable extends React.Component {
  render() {
    const filterText = this.props.filterText;
    const inStockOnly = this.props.inStockOnly;

    const rows = [];
    let lastCategory = null;

    this.props.products.forEach((product) => {
      if (product.name.indexOf(filterText) === -1) {
        return;
      }
      if (inStockOnly && !product.stocked) {
        return;
      }
      if (product.category !== lastCategory) {
        rows.push(
          <ProductCategoryRow
            category={product.category}
            key={product.category} />
        );
      }
      rows.push(
        <ProductRow
          product={product}
          key={product.name}
        />
      );
      lastCategory = product.category;
    });

    return (
      <table>
        <thead>
          <tr>
            <th>Name</th>
            <th>Price</th>
          </tr>
        </thead>
        <tbody>{rows}</tbody>
      </table>
    );
  }
}

class SearchBar extends React.Component {
  constructor(props) {
    super(props);
    this.handleFilterTextChange = this.handleFilterTextChange.bind(this);
    this.handleInStockChange = this.handleInStockChange.bind(this);
  }
  
  handleFilterTextChange(e) {
    this.props.onFilterTextChange(e.target.value);
  }
  
  handleInStockChange(e) {
    this.props.onInStockChange(e.target.checked);
  }
  
  render() {
    return (
      <form>
        <input
          type="text"
          placeholder="Search..."
          value={this.props.filterText}
          onChange={this.handleFilterTextChange}
        />
        <p>
          <input
            type="checkbox"
            checked={this.props.inStockOnly}
            onChange={this.handleInStockChange}
          />
          {' '}
          Only show products in stock
        </p>
      </form>
    );
  }
}

class FilterableProductTable extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      filterText: '',
      inStockOnly: false
    };
    
    this.handleFilterTextChange = this.handleFilterTextChange.bind(this);
    this.handleInStockChange = this.handleInStockChange.bind(this);
  }

  handleFilterTextChange(filterText) {
    this.setState({
      filterText: filterText
    });
  }
  
  handleInStockChange(inStockOnly) {
    this.setState({
      inStockOnly: inStockOnly
    })
  }

  render() {
    return (
      <div>
        <SearchBar
          filterText={this.state.filterText}
          inStockOnly={this.state.inStockOnly}
          onFilterTextChange={this.handleFilterTextChange}
          onInStockChange={this.handleInStockChange}
        />
        <ProductTable
          products={this.props.products}
          filterText={this.state.filterText}
          inStockOnly={this.state.inStockOnly}
        />
      </div>
    );
  }
}


const PRODUCTS = [
  {category: 'Sporting Goods', price: '$49.99', stocked: true, name: 'Football'},
  {category: 'Sporting Goods', price: '$9.99', stocked: true, name: 'Baseball'},
  {category: 'Sporting Goods', price: '$29.99', stocked: false, name: 'Basketball'},
  {category: 'Electronics', price: '$99.99', stocked: true, name: 'iPod Touch'},
  {category: 'Electronics', price: '$399.99', stocked: false, name: 'iPhone 5'},
  {category: 'Electronics', price: '$199.99', stocked: true, name: 'Nexus 7'}
];

ReactDOM.render(
  <FilterableProductTable products={PRODUCTS} />,
  document.getElementById('container')
);

~~~

## 출처

* https://ko.reactjs.org/docs/