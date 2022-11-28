### ✨ JadenCase 문자열 만들기

> [JadenCase 문자열 만들기](https://school.programmers.co.kr/learn/courses/30/lessons/12951)

레벨 1 거의 다 풀어가서...레벨 2에서 정답률이 높은 문제부터 풀어보고 있다. 한 레벨 올라갔다고 버벅거리는 중...

#### 내 풀이

```js
function solution(s) {
  let arr = [];
  arr = s.split(" ");
  arr = arr
    .map((i) => i.charAt(0).toUpperCase() + i.slice(1).toLowerCase())
    .join(" ");

  return arr;
}
```

처음엔 #### charAt()으로 접근 못하고 계속 인덱스로만 접근했는데, 내가 원하는 답이 안 나오더라.
charAt()을 사용하는게 맞았는 듯... 이거 하나 썼다고 문제가 간결하게 풀렸다.

#### 🎇 charAt()

```js
str.charAt(index);
```

매개변수는 0과 문자열의 length - 1 사이의 정수 값. 걍 배열 인덱스 같은 느낌으로 생각하면 될 듯. 인자 생략하면 디폴트로 0이 설정된다.

**반환하는 값은 지정된 인덱스에 해당하는 유니코드 단일문자**
만약 인덱스보다 큰 값을 넣으면 빈 문자열 `''`를 반환한다.

예제

```js
const anyString = "Brave new world";

console.log("The character at index 0   is '" + anyString.charAt(0) + "'");
// The character at index 0   is 'B'
console.log("The character at index 1   is '" + anyString.charAt(1) + "'");
// The character at index 1   is 'r'
console.log("The character at index 2   is '" + anyString.charAt(2) + "'");
// The character at index 2   is 'a'
console.log("The character at index 3   is '" + anyString.charAt(3) + "'");
// The character at index 3   is 'v'
console.log("The character at index 4   is '" + anyString.charAt(4) + "'");
// The character at index 4   is 'e'
console.log("The character at index 999 is '" + anyString.charAt(999) + "'");
// The character at index 999 is ''
```
