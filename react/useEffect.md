## useEffect에 대해서

### 생애주기(Lifecycle)?

![](https://velog.velcdn.com/images/chaehe_3210/post/4431b4bb-3989-4341-995f-5f4f0a311cfb/image.png)

#### 리액트의 생애주기

- 탄생 -> 화면에 나타나는 것 **Mount** (ex. Task A 초기화 작업)
- 변화 -> 업데이트(컴포넌트가 변하는 순간,리렌더) **Update** (ex. Task B 예외 처리 작업)
- 죽음 -> 화면에서 사라짐 **UnMount** (ex. Task C 메모리 정리 작업)

#### useEffect 기본적인 사용 방법

```js
import React, { useEffect } from "react";

useEffect(() => {
  // todo 콜백함수가 들어갈 곳

  return () => {
    // cleanup 함수
  };
}, []);
```

**두번째 파라미터에 있는 빈 배열은 뭔가요?**

#### Dependency Array(의존성 배열)

이 배열 내에 들어있는 **값이 변화하면 콜백 함수가 실행**된다.

`[]` : 이펙트에 리액트 데이터 흐름에 관여하는 어떠한 값도 사용하지 않겠다는 뜻

1. **의존성 배열(deps)이 없을 때**

컴포넌트가 렌더링 될 때(mount)마다 실행이 된다. 보통 이렇게 사용하면 데이터 페칭이 무한 루프에 빠지기 때문에, 지양하는 것이 좋다.

2. **deps가 빈 배열일 때**
   컴포넌트가 첫번째 렌더링이 일어날 때(mount)만 함수를 호출한다. fetch와 같은 서버에서 데이터를 요청해서 받아올 때와 같은 상황에서 많이 쓰인다.

3. **deps에 특정 props, state가 담겼을 때**
   deps에 props나 state를 전달하면 그 값이 변경(u)될 때마다 콜백 함수를 실행한다.

### side effect란?

데이터 가져오기, 구독(subscription) 설정하기, 수동으로 React 컴포넌트의 DOM을 수정하는 것 등 **React 컴포넌트가 화면에 렌더링된 이후에 비동기로 처리 되어야 하는 부수적인 효과.**

### cleanup 함수

`useEffect`에서는 함수를 반환할 수 있는데, 이것을 `cleanup` 함수라고 부른다. cleanup 함수는 useEffect에 대한 뒷정리를 해주는 것이라고 생각하면 되는데, deps가 비어있는 경우에는 컴포넌트가 사라질 때 cleanup 함수가 호출된다.
