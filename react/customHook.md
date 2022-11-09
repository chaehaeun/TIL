### custom hook

일반적인 함수지만, 내부에서 상태를 설정할 수 있는 로직을 포함한 재사용 가능 함수. 커스텀 훅은 다른 커스텀 훅을 포함한 다른 리액트 훅을 사용할 수 있다. useState나 useReducer를 통해 관리하는 상태를 활용 가능하다는 말. (물론 useEffect에도 접근할 수 있다.)

#### 작성방법

![](https://velog.velcdn.com/images/chaehe_3210/post/3b38af03-7b97-4474-b803-35212c5948f4/image.png)

```js
const useCounter = (something = true) => {
  //
};
```

관례에 따르면 커스텀훅은 hook이라는 폴더를 만든 뒤 그 안에 작성하는 것이 일반적이다. 이때 이름의 시작은 `use`로 통일해서 리액트에게 이 함수가 커스텀 훅임을 알려주어야 한다.

그러면 리액트는 이 함수를 훅의 규칙에 따라 사용하게 된다. 즉, 이 커스텀 훅을 내장 훅과 같은 방식으로 사용하겠다고 인식하는 것.

#### 사용 예시

![](https://velog.velcdn.com/images/chaehe_3210/post/5ad81cae-919a-4986-9282-78b7ce58fa2b/image.png)
1초마다 +1을 해주는 카운터와 -1을 하는 카운터
+1 과 -1 을 한다는 차이 외엔 똑같은 로직을 가질 것이기 때문에 간단하게 커스텀훅을 써보기에 좋은 예시인듯.

```js
// +1 카운터 컴포넌트
import React from "react";
import useCounter from "../hooks/use-counter";

import Card from "./Card";

const ForwardCounter = () => {
  const counter = useCounter(true);

  return <Card>{counter}</Card>;
};

export default ForwardCounter;

// -1 카운터 컴포넌트
import React from "react";
import useCounter from "../hooks/use-counter";

import Card from "./Card";

const BackwardCounter = () => {
  const counter = useCounter(false);

  return <Card>{counter}</Card>;
};

export default BackwardCounter;
```

파라미터로 true를 가져가면 counter 스테이트를 1초마다 이전 값에서 +1을 하고, false를 가져가면 이전 값에서 -1 을 하겠다는 로직.

```js
import { useEffect, useState } from "react";

const useCounter = (something = true) => {
  const [counter, setCounter] = useState(0);

  useEffect(() => {
    const interval = setInterval(() => {
      something
        ? setCounter((prevCounter) => prevCounter + 1)
        : setCounter((prevCounter) => prevCounter - 1);
    }, 1000);

    return () => clearInterval(interval);
  }, [something]);

  return counter;
};

export default useCounter;
```

중복되는 코드를 줄일 수 있는 좋은 리팩토링 방법이당!
