## ✨ 웹 접근성을 고려한 마크업 (논리적 구조 마크업 / aria-label, aria-hidden)

### 🎇 논리적 구조로 마크업하기

![마크업순서](https://velog.velcdn.com/images/chaehe_3210/post/428feee1-fdc5-41be-b5ba-8aecf0dda59b/image.png)

예제의 헤더로 봤을 때, 디자인적 관점으로는(그냥 보이는 순서로)

1. 텍스트 링크(LNB)
2. 로고
3. 메인 메뉴(GNB)
   순서로 마크업을 해야겠다는 생각을 한다.
   (나도 그렇게 생각했는데 예제 html을 보고 어 이게 뭐지!? 했다.)

하지만 이 사이트의 구조상

1. 제일 첫번째로 로고를,
2. 텍스트 링크,
3. 메인 메뉴
   순서로 기계가 인식하게 하는 것이 합당하다.

### 🎇 aria-label, aria-hidden

#### aria-label

[예제 사이트 링크](http://www.responsivelogos.co.uk/)

![](https://velog.velcdn.com/images/chaehe_3210/post/88d4a665-8c12-432b-b38b-549e9b729415/image.png)

사이트를 들어가보면 로고들의 마크업이 이렇게 되어있다. logo1, logo2, logo3 이런 식으로 클래스 네이밍을 하는 것도 좋지 못한 방식일 뿐더러, 웹 접근성의 측면에서 보면 백그라운드로 이미지가 삽입되어있기 때문에(alt가 없음) 스크린리더를 사용하는 사람들은 이 로고가 무슨 의미인지 알 수 없게 된다.

이것의 단편적인 해결방법으로

![](https://velog.velcdn.com/images/chaehe_3210/post/e29d5e36-2eea-408d-a805-8353283d6e86/image.png)

`aria-label`을 사용할 수 있다.
`aria-label = "coca cola"`를 작성해주면 스크린리더가 이 이미지는 coca cola다! 를 알려주게 된다. (alt를 대체할 수 있는 느낌으로 이해하면 될 것 같음)

아래는 aria-label을 사용할 수 없는 요소이다.

- code
- caption
- deletion
- emphasis
- generic
- insertion
- mark
- paragraph
- presentation / none
- strong
- subscript
- superscript
- suggestion
- term
- time

#### 🚀 aria-hidden

[예시 이미지 출처 : 스크린리더 사용자를 위한 착한마크업 4편(특수문자 - 구분선)](https://www.youtube.com/watch?v=hvEfSbHJAfU&list=PLtaz5vK7MbK3EAPhmB2gFnCU9qU72YMq3&index=4)

![](https://velog.velcdn.com/images/chaehe_3210/post/2770c050-3171-4b0a-992d-6156dc3aafb1/image.png)

상품별배송과 택배배송, 2일이내 출고라는 단어 사이에 있는 ㅣ 라는 글자(아무 의미 없는 꾸밈 요소)를 스크린리더는 "상품별배송. 이. 택배배송. 이. 2일이내 출고" 와 같이 "이"로 읽어버린다. 실제로 마크업에 "ㅣ"가 들어있기 때문... 아무튼 사용자에게 혼란을 주는 상황이다.

이럴 땐

```html
<span class="line">ㅣ</span>
```

에

```html
<span class="line" aria-hidden="true">ㅣ</span>
```

`aria-hidden="true"`를 넣어주면 스크린리더가 읽지 않게 된다.

이처럼 꾸밈으로만 사용된 마크업 요소 때문에 스크린리더 사용자에게 혼란을 주는 경우에 넣어주면 유용함.

<strong style="color:#cc2d2d;">🚨 읽어들여야 되는 정보가 있는 요소에는 aria-hidden="true"를 사용하면 안된다. 또 이 속성은 자식에게 상속되기 때문에 읽어들여야되는 요소의 상위에 추가하지 않도록 주의한다!!!!</strong>
