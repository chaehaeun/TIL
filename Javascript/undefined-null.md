## undefined와 null의 미세한 차이

**null** : 어떤 값이 의도적으로 비어있음을 표현하며 불리언 연산에서는 거짓으로 취급함. null은 식별되지 않은 것을 표현한다.

즉, 변수가 아무런 객체를 가리키지 않음을 표현한다는 말. API에서는 null을 종종 관련된 객체가 존재하지 않을 때 그 객체 대신 사용하기도 한다.

**undefined** : undefined는 전역 객체의 속성이다. 즉, 전역 스코프에서의 변수이다.

값을 할당하지 않은 변수는 undefined 자료형이다. 또, 메서드와 선언도 평가할 변수가 값을 할당받지 않은 경우에 undefined를 반환한다. 함수는 값을 명시적으로 반환하지 않으면 undefined를 반환한다.

```js
typeof null; // "object" (하위호환 유지를 위해 "null"이 아님)
typeof undefined; // "undefined"
null === undefined; // false
null == undefined; // true
null === null; // true
null == null; // true
!null; // true
isNaN(1 + null); // false
isNaN(1 + undefined); // true
```

null의 타입이 object로 나온다는 게 정말 기묘한 일임...( 초기 자바스크립트의 버그인데 하위호환을 위해서 걍 놔뒀다고 함 )
