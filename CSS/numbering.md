## ✨ css js없이 순서대로 넘버링 붙이기

![](https://velog.velcdn.com/images/chaehe_3210/post/ddcd1dce-3b89-4e76-a435-c549615c28df/image.png)

```html
<h3>Introduction</h3>
<h3>Body</h3>
<h3>Conclusion</h3>
```

```css
body {
  counter-reset: section;
  /* counter 이름을 'section'으로 지정합니다. 초깃값은 0입니다. */
}

h3::before {
  counter-increment: section;
  /* section의 카운터 값을 1씩 증가시킵니다. */
  content: "Section " counter(section) ": ";
  /* section의 카운터 값을 표시합니다. */
}
```
