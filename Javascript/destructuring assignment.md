### 구조 분해 할당(destructuring assignment)

#### 디스트럭처링이란!?

- 구조화된 배열 또는 객체를 Destructuring(비구조화, 파괴)하여 **개별적인 변수에 할당**하는 것이다. 배열 또는 객체 리터럴에서 필요한 값만을 추출하여 변수에 할당하거나 반환할 때 유용하다.

#### 배열 디스트럭처링

- ES6 배열 디스트럭처링은 배열의 각 요소를 배열로부터 추출해서 변수 리스트에 할당한다. 이때 추출/할당 기준은 **배열의 인덱스**이다!

```javascript
// ES6 Destructuring
const arr = [1, 2, 3];

// 배열의 인덱스를 기준으로 배열로부터 요소를 추출하여 변수에 할당
// 변수 one, two, three가 선언되고 arr(initializer(초기화자))가
// Destructuring(비구조화, 파괴)되어 할당된다.
const [one, two, three] = arr;
// 디스트럭처링을 사용할 때는 반드시 initializer(초기화자)를 할당해야 한다.
// const [one, two, three];
// SyntaxError: Missing initializer in destructuring declaration

console.log(one, two, three); // 1 2 3
```

배열 디스트럭처링을 위해서는 할당 연산자 왼쪽에 배열 형태의 변수 리스트가 필요하다

```javascript
let x, y, z;

[x, y] = [1, 2];
console.log(x, y); // 1 2

[x, y] = [1];
console.log(x, y); // 1 undefined

[x, y] = [1, 2, 3];
console.log(x, y); // 1 2

[x, , z] = [1, 2, 3];
console.log(x, z); // 1 3

// 기본값
[x, y, z = 3] = [1, 2];
console.log(x, y, z); // 1 2 3

[x, y = 10, z = 3] = [1, 2];
console.log(x, y, z); // 1 2 3

// spread 문법
[x, ...y] = [1, 2, 3];
console.log(x, y); // 1 [ 2, 3 ]
```

![](https://velog.velcdn.com/images/chaehe_3210/post/9ca9d3f5-144c-49d0-9241-9619b8885773/image.png)
요러케 변수끼리 값을 스왑하기가 개쉬워진다. (출처 : 한입 크기로 잘라 먹는 어쩌고 강의)

<br/>

- 쉼표를 사용하여 요소 무시하기

```javascript
// 두 번째 요소는 필요하지 않음
let [firstName, , title] = [
  "Julius",
  "Caesar",
  "Consul",
  "of the Roman Republic",
];

alert(title); // Consul
```

정리하다가 신기해서 가져옴 자바스크립트는 참 유연하구나...
좀 알게 됐다 싶으면 고양이 마냥 액체처럼 흘러내리는 느낌 (╯°□°）╯︵ ┻━┻

<br/>

### 객체 디스트럭처링

- ES6의 객체 디스트럭처링은 객체의 각 프로퍼티를 객체로부터 추출하여 변수 리스트에 할당한다. 이때 **할당 기준은 프로퍼티 이름(키)**이다.

```javascript
// ES6 Destructuring
const obj = { firstName: "Ungmo", lastName: "Lee" };

// 프로퍼티 키를 기준으로 디스트럭처링 할당이 이루어진다. 순서는 의미가 없다.
// 변수 lastName, firstName가 선언되고 obj(initializer(초기화자))가 Destructuring(비구조화, 파괴)되어 할당된다.
const { lastName, firstName } = obj;

console.log(firstName, lastName); // Ungmo Lee
```

- 객체 디스트럭처링은 객체에서 프로퍼티 이름(키)으로 필요한 프로퍼티 값만을 추출할 수 있다.

```javascript
const todos = [
  { id: 1, content: "HTML", completed: true },
  { id: 2, content: "CSS", completed: false },
  { id: 3, content: "JS", completed: false },
];

// todos 배열의 요소인 객체로부터 completed 프로퍼티만을 추출한다.
const completedTodos = todos.filter(({ completed }) => completed);
console.log(completedTodos); // [ { id: 1, content: 'HTML', completed: true } ]
```
