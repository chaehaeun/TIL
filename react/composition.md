## Composition 컴포넌트

컴포넌트 안에 다른 컴포넌트를 담는 방식! 범용적인 **박스 역할을 하는 컴포넌트**에 자주 쓰이는 방식이다.
![](https://velog.velcdn.com/images/chaehe_3210/post/44861bd3-60b4-475f-b372-a926bce03dee/image.png)
이렇게 생긴 결과물을 만든다고 할 때, 자세히 보면 큰 박스와 내부의 작은 컴포넌트 하나하나에 `box-shadow`와 같은 정도의 `border-radius`가 입력되어있는 것을 볼 수 있다.

이런 때엔 컴포넌트마다 따로 같은 값을 넣어주는 것보다는 저런 스타일을 가지고 있는 박스 역할의 컴포넌트를 만들어서 감싸주는 것이 조금 더 재사용성이 좋다.

다른 말로, 이러한 컴포넌트에서는 특수한 **children prop을 사용하여 자식 엘리먼트를 출력에 그대로 전달하는 것이 좋다.**

👇 컴포넌트 예시

```js
import React from "react";
import "./Card.css";

export default function Card(props) {
  const classes = "card " + props.className;
  // 자식 요소의 클래스네임을 가져오려고 이런 식으로 작성한 것이다 wow!
  return <div className={classes}>{props.children}</div>;
}
```

👇 적용 예시

```js
export default function ExpenseItem({ expense }) {
  return (
    <Card className="expense-item">
      <ExpenseDate date={expense.date} />
      <div className="expense-item__description">
        <h2>{expense.title}</h2>
        <div className="expense-item__price">${expense.amount}</div>
      </div>
    </Card>
  );
}
```

👇 children 대신 자신만의 고유한 방식을 적용할 수도 있다!

```js
function SplitPane(props) {
  return (
    <div className="SplitPane">
      <div className="SplitPane-left">{props.left}</div>
      <div className="SplitPane-right">{props.right}</div>
    </div>
  );
}

function App() {
  return <SplitPane left={<Contacts />} right={<Chat />} />;
}
```
