### 제어 컴포넌트 / 비제어 컴포넌트

#### 제어 컴포넌트?

`<input>`, `<textarea>`, `<select>`와 같은 폼 엘리먼트를 사용자의 입력을 기반으로 state를 사용해 업데이트하는 컴포넌트. **리액트를 통해서 제어할 수 있기 때문에 제어 컴포넌트라고 부른다.**

제어 컴포넌트는 사용자가 입력한 값과 저장되는 값이 실시간으로 동기화된다. (키를 입력할 때마다 리렌더링이 일어난다는 말)

```js
const [enteredUsername, setEnteredUsername] = useState("");
const [enteredAge, setEnteredAge] = useState("");
const [error, setError] = useState();

///

const usernameChangeHandler = (event) => {
  setEnteredUsername(event.target.value);
};

const ageChangeHandler = (event) => {
  setEnteredAge(event.target.value);
};

const errorHandler = () => {
  setError(null);
};
```

```js
<Card className={classes.input}>
  <form onSubmit={addUserHandler}>
    <label htmlFor="username">Username</label>
    <input
      id="username"
      type="text"
      value={enteredUsername}
      onChange={usernameChangeHandler}
    />
    <label htmlFor="age">Age (Years)</label>
    <input
      id="age"
      type="number"
      value={enteredAge}
      onChange={ageChangeHandler}
    />
    <Button type="submit">Add User</Button>
  </form>
</Card>
```

### 비제어 컴포넌트?

주로 `useRef()`를 사용해 실시간으로 입력값을 가져오는 것이 아닌 DOM에서 직접 값을 뽑아와서 폼을 제출할 때만 값을 업데이트하는 방식이다. 바닐라자바스크립트의 제어 방식과 비슷. **리액트를 통해서 제어되지 않기 때문에 비제어 컴포넌트라고 불린다.**

```js
const nameInputRef = useRef();
const ageInputRef = useRef();
//
//
const addUserHandler = (event) => {
  event.preventDefault();
  const enteredUsername = nameInputRef.current.value;
  const enteredAge = ageInputRef.current.value;
  if (enteredUsername.trim().length === 0 || enteredAge.trim().length === 0) {
    setError({
      title: "Invalid input",
      message: "Please enter a valid name and age (non-empty values).",
    });
    return;
  }
  if (+enteredAge < 1) {
    setError({
      title: "Invalid age",
      message: "Please enter a valid age (> 0).",
    });
    return;
  }
  props.onAddUser(enteredUsername, enteredAge);
  nameInputRef.current.value = null;
  ageInputRef.current.value = null;
};
```

- 즉각적으로, 실시간으로 값에 대한 피드백이 필요하다 > 제어 컴포넌트 사용
- 즉각적인 피드백이 불필요하고 제출시에만 값이 필요하다, 불필요한 렌더링과 값 동기화가 싫다 > 비제어 컴포넌트 사용
