---
title: "[React][Chapter2] React 기본(2)"
last_modified_at: "2021-12-15 16:20"
categories:
    - react
tags:
    - javascript
    - front-end
    - react
---

## 학습 목표

* 조건부 렌더링이 무엇인지 설명할 수 있다.
* 리액트에서 사용하는 배열을 어떻게 다루는지 설명할 수 있다.
* 폼태그를 리액트형식으로 사용할 수 있다.
* State 끌어올리기에 대해 설명할수 있다.
* React에서 사용하는 합성과 상속에 대해 구분과 사용법에 대해 설명 할 수 있다.

## 1. 조건부 렌더링

* JavaScript에서의 조건 처리(if & 조건연산자)와 똑같이 동작
* 조건을 현 상태를 나타내는 엘리먼트를 만드는데에 사용.

### 1.1 기본 예시(권한제어)

* 컴포넌트 정의

    ```js
    // 회원용
    function UserGreeting(props) {
        return <h1>Welcome back!</h1>;
    }
    // 게스트용
    function GuestGreeting(props) {
        return <h1>Please sign up.</h1>;
    }
    // 부모 컴포넌트
    function Greeting(props) {
    // 로그인 상태변수
    const isLoggedIn = props.isLoggedIn;
        // true: UserGreeting / false : GuestGreeting
        return (isLoggedIn) ? <UserGreeting /> : <GuestGreeting />;
    }
    
    ReactDOM.render(
        // Try changing to isLoggedIn={true}:
        <Greeting isLoggedIn={false} />,
        document.getElementById('root')
    );
    ```

### 1.2 엘리먼트 변수

* element를 저장하기 위해 변수를 사용할 수 있음.
* 출력의 다른 부분은 변하지 않은 채로 컴포넌트의 일부를 조건부 렌더링.

* 예시(로그아웃 & 로그인)

    ~~~js
    // 로그인 버튼
    function LoginButton(props){
        return(
            <button onClick={props.onClick}>
            Login
            </button>
        );
    }

    // 로그아웃 버튼
    function LogoutButton(props){
        return(
            <button onClick={props.onClick}>
             Logout
            </button>
        );
    }
    ~~~

    ~~~js
    //로그인 제어
    class LoginControl extends React.Component{
        constructor(props){
            super(props);
            this.handleLoginClick = this.handleLoginClick.bind(this);
            this.handleLogoutClick = this.handleLogoutClick.bind(this);
            this.state = {isLoggedIn : false};
        }
        
        handleLoginClick(){
            this.setState({isLoggedIn : true})
        }

        handleLogoutClick(){
            this.setState({isLoggedIn : false});
        }

        render(){
            const isLoggedIn = this.state.isLoggedIn;
            let button = (isLoggedIn) ? <LogoutButton onClick={this.handleLogoutClick}/> : <LoginButton onClick={this.handleLoginClick}/>;
            return(
                <div>
                <Greeting isLoggedIn={isLoggedIn} />
                {button}
                </div>
            );
        }
    }
    ReactDOM.render(
        <LoginControl />,
        document.getElementById('root')
    );
    ~~~

### 1.3 '&&' 연산자로 If문 인라인 표현

* 중괄호를 이용해서 '&&' 연산자를 사용할 표현식을 포함 할 수 있음.
* \{True && expression\} => 항상 expression 의 상태
* \{False && expression\} => 항상 false

~~~js
function Mailbox(props) {
  const unreadMessages = props.unreadMessages;
  return (
    <div>
      <h1>Hello!</h1>
      {unreadMessages.length > 0 &&
        <h2>
          You have {unreadMessages.length} unread messages.
        </h2>
      }
    </div>
  );
}

const messages = ['React', 'Re: React', 'Re:Re: React'];
ReactDOM.render(
  <Mailbox unreadMessages={messages} />,
  document.getElementById('root')
);
~~~

* falsy 표현식을 반환시 뒤에 선언한 표현식은 건너뛰지만, falsy 표현식이 반환.

    ~~~js
    // <div>0</div> 이 반환됨.
    render() {
    const count = 0;    //  falsy
    return (
        <div>
        { count && <h1>Messages: {count}</h1>}
        </div>
    );
    }
    ~~~

