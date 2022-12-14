## ✨ 리덕스 기본적인 작성 순서

### 🎇 설치

```bash
yarn add react-redux
yarn add @reduxjs/toolkit
```

### 🎇 slice 기본 틀

```js
import { createSlice } from "@reduxjs/toolkit";

const uiSlice = createSlice({
  name: "ui",
  initialState: { cartIsVisible: false },
  reducers: {
    toggle(state) {
      state.cartIsVisible = !state.cartIsVisible;
    },
  },
});

export const uiAction = uiSlice.actions;
export default uiSlice;
```

### 🎇 기본 파일 index.js

```js
import { configureStore } from "@reduxjs/toolkit";
import uiSlice from "./ui-slice";

const store = configureStore({
  reducer: {
    ui: uiSlice.reducer,
  },
});

export default store;
```

### 🎇 프로바이더를 넣을 index.jsx

```jsx
import ReactDOM from "react-dom/client";
import { Provider } from "react-redux";

import "./index.css";
import App from "./App";
import store from "./store";

const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(
  <Provider store={store}>
    <App />
  </Provider>
);
```

### 🎇 액션을 dispatch할 컴포넌트

```jsx
import { useDispatch } from "react-redux";
import { uiAction } from "../../store/ui-slice";
import classes from "./CartButton.module.css";

const CartButton = (props) => {
  const dispatch = useDispatch();

  const toggleCartHandler = () => {
    dispatch(uiAction.toggle());
  };

  return (
    <button className={classes.button} onClick={toggleCartHandler}>
      <span>My Cart</span>
      <span className={classes.badge}>1</span>
    </button>
  );
};

export default CartButton;
```

### 🎇 실제 렌더링을 하는 App.jsx

```jsx
import { useSelector } from "react-redux";
import Cart from "./components/Cart/Cart";
import Layout from "./components/Layout/Layout";
import Products from "./components/Shop/Products";

function App() {
  const showCart = useSelector((state) => state.ui.cartIsVisible);
  return (
    <Layout>
      {showCart && <Cart />}
      <Products />
    </Layout>
  );
}

export default App;
```
