## useRef

리액트를 사용하는 프로젝트에서 DOM 을 직접 선택해야 하는 상황이 발생 할 때가 있다. (ex. 특정 엘리먼트의 크기를 가져와야 한다든지, 스크롤바 위치를 가져오거나 설정해야 된다든지, 또는 포커스를 설정해줘야 된다든지)

이럴 때 사용하는 것이 `useRef`이다.

```js
const txtArea = useRef();
```

```js
// 코드
const handleEdit = () => {
  if (localContent.length < 5) {
    txtArea.current.focus(); // 👈
    return;
  }
  if (window.confirm(`${id}번째 일기를 수정하시겠습니까?`)) {
    onEdit(id, localContent);
    toggleIsEdit();
  }
};

// DOM
<div className="content">
  {isEdit ? (
    <textarea
      value={localContent}
      ref={txtArea} // 👈 이런 식으로 DOM에 접근할 수 있다
      onChange={(e) => setLocalContent(e.target.value)}
    ></textarea>
  ) : (
    <>{content}</>
  )}
</div>;
```

### forwardRef

컴포넌트에 useRef를 적용시키는 방법이다.

```js
export default function MealItemForm(props) {
  const submitHandler = (e) => {
    e.preventDefault();
  };

  const amountInputRef = useRef();

  return (
    <form className={classes.form} onSubmit={submitHandler}>
      <Input
        input={{
          ref: { amountInputRef },
          id: `amount_${props.id}`,
          type: "number",
          min: "1",
          max: "5",
          step: "1",
          defaultValue: "1",
        }}
        labelVal="Amount"
      />
      <button>+ Add</button>
    </form>
  );
}
```

현재 상태에서는 input의 ref를 사용할 수가 없기 때문에 forwardRef를 사용해야 한다.

```js
const Input = React.forwardRef((props, ref) => {
  return (
    <div className={classes.input}>
      <label htmlFor={props.input.id}>{props.label}</label>
      <input ref={ref} {...props.input} />
    </div>
  );
});

export default Input;
```

컴포넌트를 `React.forwardRef`로 감싸주고, 프롭으로 ref를 가져온 뒤, 값을 가져와야 되는 요소에 ref를 달아주면 된다!

**DOM을 직접 제어하는 것이기 때문에 아무데나 ref를 남발하는 것은 권장하지 않는다!**
