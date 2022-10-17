## for문으로 최대공약수, 최대공배수 구하기

```javascript
function solution(n, m) {
  var answer = [];
  let gcd = 1;
  let lcm = 1;

  for (let i = 2; i <= Math.min(n, m); i++) {
    if (n % i == 0 && m % i == 0) {
      gcd = i;
    }
  }

  while (true) {
    if (lcm % n === 0 && lcm % m === 0) {
      break;
    }
    lcm++;
  }
  return (answer = [gcd, lcm]);
}
```

for문 돌리면 이렇게 간단하다...
하지만 시간복잡도를 따지게 되면 **유클리드 호제법**이 더 성능적으로 우수하다고 한다.

## 유클리드 호제법이란?

> 2개의 자연수(또는 정식) a, b에 대해서 a를 b로 나눈 나머지를 r이라 하면(단, a>b), a와 b의 최대공약수는 b와 r의 최대공약수와 같다. 이 성질에 따라, b를 r로 나눈 나머지 r'를 구하고, 다시 r을 r'로 나눈 나머지를 구하는 과정을 반복하여 나머지가 0이 되었을 때 나누는 수가 a와 b의 최대공약수이다. - **위키피디아 긁어옴**

유클리드 호제법을 따라 정수 X와 Y(X>=Y)의 최대공약수를 변수 GCD에 구하는 알고리즘을 순서대로 써보자.(출처 : 그림으로 배우는 알고리즘 basic)

1. 변수 R에 X/Y의 나머지 값을 대입한다.
2. 변수 R이 0이 아니라면 다음 3~5단계를 반복한다.
3. 변수 X에 변수 Y값을 대입한다.
4. 변수 Y에 변수 R의 값을 대입한다.
5. 변수 R에 X/Y의 나머지 값을 대입한다.
6. 변수 GCD에 변수 Y의 값을 대입한다.

> 요점은 **X를 Y로 나눈 나머지 값을 R이라고 할 때, X와 Y의 최대공약수는 Y와 R의 최대공약수와 같다. 라는 말**

순서를 본 후 나는 while을 사용해서 값을 구해봤다.

```javascript
let a = 24;
let b = 45;

function getGCD(a, b) {
  let gcd = 1;

  let X = Math.max(a, b);
  let Y = Math.min(a, b);
  let r = X % Y;

  while (Y > 0) {
    // y는 1이상의 숫자일 것이기 때문에 0이 되기 전까지만 반복한다
    if (X % Y == 0) {
      break;
    }
    X = Y;
    Y = r;
    r = X % Y;
  }
  gcd = Y;
  console.log(Y);
}
getGCD(24, 45);
```

a랑 b 중에 더 큰 값이 순서가 다르게 들어갈 가능성도 있다고? 봐서 변수를 더 지정했더니 좀 너저분한 것 같다.
최소공배수는 최대공약수를 구하고 나면 간단하다.

```javascript
let gcf = (X * Y) / gcd;
```

두 수의 곱에 최대공약수를 나누면 그게 최소공배수이다.

![](https://velog.velcdn.com/images/chaehe_3210/post/1a062b02-57c2-4dc2-a886-947f1acf406a/image.jpg)

다른 사람 풀이는 아주 간결하다.

```javascript
function gcdlcm(a, b) {
  var r;
  for (var ab = a * b; (r = a % b); a = b, b = r) {}
  return [b, ab / b];
}
// true false를 판별하는 식으로 r = a%b 를 썼고
// true면 뒤의 a = b, b = r 스왑을 하고,
// r = a % b 의 나머지가 0이 될 때는 false가 되어서 반복이 종료된다.
// 따라서 a % b 의 값이 0 이기 때문에, b 는 입력값의 최대공약수가 된다.
```

이 풀이를 보고 나는 for문을 정확하게 파악하고 있는 게 아니었다는 것을 깨달았다. 기초가 이렇게 중요하다.
