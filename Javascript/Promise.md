## 프로미스

오늘은 봐도봐도 까먹는 프로미스를 다시 봤다...

![callback hell](https://velog.velcdn.com/images/chaehe_3210/post/07fcf48c-672b-49b8-989c-4d0ae9c83a4d/image.jpg)

### 콜백지옥이란?

- 비동기 함수의 처리 결과를 가지고 다른 비동기 함수를 호출해야 하는 경우, 함수의 호출이 중첩(nesting)이 되어 복잡도가 높아지는 현상이 발생하는데 이를 Callback Hell이라 한다. 콜백지옥은 코드의 가독성을 해치고 복잡도를 증가시켜 실수를 유발하는 원인이 된다. 에러처리도 곤란!!

```javascript
// 콜백지옥의 예시
function taskA(a, b, cb) {
  setTimeout(() => {
    const res = a + b;
    cb(res);
  }, 3000);
}

function taskB(a, cb) {
  setTimeout(() => {
    const res = a * 2;
    cb(res);
  }, 1000);
}

function taskC(a, cb) {
  setTimeout(() => {
    const res = a * -1;
    cb(res);
  }, 2000);
}

taskA(3, 4, (a_res) => {
  // 콜백지옥~~
  console.log("task A : ", a_res);
  taskB(a_res, (b_res) => {
    console.log("task B : ", b_res);
    taskC(b_res, (c_res) => {
      console.log("tesk C : ", c_res);
    });
  });
});
```

<br/>

### 프로미스란?

- 콜백지옥과 같은 문제를 극복하기 위해 제안된 ES6 문법. Promise 생성자 함수는 비동기 작업을 수행할 콜백 함수를 인자로 전달받는데 이 콜백 함수는 resolve와 reject 함수를 인자로 전달받는다.

```javascript
// Promise 객체의 생성
const promise = new Promise((resolve, reject) => {
  // 비동기 작업을 수행한다.

  if (/* 비동기 작업 수행 성공 */) {
    resolve('result');
  }
  else { /* 비동기 작업 수행 실패 */
    reject('failure reason');
  }
});
```

- Promise는 비동기 처리가 성공(fulfilled)하였는지 또는 실패(rejected)하였는지 등의 상태(state) 정보를 갖는다.

| 상태      |                    의미                    |                                               구현 |
| :-------- | :----------------------------------------: | -------------------------------------------------: |
| pending   |   비동기 처리가 아직 수행되지 않은 상태    | resolve 또는 reject 함수가 아직 호출되지 않은 상태 |
| fulfilled |      비동기 처리가 수행된 상태(성공)       |                         resolve 함수가 호출된 상태 |
| rejected  |      비동기 처리가 수행된 상태(실패)       |                          reject 함수가 호출된 상태 |
| settled   | 비동기 처리가 수행된 상태 (성공 또는 실패) |             resolve 또는 reject 함수가 호출된 상태 |

promise 생성자 함수가 인자로 전달 받은 콜백 함수는 내부에서 비동기 처리 작업을 수행한다.
성공하면 resolve 함수를, 실패하면 reject 함수를 호출한다. 이때 reject 함수의 인자로 에러 메시지를 전달한다.
이 에러 메시지는 **Promise의 후속 처리 메서드로 전달**된다!

<br/>

### then / catch

- Promise로 구현된 비동기 함수는 Promise 객체를 반환하여야 한다.
  Promise로 구현된 비동기 함수를 호출하는 측에서는 객체의 후속 처리 메서드를 통해 처리 결과 또는 에러메시지를 전달받아 처리한다.
  Promise 객체는 상태를 갖는데, 이 상태에 따라 후속 처리 메서드를 체이닝 방식으로 호출한다.

#### then

- then 메서드는 두 개의 콜백 함수를 인자로 전달 받는다. 첫 번째 콜백 함수는 성공(fulfilled, resolve 함수가 호출된 상태) 시 호출되고 두 번째 함수는 실패(rejected, reject 함수가 호출된 상태) 시 호출된다.
- then 메서드는 Promise를 반환한다.

#### catch

- 예외(비동기 처리에서 발생한 에러와 then 메소드에서 발생한 에러)가 발생하면 호출된다! catch 메소드는 Promise를 반환한다.

```javascript
function isPositiveP(number) {
  const executor = (resolve, reject) => {
    // executor -> 실행자, 비동기 작업을 실질적으로 수행하는 함수
    setTimeout(() => {
      // 성공 -> resolve
      if (typeof number === "number") {
        resolve(number >= 0 ? "양수" : "음수");
      } else {
        // 실패 -> reject
        reject("주어진 값이 숫자형 값이 아닙니다");
      }
    }, 2000);
  };

  const asyncTask = new Promise(executor);
  return asyncTask;
}

const res = isPositive(101);

res
  .then((res) => {
    console.log("작업 성공 : ", res);
  })
  .catch((err) => {
    console.log("작업 실패 : ", err);
  });
```

<br/>

아까의 콜백지옥을 프로미스를 이용해서 다시 해보자

```javascript
function taskA(a, b) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      const res = a + b;
      resolve(res);
    }, 3000);
  });
}

function taskB(a) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      const res = a * 2;
      resolve(res);
    }, 1000);
  });
}

function taskC(a) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      const res = a * -1;
      resolve(res);
    }, 2000);
  });
}

const bPromiseResult = taskA(5, 1).then((a_res) => {
  console.log("A result : ", a_res);
  return taskB(a_res); // taskB의 값을 리턴했기 때문
});

console.log("비동기 처리 공부 중");

bPromiseResult
  .then((b_res) => {
    console.log("B result : ", b_res);
    return taskC(b_res);
  })
  .then((c_res) => {
    console.log("C result : ", c_res);
  });
```

<br/>

### 프로미스 에러처리

- 비동기 처리 결과에 대한 후속 처리는 Promise 객체가 제공하는 후속 처리 메서드 **then, catch, finally**를 사용하여 수행한다.
  비동기 처리 시에 발생한 에러는 then 메서드의 두번째 콜백 함수로 처리할 수 있다.
  **catch도 사용할 수 있음! catch 사용이 더 권장되는 것 같다.**

```javascript
const wrongUrl = "https://jsonplaceholder.typicode.com/XXX/1";

// 부적절한 URL이 지정되었기 때문에 에러가 발생한다.
promiseAjax(wrongUrl)
  .then((res) => console.log(res))
  .catch((err) => console.error(err)); // Error: 404
```

catch 메서드를 호출하면 내부적으로 then(undefined, onRejected)을 호출한다.
catch 메서드를 모든 then 메서드를 호출한 이후에 호출하면 **비동기 처리에서 발생한 에러(reject 함수가 호출된 상태)뿐만 아니라 then 메서드 내부에서 발생한 에러까지 모두 캐치할 수 있다.**

대충 봐도 then의 두번째 콜백으로 에러 처리를 하는 것보다는 catch를 사용하는 것이 가독성이 좋아보인다. 에러처리는 catch로만 하는 걸로!
따봉 캐치야 고마워!

<br/>

### 프로미스의 정적 메서드

- Promise는 주로 생성자 함수로 사용되지만 함수도 객체이기 때문에 메서드를 가질 수 있다. 프로미스 객체는 4가지 정적 메서드를 제공한다고 한다.

#### Promise.resolve/Promise.reject

-Promise.resolve와 Promise.reject 메소드는 존재하는 값을 Promise로 래핑하기 위해 사용한다.
정적 메소드 Promise.resolve 메소드는 인자로 전달된 값을 resolve하는 Promise를 생성한다.

```javascript
const resolvedPromise = Promise.resolve([1, 2, 3]);
resolvedPromise.then(console.log); // [ 1, 2, 3 ]

const rejectedPromise = Promise.reject(new Error("Error!"));
rejectedPromise.catch(console.log); // Error: Error!
```

#### Promise.all

- Promise.all 메서드는 프로미스가 담겨있는 배열 등의 이터러블을 인자로 받는다. 그리고 **전달 받은 모든 프로미스를 병렬로 처리**하고 그 처리 결과를 resolve하는 새로운 프로미스를 반환한다.

```javascript
Promise.all([
  new Promise((resolve) => setTimeout(() => resolve(1), 3000)), // 1
  new Promise((resolve) => setTimeout(() => resolve(2), 2000)), // 2
  new Promise((resolve) => setTimeout(() => resolve(3), 1000)), // 3
])
  .then(console.log) // [ 1, 2, 3 ]
  .catch(console.log);
//첫번째 프로미스는 3초 후에 1을 resolve하여 처리 결과를 반환한다.
//두번째 프로미스는 2초 후에 2을 resolve하여 처리 결과를 반환한다.
//세번째 프로미스는 1초 후에 3을 resolve하여 처리 결과를 반환한다.
```

- Promise.all 메서드는 전달 받은 모든 프로미스를 병렬로 처리하고, 모든 프로미스의 처리가 종료될 때까지 기다린 후 모든 처리 결과를 resolve 또는 reject한다.

1. 모든 프로미스의 처리가 성공하면 **각각의 프로미스가 resolve한 처리 결과를 배열에 담아 resolve하는 새로운 프로미스를 반환**한다.
2. 이때 첫번째 프로미스가 가장 나중에 처리되어도 Promise.all 메소드가 반환하는 프로미스는 첫번째 프로미스가 resolve한 처리 결과부터 차례대로 배열에 담아
3. 그 배열을 resolve하는 새로운 프로미스를 반환한다. 즉, 처리 순서가 보장된다.

- 프로미스의 처리가 하나라도 실패하면 가장 먼저 실패한 프로미스가 reject한 에러를 reject하는 새로운 프로미스를 즉시 반환한다.

```javascript
Promise.all([
  new Promise((resolve, reject) =>
    setTimeout(() => reject(new Error("Error 1!")), 3000)
  ),
  new Promise((resolve, reject) =>
    setTimeout(() => reject(new Error("Error 2!")), 2000)
  ),
  new Promise((resolve, reject) =>
    setTimeout(() => reject(new Error("Error 3!")), 1000)
  ),
])
  .then(console.log)
  .catch(console.log); // Error: Error 3!

// 가장 먼저 처리된 Error 3 이 콘솔로 찍힌다!
```

```javascript
Promise.all([
  1, // => Promise.resolve(1)
  2, // => Promise.resolve(2)
  3, // => Promise.resolve(3)
])
  .then(console.log) // [1, 2, 3]
  .catch(console.log);
// Promise.all 에 전달 받은 이터러블 요소가 프로미스가 아닐 때, Promise.resolve를 통해 프로미스로 래핑할 수 있다.
```

#### Promise.race

- Promise.all 과 동일하게 프로미스가 담겨있는 이터러블을 인자로 받는다. ㄹㅇ레이스처럼 가장 먼저 처리된 프로미스가 resolve한 결과를 resolve 하는 새로운 프로미스를 반환한다.

```javascript
Promise.race([
  new Promise((resolve) => setTimeout(() => resolve(1), 3000)), // 1
  new Promise((resolve) => setTimeout(() => resolve(2), 2000)), // 2
  new Promise((resolve) => setTimeout(() => resolve(3), 1000)), // 3
])
  .then(console.log) // 3
  .catch(console.log);
```

- 에러처리는 Promise.all과 동일하게 처리됨.

<br/>
