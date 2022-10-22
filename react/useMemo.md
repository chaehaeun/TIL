### useMemo

#### memoization

이미 계산해 본 결과를 기억해 두었다가
동일한 계산을 시키면, 다시 연산하지 않고 기억해 두었던 데이터를 반환시키게 하는 연산 최적화 과정.

**마치 시험을 볼 때, 이미 풀어본 문제는 다시 풀어보지 않아도 답을 알고 있는 것과 유사하다.**

예시를 통해 알아보자

```js
const getDiaryAnalysis = () => {
  // 왜 새로고침하면 콘솔이 두번 찍힐까?
  // 데이터의 디폴트 값은 빈 배열임
  // 화면이 렌더링될 때 일기 분석을 한 번 했을 것임
  // 그리고 useEffect를 통해서 data 값이 바뀌고
  // 스테이트의 값이 바뀌었기 때문에 한 번 더 렌더링하게 됨
  console.log("일기 분석 시작");

  const goodCount = data.filter((item) => item.emotion >= 3).length;
  const badCount = data.length - goodCount;
  const goodRatio = (goodCount / data.length) * 100;
  return { goodCount, badCount, goodRatio };
};

// 지역 변수를 가지고 오고 싶을 땐 비구조화 할당을 써도 되겠구나
const { goodCount, badCount, goodRatio } = getDiaryAnalysis();
```

`getDiaryAnalysis()` 라는 함수를 호출하면, 현재 상태(데이터) 내의 감정 점수를 가져와서 좋은 기분, 나쁜 기분, 좋은 기분의 퍼센트를 구할 수 있다.

지금 상태에서는 데이터의 개수를 건드리지 않는 수정 기능을 쓸 때마다 `getDiaryAnalysis()` 를 호출해서 연산을 하게 되는데(데이터의 상태가 변화하기 때문에 컴포넌트를 계속 리렌더링하기 때문!!)

이 함수는 수정 기능을 써도 값이 변화하지 않기 때문에, 리렌더링 될 때마다 연산을 새로 하는 것은 비효율적이다. **값을 기억해놓고, 특정 값이 변화할 때만 이 함수를 연산하게 되는 것이 가장 바람직할 것 같다.**

> 이런 상황에서 사용되는 것이 **useMemo()** 이다!!

```js
const getDiaryAnalysis = useMemo(() => {
  console.log("일기 분석 시작");

  const goodCount = data.filter((item) => item.emotion >= 3).length;
  const badCount = data.length - goodCount;
  const goodRatio = (goodCount / data.length) * 100;
  return { goodCount, badCount, goodRatio };
  // 데이터렝쓰의 값이 변화할 때만 useMemo 안의 함수를 연산하게 된다.
}, [data.length]);

const { goodCount, badCount, goodRatio } = getDiaryAnalysis;
```

이런 식으로 함수를 useMemo로 감싸주고, 두번째 파라미터에 감지해야 되는 값을 입력하면, 입력되어 있는 값이 변화할 때만 useMemo 안의 함수를 연산하게 된다.

🚨 여기서 주의할 점은, 이 상황에서 `getDiaryAnalysis` 는 더이상 함수 자체가 아닌 함수를 가지고 있는 변수이므로, 호출할 때는 `()` 를 붙이면 안된다. 흔히 하는 실수라고 하니 기억해놓자!!
