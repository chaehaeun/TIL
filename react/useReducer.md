## useReducer

```js
import { useReducer } from 'react';

function reducer(state, action) {
  // ...
}

function MyComponent() {
  const [state, dispatch] = useReducer(reducer, 초기값);
  // ...
```

**useState의 대체 함수이다.**
이 Hook 함수를 사용하면 컴포넌트의 상태 업데이트 로직을 컴포넌트에서 분리시킬 수 있다. 상태 업데이트 로직을 컴포넌트 바깥에 작성 할 수도 있고, 심지어 다른 파일에 작성 후 불러와서 사용 할 수 있음.

### dispatch

`useReducer`에 의해서 반환되는 디스패치 함수는 state를 **다른 값으로 업데이트하고 리렌더를 트리거할 수 있게 해준다**. 인자로는 전달할 함수와, 스테이트값을 넣을 수 있다.

```js
const [state, dispatch] = useReducer(reducer, 스테이트 초기값);

function handleClick() {
  dispatch({ type: 'incremented_age', 리듀서 함수로 전달시킬 데이터 });
  // ...
```

첫번째 인자로 들어간 값을 리듀서 함수의 매개변수로 써서,

```js
function reducer(state, action) {
  // ...
}
```

🚨 **디스패치는 다음 렌더링에서 변화될 스테이트 값을 업데이트한다.** 그렇기 떄문에 디스패치를 호출했다고 해서 바로 아래에 스테이트를 찍어봐야 현재 상태의 스테이트 값만 나오니 주의!!

다음은 인강을 보면서 `useReducer`를 사용해본 예제이다

```js
const reducer = (state, action) => {
  switch (action.type) {
    case "INIT": {
      return action.data;
    }
    case "CREATE": {
      const created_date = new Date().getTime();
      const newItem = {
        ...action.data,
        created_date,
      };
      return [newItem, ...state];
    }
    case "REMOVE": {
      return state.filter((item) => item.id !== action.targetId);
    }
    case "EDIT": {
      return state.map((item) =>
        item.id === action.targetId
          ? { ...item, content: action.newContent }
          : item
      );
    }
    default:
      return state;
  }
};

function App() {
  const [data, dispatch] = useReducer(reducer, []);
  // ...
  const onCreate = useCallback((author, content, emotion) => {
    dispatch({
      type: "CREATE",
      data: { author, content, emotion, id: dataId.current },
    });

    dataId.current += 1;
  }, []);
  // ...
}
```

앱 컴포넌트 내부에서 `useReducer`를 호출하고, 상태 변화가 필요한 곳에서 `dispatch`를 가져와 type과 현재 데이터(payload)를 `reducer`함수에 매개변수로 전달한다. 그리고 컴포넌트 외부에 있는 `reducer`함수를 실행하는데, 스위치 문을 이용해서 조건에 해당하는 액션을 실행한다.

디스패치를 사용할 때 인자를 하나만 넘기는 예제도 있고, 두 개를 넘기는 예제도 있어서 조금 더 검색해봤다.

ex1) action type만 정의하여 사용

```js
<button onClick={() => dispatch({ type: "INCREMENT" })}>증가</button>
```

ex2) action type과, 데이터를 정의하여 사용

```js
<button onClick={() => dispatch({ type: "INCREMENT", payload: 1 })}>
  증가
</button>
```

이제 여기서 리듀서로 넘어가고,

ex1) action type만 정의하여 사용

```js
function reducer(state, action) {
  switch (action.type) {
    case "INCREMENT":
      return { count: state.count + 1 };
    case "DECREMENT":
      return { count: state.count - 1 };
    default:
      throw new Error("unsupported action type: ", action.type);
  }
}
```

ex2) action type과, 데이터를 정의하여 사용

```js
function reducer(state, action) {
  switch (action.type) {
    case "INCREMENT":
      return { count: state.count + action.payload };
    case "DECREMENT":
      return { count: state.count - action.payload };
    default:
      throw new Error("unsupported action type: ", action.type);
  }
}
```

리덕스를 쓰게 되면 더욱 자주 보게될 사용법이라는데, 머리에 개념을 잘 넣어놓는 게 좋겠다.
