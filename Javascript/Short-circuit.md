### &&

```js
console.log(true && "hello"); // hello
console.log(false && "hello"); // false
console.log("hello" && "bye"); // bye
console.log(null && "hello"); // null
console.log(undefined && "hello"); // undefined
console.log("" && "hello"); // ''
console.log(0 && "hello"); // 0
console.log(1 && "hello"); // hello
console.log(1 && 1); // 1
```

A && B 연산자를 사용하게 될 때에는 A 가 Truthy 한 값이라면, B 가 결과값이 된다. 반면, A 가 Falsy 한 값이라면 결과는 A 가 된다.

|| 는 안 까먹는데 &&는 자꾸 까먹으니 제대로 기억해놓자...
