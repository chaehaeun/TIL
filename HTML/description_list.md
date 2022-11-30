## Description List

`<dl>`은 `<dt>`로 표기한 용어와 `<dd>` 요소로 표기한 설명 그룹의 목록을 감싸서 설명 목록을 생성한다. 주로 용어사전 구현이나 메타데이터(키-값 쌍 목록)를 표시할 때 사용.

```html
🎈 하나의 용어와 하나의 정의
<dl>
  <dt>Firefox</dt>
  <dd>
    Mozilla 재단과 수 백명의 자원봉사자가 개발한 무료 오픈소스 크로스 플랫폼
    그래픽 웹 브라우저.
  </dd>

  <!-- 다른 용어 및 정의 -->
</dl>

🎈 하나의 용어와 여러 개의 정의
<dl>
  <dt>Firefox</dt>
  <dd>
    Mozilla 재단과 수 백명의 자원봉사자가 개발한 무료 오픈소스 크로스 플랫폼
    그래픽 웹 브라우저.
  </dd>
  <dd>
    붉은 판다, 레서 판다, 랫서 판다, 혹은 "Firefox"는 초식성 포유류이다. 몸
    길이는 애완용 고양이보다 약간 큰 정도인 60cm다.
  </dd>

  <!-- 다른 용어 및 정의 -->
</dl>

🎈 여러 개의 용어와 여러 개의 정의
<dl>
  <dt>Name</dt>
  <dd>Godzilla</dd>
  <dt>Born</dt>
  <dd>1952</dd>
  <dt>Birthplace</dt>
  <dd>Japan</dd>
  <dt>Color</dt>
  <dd>Green</dd>
</dl>

🎈 여러 개의 용어와 여러 개의 정의
<dl>
  <div>
    <dt>Name</dt>
    <dd>Godzilla</dd>
  </div>
  <div>
    <dt>Born</dt>
    <dd>1952</dd>
  </div>
  <div>
    <dt>Birthplace</dt>
    <dd>Japan</dd>
  </div>
  <div>
    <dt>Color</dt>
    <dd>Green</dd>
  </div>
</dl>
```

남용하라고 만들어져있는 태그는 아니니 정확한 쓰임새에만 쓰도록 하자!

[참고 글 : dl, dt, dd 태그를 남용하지 마세요.](https://aoa.gitbook.io/skymimo/aoa-2019/tips-2/dl-dt-dd-.)
