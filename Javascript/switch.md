### switch

쓰는 방식은 알고 있지만 자주 까먹고 다시 검색하게 되는 그 조건문이다.
뭐 if나 while 대충 쓸 줄 알고 간단하게 쓸 때는 삼항연산자도 쓰고, 어떨 때는 논리연산자로 조건문 비슷하게 쓰기도하고 그러니 별로 중요성을 못 느꼈는데 얼마 전 리액트 강의를 보니 reducer를 사용할 때 좋아보이더라.

그래서 한 번 확실하게 보고가자 싶어서 이것저것 봤다.

> if…else 문의 조건식은 반드시 불리언 값으로 평가되지만 switch 문의 표현식은 불리언 값보다는 문자열, 숫자 값인 경우가 많다. if…else 문은 논리적 참, 거짓으로 실행할 코드 블록을 결정한다. **switch 문은 논리적 참, 거짓보다는 다양한 상황(case)에 따라 실행할 코드 블록을 결정할 때 사용한다.**

#### 문법

```js
switch (표현식) {
  case 표현식1:
    switch 문의 표현식과 표현식1이 일치하면 실행될 문;
    break;
  case 표현식2:
    switch 문의 표현식과 표현식2가 일치하면 실행될 문;
    break;
  default:
    switch 문의 표현식과 일치하는 표현식을 갖는 case 문이 없을 때 실행될 문;
}
}
```

( break로 조건문을 탈출 시켜주는 것도 잊지 말자! )
default 문에는 break 문을 생략하는 것이 일반적이다. default 문은 switch 문의 가장 마지막에 위치하므로 default 문의 실행이 종료하면 switch 문을 빠져나간다. 따라서 별도로 break 문이 필요없다.

case 문은 반드시 단독으로 사용되어야 하는 것은 아니다!아래는 윤년인지 판별해서 2월의 일수를 계산하는 예제

```js
var year = 2000; // 2000년은 윤년으로 2월이 29일이다.
var month = 2;
var days = 0;

switch (month) {
  case 1:
  case 3:
  case 5:
  case 7:
  case 8:
  case 10:
  case 12:
    days = 31;
    break;
  case 4:
  case 6:
  case 9:
  case 11:
    days = 30;
    break;
  case 2:
    // 윤년 계산 알고리즘
    // 1. 년도가 4로 나누어 떨어지는 해는 윤년(2000, 2004, 2008, 2012, 2016, 2020…)
    // 2. 그 중에서 년도가 100으로 나누어 떨어지는 해는 평년(2000, 2100, 2200...)
    // 3. 그 중에서 년도가 400으로 나누어 떨어지는 해는 윤년(2000, 2400, 2800...)
    days = (year % 4 === 0 && year % 100 !== 0) || year % 400 === 0 ? 29 : 28;
    break;
  default:
    console.log("Invalid month");
}

console.log(days); // 29
```

요까지 출처 : 모던자스 딥다이브

<br/>

아래는 내가 봤던 리액트에서의 switch 활용 예시! ( `useReducer` )

```js
const reducer = (state, action) => {
  switch (action.type) {
    case "INIT": {
      return action.data;
    }
    case "CREATE": {
      const created_date = new Date().getTime();
      const newItem = {
        ...action.data,
        created_date,
      };
      return [newItem, ...state];
    }
    case "REMOVE": {
      return state.filter((item) => item.id !== action.targetId);
    }
    case "EDIT": {
      return state.map((item) =>
        item.id === action.targetId
          ? { ...item, content: action.newContent }
          : item
      );
    }
    default:
      return state;
  }
};

/////

const [data, dispatch] = useReducer(reducer, []);

const onCreate = useCallback((author, content, emotion) => {
  dispatch({
    type: "CREATE",
    data: { author, content, emotion, id: dataId.current },
  });
  dataId.current += 1;
}, []);

const onRemove = useCallback((targetId) => {
  dispatch({ type: "REMOVE", targetId });
}, []);

const onEdit = useCallback((targetId, newContent) => {
  dispatch({ type: "EDIT", targetId, newContent });
}, []);
```

useReducer 사용은 이전에 봤던 어떤 강의에선 하나하나 타입을 중첩 if문을 작성해서 판단하던데, switch를 쓰니까 가독성은 훨씬 좋아지는 것 같다. 아무래도 케이스 별로 착착 나뉘어져있으니?
