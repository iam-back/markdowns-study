# State

---

> ***REACT 컴포넌트는 한 번 랜더링하면 다시 실행되지 않는다***

* React 는 DOM 구조의 상태(State) 를 변경할 수 있도록 state 라는 것을 지원
* React 에서 지원하는 useState hoc 을 사용해야 함

```TypeScript

import {useState} from react;


function App(){
    // useState() 는 변수를 state 로 만듦
    const [mode, setMode] = useState('INDEX');
}
```

## state

* useState() 는 배열을 반환
* 첫번째 요소는 상태의 값을, 두번째 요소는 상태의 변화를 주기 위한 함수를 구현하면 됨
