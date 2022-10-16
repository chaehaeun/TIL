## 클로저

### 렉시컬 스코프

클로저를 이해하려면 렉시컬 스코프(자바스크립트가 어떻게 변수의 유효범위를 지정하는지)에 대해 먼저 이해해야 된다고 한다.

```js
var x = 1;

function foo() {
  var x = 10;
  bar();
}

function bar() {
  console.log(x);
}

foo(); // ?
bar(); // ?
// 출처 : https://poiemaweb.com/js-execution-context
```

이 상황에서 두 함수의 결과는 어떻게 나올까? 별 생각 없이 둘 다 10이 나오지 않을까? 했다...

정답은 1이다.

자바스크립트는 렉시컬 스코프를 따른다. 렉시컬 스코프는 **<span style="color: rgb(255, 54, 104)">함수를 어디서 호출하는지가 아니라 어디에 선언하였는지에 따라 결정된다.</span>**
foo()와 bar()의 상위 스코프는 전역 스코프이기 때문에 전역 변수 x의 값인 1을 두번 출력하게 된다.

또 다른 예시로 살펴보자.

```js
function init() {
  var name = "Mozilla"; // name은 init에 의해 생성된 지역 변수이다.
  function displayName() {
    // displayName() 은 내부 함수이며, 클로저다.
    alert(name); // 부모 함수에서 선언된 변수를 사용한다.
  }
  displayName();
}
init();
// 출처 : MDN 클로저 문서
```

`displayName()` 내부엔 자신만의 지역 변수가 없지만, 함수 내부에서 외부 함수의 변수에 접근할 수 있기 때문에 `init()`에서 선언된 변수 `name`에 접근할 수 있다. 만약 `displayName()`이 자신만의 `name`변수를 가지고 있다면, name 대신 `this.name`을 사용해야 된다.

#### let 과 const

es6에서 자바스크립트는 블록 스코프 변수를 생성할 수 있도록 let과 const 선언과 함께 시간상 사각지대를 도입했다.

> 시간상 사각지대가 뭔데?

let 과 const 변수는 초기화하기 전에는 읽거나 쓸 수 없다. 초기화 전에 접근을 시도하면 레퍼런스에러가 발생한다. "시간상" 사각지대인 이유는 코드의 작성 순서(위치)가 아니라 코드의 실행 순서(시간)에 의해 형성되기 때문.

```js
{
  // TDZ가 스코프 맨 위에서부터 시작
  const func = () => console.log(letVar); // OK

  // TDZ 안에서 letVar에 접근하면 ReferenceError

  let letVar = 3; // letVar의 TDZ 종료
  func(); // TDZ 밖에서 호출함
}
```

이런 경우 `letVar` 코드가 변수에 접근하는 함수보다 아래에 위치하지만, 함수의 호출 시점이 사각지대 밖이기 때문에 정상적으로 작동한다.

다시 돌아와서, 클로저는 모든 스코프의 변수를 캡쳐할 수 있다.
