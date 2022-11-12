### 폼은 왜 복잡하고 어려운가?

개발자의 시각에서 폼은 폼과 그 입력 때문에 넓고 다양한 상태를 나타낼 수 있어서 굉장히 복잡해질 수 있다.

하나 이상의 입력 값이 모두 유효하지 않을 수도 있고, 모두 유효할 수도 있고, 심지어는 서버로 리퀘스트를 보낸 후에 특정 값이 사용 가능한지 확인해야 하는 비동기 유효성 검사를 이용해야 하기 때문에 상태를 알 수 없을 수도 있다. 😨😨 (이메일 유효성 검사 같은 거)

👇 폼 유효성 검사 3가지 방식
![](https://velog.velcdn.com/images/chaehe_3210/post/da742983-0783-46be-81f8-5e1ec85b2cf6/image.png)

폼을 서브밋 할 때 / 인풋에서 포커스를 잃을 때 / 키를 타이핑할 때마다 이렇게 세가지로 나눌 수 있음. 잘 결합해서 사용하는 것이 좋다.

- 폼을 제출했을 때 딱 한 번만 유효성 검사를 하고 싶다 = useRef 이용
- 즉각적인 유효성 검증을 위해 키 입력마다 값이 필요하다면 = state 이용

> 폼 제출 후 인풋을 비워주는 작업은 state로 하는 것이 좋다. ( ref는 DOM을 직접적으로 조작하는 것이기 때문에 권장하지 않음 )

<br/>

#### 좋지 못한 유효성 검사 예시

```js
const SimpleInput = (props) => {
  const [enteredName, setEnteredName] = useState("");
  const [enteredNameIsValid, setEnteredNameIsValid] = useState(true);

  const nameInputRef = useRef();

  const nameInputChangeHandler = (e) => {
    setEnteredName(e.target.value);
  };

  const formSubmissionHandler = (e) => {
    e.preventDefault();

    if (enteredName.trim().length === 0) {
      setEnteredNameIsValid(false);
      return;
    }

    setEnteredNameIsValid(true);

    const enteredValue = nameInputRef.current.value;
    console.log(enteredValue);

    setEnteredName("");
  };

  const nameInputClasses = enteredNameIsValid
    ? "form-control"
    : "form-control invalid";

  return (
    <form onSubmit={formSubmissionHandler}>
      <div className={nameInputClasses}>
        <label htmlFor="name">Your Name</label>
        <input
          value={enteredName}
          type="text"
          ref={nameInputRef}
          id="name"
          onChange={nameInputChangeHandler}
        />
        {enteredNameIsValid ? (
          ""
        ) : (
          <p className="error-text">이름은 비워둘 수 없습니다.</p>
        )}
      </div>
      <div className="form-actions">
        <button>Submit</button>
      </div>
    </form>
  );
};

export default SimpleInput;
```

<br/>

디폴트 `enteredNameIsValid` 를 true로 설정해 초기 화면에는 에러메세지를 띄우지 않게 해놓고,
👇 초기 화면

![](https://velog.velcdn.com/images/chaehe_3210/post/02af4c1f-5e22-4d8d-a194-83d69461eb19/image.png)

<br/>

빈 값으로 submit 제출을 하면 `enteredNameIsValid` 상태 값을 false 로 바꾸어 에러메세지를 띄우게 해놓았다.

👇 유효하지 않은 값을 입력했을 때
![](https://velog.velcdn.com/images/chaehe_3210/post/d29b0e96-5d19-4de7-a8e6-17ee2ec412d6/image.png)

**이 방식이 좋지 못한 이유는 무엇일까?**

```js
useEffect(() => {
  if (enteredNameIsValid) {
    console.log("Name Input Is valid!!");
  }
}, [enteredNameIsValid]);

// 새로고침과 동시에 콘솔에 Name Input Is valid!! 가 띄워진다
```

enteredNameIsValid의 값이 true일 때 실행되는 사이드 이펙트가 있다고 치자.
초기값이 true이기 때문에 아무것도 하지 않았는데도 처음부터 유효한 상태라고 인식하게 된다.

**useEffect를 쓰지 않았다면 큰 문제가 없는 상황이긴 하지만 좋은 코드는 아님.** 초기 상태는 false인 것으로 지정해놓는 게 자연스러울 것이다. ( 처음엔 유효한 값이 아니기 때문!! )

#### 그럼 어떻게 하는 게 좋을까? 🤔

![](https://velog.velcdn.com/images/chaehe_3210/post/99fef678-79d1-4f86-9695-fbc7e2e9f75e/image.png)

```js
const [enteredNameTouched, setEnteredNameTouched] = useState(false);
```

스테이트를 하나 더 추가해서 사용자가 인풋을 입력한 적이 있는지를 확인한다. 초기 상태에선 인풋이 건드려지지 않았고 사용자도 아무것도 하지 않았기 때문에 false를 초기값으로 지정한다.

```js
const nameInputIsInvaild = !enteredNameIsValid && enteredNameTouched;
// !enteredNameIsValid이 true이고( enteredNameIsValid이 false일 때 -> 유효한 값이 아닐 때 )
// enteredNameTouched도 true 일 때( 사용자가 터치를 했을 때 ) nameInputIsInvaild를 true로 바꾼다
// 즉 유효한 값도 아니고, 사용자가 터치한 적이 있을 때 true가 됨
```

불리언 값을 가진 상수 `nameInputIsInvaild`를 만든다. **nameInputIsInvaild는 인풋값이 유효한 값이 아니고, 사용자가 인풋을 건드린 적 있을 때 true 값을 가진다.**

```js
const nameInputClasses = nameInputIsInvaild
  ? "form-control invalid"
  : "form-control";
```

삼항연산자를 이용해 nameInputIsInvaild가 true 일 때는 유효하지 않을 때의 스타일을, false일 때는 디폴트의 스타일을 적용 시킨다.

```js
{
  nameInputIsInvaild ? (
    <p className="error-text">이름은 비워둘 수 없습니다.</p>
  ) : (
    ""
  );
}
```

nameInputIsInvaild 가 true 일 때만 에러메세지가 뜨게 바꾼다.( 이전엔 enteredNameIsValid로 판단 했음 )

🚨 **이 상태로 끝내면 초기 화면일 때 submit을 하면 아무런 반응이 없다!!!**

```js
const formSubmissionHandler = (e) => {
  e.preventDefault();
  setEnteredNameTouched(true);

  if (enteredName.trim().length === 0) {
    setEnteredNameIsValid(false);
    return;
  }

  setEnteredNameIsValid(true);

  const enteredValue = nameInputRef.current.value;
  console.log(enteredValue);

  setEnteredName("");
};
```

근데 이 방법은 사용자 경험 측면에선 별로 좋은 방식이 아닌 것 같음. 폼을 제출 전까지 유효한지 아닌지 알 수 없는 것은 좋은 서비스가 아님

인풋을 작성하다가 포커스를 다른 곳으로 맞추었을 때 유효성 검사가 되는 게 낫지 않을까?

<br/>

### 포커스를 잃었을 때 인풋 유효성 검사

#### 필드에서 포커스가 벗어났을 때 현재 인풋의 상태가 유효한지 검사하는 기능

```js
<input
  value={enteredName}
  type="text"
  ref={nameInputRef}
  id="name"
  onChange={nameInputChangeHandler}
  onBlur={nameInputBlurHandler}
/>
```

포커스만 자주 봤지 blur 함수라는 것도 있는 줄은 최근에 알았다.
**blur()** : 포커스를 잃을 때 발생하는 이벤트 메서드.

> 포커스와 블러는 입력 필드 값 검증에 자주 사용되는 것 같다.

```js
const nameInputBlurHandler = (e) => {
  setEnteredNameTouched(true);
  if (enteredName.trim().length === 0) {
    setEnteredNameIsValid(false);
    return;
  }
};
```

인풋에서 포커스를 잃었다 = 입력 값에서 무언가를 한 뒤 빠져나왔다는 말
이기 때문에, `enteredNameTouched`를 true로 변경 시켜준다. 그리고 이전에 사용되었던 유효성 검사를 다시 해준 후 유효하지 않는다면 에러메세지를 보이게 한다.

#### 다시 인풋에 포커스를 맞추고 값을 입력했을 때 다시 유효성을 검사하는 기능

```js
const nameInputChangeHandler = (e) => {
  setEnteredName(e.target.value);

  if (e.target.value.trim().length !== 0) {
    setEnteredNameIsValid(true);
    // 0인지 아닌지를 판단해서 false로 바꾸는 게 아닌
    // 길이가 0이 넘는지를 확인해서 최대한 빨리 true로 바꿔야 되는 것임
  }
};
```

false로 바꾸는 것이 아닌 true로 바꾸는 것이기 때문에 길이가 0이 넘으면 바로 true로 바꿔줬음 (유효한 값이 된다는 말임)

> 🚨 이때 조건문 내의 밸류를 enteredName에서 e.target.value로 바꾼 이유 : enteredName은 현재의 최신의 값을 가지고 있지 않기 때문 (이전의 상태를 참조하기 때문임)

<br/>

#### 코드 리팩토링

생각해보면, enteredNameIsValid 라는 스테이트가 필요하지 않다는 것을 알 수 있다.

```js
const enteredNameIsValid = enteredName.trim() !== "";
// 유효할 때 true를 가리켜야 되므로 trim의 결과가 빈 값이 아닐 때 true로 설정
```

iuput 내부의 value는 enteredName 안의 값이 바뀔 때마다 유효성을 검증하기 때문에

```js
const nameInputChangeHandler = (e) => {
  setEnteredName(e.target.value);
};
// 키 입력 때마다 유효성을 검증할 수 있는 이유
```

유효성을 판단하는 식의 결과 자체를 enteredNameIsValid 로 상수 지정을 해버리면, 자동으로 true 또는 false를 가리키게 된다.

```js
const nameInputChangeHandler = (e) => {
  setEnteredName(e.target.value);

  //  if (e.target.value.trim().length !== 0) {
  //    setEnteredNameIsValid(true);
  //  }
};
```

이제 제출할 때의 유효성 검사를 제외하곤 검증 조건문을 다 지워도 된다!

```js
const formSubmissionHandler = (e) => {
  e.preventDefault();
  if (!enteredNameIsValid) {
    return;
  }

  setEnteredNameTouched(true);
  setEnteredName("");
};
```

submit했을 때의 검증도 `enteredNameIsValid`이 false일 때만 리턴하면 되므로 !enteredNameIsValid 로 조건문을 써줬다. 코드가 조금 더 간결해졌당!

<br/>

### 동시에 여러 개의 인풋 유효성을 검사해야 될 때? 🤔

전체 유효성을 검사할 때는

```js
  let formIsValid = false;

  if (enteredNameIsValid && 그 외 같이 검사해야되는 값) {
    formIsValid = true;
  }
```

useState나 useEffect 같은 복잡한 거 쓸 필요 없고 간단한 조건문 쓰면 된다 😀
이게 성능도 더 좋음!

가끔 어려운 거 쓰면 더 있어보일 것 같다는 생각이 들곤 하는데 오히려 저런 베이직한 것들이 성능에도, 가독성에도 더 좋은 경우도 꽤 많으니 여러 상황을 고려해서 코드를 짜는 것이 좋겠다~

#### 인풋이 여러 개일 때는??

같은 로직을 반복해서 사용하는 것은 가독성을 죽여버리는 일이다. 커스텀 훅을 사용해서 효율적으로 코딩을 할 수 있는 방법을 알아보자.(내 머리로는 혼자서 못하겠어서 하드코딩한 후에 영상 봤다ㅋㅋㅋ...............ㅠ)

👇 컴포넌트 코드

```js
import React from "react";
import useInput from "../hooks/use-input";

const SimpleInput = (props) => {
  const {
    value: enteredName,
    isValid: enteredNameIsValid,
    hasError: nameInputHasError,
    valueChangeHandler: nameChangeHandler,
    inputBlurHandler: nameBlurHandler,
    reset: resetNameInput,
  } = useInput((value) => value.trim() !== "");

  const {
    value: enteredEmail,
    isValid: enteredEmailIsValid,
    hasError: emailInputHasError,
    valueChangeHandler: emailChangeHandler,
    inputBlurHandler: emailBlurHandler,
    reset: resetEmailInput,
  } = useInput((value) => value.includes("@"));

  let formIsValid = false;

  if (enteredNameIsValid && enteredEmailIsValid) {
    formIsValid = true;
  }

  const formSubmissionHandler = (e) => {
    e.preventDefault();

    if (!enteredNameIsValid && !enteredEmailIsValid) {
      return;
    }
    resetNameInput();
    resetEmailInput();
  };

  const nameInputClasses = nameInputHasError
    ? "form-control invalid"
    : "form-control";

  const emailInputClasses = emailInputHasError
    ? "form-control invalid"
    : "form-control";

  return (
    <form onSubmit={formSubmissionHandler}>
      <div className={nameInputClasses}>
        <label htmlFor="name">Your Name</label>
        <input
          type="text"
          id="name"
          onChange={nameChangeHandler}
          onBlur={nameBlurHandler}
          value={enteredName}
        />
        {nameInputHasError && (
          <p className="error-text">이름은 비워둘 수 없습니다.</p>
        )}
      </div>

      <div className={emailInputClasses}>
        <label htmlFor="email">Your Email</label>
        <input
          type="email"
          id="email"
          onChange={emailChangeHandler}
          onBlur={emailBlurHandler}
          value={enteredEmail}
        />
        {emailInputHasError && (
          <p className="error-text">유효한 이메일이 아닙니다.</p>
        )}
      </div>

      <div className="form-actions">
        <button disabled={!formIsValid}>Submit</button>
        {/* forIsValid가 false일 때 버튼을 비활성화 하기 */}
      </div>
    </form>
  );
};

export default SimpleInput;
```

👇 커스텀훅 코드

```js
import { useState } from "react";

const useInput = (validateValue) => {
  const [enteredValue, setEnteredValue] = useState("");
  const [isTouched, setIsTouched] = useState(false);

  const valueIsValid = validateValue(enteredValue);
  const hasError = !valueIsValid && isTouched;

  const valueChangeHandler = (e) => {
    setEnteredValue(e.target.value);
  };

  const inputBlurHandler = (e) => {
    setIsTouched(true);
  };

  const reset = () => {
    setEnteredValue("");
    setIsTouched(false);
  };

  return {
    value: enteredValue,
    isValid: valueIsValid,
    hasError,
    valueChangeHandler,
    inputBlurHandler,
    reset,
  };
};

export default useInput;
```

### 폼을 만드는데 유용한 서드파티 라이브러리 formik

[formik](https://formik.org/)
폼을 렌더링하고 더 복잡한 폼을 만들고 검증할 때는 매우 좋은 라이브러리라고 한다. 👍

👇 사용방법

```js
import React from "react";
import ReactDOM from "react-dom";
import { Formik, Field, Form } from "formik";
import "./styles.css";

function App() {
  return (
    <div className="App">
      <h1>Contact Us</h1>
      <Formik
        initialValues={{ name: "", email: "" }}
        onSubmit={async (values) => {
          await new Promise((resolve) => setTimeout(resolve, 500));
          alert(JSON.stringify(values, null, 2));
        }}
      >
        <Form>
          <Field name="name" type="text" />
          <Field name="email" type="email" />
          <button type="submit">Submit</button>
        </Form>
      </Formik>
    </div>
  );
}

ReactDOM.render(<App />, document.getElementById("root"));
```

검증 로직만 만들고 어떤 값을 입력 받을지 정하면 formik이 나머지는 다 해준다고 😋