* falsy 표현식 : false 같은 것; 즉, 아래와 같이 조건부가 기본적으로 false의 조건을 갖는 것이다.

    ~~~js
    // data에 해당 값들을 넣으면
    // if(data) => data = false 가 되는 것들
    console.log(!3);
    console.log(!'hello');
    console.log(!['array?']);
    console.log(![]);
    console.log(!{ value: 1 });
    ~~~

### 1.4 컴포넌트가 렌더링하는 것을 막기.

* 다른 컴포넌트에 의해 렌더링될 때, 컴포넌트 자체를 숨기고자 랜더링 결과대신 null을 반환.
* 예시

    ```js
    function WarningBanner(props){
        if(!props.warn){
            // 숨김
            return null;
        }
        return (
            // 표현
                <div className="warning">
                    Warning!
                </div>
        );
    }

    class Page extends React.Component{
        constructor(props){
            super(props);
            this.state = {showWarning : true};
            this.handleToggleClick = this.handleToggleClick.bind(this);
        }

        handleToggleClick(){
            this.setState(state=> ({
                showWarning: !state.showWarning
            }));
        }

        render(){
            return(
                <div>
                    <WarningBanner warn={this.state.showWarning} />
                    <button onClick={this.handleToggleClick}>
                    {this.state.showWarning ? 'Hide' : 'Show'}
                    </button>
                </div>

            );
        }
    }

    ReactDOM.render(
        <Page />,
        document.getElementById('root')
    );
    ```

## 2. 리스트와 Key

### 2.1 JavaScript

* map()을 이용한 각 원소 값을 2배로 만들기.

    ~~~js
    const numbers = [1, 2, 3, 4, 5];
    const doubled = numbers.map((number) => number * 2);
    console.log(doubled);
    ~~~

### 2.2 React

* React에서 배열을 엘리면트 리스트로 만드는 방식은 JavaScript와 거의 동일.

* 여러개의 컴포넌트 렌더링

    ~~~js
    const numbers = [1,2,3,4,5];
    const listItems = numbers.map((numbers)=>
        <li>{numbers}</li>
    );

    ReactDOM.render(
        <ol>{listItems}</ol>,
        document.getElementById('root')
    );
    ~~~

    ~~~js
    //출력
    1.1
    2.2
    3.3
    4.4
    5.5
    ~~~

### 2.3 Key

* 2.2의 소스코드대로 출력하면 key가 필요하다는 경고 메시지 발생.
* React가 어떤 항목을 업데이트 하는데 <strong>Key</strong>를 통해 식별.
* key를 배열 내부의 엘리먼트에 지정.
* 숫자배열

    ~~~js
    const numbers = [1,2,3,4,5];
    const listItems = numbers.map((number)=>
        <li key={number.toString()}>
            {number}
        </li>
    );
    ~~~


* 데이터의 key 적용(String)

    ~~~js
    const todoItems = todos.map((todo)=>{
        <li key={todo.id}>
            {todo.text}
        </li>
    });
    ~~~

* 배열의 INDEX(권장 하지 않음)

    ~~~js
    const todoItems = todos.map((todo, index)=>
        <li key={index}>
            {todo.text}
        </li>
    )
    ~~~

* 잘못된 예시 - Key는 주변 배열의 context에서만 의미가 있음.

    ~~~js
    function ListItem(props) {
    const value = props.value;
    return (
        // 틀렸습니다! 여기에는 key를 지정할 필요가 없습니다!
        <li key={value.toString()}>
        {value}
        </li>
    );
    // collect case
    // return <li>{props.value}</li>;
    }

    function NumberList(props) {
    const numbers = props.numbers;
    const listItems = numbers.map((number) =>
        // 틀렸습니다! 여기에 key를 지정해야 합니다.
        // collect case
        // <ListItem key={number.toString()} value={number} />
        <ListItem value={number} />

    );
    return (
        <ul>
        {listItems}
        </ul>
    );
    }

    const numbers = [1, 2, 3, 4, 5];
    ReactDOM.render(
    <NumberList numbers={numbers} />,
    document.getElementById('root')
    );
    ~~~

