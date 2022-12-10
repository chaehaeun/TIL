## 텍스트 요소

### 🎇 sup, sub, abbr, s, mark, small

#### 🚀 sup, sub

**- sup 윗첨자**

<p><var>a<sup>2</sup></var> + <var>b<sup>2</sup></var> = <var>c<sup>2</sup></var></p>

```html
<p>
  <var>a<sup>2</sup></var> + <var>b<sup>2</sup></var> = <var>c<sup>2</sup></var>
</p>
```

위키사이트 볼 때

![나무위키](https://velog.velcdn.com/images/chaehe_3210/post/f6dc7332-d2cd-4232-a347-be8501ca3bed/image.png)

이런 각주 용도로 쓸 수도 있다고 한다 (정작 여기는 걍 a태그였음...)

**- sup 아래첨자**
C<sub>8</sub>H<sub>10</sub>N<sub>4</sub>O<sub>2</sub>

```html
<p>
  <var>a<sup>2</sup></var> + <var>b<sup>2</sup></var> = <var>c<sup>2</sup></var>
</p>
```

#### 🚀 mark

![](https://velog.velcdn.com/images/chaehe_3210/post/100cd1cf-ee79-48e3-949b-3f8a1d707fa5/image.png)

사용자 검색 결과에서 검색한 키워드를 시각적 하이라이트 줄 때 주로 사용

#### 🚀 abbr

읽는 사람에게 익숙하지 않을 수 있는 줄임말 같은 거 사용할 때 `<abbr>` 주로 사용
![](https://velog.velcdn.com/images/chaehe_3210/post/90e53625-f958-4777-bf1e-a23d690100a6/image.png)

#### 🚀 s

strikethrough, 취소선. 더 이상 정확하지 않은 부분에 사용하라고 함.

![](https://velog.velcdn.com/images/chaehe_3210/post/6a6f12c2-6820-4e77-b535-788298b9b125/image.png)
<small>아무거나 검색해서 가져온거</small>

예시로 할인가 같은 거 쓸 때 <s>15,000원</s> 이런 식으로 적혀있는 걸 볼 수 있음

<br/>

### 🎇 address

가까운 HTML 요소의 사람, 단체, 조직 등에 대한 연락처 정보를 나타냄

![](https://velog.velcdn.com/images/chaehe_3210/post/5468250f-7f86-4095-b74b-9ecdb12adfef/image.png)

예시 : 푸터의 기업 정보

### 🎇 pre

HTML에 작성한 내용 그대로 표현. 텍스트는 보통 고정폭 글꼴을 사용해 렌더링하고, 요소 내 공백문자를 그대로 유지함.

![](https://velog.velcdn.com/images/chaehe_3210/post/25af3360-566b-4bcc-a10b-d6b52750c068/image.png)

요런 요상한 모양으로 표현도 가넝

<pre>
<code class="language-javascript">console<span class="token punctuation">.</span><span class="token function">log</span><span class="token punctuation">(</span><span class="token string">'hello world!'</span><span class="token punctuation">)</span><span class="token punctuation">;</span></code>
</pre>

![](https://velog.velcdn.com/images/chaehe_3210/post/0b3bfcb8-8f21-467a-bdaa-b13405bc9202/image.png)

(위의 코드 블록?의 코드)

늘 쓰던 백틱으로 감싼 코드 블럭?은 code 태그를 감싼 pre 태그로 만들어져있는 것임
