## 원시 타입과 참조 타입

### 원시 타입 데이터

immutable value. 단어 그대로 변경이 불가능한 데이터 타입이다. 객체가 아니면서 메서드도 가지지 않는다.

종류로는 `Boolean` `null` `undefined` `Number` `String` `Symbol` 이 있다.

이외의 모든 값은 변경이 가능한 값(mutable value)이다!
변경이 가능하다는 것이 처음엔 변수 재할당 같은 이야기인 줄 알았는데 착각하고 있었다.

<br/>

👇 원시 값 불변성 예제

```js
// 문자열 메서드는 문자열을 변형하지 않음
let bar = "baz";
console.log(bar); // baz
bar.toUpperCase();
console.log(bar); // baz

// 배열 메소드는 배열을 변형함
let foo = [];
console.log(foo); // []
foo.push("plugh");
console.log(foo); // ["plugh"]

// 할당은 원시 값에 새로운 값을 부여 (변형이 아님)
bar = bar.toUpperCase(); // BAZ
```

값을 교체는 가능하지만, 직접적으로 변형은 불가능하다.

<br/>

👇 JS가 원시값을 다루는 방법에 대한 예제

```js
// 원시 값
let foo = 5;

// 원시 값을 변경해야 하는 함수 정의
function addTwo(num) {
  num += 2;
}
// 같은 작업을 시도하는 다른 함수
function addTwo_v2(foo) {
  foo += 2;
}

// 원시 값을 인수로 전달해 첫 번째 함수를 호출
addTwo(foo);
// 현재 원시 값 반환
console.log(foo); // 5

// 두 번째 함수로 다시 시도
addTwo_v2(foo);
console.log(foo); // 5
```

값이 7이 아닌 이유가 뭘까? <br/>
① 함수 호출을 위해서 js는 식별자 foo의 값을 찾는다. <br/>
② 값을 찾은 후, js는 인수를 함수의 매개변수로 전달한다. <br/>
③ 함수 본문 내의 구문 실행 이전에, **js는 원래 전달된 인수(원시 값)를 복사해 로컬 복사본을 생성** 한다. <br/>
④ 복사본은 함수 안에서만 존재하기 때문에 외부의 foo 값은 변함이 없는 것....!!!!

<br/>

아직 조금 아리까리해서 다른 예제, 설명도 가져왔다.

```js
let str = "Hello!";
str = "world";
```

1. 첫번째 구문이 실행되면 메모리에 문자열 ‘Hello’가 생성되고 식별자 str은 메모리에 생성된 문자열 ‘Hello’의 메모리 주소를 가리킨다.
2. 그리고 두번째 구문이 실행되면 이전에 생성된 문자열 ‘Hello’을 수정하는 것이 아니라 새로운 문자열 ‘world’를 메모리에 생성하고 식별자 str은 이것을 가리킨다.

이때 문자열 ‘Hello’와 ‘world’는 모두 메모리에 존재하고 있다.

> 대충 원시 타입 데이터들은 재할당을 해도 메모리는 남아있고, 그 변수에 새로운 데이터가 생성된다는 말인듯

<br/>

### 참조 타입 데이터

참조 타입 즉 객체 타입은 참조에 의해 값이 저장되고 복사된다. <br/>
원시타입은 변수에 값이 할당 될 때 직접 해당 변수에 저장된다면, 참조 타입은 **변수의 값이 저장된 메모리 주소 값을 저장**한다.
![](https://velog.velcdn.com/images/chaehe_3210/post/17fecf40-6e09-4e8d-8e99-4ec97f7be96c/image.png)

> 참조 타입은 주소 값을 참조하기 때문에 원본 데이터의 값이 바뀔 때 복사한 데이터의 값도 변경이 된다. (얕은 복사)

#### 얕은 복사와 깊은 복사

> 참조 타입의 복사 방식은 얕은 복사(Shallow copy)와 깊은 복사(Deep copy) 두 가지로 나뉜다.

**얕은 복사**
객체의 값을 복사할 때는 대상 전체가 아닌 객체 내 주소 값만 복사되는 문제가 생긴다.

```js
var user1 = {
  name: "Lee",
  address: {
    city: "Seoul",
  },
};

var user2 = user1; // 변수 user2는 객체 타입이다.

user2.name = "Kim";

console.log(user1.name); // Kim
console.log(user2.name); // Kim
```

같은 메모리를 참조하고 있기 때문에 user2의 name을 변경하면 user1의 name도 함께 변경된 것을 알 수 있다. ( 전개 연산자를 사용한 복사도 이 얕은 복사에 해당된다! )

<br/>

**깊은 복사**

얕은 복사와 다르게 새로운 메모리 주소를 생성해서 기존 객체의 내부 값만 복사해올 수 있다. 재귀 함수를 사용하는 방법과 json 객체를 이용하는 두 가지 방식을 주로 쓰는 것 같다.

- 재귀 함수를 이용한 깊은 복사
  함수 내부에서 본인..?을 다시 호출하는 것을 재귀 함수라고 한다.(뭐라해야돼)
  ![](https://velog.velcdn.com/images/chaehe_3210/post/74c5e05c-195c-4adf-b40f-fd1f676d6e3f/image.png)
  (어려우니 나중에 다시 보는 걸로)

- JSON을 이용한 복사
  기존 객체를 json화 시키고(이때 원본 객체와의 참조가 끊어진다), 다시 객체로 바꾸면 됨

```js
let user = {
  name: "John",
  age: 23,
  size: {
    height: 180,
    weight: 72,
  },
};

// stringify : js object -> string, parse : string -> js object
let admin_json = JSON.parse(JSON.stringify(user));
// user 부분을 문자열로 만들고, 문자열을 다시 객체열로 만들어서 admin에 넣어줘!!
```
