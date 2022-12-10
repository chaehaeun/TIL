## ✨ 임베디드(Embedded) 요소

### 🎇 picture

미디어 속성 (구형 브라우저일 때는 img가 화면에 보임, source가 우선순위가 더 높다는 말)

```html
<picture>
  <source media="(min-width:650px)" srcset="img_pink_flowers.jpg" />
  <source media="(min-width:465px)" srcset="img_white_flower.jpg" />
  <img src="img_orange_flowers.jpg" alt="Flowers" style="width:auto;" />
</picture>
```

타입 속성

```html
<picture>
  <source srcset="bamboo-pen.svg" type="image/svg+xml" />
  <img src="bamboo-pen-narrow.png" alt="Bamboo Pen" />
</picture>
```

두 개 이상의 이미지를 포함하는 컨테이너 요소
화면의 크기에 따라 보여지는 이미지의 크기와 종류가 달라질 때 사용하면 좋음. sns에서 작은 이미지와 큰 이미지 직접 설정해서 다운받을 수 있는 게 이거 덕분인가보당

### 🎇 track

비디오/오디오 재생 시, 자막을 표시

```html
<video src="videofile.mp4" poster="posterimage.jpg">
  <track
    kind="subtitles"
    src="videofile.ko.vtt"
    srclang="ko"
    label="한국어"
    default
  />
  <track kind="subtitles" src="videofile.en.vtt" srclang="en" label="English" />
</video>

<audio src="audiofile.mp3">
  <track kind="subtitles" src="audiofile.ko.vtt" srclang="ko" label="한국어" />
  <track kind="subtitles" src="audiofile.en.vtt" srclang="en" label="English" />
</audio>
```

default 속성을 설정하지 않을 경우, 자막 사용 안함으로 설정됨