* Key는 배열 안에서 형제 사이에서만 고유한 값이어야 함.

    ~~~js
    // 동일한 key(post.id) 사용가능
    const sidebar = (
        <ul>
        {props.posts.map((post) =>
            <li key={post.id}>
            {post.title}
            </li>
        )}
        </ul>
    );

    const content = props.posts.map((post) =>
        <div key={post.id}>
        <h3>{post.title}</h3>
        <p>{post.content}</p>
        </div>
    );

    const posts = [
    {id: 1, title: 'Hello World', content: 'Welcome to learning React!'},
    {id: 2, title: 'Installation', content: 'You can install React from npm.'}
    ];
    ~~~

* key는 힌트를 제공하지만, 컴포넌트로 전달하지 않음.

    ~~~js
    // 컴포넌트에서 key와 동일한 값이 필요하면 다른 이름의 prop으로 명시적으로 전달함.
    // Post 컴포넌트는 props.id는 읽을수 있지만, props.key는 읽을 수 없음.
    const content = posts.map((post)=>
        <Post
            key={post.id}
            id={post.id}
            title={post.title}
        />
    );
    ~~~

* JSX에 map() 포함시키기
    ~~~js
    function NumberList(props){

        const numbers = props.numbers;
        const listItems = numbers.map((number)=>
            <ListItem key={numbers.toString()}
                value={number} />
        );

        return (
            <ul>
                {listItems}
                /* 인라인 처리
                    <ListItem key={number.toString()}
                        value={number}
                    />
                */
                // 인라인 방식을 남용하기 보단, 컴포넌트로 추출하는 것을 권장.
            </ul>
        );
    }
    ~~~

## 3. 폼(form) 엘리먼트

* HTML 에서의 form 은 자체가 내부 상태를 가지므로, React의 다른 DOM 엘리먼트와 다르게 동작함.
* 앞서 나올 \<input\>, \<textarea\>, \<selelct\> 태그들이 모두 비슷하게 동작하고, 구현하는데 value 속성을 허용.
* 순수 HTML

    ~~~js
    <form>
        <label>
        Name:
            // name을 입력받음
            <input type="text" name="name"/>
        </label>
        <input type="submit" value="Submit"/>
    </form>
    ~~~

* React에서 동일하게 사용 가능

### 3.1 제어 컴포넌트(Controlled Component)

* 대부분 JavaScript 함수로 폼의 submit을 처리하고 사용자가 폼에 입력한 데이터에 접근하도록 하는 것이 편리해서 이를 위한 표준 방식의 기술을 이용.
* HTML에서 \<input\> , \<textarea\>, \<select\> 와 같은 폼 엘리먼트는 일반적으로 사용자의 입력을 기반으로 자신의 state를 관리하고 업데이트 함.
* React에서는 변경할 수 있는 state가 일반적으로 컴포넌트의 state 속성에 유지되며, setState()에 의해 업데이트 됨.
* React state를 신뢰 가능한 단일 출처(Single sourece of truth)로 만들어서 두 개의 요소를 결합할 수 있음.
* 폼을 렌더링 하는 React 컴포넌트는 폼에 발생하는 사용자 입력값을 제어.
* 이러한 방식으로 React에 의해 값이 제어되는 input form 엘리먼트를 <strong>제어 컴포넌트</strong>라고 칭함.
* 예시

    ~~~js
    // value 속성은 폼 엘리먼트에 설정되므로 표시되는 값은 항상 this.state.value가 되고 React state는 신뢰가능한 단일출저가 된다.
    // React state를 업데이트하기 위해, 모든 키 입력에서 handleChange가 동작하기 때문에 사용자가 입력할 때 보여지는 값이 업데이트 된다.
    class NameForm extends React.Component{
        constructor(props){
            super(props);
            this.state={value: ''};
            
            this.handleChange = this.handleChange.bind(this);
            this.handleSubmit = this.handleSubmit.bind(this);
        }
        handleChange(event){
            // 동작마다 React state 업데이트
            this.setState({
                value: event.target.value
            })
        }
        handleSubmit(event){
            // submit event
            alert("He's name is " + this.state.value);
            event.preventDefault();
        }
        render(){
            return (
                <form onSubmit={this.handleSubmit}>
                    <label>
                        Name:
                            <input type="text" value={this.state.value} onChange={this.handleChange}/>
                    </label>
                    <input type="submit" value="Submit"/>
                </form>
            );
        }
    }

    ReactDOM.render(<NameForm/>, document.getElementById('root'));
    ~~~


