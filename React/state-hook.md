# State Hook

state 를 다루는 Hook 은 function component 에서 다음과 같이 사용할 수 있다

```JSX
function Counter(){

    /*
        State Hook with destructuring assignment : 배열 구조 분해 할당

    */

    const [count, setCount] = useState(0);

    return(
        //react element logic
        <>
            <p>You clicked {count} times</p>
            <button onClick={()=>setCount(count+1)}>
                plus
            </button>
        </>
    );
}
```

## Features

1. component 가 렌더링될 때, 단 한 번만 생성된다. Hook naming 이 create 가 아닌 use 인 이유이기도 하다.
2. function coponent 에서는 this 를 통해 불러올 수 없는 대신 {state} 와 같이 바로 불러올 수 있다.

> ***JavaScript this keyword*** <br>
다른 언어에서의 this 는 어느 타입에서 정의되어 있느냐가 기준이었지만 JavaScript 는 누가 호출했는가를 기준으로 this 가 동작한다


