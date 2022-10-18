# 소수 판별법(javascript)

> ## 소수란?

_1보다 큰 자연수 중 1과 자기 자신만을 약수로 가지는 수다. 예를 들어, 5는 1×5 또는 5×1로 수를 곱한 결과를 적는 유일한 방법이 그 수 자신을 포함하기 때문에 5는 소수이다. (위키피디아 복붙)_

## JS로 소수를 판별해보자

여기저기에서 구글링해보고 내가 이해하기 제일 쉬운 풀이만 몇 가지 가져왔다!!
<br/>

### 1. 반복문 싹 다 돌려서 나눠보기

```javascript
const isPrime = (n) => {
  for (let i = 2; i < n; i++) {
    if (n % i === 0) {
      return false;
    }
  }

  return true;
};
// 출처 : https://gurtn.tistory.com/103
```

<br/>

### 2. n / 2 값 까지만 반복문 돌려 나눠보기

무슨 말인가 했는데 주어진 값 n 의 절반 이상부터는 n의 약수가 존재할 수 없다고 한다.

```javascript
const isPrime = (n) => {
  for (let i = 2; i < n / 2; i++) {
    if (n % i === 0) {
      return false;
    }
  }

  return true;
};
```

1번 방법보다 아주 조금 더 나은 방법이다(n번의 절반만 돌리니까)
<br/>

### 3. 제곱근을 이용하기

> 🎇제곱근(sqrt) 범위 나누기법이란!?

- 소수 여부를 검사할 수에 대해서 그 값의 제곱근을 기준으로 그 곱은 대칭적으로 곱이 일어나므로 제곱근 이하의 작은 값까지만 검사를 하면 나머지는 검사를 할 필요가 없다는 방법으로 검사할 데이터를 제곱근 개 이하로 줄 일 수 있는 방법.

출처: https://www.it-note.kr/308 [IT 개발자 Note:티스토리]

```javascript
const isPrime = (n) => {
  for (let i = 2; i <= Math.ceil(Math.sqrt(n)); i++) {
    if (n % i === 0) {
      return false;
    }
  }

  return true;
};
```

Math.sqrt 는 주어진 값의 제곱근을 구하는 함수, Math.ceil() 은 소수점 이하 값을 <span style="color:red;">"올림"</span> 하는 함수다.
2번보다도 연산 과정이 확 줄어든 것을 알 수 있다 GOOD👍

## (+221018) 제곱근 구하는 다른 방법

### \*\* 연산자 사용하기

```js
n ** (1 / 2);
```
