### useCallback()

컴포넌트 실행 전반에 걸쳐 함수를 저장할 수 있게 해주는 훅. 앱의 실행마다 함수를 재생성할 필요가 없다는 것을 알리는 훅이다.

```js
let obj1 = {};
let obj2 = {};

obj1 === obj2; // output false

obj2 = obj1; // {}

obj1 === obj2; // output true
```

useCallback이 하는 일이 이러한 과정.
함수를 리액트의 내부 저장공간에 저장해서 함수가 실행될 때마다 재사용할 수 있게 된다. 주의할 점은 함수 안에서 사용하는 state나 props가 있다면 의존성 배열에 포함시켜야 된다.

```js
const btnHandler = useCallback(() => {
  setShowParagraph((prev) => !prev);
}, [setShowParagraph]);
```

이 상황에서는 `setShowParagraph` (state를 변경 시켜주는 콜백 **'함수'**) 를 deps 안에 넣을 수 있으나 `setShowParagraph` 또한 `useCallback()`를 통해서 바뀔 일이 없기 때문에 굳이 적을 필요가 없다. `(prev) => !prev` 이 부분 또한 `setShowParagraph`에 전달된 함수라는 것을 기억해야 됨!!

```js
const btnHandler = useCallback(() => {
  setShowParagraph((prev) => !prev);
}, []);
```

빈 deps는 이 콜백 함수는 절대 변경되지 않을 것이라고 리액트에 알리게 된다. 따라서 App 컴포넌트가 다시 렌더링 되어도 항상 같은 함수 객체가 사용된다.

<br/>

#### 의존성 배열은 왜 필요한 걸까?

클로저 때문임. 이 환경에서 사용할 수 있는 값에 클로저를 생성하게 됨

```js
const [showParagraph, setShowParagraph] = useState(false);
const [allowToggle, setAllowToggle] = useState(false);

console.log("App RUNNING!");

const btnHandler = useCallback(() => {
  if (allowToggle) {
    setShowParagraph((prev) => !prev);
  }
}, []);

const allowToggleHandler = () => {
  setAllowToggle(true);
};

return (
  <div className="app">
    <h1>Hi there!</h1>
    <DemoOutput show={showParagraph} />
    <Button onClick={allowToggleHandler}>Allow Toggling</Button>
    <Button onClick={btnHandler}>Toggle Paragraph!</Button>
  </div>
);
```

이 경우 토글 버튼을 눌러도 아무 반응이 없다. allowToggle이라는 상수를 함수를 정의할 때 사용하기 위해, **상수를 저장해버렸기 때문**. useCallback으로 함수 자체가 재생성 되지 않으므로 allowToggle은 **저장 된 시점의 값**만 계속 가지게 된다.(최신의 값을 불러오지 못한다는 말)

```js
const btnHandler = useCallback(() => {
  if (allowToggle) {
    setShowParagraph((prev) => !prev);
  }
}, [allowToggle]);

const allowToggleHandler = () => {
  setAllowToggle(true);
};
```

allowToggle의 값이 바뀔 때마다 함수는 재생성 되는 것을 원하기 때문에, 계속해서 최신 값을 불러오기 위해서는 deps 내에 함수 안에서 사용하는 상태 또는 props를 입력해야 된다.
