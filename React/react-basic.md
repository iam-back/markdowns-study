# React : Basic

***사용자 인터페이스를 만들기 위한 JavaScript 라이브러리***

React 를 가장 제대로 설명하는 문장. React 는 다음과 같은 특징을 가진다

* Single Page Application
* Vitual DOM 을 통한 브라우저 렌더링 최적화


## JSX : JavaScript Xml

JSX 는 react 에서 html 을 JavaScript 로 다루기 위한 특수한 문법사항이다. JavaScript 공식 문법은 아니며, babel 에 의해 ES5 로 트랜스파일된다.

## React Element

react element 는 ***JavaScript 불변 객체*** 이며 react 가 다루는 가장 작은 단위이다. react element 는 html element 를 JavaScript 적으로 대체하도록 고안되었다.

```javaScript
// react element with JSX
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
);

// react 는 근본적으로 JavaScript 불변 객체이며 위의 JSX 를 다음과 같이 변환한다. 아래는 단순화한 예이다.
const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, world!'
);
```

## ReactDOM.render()

react 는 근본적으로 JavaScript 이므로 react element 는 별도의 호출을 통해 렌더링된다.

## root DOM node

react 는 Single Page Application 으로 구현한다. create-react-app 을 기준으로 public/index.html 을 보면, `<div id="root"></div>` 인 root DOM node 가 존재하며, src/index.tsx 에서 렌더링하고 있음을 알 수 있다.

```TypeScript
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';
import reportWebVitals from './reportWebVitals';

const root = ReactDOM.createRoot(
    //browser 의 HTML element id 가 root 인 불변 객체 root 에 할당
  document.getElementById('root') as HTMLElement
);

root.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);

reportWebVitals();

```


## React Component

react component 는 react element 를 JavaScript 문법 내에 위치하게 하여, 마치 HTML 내에서 JavaScript 를 사용하는 듯한 경험을 주도록 한다. component 는 function style, class style 로 구현할 수 있으며, 요즘 추세는 function style 을 채택한다. Pascal case naming convention 을 지킨다. Lower case 로 사용하게 되면 DOM Element 로 인식하게 된다.

***Component = Element + JavaScript Programming***

### Component attribute

react component 는 HTML attribute 처럼 attribute 를 가질 수 있다. 단, HTML attribute 와 달리 camel case naming convention 을 지킨다. 또 component 로 넘어온 props 나 component 내부적으로 관리하는 state 를 {props.value} 와 같이 사용하여 attribute 를 바꿀 수 있다.

## props

component 에 특정한 데이터를 전달하고 싶을 때 사용. 부모 컴포넌트에서 자식 컴포넌트로 넘어온다거나 외부에서 데이터가 넘어오는 경우 props 로 처리한다. 읽기 전용으로만 사용해야 한다. 즉, 순수 함수 형태이어야 한다. props naming 은 context(문맥) 이 아닌 component 관점에서 정의하는 것을 권장한다.

> ***React Component Constraint*** <br>
모든 React Component 는 props 를 다룰 때 순수 함수이어야 한다. 즉, Side-effect 가 없어야 한다.

## state

componenet 내부에서 생성, 변경하는 데이터. state 는 자기 자신이 만들어낸 혹은 가지고 있는 데이터를 사용하며, 오직 해당 컴포넌트 내에서만 변경 가능한 값. local scope 를 가진다고 생각하면 쉽다. state 는 

***this.setState vs useState()***

기존의 this.setState 는 반드시 객체나 null 타입이어야만 함. 하지만 React Hook 의 useState() 는 객체일 필요가 없다

