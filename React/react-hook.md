# React Hook

***React Hook 은 class 을 정의하지 않고도 state 와 React 기능을 사용할 수 있도록 한다***

## Hook

function component 에서 React state 와 LifeCycle 기능을 연동(hook into) 할 수 있게 하는 함수. Hook 은 class 안에서는 동작하지 않는다.


## useState()

React Hook 에서 제공하는 내장 Hook. this.setState 와 달리 이전 state 와 새로운 state 를 합치지 않으며, component 가 다시 렌더링 되어도 state 가 변하지 않는다. 또 객체나 null 일 필요도 없다.

## Hook Constraint

Hook 은 JavaScript 함수이지만 몇 가지 제약사항을 준수해야 한다.

* 최상위(at the top level) 에서만 Hook 을 호출해야 함. 반복문, 조건문, 중첩된 함수 내에서는 Hook 을 실행하지 않아야 함
* React function Component 에서만 Hook 을 호출해야 함. 일반 JavaScript 함수에서는 호출해서는 안된다.


