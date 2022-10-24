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
