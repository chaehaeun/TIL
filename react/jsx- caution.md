## JSX

```js
const element = <h1>Hello, world!</h1>;
```

자바스크립트를 확장한 문법!

### 중괄호 안에는 유효한 모든 js표현식을 넣을 수 있다!

```js
const name = "Josh Perez";
const element = <h1>Hello, {name}</h1>;
```

삼항연산자를 써서 조건에 따라 다른 문자열을 렌더링할 수도 있다. 이것을 조건부 렌더링이라고 한다!

```js
const number = 5;

return (
  <div style={style.App}>
    <MyHeaders />
    {/* 숫자나 문자열만 들어갈 수 있음 */}
    <h2 style={style.h2}>Hi react {name}</h2>
    <b id="bold_text" style={style.bold_text}>
      {number}는 : {number % 2 === 0 ? "짝수" : "홀수"}
    </b>
  </div>
);
```

### JSX도 표현식이다!

컴파일이 끝나면, JSX 표현식이 정규 JS 함수로 호출되고 JS 객체로 인식된다.
<br/>

**즉 JSX를 if나 for문 안에서 사용하고, 변수에 할당하고, 인자로 받아들이고, 함수로부터 반환할 수도 있다!!**

## JSX를 작성할 때 주의해야 되는 두가지

### 1. 닫힘 규칙

반드시 모든 태그를 닫아주어야 된다. `img`와 같은 self closing 태그도 닫아줘야 됨

### 2. 최상위 태그 규칙

가장 바깥에 있는 태그를 최상위 태그라고 한다.(아래의 코드에서 ` <div className="App">`이 최상위 태그)

```js
function App() {
  let name = "이름";
  return (
    // App div가 최상위 태그
    <div className="App">
      <MyHeaders />
      <header className="App-header">
        <h2>Hi react {name}</h2>
      </header>
      <MyFooter />
    </div>
  );
}
```

만약 최상위 태그를 지워버리게 되면 에러가 발생(JSX문법은 반드시 하나의 부모 요소를 가지고 있어야 된다.)한다.
<br/>
최상위 태그 없이 하고 싶으면 리액트 프레그먼트라는 리액트의 기능을 써야 됨. 이 기능을 쓰려면

```js
import React from "react";
```

로 리액트를 불러오고,

```js
function App() {
  let name = "이름";
  return (
    <React.Fragment>
      <MyHeaders />
      <header className="App-header">
        <h2>Hi react {name}</h2>
      </header>
      <MyFooter />
    </React.Fragment>
  );
}
```

`<React.Fragment>`로 요소들을 감싸주면 된다.

> React.Fragment 너무 길고 귀찮은데;

```js
function App() {
  let name = "이름";
  return (
    <>
      <MyHeaders />
      <header className="App-header">
        <h2>Hi react {name}</h2>
      </header>
      <MyFooter />
    </>
  );
}
```

그냥 이렇게 빈 태그를 열고 닫아주기만 해도 해결
<br/>
💡 리액트의 기능이 필요하지 않은 컴포넌트에는 굳이 리액트를 임포트하지 않아도 된다.