### 3.2 textarea 태그

* HTML 에서 textarea 엘리먼트는 텍스트를 자식으로 정의함.
* React에서는 value 속성을 대신 사용.
* 예시

        ~~~js
        // 이전 내용과 같음
        render() {
            return (
                <form onSubmit={this.handleSubmit}>
                    <label>
                    Essay:
                    <textarea value={this.state.value} onChange={this.handleChange} />
                    </label>
                    <input type="submit" value="Submit" />
                </form>
            );
        }
        ~~~


### 3.3 select 태그

* HTML 에서는 select는 드롭다운 목록
* React에서는 selected 속성을 사용하는 대신, 최상단 select태그에 value 속성을 사용.
* 한 곳에서 업데이트만 하면 되기 때문에, 제어 컴포넌트에서 사용하기 편리함.
* 예시

    ~~~js
    class FlavorForm extends React.Component{
        constructor(props){
            // selected 정의
            this.state = {value="coconut"}
            // 생략
        }
        // 이벤트정의 생략
        render(){
            return(
                <form onSubmit={this.handleSubmit}>
                    <label>
                    Pick your favorite flavor:
                        <select value={this.state.value} onChange={this.handleChange}>
                            <option value="grapefruit">Grapefruit</option>
                            <option value="lime">Lime</option>
                            // 기존 HTML과 다르게 selected 속성을 여기에 정의하지 않음.
                            <option value="coconut">Coconut</option>
                            <option value="mango">Mango</option>
                        </select>
                    </label>
                    <input type="submit" value="Submit" />
            </form>
            );
        }
    }
    ~~~

* selelct 태그에 multiple 옵션을 허용한다면, value 속성에 배열을 전달할 수 있음.

    ~~~js
    <select multiple={true} value={['B', 'C']}>
    ~~~

### 3.4 file input 태그

* HTML에서 \<input type="file"\> 는 사용자가 하나이상의 파일을 자신의 디바이스 저장소에서 서버로 업로드하거나 File API를 통해 JavaScript로 조작할 수 있음.
* 값이 Read-Only 이여서, React에서는 <strong>비제어</strong> 컴포넌트임.

### 3.5 다중 입력 제어

* 다중 input 엘리먼트를 제어해야 할때, 각 엘리먼트에 name 속성을 추가하고 event.target.name 값을 통해 핸들러가 어떤 작업을 할지 선택할 수 있게 해줌.
* 예시

    ~~~js
    class Reservation extends React.Component {
        constructor(props) {
            super(props);
            this.state = {
            isGoing: true,
            numberOfGuests: 2
            };

            this.handleInputChange = this.handleInputChange.bind(this);
        }

        handleInputChange(event) {
            const target = event.target;
            const value = target.type === 'checkbox' ? target.checked : target.value;
            const name = target.name;

            // input 태그의 name에 일치하는 state를 업데이트 하기위해서, ES6 computed propety name 구문 사용
            // [태그이름 변수] : 해당 값
            this.setState({
            [name]: value
            });
        }

        render() {
            return (
            <form>
                <label>
                Is going:
                /*
                    속성 정의 형식
                    name=변수 이름
                    type=event type
                    value(checked)={변수 이름}
                    onChange={this.이벤트 함수명}
                */
                <input
                    name="isGoing"
                    type="checkbox"
                    checked={this.state.isGoing}
                    onChange={this.handleInputChange} />
                </label>
                <br />
                <label>
                Number of guests:
                <input
                    name="numberOfGuests"
                    type="number"
                    value={this.state.numberOfGuests}
                    onChange={this.handleInputChange} />
                </label>
            </form>
            );
        }
    }
    ~~~

### 3.6 제어되는 Input Null 값

* 제어 컴포넌트에 value prop을 지정하면, 의도 하지 않는 한 사용자가 변경할 수 없다. -> 수정 가능하면 실수로 value가 falsy(null, undefined) 하다는 것
* 나쁜 예

    ~~~js
    ReactDOM.render(<input value="hi" />, mountNode);

    setTimeout(function() {
    ReactDOM.render(<input value={null} />, mountNode);
    }, 1000);
    ~~~

