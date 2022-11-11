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
