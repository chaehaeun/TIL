## ✨ Critical rendering path

[MDN : Critical rendering path 링크](https://developer.mozilla.org/ko/docs/Web/Performance/Critical_rendering_path)

> 중요 렌더링 경로 (Critical Rendering Path)는 브라우저가 HTML, CSS, Javascript를 화면에 픽셀로 변화하는 일련의 단계를 말하며 이를 최적화하는 것은 렌더링 성능을 향상시킵니다. - mdn

![crp](https://weareadaptive.com/wp-content/uploads/2020/04/critical-rendering-path.jpg)

👆 [이미지 출처](https://guillermo.at/browser-critical-render-path)

> 도큐먼트 오브젝트 모델은 HTML을 분석함으로써 만들어집니다. HTML은 Javascript를 요청할 수 있으며, 이 경우 DOM 이 변경될 수 있습니다. HTML은 차례대로 CSS 오브젝트 모델을 만들기 위한 스타일에 대한 요청을 만들거나 포함합니다. 브라우저 엔진은 이 두가지를 결합하여 렌더 트리를 생성하며 레이아웃은 페이지의 모든 것에 대한 크기와 위치를 결정합니다. 일단 레이아웃이 결정되면 화면에 픽셀을 그립니다.

대충 요약하면

브라우저는

1. html, css, js를 서버에서 받아와 (request / response)
2. 로딩 과정을 거쳐(loading)
3. DOM과 CSSOM을 구성하고(scripting)
4. 브라우저에 표시를 하기 위해 렌더트리를 생성한다. (render tree)
5. 그리고 요소들이 어떤 위치에, 어떤 크기로 그려질 것인지 계산을 하고(layout),
6. 레이아웃이 결정되면 브라우저에 그림을 그림(painting)

아... 벌써 머리가 아프지만 **성능 개선에 아주 중요한 개념**이다. 버벅거리는 사이트는 나도 싫고 답답하니 나는 그런 사이트 안 만들기 위해서 공부해야지...

### 🎇 Render Tree

![렌더트리](https://web-dev.imgix.net/image/C47gYyWYVMMhDmtYSLOWazuyePF2/b6Z2Gu6UD1x1imOu1tJV.png?auto=format&w=845)

👆 렌더트리 이미지

[렌더트리가 만들어지는 과정 영상](https://www.youtube.com/watch?v=lvb06W_VKVE)

DOM과 CSSOM가 결합되어 **브라우저에 실제로 보이게 될 것들을 추려 트리 구조를 생성**하는 것...!
![](https://velog.velcdn.com/images/chaehe_3210/post/d8822c11-fbf6-40ca-be67-75d04c585785/image.jpg)

실제로 보이는 것들만 추린다는 것은 `display : none` 같은 안 보이는 것들은 렌더트리에 추가 되지 않는 다는 것이다.

이때 속성에 따라 필요한 경우 Render Layer가 만들어진다.

- CSS 3D Transform(translate3d, preserve-3d 등)이나 perspective 속성이 적용된 경우
- `<video>` 또는 `<canvas>` 요소
- CSS3 애니메이션함수나 CSS 필터 함수를 사용하는 경우
- 자식 요소가 레이어로 구성된 경우
- z-index 값이 낮은 형제 요소가 레이어로 구성된 경우

왜 굳이 레이어가 나누어지지? 성능을 위해서ㅇㅇ

만약 레이어가 하나만 있고, 그 중 한 요소만 위의 속성으로 보이는 것을 변경한다면, 브라우저는 이거 하나를 바꾸기 위해 모든 요소를 전체적으로 다시 그리고 업데이트를 해야 된다. 하지만 레이어가 나뉘어져있다면 변경되어야 되는 요소만 부분적으로 업데이트를 할 수 있음. ㄹㅇ 포토샵 레이어랑 개념이 비슷하다고 보면 됨
(그렇다고 또 레이어를 너무너무 많이 나누어 놓으면 그것도 성능에 타격이 감)

근데 위의 속성 이런 거 하나도 없이 div에 간단한 속성만 있다면 레이어는 걍 하나만 사용한다.

### 🎇 layout (reflow)

요소들이 페이지에서 배치되는 위치와 방법, 각 요소의 너비와 높이 그리고 서로 관련된 위치를 결정하는 단계. position(relative, absolute, fixed..), width, height 등등 틀과 위치에 관련된 부분들이 계산된다.

화면에 보이는 요소 각각이 어디에 어떻게 위치할 지를 정해주는 과정을 Webkit에서는 layout으로, **Gecko에서는 reflow로 부르고 있다.**

드디어 나왔다 reflow... 슬비쌤이 강의에서 언급하신 reflow와 repaint 알아보려다가 여기까지 온 거임😭

### 🎇 paint (repaint)

화면에 픽셀을 그리는 단계. visibility, outline, background-color 같은 진짜 눈에 보이는 것들이 이때 그려진다.

이때 만약, Render Layer가 두 개 이상이 있다면 레이어들을 하나로 컴포짓하는 과정을 추가로 거치고 찐으로 그려지게 된다.

레이아웃과 페인트의 과정을 시각화 해놓은 영상 두개가 있다...
[Gecko reflow~paint 시각화 영상 링크](https://www.youtube.com/watch?v=ZTnIxIA5KGw)

[facebook css reflow 시각화 영상](https://www.youtube.com/watch?v=9-ezi9pzdj0)

<br/>

### 🎇 그래서 성능 좋게 하려면 어쩌라는 건데

기껏 다 적어놓고 중요한 걸 까먹고 있었다... 잘 준비하려다가 후다닥 와서 추가하는 중...

웹 성능을 위해서는 layout, 그니까 reflow 단계까지 가지 않게 css를 짜는 것이 좋다.

![](https://velog.velcdn.com/images/chaehe_3210/post/8765b157-6041-4a30-9827-f693e2f188aa/image.png)
![](https://velog.velcdn.com/images/chaehe_3210/post/29434595-54c5-4c38-a0e0-a405cb5815b1/image.png)

대표적인 예시로 엘레멘트 하나 위치를 옮기는 애니메이션을 구현하고 싶다고 칠 때, position top, left 같은 layout을 변형 시키는 속성보다 컴포짓 단계에서만 변형 시키는 transform translate로 옮기는 게 성능면에서 훨씬 좋다는 것이다.

근데 이것도 위의 이미지보면 알 수 있듯이 브라우저마다 또 차이가 있음 어후 진짜ㅡㅡ; 그래도 position은 모든 브라우저가 layout을 건드리는 반면, transform은 사파리랑 엣지 말고는 제법 괜찮은 성능을 보여준다.

참고로 위의 b / g / w / e 는

- 크롬 (Blink)
- 파이어폭스 (Gecko)
- 사파리 (WebKit)
- 엣지 (EdgeHTML)

의 웹 브라우저 엔진을 의미한다.

위의 이미지처럼 브라우저마다 reflow, repaint, composite 어쩌고를 보고 싶으면
[요기나](https://csstriggers.com/)
[요기를 들어가보셔요](https://www.lmame-geek.com/css-triggers/)
