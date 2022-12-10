## ✨ br 태그 대신 span을 써보는 것은?

일할 때 이걸로 고민 많이 했더랬다. 처음엔 줄바꿈이 있는데 화면이 작아지면 이 줄바꿈을 없애고 한 줄로만 써줘야 더 예쁠 때가 많았음.

나는 일할 때 이것저것 생각해보고

```html
<p class="mb-24 font-bold text-sTxt-l md:text-sTxt-xl">
  다양한 디바이스에서 시청하세요. <br class="sm:hidden" />언제든 해지하실 수
  있습니다.
</p>
```

br 태그를 반응형으로 숨기는 식으로 줄바꿈을 없앴다.

근데 오늘 피드백 들으면서 알게 된 건 여기에 `span`을 써도 문제가 없다는 것이다.

![링크텍스트](https://pbs.twimg.com/media/CY1tHOnUoAEqJd-?format=jpg&name=360x360)

엥 그러네?

```html
<p class="mb-24 font-bold text-sTxt-l md:text-sTxt-xl">
  <span>다양한 디바이스에서 시청하세요.</span>
  <span>언제든해지하실 수 있습니다.</span>
</p>
```

이렇게 span 태그로 묶어놓고 화면 크기에 따라 블록 요소, 인라인 요소로 바꾸면 되는 것임... 생각도 못해본 발상이라 띠용! 했음.

새로운 관점에서 접근해보니 시야가 정말 넓어지는 느낌이다. 뿌듯!
