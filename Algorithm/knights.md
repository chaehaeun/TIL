### 프로그래머스 lv.1 기사단원의 무기

[기사단원의 무기 링크](https://school.programmers.co.kr/learn/courses/30/lessons/136798)

```js
function solution(number, limit, power) {
  let result = 0;
  let arr = [];
  for (let i = 1; i <= number; i++) {
    arr.push(i);
  }

  for (let j = 0; j < arr.length; j++) {
    let count = 0;
    for (let k = 1; k * k <= arr[j]; k++) {
      if (arr[j] % k === 0) {
        count++; // k 가 arr[j]의 약수일 때 +1을 해주고
        if (k * k < arr[j]) {
          //  arr[j]도 카운트 해야 되기 때문에 +1 해줌
          count++;
        }
      }
    }
    if (count > limit) {
      result += power;
    } else {
      result += count;
    }
  }
  return result;
}
```

오늘도 약수 문제에 머리 깨졌다! 약수 단어만 보이면 일단 Math.sqrt()를 가져왔는데 오늘은 동기분이 새로운 방법을 알려주셨다.

기존엔

```js
k < Math.sqrt(arr[j]);
```

이런 식으로 범위를 지정했다면,

이번엔

```js
k * k <= arr[j];
```

이런 식으로 바꿨다. 결국 위의 Math 메서드랑 결론적으론 같은 의미인데, 이게 더 간결하고 좋은듯...
그리고 애초에 이게 문제는 아니었고

범위를 제곱근보다 작은 수의 약수를 구하는 방식으로 풀었기 때문에,
k의 제곱이 arr[j] 보다 작을 때는 +2, k의 제곱일 때 +1 (제곱근은 쌍을 이루는 수가 없기 때문에)을 해줬다.