### 3.7 제어 컴포넌트의 대안

* 데이터를 변경할 수 있는 모든 방법에 대해 이벤트 핸들러를 작성한다는 것은 매우 지루하고 힘든 작업임.
* 기존의 코드베이스를 React로 변경하고자 할 떄나 다른 것과 통합할 때 입력 폼을 구현하기 위한 대체기술인 <strong>비제어 컴포넌트</strong>가 있음.
* 유효성 검사, 방문한 필드 추적 및 폼 제출 처리와 같은 완벽한 해결을 원한다면, <a href="https://formik.org/">Formik</a>이 가장 대중적임.
* Formik은 제어 컴포넌트 및 state 관리에 기초하기 때문에, 기본기를 탄탄히 해야 함.

## 4. State 끌어올리기(Lifting State Up)

### 4.1 기본 개념

* 동일한 데이터에 대한 변경사항을 여러 컴포넌트에 반영해야 할 필요가 있음.
* 가장 가까운 공통 부모로 state를 끌어올리는 방법이 있음.
* 기본 예시

    ~~~js
    function BoilingVerdict(props){
        if(props.celsius >= 100) return <p>물이 끓습니다. </p>;
        return <p>물이 끓지 않았습니다.</p>
    }
    ~~~

    ~~~js
    class Calculator extends React.Component {
    constructor(props) {
        super(props);
        this.handleChange = this.handleChange.bind(this);
        this.state = {temperature: ''};
    }

    handleChange(e) {
        this.setState({temperature: e.target.value});
    }

    render() {
            const temperature = this.state.temperature;
            return (
            <fieldset>
                <legend>Enter temperature in Celsius:</legend>
                <input
                value={temperature}
                onChange={this.handleChange} />
                <BoilingVerdict
                celsius={parseFloat(temperature)} />
            </fieldset>
            );
        }
    }
    ~~~

* 두 번째 Input에 추가하기

    ~~~js
    //Calulator에서 TemperatureInput 컴포넌트를 빼냄.
    const scaleNames = {
    c: 'Celsius',
    f: 'Fahrenheit'
    };

    class TemperatureInput extends React.Component {
    constructor(props) {
        super(props);
        this.handleChange = this.handleChange.bind(this);
        this.state = {temperature: ''};
    }

    handleChange(e) {
        this.setState({temperature: e.target.value});
    }

    render() {
        const temperature = this.state.temperature;
        const scale = this.props.scale;
        return (
        <fieldset>
            <legend>Enter temperature in {scaleNames[scale]}:</legend>
            <input value={temperature}
                onChange={this.handleChange} />
        </fieldset>
        );
    }
    }

    // 분리된 두 개의 온도 입력 필드를 렌더링 해줌
    class Calculator extends React.Component {
    render() {
        return (
        <div>
            <TemperatureInput scale="c" />
            <TemperatureInput scale="f" />
        </div>
        );
    }
    }
    ~~~

### 4.2 변환 함수 작성

* 섭씨-화씨 변환함수
    ~~~js
    function toCelsius(fahrenheit) {
    return (fahrenheit - 32) * 5 / 9;
    }

    function toFahrenheit(celsius) {
    return (celsius * 9 / 5) + 32;
    }
    ~~~

* 올바르지 않은 temperature 값에 대한 방지

    ~~~js
    function tryConvert(temperature, convert) {
    const input = parseFloat(temperature);
    if (Number.isNaN(input)) {
        return '';
    }
    const output = convert(input);
    const rounded = Math.round(output * 1000) / 1000;
    return rounded.toString();
    }
    // tryConvert('abc', toCelsius) => 빈 문자열
    // tryConvert('10.22', toFahrenheit) =>  '50.396'
    ~~~

### 4.3 State 끌어올리기

