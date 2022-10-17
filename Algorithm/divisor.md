> https://school.programmers.co.kr/learn/courses/30/lessons/77884

내 풀이

```js
function solution(left, right) {
  let range = [];
  let odd = [];
  let divisor = [];

  for (i = left; i <= right; i++) {
    range.push(i);
  }

  for (let i = 0; i < range.length; i++) {
    for (let j = 1; j <= range[i]; j++) {
      if (range[i] % j == 0) {
        divisor.push(j);
      }
    }
    if (divisor.length % 2 !== 0) {
      odd.push(range[i]);
    }

    divisor = [];
  }

  let sum =
    range.reduce((a, b) => (a += b)) - odd.reduce((a, b) => (a += b)) * 2;

  return sum;
}
// 코드 쓰면서도 아 이거 아닌데...했다.
```

일단 정답 처리가 되긴 했는데 **성능은 전혀 고려하지 않은 코드다.**

다른 사람 풀이

```js
function solution(left, right) {
  var answer = 0;
  for (let i = left; i <= right; i++) {
    // 만약 i의 제곱근이 정수라면(약수의 개수가 홀수라면)
    if (Number.isInteger(Math.sqrt(i))) {
      // i를 빼고
      answer -= i;
    } else {
      // 아니라면 더해라
      answer += i;
    }
  }
  return answer;
}
```

제곱근이 정수로 딱 맞게 떨어지면 약수의 개수는 홀수다. 그냥 보고는 왜지? 싶었는데
예시를 들어보니 아!! 싶었다.

🎈 예시

16의 약수는 1, 2, 4, 8, 16이다. 4를 제곱하면 16이 되기 때문에 제곱근이 정수면 약수의 개수가 홀수일 수 밖에 없는 것이다.
