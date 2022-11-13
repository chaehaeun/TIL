### for ... in 관련으로 헷갈리는 거 발견 🙄

```js
const array = [];

const objs = {
  m1: {
    name: "name 1",
  },
  m2: {
    name: "name 2",
  },
  m3: {
    name: "name 3",
  },
  m4: {
    name: "name 4",
  },
};

for (const obj in objs) {
  array.push({
    id: obj,
    name: objs[obj].name,
  });
}

console.log(array[0].id); // output : m1
```

오늘은 fetch로 중첩 객체를 다루는 어쩌고를 했는데... 저 콘솔 값의 결과가 m1인 것에 갑자기 머리가 복잡해지는 것이다. 어? id에 obj라는 객체를 value로 할당했으면 결과값으로 객체가 나와야 되는 것이 아닌가?하고...ㅎㅎ;

#### 착각하고 있던 부분

```js
const obj2 = {
  a: 1,
  b: 2,
  c: 3,
};

for (const prop in obj2) {
  console.log(prop, obj2[prop]); // a 1, b 2, c 3
}
```

원래 변수로 지정한 값 자체를 콘솔로 찍으면 key 값만 문자열로 찍힌다... 그 키값의 밸류를 뽑으려면 `obj2[prop]` 이런 식으로 객체 이름에 대괄호로 프로퍼티를 적어줘야 됨...ㅎㅎ

for 쩌구 문법 맨날 헷갈려서 흐린눈하면서 지나쳤더니 꽤 고생했다.... 헷갈리지 말자 이제
