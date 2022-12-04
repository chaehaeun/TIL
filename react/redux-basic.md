## ✨ 리덕스 찍먹

### 🎇 리액트 컨텍스트 vs 리덕스

계속 궁금했던 부분이다!
리액트 자체에서 제공하는 컨텍스트 기능이 있는데 왜 굳이 리덕스를 써야될까?

리액트 컨텍스트는 몇 개의 잠재적인 단점이 있음

1. 설정과 상태 관리가 매~우 복잡해질 수도 있음 (작은 프로젝트에선 문제될 것이 없지만 대형 프로젝트에선 머리 터진다고 함)

![컨텍스트로 지옥 표현하기](https://velog.velcdn.com/images/chaehe_3210/post/f39d9d1b-8e68-44e7-82e9-90c12e6ff381/image.png)

바로 요런 꼬라지가 날 수 있다는 것...
**그럼 하나의 컨텍스트에서 관리하면 되지 않을까?** 🤔

![성능bye](https://velog.velcdn.com/images/chaehe_3210/post/061da210-5bf8-45b5-9d68-d5777b85adbf/image.png)

그럼 이제 한 컨텍스트에서 관리하는 스태이트가 너무 많아져서 또 유지 관리가 어려워짐 한 컨텍스트가 인증, 테마, 사용자 입력, 모달 표시 여부 등등 굉~장히 많은 것을 관리하게 될 것

이걸 수정하려고 하면 이제 또 위의 컨텍스트 지옥도가 펼쳐진다...

2. 성능 이슈
   ![](https://velog.velcdn.com/images/chaehe_3210/post/90367d2e-6dbf-418a-8cd1-7adbca492baf/image.png)

테마를 변경하거나 인증 같은 빈도가 낮은 업데이트에는 괜찮지만, 자주 변경되는 상태 관리에는 별로임

But 리덕스는 유동적인 상태 관리 라이브러리라 이런 문제가 별로 없음

### 🎇 리덕스의 작동 방식

![리덕스 작동방식](https://velog.velcdn.com/images/chaehe_3210/post/5f618482-a9db-4022-839f-fae02a619e29/image.png)

리덕스는 애플리케이션 안의 하나의 중앙 상태 저장소를 가지고 있음. 한 장소에 전체 앱의 모~든 상태를 저장함.

그 저장소에 인증 상태, 테마, 사용자의 입력 상태 들들 다 저장 가능

### 🎇 간단 리덕스 기능 찍먹

```js
const redux = require("redux");

const counterReducer = (state = { counter: 0 }, action) => {
  // 기본적인 건 useReducer랑 비슷해보이는 것 같기도..?
  if (action.type === "increment") {
    return {
      counter: state.counter + 1,
    };
  }
  if (action.type === "decrement") {
    return {
      counter: state.counter - 1,
    };
  }

  return state;
};

const store = redux.legacy_createStore(counterReducer);
// 강의는 조금 예전 버전인지 createStore 기능을 쓰려면 legacy_createStore라고 써줘야 되더라

const counterSubscriber = () => {
  const latestState = store.getState();
  // 최신 상태의 스냅샷을 내뱉는 함수

  console.log(latestState);
};

store.subscribe(counterSubscriber);
// dispatch로 스테이트가 변할 때 같이 트리거 됨

store.dispatch({ type: "increment" }); // 1
store.dispatch({ type: "decrement" }); // 0
```
