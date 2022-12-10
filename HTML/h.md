## ✨ hgroup에 대해

![](https://velog.velcdn.com/images/chaehe_3210/post/16628858-24c6-472d-8860-fb252f840d4b/image.png)

오늘의 발견은 h2 태그를 썼는데, 뒤의 오늘의 어쩌고를 뭘로 묶어야 되는지 모르겠는 것이다.
처음에 생각을 좀 잘못해서 저 문장이 없어도 본문에 영향이 없지 않나? 하고 `<aside>` 썼다가 이거 아녀서 머쓱머쓱...😂 뒤의 문장은 본문에 영향이 감... 조금 생각해보면 그게 맞는 말이다ㅎㅎ...

이런 경우에선 오늘~ 핫한 제품까지가 다 제목 영역이라고 보면 된다.

이럴 때 쓰기 좋은 게 `hgroup`이다.

```html
<hgroup>
  <h1>Frankenstein</h1>
  <p>Or: The Modern Prometheus</p>
</hgroup>
```

다수의 `<h1>-<h6>` 요소를 묶을 때 사용한다... 그렇다고 h태그만 묶어야 되는 것도 아니고... `p`태그 같은 걸 섞어 써도 됨

<br/>

## ✨ 글자가 크다고 h 태그 쓰는 거 제발 그만...!

![](https://velog.velcdn.com/images/chaehe_3210/post/9fa71e18-aff4-4e1e-86c4-79536764d8c1/image.png)

이런 것만 보면 왠지 '영화, TV 프로그램을 무제한으로.'가 제목일 것 같은 느낌이 들죠? 그치만 제목으로 쓰는 건 좋지 못한 습관이다.

그도 그럴 게 이 메인 영역에서 저 문장은 제목이 아니다. 워드 같은 문서로 생각했을 때 저게 카테고리의 제목이면 읽는 사람 띠용 먼소리? 싶을 것... 제목은 따로 주고 시각적으로 안 보이게 해주면 된다.

머리론 알지만 글자가 크기만 하면 왠지 저게 제목 같고 그렇다.. 디자인적 관점에서만 보는 거 제발 멈춰!!!🤚🤚

```html
<h2 class="sr-only">넷플릭스 멤버십 등록 또는 재시작</h2>
<div class="flex flex-col items-center my-48 text-center">
  <p class="mb-24 text-sTit-l md:text-sTit-2xl">
    영화, TV 프로그램을 <br />무제한으로.
  </p>
  <p class="mb-24 font-bold text-sTxt-l md:text-sTxt-xl">
    다양한 디바이스에서 시청하세요. <br class="sm:hidden" />언제든 해지하실 수
    있습니다.
  </p>
</div>
```

이렇게 바꿨다 그래서. 테일윈드에서는 `sr-only` 라는 속성을 처음부터 제공해준다.
