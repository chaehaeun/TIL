## ✨ 고해상도 디바이스를 위한 css

### 🎇 우선 내 기기의 CSS pixel-ratio는?

[지금 내가 쓰고 있는 기기의 pixel-ratio 확인해볼 수 있는 사이트](https://www.mydevice.io/)

![](https://velog.velcdn.com/images/chaehe_3210/post/a0add61f-8ab9-4e35-a003-3b6464d4cb53/image.png)

애플이 픽셀 밀도를 겁나 중시하는구나 싶다 ㅎㅎ

아무튼, 저렇게 배율이 높은 기기는 일반적인 이미지는 해상도 화질 가능성이 있다는 말이다.
(배율이 1인 윈도 환경에서는 멀쩡하게 잘 보이던 이미지가 배율 2인 맥북에서 보면 살짝 불-편하게 화질저하가 생긴다든지)

그렇다면 어떻게 하는 게 좋을까!?

```css
-webkit-min-device-pixel-ratio: 2;
```

![](https://velog.velcdn.com/images/chaehe_3210/post/3adc6ce2-6454-406a-955e-23eff88a48f3/image.jpg)

이런 상황에 써먹을 수 있는 미디어쿼리도 존재한다는 사실

```css
/* 고해상도 배경이미지 미디어쿼리 */
@media (-webkit-min-device-pixel-ratio: 2),
  /* 2배율 이상일 때 */ (min--moz-device-pixel-ratio: 2),
  (-o-min-device-pixel-ratio: 2/1),
  (min-device-pixel-ratio: 2),
  (min-resolution: 192dpi),
  (min-resolution: 2dppx) {
  .logo__link {
    background-image: url(./../images/webcafe-logo@2x.png);
  }
}
```

특정 pixel Ratio 이상의 디바이스에서는 더 고화질의 이미지를 보여주는 방식이다.

📌 참고글
[Retina Ready Images and Responsive Web Design](https://designmodo.com/responsive-retina-images/)
