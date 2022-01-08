---
title: "[React][Chapter3]React Advanced(1)"
last_modified_at: "2021-12-17 02:31"
categories:
    - react
tags:
    - javascript
    - front-end
    - react
---

## 학습 목표

* React의 Context에 대해 설명할 수 있다.
* 에러경계, Ref 전달 등 사용 할 수있다.
* Fragment와 고차 컴포넌트, 다른 라이브러리와 통합에 대해서 설명할 수 있다.
* JSX의 근본적인 역할에 대해 설명할 수 있다.
* React 앱의 성능을 최적화 하는 방법에 대해서 간단히 설명할 수 있다.

## 1. 접근성

* 웹 접근성(a11y)은 모두가 사용할 수 있도록 디자인, 개발하는 것을 의미.
* React는 접근성을 갖출수 있도록 모든 지원을 하고 있음.(대부분 표준 HTML기술로 사용)

### 1.1 표준 및 지침

* WCAG(Web Content Accessibility Guidelines): 접근성을 갖춘 웹사이트를 만드는 데 필요한 지침서

    * [Wuhcag의 WCAG 체크리스트](https://www.wuhcag.com/wcag-checklist/)
    * [WebAIM의 WCAG 체크리스트](https://webaim.org/standards/wcag/checklist)
    * [The A11Y Project의 체크리스트](https://a11yproject.com/checklist.html)

* WAI-ARIA - [링크](https://www.w3.org/WAI/standards-guidelines/aria/)

    * JSX에서 aria-* HTML 지원 

    ~~~js
    // aria-* 은 hypen-case 사용
    <input
        type="text"
        aria-label={labelText}
        aria-required="true"
        onChange={onchangeHandler}
        value={inputValue}
        name="name"
    />  
    ~~~

### 1.2 시멘틱 HTML

* 가끔 React로 구성한 코드가 작동하게 하기위해 div태그와 같은 엘리먼트로 HTML의 의미를 깨기도 함. (ex: ol, ul, dl, table)
* <strong>React Fragment</strong>를 사용하여 여러 엘리먼트를 하나로 묶어주는 것을 권장.
* [참고자료](https://developer.mozilla.org/ko/docs/Web/HTML/Element)
* 예시

    ~~~js
    import React, { Fragment } from 'react';

    function ListItem({ item }) {
    return (
        <Fragment>
        <dt>{item.term}</dt>
        <dd>{item.description}</dd>
        </Fragment>
    );
    }

    function Glossary(props) {
    return (
        <dl>
        {props.items.map(item => (
            <ListItem item={item} key={item.id} />
        ))}
        </dl>
    );
    }
    ~~~

* 각 항목을 매핑할 경우(key)

    ~~~js
    function Glossary(props) {
        return(
            <dl>
                {props.items.map(item=>(
                    // 매핑할 때, Fragment는 반드시 key라는 속성을 가져야 함.
                    <Fragment key={item.id}>
                        <dt>{item.term}</dt>
                        <dt>{item.description}</dt>
                    </Fragment>
                ))}
            </dl>
        );
    }
    ~~~

* 축약예시

    ~~~js
    function ListItem({item}) {
        return(
            // Fragment 표시가 생략됨
            <>
                <dt>{item.term}</dt>
                <dt>{item.description}</dt>
            </>
        );
    }
    ~~~


### 1.3 접근성 있는 폼(Form)

#### 라벨링(Labelling)

* input과 textarea 태그는 구분할 수 있는 라벨이 필요.
* 참고 자료
    * [W3C에서 제공하는 엘리먼트 라벨링 방법](https://www.w3.org/WAI/tutorials/forms/labels/)
    * [WebAIM에서 제공하는 엘리먼트 라벨링 방법](https://webaim.org/techniques/forms/controls)
    * [The Paciello Group이 설명한 접근 가능한 이름들](https://www.paciellogroup.com/blog/2017/04/what-is-an-accessible-name/)

* 예시

    ~~~js
    // React에 대해 바로 사용 될 수 있음.
    <label htmlFor="namedInput">Name:</label>
    <input id="namedInput" type="text" name="name"/>
    ~~~

* for문 만은 JSX에서 htmlFor로 사용하는 것에 주의해야 함.

### 1.4 포커스 컨트롤

* 모든 웹 애플리케이션은 키보드만 사용하여 모든 동작을 할 수 있어야 한다.
* 키보드 포커스와 포커스 윤곽선

    * 키보드 포커스: 키보드 입력을 받아 들일수 있는 DOM내의 현재 엘리먼트
    * 포커스 윤곽선: 키보드 포커스에 의해 표시된 선(윤곽선 지우려면 outline: 0)

        ![focus](https://ko.reactjs.org/static/dec0e6bcc1f882baf76ebc860d4f04e5/4fcfe/keyboard-focus.png)
    
* 원하는 콘텐츠로 건너뛸 수 있는 방법

    * 탐색속도를 높이기 위해, 이전 탐색영역을 건너뛸 방법을 제공해야 함.
    * Skiplinks 또는 Skip Navigation Link들은 키보드 사용자가 페이지와 상호작용할 때만 표시되는 숨겨진 탐색링크 -> 내부의 페이지 앵커와 약간의 스타일링으로 매우 쉽게 구현
    * \<main\> 과 \<aside\> 같이 대표성을 띠는 랜드마크 엘리먼트와 역할들을 사용해 페이지 영역을 나누어야 함.

* 프로그래밍으로 포커스 관리하기

    * 런타임 동안 지속해서 HTML DOM을 변경하므로, 가끔 키보드 포커스를 잃을 때가 있음.
    * 프로그래밍적으로 올바른 방향으로 변경해야함.
    * 예시

        ~~~js
        class CustomTextInput extends React.Component {
            constructor(props) {
                super(props);
                // DOM 엘리먼트를 저장할 textInput이라는 ref을 생성
                this.textInput = React.createRef();
            }
            render() {
            // `ref` 콜백으로 텍스트 input DOM을 저장
            // 인스턴스 필드의 엘리먼트 (예를 들어, this.textInput)
                return (
                <input
                    type="text"
                    ref={this.textInput}
                />
                );
            }
        }

        ~~~

    * 포커스 지정 예시(컴포넌트 내에서 필요할 때마다 함)

        ~~~js
        focus() {
        // DOM API를 사용해 텍스트 input에 정확히 포커스를 맞춥니다.
        // 주의: ‘현재’의 DOM 노드에 접근하고 있습니다.
        this.textInput.current.focus();
        }
        ~~~

    * 부모컴포넌트가 자식 내부의 엘리먼트에 포커스를 잡아야할 경우- 자식 컴포넌트에 프로퍼티를 주어 DOM ref를 부모 컴포넌트로 노출하는 방식으로 한다.

        ~~~js
        function CustomTextInput(props) {
            return (
                <div>
                <input ref={props.inputRef} />
                </div>
            );
        }

        class Parent extends React.Component {
            constructor(props) {
                super(props);
                this.inputElement = React.createRef();
            }
            render() {
                return (
                <CustomTextInput inputRef={this.inputElement} />
                );
            }
        }

        // 이제 필요할 때마다 포커스를 잡을 수 있습니다.
        this.inputElement.current.focus();

        ~~~

    * 매우 좋은 포커스 관리

        * 첫 포커스를 취소 버튼에 맞춘다.
        * 키보드 포커스를 모달 안으로 한정시켜줌(실수 방지)
        * 모달이 닫힐 때, 모달을 열게 했던 엘리먼트에 포커스를 다시 잡아 줌.
        
    * [좋은 예시](https://github.com/davidtheclark/react-aria-modal) 


### 1.5 마우스와 포인터 이벤트

* 마우스, 포인터 이벤트로 노출된 모든 기능을 키보드만으로 사용할 수 있도록 보장해야 한다.
* 포인터 장치만 고려할 경우, 키보드 사용자들이 앱을 제대로 사용하지 못할 수도 있기 때문
* 일반적으로 팝오버를 닫는 click이벤트를 window객체에 붙여 구현 함.
* 포인터 장치만 고려(window객체에 직접 연결)

    ~~~js

      componentDidMount() {
        window.addEventListener('click', this.onClickOutsideHandler);
    }

    componentWillUnmount() {
        window.removeEventListener('click', this.onClickOutsideHandler);
    }
    ~~~

* 포인터 장치 & 키보드 
    ~~~js
    class BlurExample extends React.Component {
        constructor(props) {
            super(props);

            this.state = { isOpen: false };
            this.timeOutId = null;

            this.onClickHandler = this.onClickHandler.bind(this);
            this.onBlurHandler = this.onBlurHandler.bind(this);
            this.onFocusHandler = this.onFocusHandler.bind(this);
        }

        onClickHandler() {
            this.setState(currentState => ({
            isOpen: !currentState.isOpen
            }));
        }

        // setTimeout을 사용해 다음 순간에 팝오버를 닫습니다.
        // 엘리먼트의 다른 자식에 포커스가 맞춰져있는지 확인하기 위해 필요합니다.
        // 새로운 포커스 이벤트가 발생하기 전에
        // 블러(blur) 이벤트가 발생해야 하기 때문입니다.
        onBlurHandler() {
            this.timeOutId = setTimeout(() => {
            this.setState({
                isOpen: false
            });
            });
        }

        // 자식이 포커스를 받으면, 팝오버를 닫지 않습니다.
        onFocusHandler() {
            clearTimeout(this.timeOutId);
        }

        render() {
            // React는 블러와 포커스 이벤트를 부모에 버블링해줍니다.
            return (
            <div onBlur={this.onBlurHandler}
                onFocus={this.onFocusHandler}>
                <button onClick={this.onClickHandler}                aria-haspopup="true"
                        aria-expanded={this.state.isOpen}>

                Select an option
                </button>
                {this.state.isOpen && (
                <ul>
                    <li>Option 1</li>
                    <li>Option 2</li>
                    <li>Option 3</li>
                </ul>
                )}
            </div>
            );
        }
    }
    ~~~

### 1.6 더욱 복잡한 위젯

* 접근성을 쉽게 지원하는 방법은 가능하면 HTML에 맞게 코딩하는 것.
* ARIA 역할과 ARIA상태 및 프로퍼티에 대한 지식이 필요 -> JSX에서 모두 지원되는 HTML 어트리뷰트로 채워진 도구 상자.
* 각각의 위젯 타입은 명확한 디자인 패턴이 있음.

    * WAI-ARIA Authoring Practices - 디자인 패턴과 위젯
    * Heydon Pickering - ARIA 예시
    * 포괄적 컴포넌트

### 1.7 기타 고려사항

* 언어 설정 : 스크린 리더 소프트웨어들이 올바른 음성을 선택할 수 있도록, 페이지 텍스트에 인간 언어를 나타내야 함.
* 문서 제목 설정: 문서의 \<title\>이 현재 페이지에 대한 올바른 설명을 담아야 함.
* 색 대비: 저시력 사용자들이 최대한 읽을 수 있게, 모든 글에 충분한 색 대비를 주어야 한다.

    * WCAG - 색 대비 요건 이해하기
    * 색 대비에 대한 모든 것과 이를 다시 생각해야 하는 이유
    * [The A11Y Project - 색 채도란](https://www.a11yproject.com/posts/what-is-color-contrast/)

* 색 대비에 대한 테스트 기능 확장 도구

    * [WebAIM - 색 채도 검사기](https://jxnblk.com/colorable/)
    * [The Paciello Group - 색 채도 분석기](https://www.paciellogroup.com/resources/contrastanalyser/)

### 1.8 개발 및 테스트 도구

* 키보드

    1. 마우스의 연결 해제
    2. Tab과 Shift+Tab을 사용해 이동
    3. Enter를 사용해 엘리먼트를 활성화
    4. 메뉴와 드롭다운과 같은 일부 엘리먼트를 필요하다면 방향키로 조작.

* 개발 보조 도구(JSX 코드에서 바로 확인 가능)

    * eslint-plugin-jsx-a11y : JSX 내의 접근성 문제에 대해 즉각적인 AST 린팅 피드백을 제공해서 많은 IDE가 코드 분석과 소스 코드 창에 이런 결과를 통합할 수 있도록 해줌.
    * Create React App에서 해당 플러그인의 일부 규칙들이 활성화 되어 있음. 
    * 더 많은 접근성 기능을 활성화 하려면, 프로젝트 루트 디렉토리에 .eslintrc 파일을 생성한다.

        ~~~js
        // .eslintrc
        {
            "extends": ["react-app", "plugin:jsx-a11y/recommended"],
            "plugins": ["jsx-a11y"]
        }
        ~~~

## 2. 코드 분할

### 2.1 Bundling

* Webpack, Rollup 또는 Browerify같은 툴을 사용해서, 여러 파일을 하나로 병합한 파일(bundled)을 웹페이지에 포함하여 한 번에 전체 앱을 로드 할 수 있음.
* 예시

    App

    ~~~js
    // app.js
    import (add) from './math.js';

    console.log(add(16,26));    //42
    ~~~

    ~~~js
    // math.js
    export function add(a, b) {
    return a + b;
    }
    ~~~

    Bundle

    ~~~js
    function add(a, b) {
        return a + b;
    }

    console.log(add(16, 26)); // 42
    ~~~

### 2.2 코드 분할

* 코드 분할로 번들을 나눔으로써 거대해 짐을 방지함.
* 코드 분할은 "lazy loading" 하게 도와줘서, 성능을 향상 시킨다.

### 2.3 import()

* 코드 분할을 도입하는 가장 좋은 방법은 동적 import() 문법을 사용하는 것.

* Before

    ~~~js
    import {add} from './math';
    console.log(add(11,22));
    ~~~

* After

    ~~~js
    import("./math").then(math=>{
        console.log(math.add(11,22));
    });
    ~~~

* Babel을 사용할 떄는 Babel이 동적 import를 인식할 수 있지만 변환하지는 않도록 해야함. ([babel-plugin-syntax-dynamic-import](https://yarnpkg.com/en/package/babel-plugin-syntax-dynamic-import) 사용)

### 2.4 React.lazy

* React.lazy 함수를 사용하면 동적 import를 사용해서 컴포넌트를 렌더링 할 수 있음.
* React.lazy 와 Suspense는 아직 서버 사이드 렌더링을 할 수 없음.

* Before

    ~~~js
    import OtherComponent from './OtherComponent';
    ~~~

* After

    ~~~js
    const OtherComponent = React.lazy(() => import('./OtherComponent'));
    ~~~

* 예시

    ~~~js
    import React, {Suspense} from 'react';
    const OtherComponent = React.lazy(()=> import('./OtherComponent'));
    
    function() MyComponent{
        return (
            <div>
                <Suspense fallback={<div>Loading...</div>}>
                    <OtherComponent />
                </Suspense>
            </div>
        );
    }
    ~~~

    * MyComponent가 렌더링 될 때, OtherComponent를 포함한 번들을 자동으로 불러 옴.

* import() 함수는 React 컴포넌트를 포함하며, default export 를 가진 모든 모듈로 결정되는 Promise로 반환해야 함.
* lazy 컴포넌트는 Suspense 컴포넌트 하위에서 렌더링 되어야 함.
* Suspense는 로딩화면과 같은 예비 컨텐츠를 보여주는 기능.
* fallback props은 load 대기시간 동안 React 엘리먼트를 받아 들임.

* Error Boundary(에러 경계)
    
    * Error Boundary를 통해 사용자의 경험과 복구 관리를 처리 할 수 있음.
    * Error Boundary를 만들고 lazy 컴포넌트를 감싸면, 장애 발생시 에러 표시 가능.
    
        ~~~js
        import MyErrorBoundary from './MyErrorBoundary';
        // 다른 import 생략

        const MyComponent = () => (
        <div>
            // Suspnse(lazy 컴포넌트)를 감싼다.
            <MyErrorBoundary>
            <Suspense fallback={<div>Loading...</div>}>
                <section>
                <OtherComponent />
                <AnotherComponent />
                </section>
            </Suspense>
            </MyErrorBoundary>
        </div>
        );
        ~~~

### 2.5 Route-based code splitting(라우트 기반 코드 분활)

* 사용자의 경험을 해치지 않으면서, 번들을 균등하게 분배하는 가장 좋은 방법.
* 사용 예시

    ~~~js
    import React, { Suspense, lazy } from 'react';
    import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';

    const Home = lazy(() => import('./routes/Home'));
    const About = lazy(() => import('./routes/About'));

    const App = () => (
    <Router>
        <Suspense fallback={<div>Loading...</div>}>
        <Switch>
            <Route exact path="/" component={Home}/>
            <Route path="/about" component={About}/>
        </Switch>
        </Suspense>
    </Router>
    );
    ~~~

### 2.6 Named Exports

* React.lazy는 현재 default exports만 지원하므로, default로 이름을 재정의한 중간 모듈을 생성해야 함.
* 예시
    
    ~~~js
    // ManyComponents.js
    export const MyComponent = /* ... */;
    export const MyUnusedComponent = /* ... */;
    ~~~

    ~~~js
    // MyComponent.js
    export { MyComponent as default } from "./ManyComponents.js";
    ~~~

    ~~~js
    // MyApp.js
    import React, { lazy } from 'react';
    const MyComponent = lazy(() => import("./MyComponent.js"));
    ~~~

## 3. Context

* Context를 이용하면, 일일이 props를 전달하지 않아도 컴포넌트 트리 전체에 데이터 제공 가능.

* API 

    * React.createContext
    * Context.Provider
    * Class.contextType
    * Context.Consumer
    * Context.displayName

### 3.1 Context 사용 시기

* context는 전역적인 데이터를 공유할 수 있도록 고안된 방법.
* 로그인한 유저, 테마, 선호하는 언어 등 있음.
* 테마 - props
    
    ```js
    class App extends React.Component {
        render() {
            return <Toolbar theme="dark" />;
        }
    }

    function Toolbar(props) {
    // Toolbar 컴포넌트는 불필요한 테마 prop를 받아서
    // ThemeButton에 전달해야 합니다.
    // 앱 안의 모든 버튼이 테마를 알아야 한다면
    // 이 정보를 일일이 넘기는 과정은 매우 곤혹스러울 수 있습니다.
        return (
            <div>
            <ThemedButton theme={props.theme} />
            </div>
        );
    }

    class ThemedButton extends React.Component {
        render() {
            return <Button theme={this.props.theme} />;
        }
    }
    ```

* 테마 - context

    ```js
    // context를 사용하면 모든 컴포넌트를 일일이 통하지 않고도
    // 원하는 값을 컴포넌트 트리 깊숙한 곳까지 보낼 수 있습니다.
    // light를 기본값으로 하는 테마 context를 만들어 봅시다.
    const ThemeContext = React.createContext('light');

    class App extends React.Component {
        render() {
            // Provider를 이용해 하위 트리에 테마 값을 보내줍니다.
            // 아무리 깊숙히 있어도, 모든 컴포넌트가 이 값을 읽을 수 있습니다.
            // 아래 예시에서는 dark를 현재 선택된 테마 값으로 보내고 있습니다.
            return (
            <ThemeContext.Provider value="dark">
                <Toolbar />
            </ThemeContext.Provider>
            );
        }
    }

    // 이젠 중간에 있는 컴포넌트가 일일이 테마를 넘겨줄 필요가 없습니다.
    function Toolbar() {
        return (
            <div>
            <ThemedButton />
            </div>
        );
    }

    class ThemedButton extends React.Component {
    // 현재 선택된 테마 값을 읽기 위해 contextType을 지정합니다.
    // React는 가장 가까이 있는 테마 Provider를 찾아 그 값을 사용할 것입니다.
    // 이 예시에서 현재 선택된 테마는 dark입니다.
    static contextType = ThemeContext;
        render() {
            return <Button theme={this.context} />;
        }
    }
    ```