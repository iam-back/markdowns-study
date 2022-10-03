# JSX Syntax

---

> ## ***Javascript XML***

* React 프로젝트를 위한 문법으로 공식적인 Javascript 문법은 아님
* 브라우저에서 실행하기 전에 바벨을 통해 Javascript 로 변환
* Javascript 에서 HTML 을 작성하도록하는 느낌을 줌 -> JSP 와 비슷하다고 느낌

> ## RULES

### 1. 반드시 부모 요소 하나가 감싸는 형태여야 함

* Virtual DOM 에서 컴포넌트 변화를 감지할 때 효율적으로 비교하기 위해 컴포넌트 내부는 하나의 DOM 트리 구조로 이루어져야 한다는 규칙

### 2. Javascript 표현식을 사용할 수 있음

* JSX 에서도 Javascript 표현식을 사용할 수 있음
* 중괄호 {} 내에 표현식을 작성하면 됨

> ***Expression : 표현식*** <br>
값으로 평가될 수 있는 문(statement) <br>
대표적으로 변수, 함수 등이 있음

### 3. 표현식이 아닌 문은 JSX 의 Javascript 표현식에는 사용할 수 없음

* 따라서 표현식 밖에서 처리한 후 표현식에 적용하는 방법 사용

### 4. React DOM 은 HTML attribute 명명 대신 Javascript 의 camelCase property 명명 규칙을 사용

* JSX 는 스타일 적용시에도 객체 형태로 넣어 주어야 함. 따라서 CamelCase 로 넣어줌
* HTML attribute 의 class 대신 className 사용

### 5. 주석

* 주석은 {/* ... */} 형식으로 사용

