## ✨ float

단어 뜻 그대로 float 해서 정렬하는 레이아웃 프로퍼티. 이 css 자체의 설명보다는 float가 어려운 이유 관련으로만 적는당.

👇**의문인 상황 가져와봄**

형제요소인 그룹 1 2 3 중 그룹 1에만 플롯을 넣은 상황

![](https://velog.velcdn.com/images/chaehe_3210/post/a94352a8-5d48-4d81-8174-67309e7b9298/image.png)

그룹1에 플롯을 넣어서 허공에 떠있기 때문에 요소가 겹쳐진 건 알겠는데 그룹2 글자는 갑자기 왜 밀려난거임?
내부의 요소가 인라인이어서 밀려남(텍스트는 인라인 요소이기 때문에 밀려난 것임)

![](https://velog.velcdn.com/images/chaehe_3210/post/868e4353-227f-413b-a0bf-05ef2c0671de/image.png)

예전에 썼던 워드 이런 거에 이미지 삽입할 때랑 똑같은 거임

float엔 고질병이 있는디...

![](https://velog.velcdn.com/images/chaehe_3210/post/255c067c-d939-4a77-b390-10edde4a5cfc/image.png)

띄워놓은 것이기 때문에, 부모 요소가 자식 요소의 높이값을 인식하지 못함
(absolute랑 비슷한 상황이라고 보면 될 듯)

예전엔 이걸 해결하려고 after 써서 클리어 넣는 방법 밖에 못 써봤는데... 사실 그 클리어픽스는 너무 어렵고 플렉스는 너무 쉬워서ㅋㅋ; 플렉스로 대충 다 해결하고 있었음...플렉스는 사람을 게을러지게 하는 군아...

> 암튼 저런 고질병을 해결하는 방법

### 🎇 display : flow-root

The element generates a block element box that establishes a new block formatting context, defining where the formatting root lies.

[📌 block formatting context, BFC에 대해서](https://developer.mozilla.org/ko/docs/Web/Guide/CSS/Block_formatting_context)

mdn 문서의 설명인데, 저걸 써주면 대충 BFC라는 요소를 하나 생성해서 다른 요소들과 상호작용을 할 수 있게 한다는 의미인듯...?

```css
/* 메인 */
.main {
  background: #fff;
  padding: 30px;
  border-radius: 15px 15px 0 0;
  display: flow-root;
}

.group1 {
  background-color: violet;
  width: 250px;
  margin-right: 30px;
  float: left;
}

.group2 {
  background-color: salmon;
  width: 380px;
  margin-right: 30px;
  float: left;
}

.group3 {
  background-color: gold;
  width: 190px;
  float: left;
}
```

부모 요소에 `display : flow-root` 를 넣어버리면 바로 해결
근데 이건 나온지 얼마 안돼서 아직 웹 호환성이 처참하다고 함. 언젠간 수월하게 지원될 수도? 있으니 알아놓는다고 손해는 아닌듯. **암튼 모던한 방식은 이거다.**

### 🎇 overflow : hidden

```css
.main {
  background: #fff;
  padding: 30px;
  border-radius: 15px 15px 0 0;
  /* display: flow-root; */
  overflow: hidden;
}
```

부모 요소한테 `overflow:hidden` 을 넣어버리면 오버플로우는 내부 자식의 크기을 알아야 제대로 작동하기 때문에 플롯을 넣어놓은 친구들의 높이도 인식하게 됨.
이건 현업에서 여전히 많이 쓰이나보다. 근데 이 방식은 잠재적인 위험이 있다고 한다. overflow로 hidden을 넣어놨기 때문에 자식 요소를 left나 top 같은 걸로 위치를 옮겨버리면 부모요소를 벗어나는 순간 안 보이게 됨.

### 🎇 clear fix(div에 직접적으로 추가하는 방법)

```css
.clearfix {
  clear: both;
}
```

결국 돌고 돌아 클리어픽스.
플로트 요소 마지막에 클리어픽스라는 div를 삽입하고, clear 속성을 주면 바로 해결되어버림.
(클리어픽스라는 이름을 관례적으로 많이 쓰는 것 뿐이지 꼭 이 이름이어야 될 필요는 없다!)

### 🎇 clear fix(after로 해결하는 방법)

```css
.main::after {
  display: block;
  content: "";
  clear: both;
}
```

부모 요소한테 after 만들어서 display를 block으로 바꿔주고 얘한테 클리어를 넣어버리면 해결된다.
**But** 가상으로 요소를 만들어낸 것이기 때문에 이런 상황에서 flex 같은 걸 써버리면 레이아웃 이슈가 생겨버린다. 알고 있어야 될듯.
