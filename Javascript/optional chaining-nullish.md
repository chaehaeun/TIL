## 옵셔널체이닝 / null 병합 연산자

### 옵셔널체이닝 연산자 ?.

- ES11에서 도입된 최신 문법
- 좌항 피연산자가 **null 또는 undefined인 경우 undefined를 변환**하고, 그렇지 않으면 우항 프로퍼티 참조를 이어간다.

```javascript
var str = "";

// 문자열의 길이를 참조, 좌항 피연산자가 falsy 값이지만
// null 이나 undefined가 아니기 때문에 우항의 프로퍼티를 참조한다
var length = str?.length;
console.log(length); // output : 0
```

### null 병합 연산자 ??

- 옵셔널체이닝 연산자와 같이 ES11에 도입된 문법이다.

```javascript
// 좌항의 피연산자가 null 또는 undefined면 우항의 피연산자를 반환하고,
// 그렇지 않으면 좌항의 피연산자를 반환한다.
var foo = null ?? "default string";
console.log(foo); // output : "default string";
```
