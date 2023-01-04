## ✨ frequency counter

말 그대로 빈도를 세는 문제 패턴.

### 🎇 예제 1

#### 📌 Q. arr2의 요소들이 순서와 상관없이 arr1의 요소들의 제곱인지 확인

👇 예시

```js
same([1, 2, 3], [4, 1, 9]); // true
same([1, 2, 3], [1, 9]); // false
```

#### 📌 A1. 순진한 풀이 방법(?)

```js
function same(arr1, arr2) {
  if (arr1.length !== arr2.length) {
    return false;
  }
  for (let i = 0; i < arr1.length; i++) {
    let correctIndex = arr2.indexOf(arr1[i] ** 2);
    if (correctIndex === -1) {
      return false;
    }
    // console.log(arr2);
    arr2.splice(correctIndex, 1);
  }
  return true;
}

console.log(same([1, 2, 3, 2], [9, 1, 4, 4]));
console.log(same([1, 2, 3], [4, 1, 9]));
console.log(same([1, 2, 3], [1, 9]));
console.log(same([1, 2, 1], [4, 4, 1]));
```

반복문을 돌면서 `indexOf`로 해당 요소가 존재하는지 확인하고, 리턴 값이 -1이면 false를 반환, -1이 아니면 기존 배열에서 해당하는 값을 하나씩 빼줌.

반복문 다 돌고나면 true를 반환

**결론. indexOf를 통해서 중첩 루프를 돌고 있기 때문에 O(n^2) **

#### 📌 A2. 내 풀이

```js
const same = (arr, arr2) => {
  let answer = null;
  let index = 0;
  arr.sort((a, b) => a - b);
  arr2.sort((a, b) => a - b);

  if (arr.length !== arr2.length) return false;

  while (index < arr.length) {
    if (index === arr.length - 1) return (answer = true);
    if (arr[index] ** 2 !== arr2[index]) return (answer = false);
    index++;
  }
  return answer;
};

console.log(same([1, 2, 3], [4, 1, 9]));
console.log(same([1, 2, 3], [1, 9]));
console.log(same([1, 2, 1], [4, 4, 1]));
```

배열 2개를 정렬 시킨 후 인덱스 비교.
동일 인덱스의 값이 다르다면 false 반환, 배열이 다 돌 때까지 다른 값을 못 찾는다면 true 반환

**결론. 중첩 되진 않았지만 sort 2회, while문 1회를 돌기 때문에 O(3n) **

#### 📌 A3. 강의의 리팩토링 코드

```js
function same(arr1, arr2) {
  if (arr1.length !== arr2.length) {
    return false;
  }
  let frequencyCounter1 = {};
  let frequencyCounter2 = {};
  for (let val of arr1) {
    frequencyCounter1[val] = (frequencyCounter1[val] || 0) + 1;
  }
  for (let val of arr2) {
    frequencyCounter2[val] = (frequencyCounter2[val] || 0) + 1;
  }
  // console.log(frequencyCounter1);
  // console.log(frequencyCounter2);
  for (let key in frequencyCounter1) {
    if (!(key ** 2 in frequencyCounter2)) {
      return false;
    }
    if (frequencyCounter2[key ** 2] !== frequencyCounter1[key]) {
      return false;
    }
  }
  return true;
}

same([1, 2, 3, 2, 5], [9, 1, 4, 4, 11]);
```

객체의 key 값마다 밸류+1을 해줘서 몇 번 나오는지 빈도를 찾아냄
그리고 루프를 돌아서 값 비교!

**결론. 중첩 되진 않았지만 for of 2회 for in 1회이기 때문에 O(3n) **

#### 🤔 3개의 풀이 방식을 본 후 내 생각 : 이 예시에선 내 방식이 더 직관적이지 않나? 아님 말구

아 그리고,

> 객체를 동적으로 생성하면(이유가 이게 맞는진 모르겠지만 정렬이 되긴 함) 자동으로 정렬이 되는 것 같다. 처음 알게 된 사실...🫢

```js
const arr1 = [1, 2, 8, 11, 4, 68, 3];
const frequencyCounter1 = {};

for (let val of arr1) {
  frequencyCounter1[val] = (frequencyCounter1[val] || 0) + 1;
}

console.log(frequencyCounter1);

// { '1': 1, '2': 1, '3': 1, '4': 1, '8': 1, '11': 1, '68': 1 }
```

### 🎇 예제 2

#### 📌 Q. 두 개의 문자열을 가지고, 두 문자열이 서로의 아나그램이면 참을 반환 아니면 거짓을 반환

👇 예시

```js
console.log(isValidAnagram("", "")); // true
console.log(isValidAnagram("aaz", "zza")); // false
console.log(isValidAnagram("rat", "car")); // false
console.log(isValidAnagram("awesome", "awesom")); // false
console.log(isValidAnagram("anagram", "nagaram")); // true
```

#### 📌 A1. 내 풀이

```js
const isValidAnagram = (string1, string2) => {
  if (string1.length !== string2.length) return false;

  let answer = true;
  const arr1 = string1.split("").sort();
  const arr2 = string2.split("").sort();

  for (let i in arr1) {
    if (arr1[i] !== arr2[i]) return (answer = false);
  }

  return answer;
};

console.log(isValidAnagram("", ""));
console.log(isValidAnagram("aaz", "zza"));
console.log(isValidAnagram("rat", "car"));
console.log(isValidAnagram("awesome", "awesom"));
console.log(isValidAnagram("anagram", "nagaram"));
```

위의 해결방식이랑 똑같이 풀었다. 다만 split을 한 번 더 했기 때문에...음...

**결론은 O(5n)?**

#### 📌 A2. 강의 풀이

```js
function isValidAnagram(first, second) {
  if (first.length !== second.length) {
    return false;
  }

  const lookup = {};

  for (let i = 0; i < first.length; i++) {
    let letter = first[i];
    // if letter exists, increment, otherwise set to 1
    lookup[letter] ? (lookup[letter] += 1) : (lookup[letter] = 1);
  }
  console.log(lookup);

  for (let i = 0; i < second.length; i++) {
    let letter = second[i];
    // can't find letter or letter is zero then it's not an anagram
    if (!lookup[letter]) {
      return false;
    } else {
      lookup[letter] -= 1;
    }
  }

  return true;
}

// {a: 0, n: 0, g: 0, r: 0, m: 0,s:1}
console.log(isValidAnagram("anagram", "nagaram"));
console.log(isValidAnagram("", ""));
console.log(isValidAnagram("aaz", "zza"));
console.log(isValidAnagram("rat", "car"));
console.log(isValidAnagram("awesome", "awesom"));
```

**결론은 O(2n)**
