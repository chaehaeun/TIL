## React.memo

![](https://velog.velcdn.com/images/chaehe_3210/post/52c3282c-1030-46af-8c62-f37bb0557057/image.png)

부모 컴포넌트의 프롭 중 하나의 값(이미지의 예시로 count)이 변경되면 자식 컴포넌트도 리렌더링 되게 되는데, 이와 관련 없는 컴포넌트(이미지에선 TextView) 또한 리렌더링이 된다. 성능의 문제가 생긴다는 의미.

> 의미없는 리렌더링을 줄이기 위해서는 어떻게 해야 될까?

❗ 자식 컴포넌트들에게 업데이트 조건을 걸어서, 자신이 전달받는 프롭이 변경될 때만 업데이트하게 한다.

### React.memo 란?

```js
const MyComponent = React.memo(function MyComponent(props) {
  /* props를 사용하여 렌더링 */
});
```

React.memo는 고차 컴포넌트(Higher Order Component)이다.

컴포넌트가 동일한 props로 동일한 결과를 렌더링해낸다면, React.memo를 호출하고 결과를 메모이징(Memoizing)하도록 래핑하여 경우에 따라 성능을 향상 시킬 수 있다. 즉, React는 컴포넌트를 렌더링하지 않고 마지막으로 렌더링된 결과를 재사용한다.(요약하면 프롭을 보내면 값이 바뀌었는지를 확인하고 변경되지 않은 값이라면 리렌더링하지 않는다.)

React.memo는 props 변화에만 영향을 준다. React.memo로 감싸진 함수 컴포넌트 구현에 useState, useReducer 또는 useContext 훅을 사용한다면, 여전히 state나 context가 변할 때 다시 렌더링된다.

```js
import React, { useEffect, useState } from "react";

const Textview = React.memo(({ text }) => {
  useEffect(() => {
    console.log(`Update :: text :: ${text}`);
  });
  // 텍스트의 값이 변경 되어도 카운트는 렌더링 하지 않는다.
  return <div>{text}</div>;
});
const CountView = React.memo(({ count }) => {
  useEffect(() => {
    console.log(`Update :: count :: ${count}`);
  });
  return <div>{count}</div>;
});

export default function OptimizeTest() {
  const [count, setCount] = useState(1);
  const [text, setText] = useState("");

  return (
    <div style={{ padding: 50 }}>
      <div>
        <h2>Count</h2>
        <CountView count={count} />
        <button onClick={() => setCount((prev) => prev + 1)}>+</button>
      </div>

      <div>
        <h2>text</h2>
        <Textview text={text} />
        <input
          type="text"
          value={text}
          onChange={(e) => setText(e.target.value)}
        />
      </div>
    </div>
  );
}
```

---

### 객체 비교

props가 갖는 복잡한 객체에 대하여 얕은 비교만을 수행하는 것이 기본 동작이다. 다른 비교 동작을 원한다면, 두 번째 인자로 별도의 비교 함수를 제공하면 된다.

```js
import React, { useEffect, useState } from "react";

const CounterA = React.memo(({ count }) => {
  useEffect(() => {
    console.log(`CounterA update - count : ${count}`);
  });
  return <div>{count}</div>;
});

const CounterB = React.memo(({ obj }) => {
  useEffect(() => {
    console.log(`CounterB update - count : ${obj.count}`);
  });
  // countB가 리렌더링돼서 출력되는 이유
  // obj가 객체라서. 자바스크립트는 객체를 얕은 비교를 하기 때문에
  return <div>{obj.count}</div>;
});

export default function OptimizeTest() {
  const [count, setCount] = useState(1);
  const [obj, setObj] = useState({
    count: 1,
  });

  return (
    <div style={{ padding: 50 }}>
      <div>
        <h2>Counter A</h2>
        <CounterA count={count} />
        <button onClick={() => setCount(count)}>A button</button>
      </div>
      <div>
        <h2>Counter B</h2>
        <CounterB obj={obj} />
        <button
          onClick={() =>
            setObj({
              count: obj.count,
            })
          }
        >
          B button
        </button>
      </div>
    </div>
  );
}
```

이런 경우, 객체는 얕은 복사를 하게 되기 때문에 버튼을 누를 때마다 새로운 메모리 공간이 만들어진다.(값이 서로 같지 않으므로 계속 렌더링 된다는 말)
![](https://velog.velcdn.com/images/chaehe_3210/post/03717061-9b22-4724-b888-c54054e68e23/image.png)

같은 메모리 공간을 참조하게 되면 '같다'의 결괏값이 나온다.
![](https://velog.velcdn.com/images/chaehe_3210/post/a67226da-aca4-4abd-8fb1-d9deebd13d95/image.png)

```JS
import React, { useEffect, useState } from "react";

const CounterA = React.memo(({ count }) => {
  useEffect(() => {
    console.log(`CounterA update - count : ${count}`);
  });
  return <div>{count}</div>;
});

const CounterB = ({ obj }) => {
  useEffect(() => {
    console.log(`CounterB update - count : ${obj.count}`);
  });
  return <div>{obj.count}</div>;
};

const areEqual = (prevProps, nextProps) => {
  // 프리브프롭스 : 이전 클릭의 값 -> 1
  // 넥스트프롭스 : 다음 클릭의 값 -> 1
  return prevProps.obj.count === nextProps.obj.count;

  //return true; // 이전 프롭스와 현재 프롭스가 같다 -> 리렌더링을 일으키지 않게 된다
  //결과 값이 true나 false를 반환하게 코드를 짜야 된다는 의미
};

const MemoizedCounterB = React.memo(CounterB, areEqual);
// 메모이징B를 클릭한 후 카운터 B에서 반환된 컴포넌트의 카운트 값이
//이전에 클릭해서 렌더링된 컴포넌트의 카운트 값과 같다는 의미.
// 전과 후의 카운트 값이 같기 때문에 렌더링을 하지 않는다.

export default function OptimizeTest() {
  const [count, setCount] = useState(1);
  const [obj, setObj] = useState({
    count: 1,
  });

  return (
    <div style={{ padding: 50 }}>
      <div>
        <h2>Counter A</h2>
        <CounterA count={count} />
        <button onClick={() => setCount(count)}>A button</button>
      </div>
      <div>
        <h2>Counter B</h2>
        <MemoizedCounterB obj={obj} />
        <button
          onClick={() =>
            setObj({
              count: obj.count,
            })
          }
        >
          B button
        </button>
      </div>
    </div>
  );
}
```
