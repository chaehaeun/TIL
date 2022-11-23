## 자바스크립트의 자료형

자바스크립트는 객체 기반의 프로그래밍 언어이며, 자바스크립트를 구성하는 거의 모든 것(원시 값 제외)이 객체이다.

### 1. 객체 자료형

원시 자료형을 제외한, **속성과 메서드를 가질 수 있는 모든 것은 객체**이다.

#### ❗ 함수도 객체임!

**객체의 특성**을 모두 가지고 있기 때문에 함수 또한 '실행이 가능한 객체'이다. typeof 연산자를 사용하면 'function'을 출력함. 자바스크립트에서는 함수를 '일급 객체'에 속한다고 표현하기도 함.

🎇 [위키피디아 - 일급 객체란](https://ko.wikipedia.org/wiki/%EC%9D%BC%EA%B8%89_%EA%B0%9D%EC%B2%B4)
다른 객체들에 일반적으로 적용 가능한 연산을 모두 지원하는 객체를 가리킨다. 보통 함수에 인자로 넘기기, 수정하기, 변수에 대입하기와 같은 연산을 지원할 때 일급 객체라고 한다. 일급 객체의 특징은 아래의 4가지가 있다.

- 모든 요소는 함수의 실제 매개변수가 될 수 있다.
- 모든 요소는 함수의 반환 값이 될 수 있다.
- 모든 요소는 할당 명령문의 대상이 될 수 있다.
- 모든 요소는 동일 비교의 대상이 될 수 있다.

🎇 [MDN - 일급 함수](https://developer.mozilla.org/ko/docs/Glossary/First-class_Function)
자바스크립트 같이 함수를 다른 변수와 동일하게 다루는 언어는 일급 함수를 가졌다고 표현한다.

<br/>


### 2. 기본 자료형 (원시 자료형)

기본 자료형은 변경 불가능한 값(immutable value)이다. 단어 그대로 변경이 불가능한 데이터 타입이다.

기본 자료형의 종류 6가지가 있다.

- Boolean
- null
- undefined
- Number
- String
- Symbol

👇 객체에 대한 챕터이기 때문에 원시 타입에 대해서는 아래의 링크 참고
🎇 [poiemaweb - 데이터 타입](https://poiemaweb.com/js-data-type-variable)

원시 타입은 객체가 아니기 때문에 프로퍼티를 가질 수 없다.

```js
const c = 273;

c.sample = 10;
console.log(c); // 273
console.log(c.sample); // undefined
```

에러가 안 나기 때문에 왠지 프로퍼티가 추가된 것처럼 보이지만, 콘솔을 찍어보면 undefined가 나온다. 숫자형을 제외한 나머지 기본 자료형들도 해당.

#### ❗ 기본 자료형을 객체로 선언하기

null, undefined를 제외한 기본 자료형을 객체로 선언하는 방법도 존재한다.

```js
const 객체 = new 객체 자료 이름();
```

생성자를 사용해 기본 자료형을 객체로 생성 시키는 것이 가능!

<br/>

👇 생성자에 대해서는 아래의 링크 참고
🎇 [MDN - new operator](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/new)
🎇 [MDN - constructor(클래스의 개념을 개략적으로 공부하고 보는 것이 좋음)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Classes/constructor)

<br/>

```js
const num = new Number(10); // num이라는 이름을 가진 넘버타입의 새로운 객체를 생성한 것

typeof num; // object

num.sample = 10; // 샘플이라는 프로퍼티를 할당

console.log(num); // [Number: 10] { sample: 10 }
console.log(num + 3); // 13
console.log(num.valueOf()); // 10
```

new 를 사용해서 숫자를 생성하면 숫자와 관련된 연산자도 모두 쓸 수 있고, 속성과 메서드 또한 활용이 가능하다.

> 하지만!!!!!! 이런 식으로 래퍼 객체를 만드는 건 추천하지 않는다. 몇몇 상황에서 오히려 혼동을 불러일으키기 때문.

```js
alert(typeof 0); // "number"

alert(typeof new Number(0)); // "object"!
```

모던자바스크립트 튜토리얼 사이트에서 긁어온 예제. 0 이라는 숫자를 생성자로 만들어서 객체화를 시킨 건데,
객체는 if문이나 논리연산자 같은 논리 평가를 할 때 항상 참을 반환하기 때문에, 아래 예시에서 얼럿창은 무조건 열린다.

```js
let zero = new Number(0);

if (zero) {
  // 변수 zero는 객체이므로, 조건문이 참이 됩니다.
  alert("그런데 여러분은 zero가 참이라는 것에 동의하시나요!?!");
}
```

하지만 자바스크립트에서 숫자 0은 falsy한 값이기 때문에 혼동이 오게 된다.

<br/>

👇 관련 내용은 아래의 링크 참고
🎇 [모던자바스크립트.info - 원시값을 객체처럼 사용하기](https://ko.javascript.info/primitives-methods#ref-541)

<br/>

#### ❗ 기본 자료형의 일시적 승급

![](https://velog.velcdn.com/images/chaehe_3210/post/6fb32e36-6ce2-472d-88c8-8d32a9c0a9b1/image.png)
문자열은 기본 자료형이기 때문에 프로퍼티를 사용할 수 없는데, 뒤에 점을 찍으면 자동완성으로 메서드가 뜨는 이유가 뭘까?

```js
const str = "안녕하세요!";

str.sample = "실험";

console.log(str.sample); // undefined
```

자바스크립트는 기본 자료형의 속성과 메서드를 호출할 때 일시적으로 기본 자료형을 객체로 승급 시킨다. 다른 말로 원시값에 메서드를 호출하려 하면 임시적으로 객체가 만들어진다. 하지만 일시적인 것이기 때문에 실제로 프로퍼티가 추가 되지는 않는 것을 볼 수 있다.

> 📌 기본 자료형은 속성과 메서드를 사용할 수는 있지만, 속성과 메서드를 추가로 가질 수는 없다.

<br/>

#### ❗ 프로토타입으로 메서드 추가하기

prototype 객체에 속성과 메서드를 추가하면 모든 객체(와 모든 자료형)에서 해당 속성과 메서드를 사용할 수 있다.

```js
자료형 이름.prototype.메소드 이름 = function(){}
```

👇 사용 예시

```js
let str = "test";

String.prototype.myMethod = function () {
  return "myMethod";
};

console.log(str.myMethod()); // myMethod
console.log("string".myMethod()); // myMethod
console.dir(String.prototype);
```

우리가 자주 쓰는 String.includes(), String.charCodeAt() 같은 것들이 자바스크립트가 자체적으로 가지고 있는 프로토타입 메서드이다. (어제 낮의 논의 거리였던 .name이 왜 제대로 안 나오는가의 답도 이거임. name이라는 이름 자체 프로토타입 메서드로 사용되는 것)

하여튼 대충 직접 프로토타입 메서드를 만들고 싶으면

1. 제일 앞에 자료형 이름(Number, Object 등등) 뒤에 점 붙이고,
2. prototype 붙이고,
3. 메서드 이름 지어서 붙이고
4. 이 메서드가 담당할 어쩌고(함수가 될 수도 있고 그냥 원시값이 될 수도 있음)를 할당하면 이제 메서드로 사용할 수 있게 됨

👇 프로토타입에 대한 더 자세한 정보는 아래의 링크 참고 (하나하나 다 하려면 무한정으로 길어짐...)
🎇 [poiemaweb - 프로토타입](https://poiemaweb.com/js-prototype#41-%EA%B0%9D%EC%B2%B4-%EB%A6%AC%ED%84%B0%EB%9F%B4-%EB%B0%A9%EC%8B%9D%EC%9C%BC%EB%A1%9C-%EC%83%9D%EC%84%B1%EB%90%9C-%EA%B0%9D%EC%B2%B4%EC%9D%98-%ED%94%84%EB%A1%9C%ED%86%A0%ED%83%80%EC%9E%85-%EC%B2%B4%EC%9D%B8)
