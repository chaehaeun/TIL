## ✨ 알고리즘 : 문제 접근 방법 / 코드 리팩토링 해보기

간단한 문제여도 여러가지의 해결방식이 있다.
처음엔 일단 풀어본(if나 for 덕지덕지 써가며) 후에 한 줄 한 줄 가독성과 성능을 고려한 코드로 바꿔보는 연습을 해보면 좋음.

### 🎇 알고리즘 문제 접근 방법

1. 문제를 명확히 이해
2. 입력값과 출력값을 이해,예외 처리 조건 이해
3. 구현해야 될 코드에 대한 계획을 몇 가지 단계로 세분화해보기(무작정 코드부터 쳐보는 건 🙅‍♀️)
4. 문제를 완전히 해결할 수 없다면, 해결할 수 있는 부분부터 먼저 처리하기. 문제를 단순화하기
5. **더 나은 해결 방법이 있는지 되돌아보고 리팩토링하기.**

### 🎇 예제

#### 📌 Q. 주어진 문자열의 글자/숫자 개수(?)를 대소문자와 상관없이 객체로 반환하기

👇 예시

```js
// 입력값 : "Hello!! Hi there!! 1234"
// 출력값
AnswerObj = {
  1: 1,
  2: 1,
  3: 1,
  4: 1,
  h: 3,
  e: 3,
  l: 2,
  o: 1,
  i: 1,
  t: 1,
  r: 1,
};
```

#### 📌 A1. 첫번째 풀이

```js
const charCount = (str) => {
  let obj = {};

  for (let char of str) {
    char = char.toLowerCase();
    // 만약 char가 문자나 숫자라면(공백, 특수문자 제외해주는 것)
    if (/[a-z0-9]/.test(char)) {
      // char 라는 키의 밸류가0 이상이면
      if (obj[char] > 0) {
        // 밸류를 1씩 더해주고
        obj[char]++;
      } else {
        // char의 밸류가 0이면 1로 설정
        // 그리고 순회를 돌면서 같은 문자를 다시 만나면 위의 ++ 작업을 해주게 됨
        obj[char] = 1;
      }
    }
  }
  return obj;
};
```

#### 📌 A2. if 개수 줄이기

```js
const charCount = (str) => {
  let obj = {};

  for (let char of str) {
    char = char.toLowerCase();
    // 만약 char가 문자나 숫자라면(공백, 특수문자 제외해주는 것)
    if (/[a-z0-9]/.test(char)) {
      // obj[char] 가 참이라면 obj[char]를 더해주고
      // 거짓이라면 1로 설정
      obj[char] = ++obj[char] || 1;
    }
  }
  return obj;
};
```

#### 📌 A3. 정규표현식을 쓰는 것이 성능적으로 좋은가?

![](https://velog.velcdn.com/images/chaehe_3210/post/f55fc5b7-d7d7-4332-9c00-95549642cf50/image.png)

아스키코드를 사용해서 푸는 게 성능면에서 우수

`charCodeAt()`을 사용해서 ASCII 코드로 문제 풀이

[아스키 코드 확인 ( ascii-code.com )](https://www.ascii-code.com/)

```js
const isAlphaNumeric = (char) => {
  const code = char.charCodeAt(0);

  if (
    !(code > 47 && code < 58) && // numeric (0~9)
    !(code > 64 && code < 91) && // upper alpha (A~Z)
    !(code > 96 && code < 123) // lower alpha (a~z)
  ) {
    // 문자나, 숫자가 아니기 때문에 false 리턴
    return false;
  }
  // 숫자나 문자이기 때문에 true 리턴
  return true;
};

const charCount = (str) => {
  let obj = {};

  for (let char of str) {
    if (isAlphaNumeric(char)) {
      // 소문자화도 굳이 처음부터 해줄 필요가 없다
      // char가 숫자나 문자인지 확인 후에 바꿔주면 됨
      char = char.toLowerCase();
      obj[char] = ++obj[char] || 1;
    }
  }
  return obj;
};

console.log(charCount("Hello!! Hi there!! 1234"));
```

> 오히려 코드는 길어진 것 같은데;

하지만 가독성과 성능을 끌어올렸으니 good, better 솔루션😏