* 두 가지 이상의 입력값이 서로의 것과 동기화된 상태를 유지해야함.
* 상위 컴포넌트가 공유될 state를 소유하고 있으면, 각 input 필드의 값에 대한 "진리의 원천(source of truth)"로 인한 동기화.
* 하위 컴포넌트에서 해당 변수{this.state.변수}를 {this.props.변수}로 대체 

    ~~~js
    render() {
    // Before: const temperature = this.state.temperature;
    // props은 Read-Only
    const temperature = this.props.temperature;
    // ...
    ~~~
* 해당 변수가 부모 props로 전달되므로, 하위 컴포넌트는 제어할 능력이 없음.
* React에서는 보통 이문제를 컴포넌트를 "제어" 가능하게 만드는 방식으로 해결.
* DOM \<input\>이 value와 onChange prop를 건네받는 것과 비슷한 방식으로, 사용자 정의된 하위 컴포넌트 역시 해당 변수(temperature)와 이벤트 함수 props를 자신의 부모 컴포넌트에 건네 받을수 있음.
    ~~~js
    handleChange(e) {
        // Before: this.setState({temperature: e.target.value});
        this.props.onTemperatureChange(e.target.value);
        // ...
    ~~~

* 지역 state를 제거하고, 공유한 결과
    ~~~js
    class TemperatureInput extends React.Component {
        // 지역 state 제거
        constructor(props) {
            super(props);
            this.handleChange = this.handleChange.bind(this);
        }
        handleChange(e) {
            // setState 대신 사용
            this.props.onTemperatureChange(e.target.value);
        }

        render() {
            const temperature = this.props.temperature;
            const scale = this.props.scale;
            return (
            <fieldset>
                <legend>Enter temperature in {scaleNames[scale]}:</legend>
                <input value={temperature}
                    onChange={this.handleChange} />
            </fieldset>
            );
    }    
    ~~~

* 최종 소스코드

    ~~~js
    class Calculator extends React.Component {
        constructor(props) {
            super(props);
            this.handleCelsiusChange = this.handleCelsiusChange.bind(this);
            this.handleFahrenheitChange = this.handleFahrenheitChange.bind(this);
            this.state = {temperature: '', scale: 'c'};
        }

        handleCelsiusChange(temperature) {
            this.setState({scale: 'c', temperature});
        }

        handleFahrenheitChange(temperature) {
            this.setState({scale: 'f', temperature});
        }

        render() {
            const scale = this.state.scale;
            const temperature = this.state.temperature;
            const celsius = scale === 'f' ? tryConvert(temperature, toCelsius) : temperature;
            const fahrenheit = scale === 'c' ? tryConvert(temperature, toFahrenheit) : temperature;

            return (
            <div>
                <TemperatureInput
                scale="c"
                temperature={celsius}
                onTemperatureChange={this.handleCelsiusChange} />
                <TemperatureInput
                scale="f"
                temperature={fahrenheit}
                onTemperatureChange={this.handleFahrenheitChange} />
                <BoilingVerdict
                celsius={parseFloat(celsius)} />
            </div>
            );
        }
    }
    ~~~

### 4.2 과정

1. React는 DOM input 태그의 onChange에 지정된 함수를 호출.
2. 하위컴포넌트(TemperatureInput)의 값 변경 메소드(handleChange)는 새로 입력된 값과 함께 공유한 이벤트(this.props.onTemperatureChange())를 호출.
3. 이전 렌더링 단계에서, 각각 정의한 상위 컴포넌트에 지정할 메소드에 해당 하위 컴포넌트의 메소드를 지정함.
4. 이 메소드들은 내부적으로 상위 컴포넌트가 수정한 입력 필드에 대한 것이 this.setState()를 호출하게 함으로써 React에게 자신을 다시 렌더링하도록 요청.
5. React UI가 어떻게 보여야 하는지 알기 위해서, 상위 컴포넌트의 render()를 호출하고 단위 같은 기능(온도의 변환)에 대해 재계산.
6. React는 상위 컴포넌트가 전달한 새 props와 함께 각 하위컴포넌트의 render()를 호출해서 UI가 어떻게 보여야 할지 파악.
7. React는 상태를 표시하는 컴포넌트(BoilingVerdict)에게 데이터를 props를 건네면서 그 컴포넌트의 render()를 호출.
8. React DOM은 물의 끓는 여부와 올바른 입력값을 일치시키는 작업과 함께 DOM을 갱신.
9. 입력 필드의 값을 변경할 때 마다 동일한 절차를 거치고 동기화 된 상태로 유지됨.

### 4.3 결론

* React App 안에서 변경이 일어나는 데이터에 대해 "진리의 원천"을 하나만 두어야 함.
* 하향식 데이터흐름을 권장
* state를 끌어올리는 작업은 양방향 바인딩보단 더 많은 "보일러 플레이트"코드를 유발하지만, 디버깅이 쉽다는 장점이 있다.
* 어떤 값이 props 또는 state에 의해 계산될 수 있다면, 아마도 그 값을 state에 두어서는 안 됨.

## 5. 합성 vs 상속

### 5.1 컴포넌트에 다른 컴포넌트를 담기

* 어떤 컴포넌트는 어떤 자식 엘리먼트가 들어올지 미리 예상할수 없는 경우가 있다. -> 범용적인 'Box'역할을 하는 Sidebar혹은 Dialog와 같은 경우
* children prop을 사용하여 자식 엘리먼트를 출력에 그대로 전달할 수 있다.

    ~~~js
    function FancyBorder(props) {
    return (
        <div className={'FancyBorder FancyBorder-' + props.color}>
        {props.children}
        </div>
    );
    }

    // JSX를 중첩하여 임의의 자식을 전달
    function WelcomeDialog() {
    return (
        // FancyBorder 컴포넌트의 자식 prop으로 전달
        // FancyBorder는 {props.children}을 <div>안에 렌더링 하므로 각 엘리먼트들이 최종 출력 됨.
        <FancyBorder color="blue">
        <h1 className="Dialog-title">
            Welcome
        </h1>
        <p className="Dialog-message">
            Thank you for visiting our spacecraft!
        </p>
        </FancyBorder>
    );
    }
    ~~~

* 여러개의 "구멍"이 필요할 수도있는데, children대신에 자신만의 고유한 방식도 적용할 수도 있음.

    ~~~js
    function SplitPane(props) {
    return (
        <div className="SplitPane">
        <div className="SplitPane-left">
            {props.left}
        </div>
        <div className="SplitPane-right">
            {props.right}
        </div>
        </div>
    );
    }

    function App() {
    return (
        <SplitPane
        left={
            <Contacts />
        }
        right={
            <Chat />
        } />
    );
    }
    ~~~

### 5.2 특수화

* React에서 합성을 통해서 "특수한 경우"인 컴포넌트를 해결 할 수있음.
* 클래스를 적용할 때도 같음.
* Dialog의 특수한 경우

    ~~~js
    // 특수한 경우
    function Dialog(props) {
    return (
        <FancyBorder color="blue">
        <h1 className="Dialog-title">
            {props.title}
        </h1>
        <p className="Dialog-message">
            {props.message}
        </p>
        </FancyBorder>
    );
    }

    function WelcomeDialog() {
    return (
        <Dialog
        title="Welcome"
        message="Thank you for visiting our spacecraft!" />
    );
    }
    ~~~

* 클래스로 적용

    ~~~js
    class SignUpDialog extends React.Component {
        constructor(props) {
            super(props);
            this.handleChange = this.handleChange.bind(this);
            this.handleSignUp = this.handleSignUp.bind(this);
            this.state = {login: ''};
        }

        render() {
            return (
            <Dialog title="Mars Exploration Program"
                    message="How should we refer to you?">
                <input value={this.state.login}
                    onChange={this.handleChange} />
                <button onClick={this.handleSignUp}>
                Sign Me Up!
                </button>
            </Dialog>
            );
        }

        handleChange(e) {
            this.setState({login: e.target.value});
        }

        handleSignUp() {
            alert(`Welcome aboard, ${this.state.login}!`);
        }
    }
    ~~~

### 5.3 상속에 대해서

* Facebook에서는 컴포넌트를 상속 계층 구조로 작성을 권장할만한 사례를 아직 찾지 못함.
* props와 합성은 명시적이고, 안전한 방법으로 컴포넌트의 모양과 동작을 사용자화하는데 필요한 모든 유연성을 제공.
* 컴포넌트가 원시 타입의 값, React 엘리먼트 혹은 함수등 어떤 props도 받을수 있음.
* UI가 아닌 기능을 여러 컴포넌트에서 재사용 하기 원한다면, 별도의 JavaScript 모듈로 분리하는 것을 권장(상속대신에 컴포넌트에서 해당 모듈들을 import해서 사용한다.).
