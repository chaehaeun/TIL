## useContext

### Context API

![](https://velog.velcdn.com/images/chaehe_3210/post/d05e75ac-40da-46e9-81a2-c333cc8e4f24/image.png)
이전에 코드를 작성하면서 전역적으로 함수를 넘기기 위해 이 함수를 사용하지도 않는 컴포넌트에도 프롭으로 넘겼다. 이것을 props drilling(굉장히 비효율적이라는 뜻!!)라고 한다.

이를 해결하기 위해서 context 라는 개념이 생겼는데,

![](https://velog.velcdn.com/images/chaehe_3210/post/6aa8a5e8-c2fb-4f84-80a1-9f9aecf7f4c0/image.png)

이처럼 모든 데이터를 가진 컴포넌트(App)가 `Provider` 라는 공급자 역할을 하는 컴포넌트에게 자신이 가진 모든 데이터를 전달하고, 이 `Provider`가 **자신의 자손에 해당하는 모든 컴포넌트들에 직통으로 데이터를 넘긴다.**

### Context 생성 방법

```js
export const MyContext = React.creatContext(defaultValue);
```

<br/>

### Context Provider를 통한 데이터 공급

부모 컴포넌트에서 context 생성 후 App을 감싸고,

```js
<MyContext.Provider value = {전역으로 전달하고자 하는 값}>
  {/* 이 context 안에 위치할 자식 컴포넌트들 */}
<MyContext.Provider/>
```

자식 컴포넌트에서 `useContext`를 이용해 값을 받아온다

```js
import { MyContext } from "../App";

export default function DiaryList({ props }) {
  const diaryList = useContext(MyContext);
}
```

<br/>

### +) 컨텍스트 활용이 가늠이 잘 안돼서 예시 코드를 더 가져와봤다

```js
import React from "react";

const CartContext = React.createContext({
  items: [],
  totalAmount: 0,
  addItem: (item) => {},
  removeItem: (id) => {},
  // 트리 안에서 적절한 프로바이더를 못 찾았을 때 쓰이는 값
});

export default CartContext;
```

- CartContext라는 이름의 컨텍스트를 생성한다. 이때 괄호 안의 배열은 트리 안에서 적절한 프로바이더를 못 찾았을 때 디폴트 값으로 쓰이는 것

```js
const cartContext = {
  items: cartState.items,
  totalAmount: cartState.totalAmount,
  addItem: addItemToCartHandler,
  removeItem: removeItemFromCartHandler,
};

return (
  <CartContext.Provider value={cartContext}>
    {props.children}
  </CartContext.Provider>
);
// 여기서 cartState는 코드 상단에 선언해놓은 useReducer 스테이트임
```

- 컨텍스트를 활용할 부모 컴포넌트에 CartContext.Provider로 감싸준다.(이 경우는 감싸는 용도의 컴포넌트를 따로 생성했기 때문에 내부엔 props.children이 들어있다.)
- value 안에 전역으로 공급할 데이터를 프롭으로 넘긴다.(위 예시에서는 cartContext라는 객체를 생성해서 한 번에 데이터를 넘겼다.)
- 🚨 위 예시 코드는 전역에서 필요한 코드와 스테이트를 앱이 아닌 컨텍스트라는 컴포넌트에서 지정했음. 앱 컴포넌트의 가독성을 높이기 위한 것으로 보인다.

```js
const cartCtx = useContext(CartContext);

const numberOfCartItems = cartCtx.items.reduce((currentNumber, item) => {
  return currentNumber + item.amount;
}, 0);
```

- 자식 컴포넌트 내에서 useContext로 컨텍스트를 불러와서 사용한다! 끝!

<br/>

🚨 **리액트 컨텍스트는 스테이트 변경이 잦은 경우엔 적합하지 않다!**

로그인 기능 같은 자주 바뀌지 않는 기능을 구현할 때는 쓰기 좋음!

> <span style='color:#706efe;'>state가 자주 변경되어도 컨텍스트 기능을 쓰고 싶어!</span>

이런 경우에 쓰는 게 리덕스
