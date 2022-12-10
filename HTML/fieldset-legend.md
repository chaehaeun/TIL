## ✨ fieldset / legend

> `<fieldset>` 태그는 `<form>` 요소에서 연관된 요소들을 하나의 그룹으로 묶을 때 사용합니다. `<fieldset>` 요소는 하나의 그룹으로 묶은 요소들 주변으로 박스 모양의 선을 그려줍니다. 또한, `<legend>` 요소를 사용하면 `<fieldset>` 요소의 캡션(caption)을 정의할 수 있습니다. - mdn

  <fieldset>
  	<legend>이 테두리가 필드셋, 이 글자가 레게노다.</legend>
    <label>이것은 라벨이다.</label>
    <input placeholder='이것은 인풋이다'></input>
  </fieldset>

위의 이상한 글자와 인풋은 사실 이렇게 마크다운 되어있다.

```html
<fieldset>
	<legend>이 테두리가 필드셋, 이 글자가 레게노다.</legend>
  <label>이것은 라벨이다.</label>
  <input placeholder='이것은 인풋이다'></input>
</fieldset>
```

![네이버 css 걷어낸거](https://velog.velcdn.com/images/chaehe_3210/post/2c738939-94aa-4711-8e03-317ee29ac5b5/image.png)
네이버 초기화면에서 css만 싹 걷어낸 모습...위의 검색이라고 적혀있는 박스가 필드셋, 이 박스의 캡션 역할을 하는 게 레전드다.

![](https://velog.velcdn.com/images/chaehe_3210/post/a0635675-8af3-4da5-a796-f2d4608be226/image.png)

폼을 쓸 때는 필드셋과 레전드를 써주는 걸 권장함. 레전드는 [마크업에 작성 후 css로 숨겨놓는 게 보편적인? 방법](https://mulder21c.github.io/2019/03/22/screen-hide-text/)인 것 같음
