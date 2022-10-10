# async & await

ES7에 추가된 가장 최신 비동기 처리 패턴. 프로미스의 단점을 보완하고 가독성 좋은 코드를 작성할 수 있다!

## async

- 함수 앞에 async를 붙이면 자동으로 Promise 객체를 리턴하는 비동기 처리 함수가 된다
- async 라는 키워드를 붙여준 함수의 리턴 값은 Promise의 resolve 결과값으로 반환된다

```javascript
async function helloAsync() {
  return "hello Async!";
}
helloAsync().then((res) => {
  console.log(res);
});
// output : hello Async!
```

## await

```javascript
function delay(ms) {
  return new Promise((resolve) => {
    setTimeOut(() => {
      resolve();
    }, ms);
  });
}
// 이때 셋타임아웃 안 콜백함수 안에 resolve 호출 밖에 없으면
// 콜백 자체를 resolve 함수로 받아도 상관 없음
function delay(ms) {
  return new Promise((resolve) => {
    setTimeOut(resolve, ms);
  });
}

// 3초 딜레이 후에 helloAsync를 호출하고 싶을 때
async function helloAsync() {
  await delay(3000);
  return "hello async!";
}

async function main() {
  const res = await helloAsync();
  console.log(res);
}

main();
// output : 3초 뒤 hello Async! 출력
```

- 말 그대로 프로미스가 처리될 때까지 함수 실행을 기다림.
- await 은 async 가 붙어있는 함수에서만 사용할 수 있다.

(조만간 더 보완해서 작성할 것)
