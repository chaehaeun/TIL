## ✨ 숨김 텍스트

[참고 글](https://mulder21c.github.io/2019/03/22/screen-hide-text/)

### 🎇 Black Hat

검색 엔진은 text/html만을 수집하는 것이 아닌, Google 알고리즘의 경우 CSS도 검색함. 이걸 활용해 검색 엔진 결과 페이지의 순위를 높이기 위해 올바르지 않은 방법을 사용한 사례도 있음. 이것 때문에 구글은 [검색엔진 최적화(SEO) 기본 가이드](https://developers.google.com/search/docs/essentials/spam-policies?visit_id=638058941402093043-357955519&rd=1#hidden-text-and-links)에서 **숨김 텍스트를 사용하지 말라고 함**. 잘못하면 검색 엔진이 접근성을 위해 안드로메다로 보내놓은 어쩌고를 스팸이라고 인식해릴 수도 있다고 함...

이것 때문에 보이지 말아야할 어쩌고를 안드로메다로 보내놓고 해결하는 방식은 옳지 못하다는 결론(text-indent 같은 게 여기 해당... 나도 가끔 썼음)

그렇다고 opasity, display:none, zero pixel sizing 같은 것들을 사용하면 이제 스크린리더가 읽지 못함. 이럴 거면 html 작성한 것부터가 의미 없어짐ㅎㅎ;

그럼... 어떻게 하란 말임?

### 🎇 clip 사용하기

```css
.hidden {
  overflow: hidden;
  position: absolute;
  border: 0;
  width: 1px;
  height: 1px;
  clip: rect(1px, 1px, 1px, 1px);
}
```

앱솔루트로 띄우고 안 보이게 클리핑 해놓는 방식. 그으으은데 이것도 문제가 있다.

이 방식은 absolute를 사용했기 때문에 숨겨야할 텍스트를 BFC(block formatting context)로 변경시켜버림. 한 문단에서 전달되어야 되는 정보를 BFC로 변경 시키면 **분리**가 일어남. 즉, 한 번 뚝. 끊어서 읽게 된다는 말임.

그래서 어쩌라는 거임?

### 🎇 해결방식

```css
.sr-only,
legend {
  position: absolute;
  width: 1px;
  height: 1px;
  overflow: hidden;
  clip: rect(0 0 0 0); /* 구형 브라우저 때문 */
  clip-path: inset(50%);
}
```

<small>참고로 클래스 이름은 sr-only, offscreen, readable-hidden 등등 다양하게 사용된다.</small>

clip-path : 원하는 모양대로 마스킹하는 기능
[clip-path 연습 사이트](https://bennettfeely.com/clippy/)
