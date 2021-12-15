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
* React적인 사고에 대해 간단히 설명 할 수 있다.

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
~~~

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