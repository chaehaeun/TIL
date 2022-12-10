## ✨ 구조 선택자 nth-child()

even, odd, 2n 이런 건 알고 있었는데 뭐가 많더라 이것도

```css
li:nth-child(n + 3) {
}
/* 3번째 요소부터 적용시키겠다는 의미 */

li:nth-child(n + 2) {
}
/* 첫번째꺼 제외하고 나머지에 전부 */

li:nth-child(-n + 3) {
}
/* 3번째 요소까지만 */

li:nth-child(n + 2):nth-child(-n + 4) {
}
/* 2번부터 4번까지만 */
```

맨날 스타일 싹 다 먹여놓고 first-child 선택해서 특정 스타일만 빼주는 식으로 css를 두 줄 작성했는데 `nth-child(n+1)`를 사용하면 두 줄 쓸 필요없이 한 줄로 작성이 가능하다. 처음 알게 된 꿀팁!

[nth-child() 연습해볼 수 있는 사이트](https://css-tricks.com/examples/nth-child-tester/)
