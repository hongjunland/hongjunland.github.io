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

* element를 저장하기 위해 벼수를 사용할 수 있음.
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