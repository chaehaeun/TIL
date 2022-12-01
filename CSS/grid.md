## ✨ display : grid

[구글 검색하면 바로 나오는 1분 코딩의 그리드](https://studiomeal.com/archives/533)

대충 이 글을 토대로 공부했다. 이분 글로 플렉스도 어느 정도 익혔는데 정말 잘 설명되어있다.

flex와 grid의 차이는 flex는 한 방향 레이아웃 시스템이고 (1차원), **Grid는 두 방향(가로-세로) 레이아웃 시스템 (2차원)**이라는 점이다.

```css
.container {
  display: grid;
}
```

flex와 동일하게 grid도 부모 요소에 `display : grid;`를 넣어주면 된다.

<br/>

### 🎇 기본 용어

기본적인 용어 말고 내가 잘 모르는 것만 가져왔다. 세부적인 것들은 그냥 개발자 도구에서 이것저것 눌러보면서 배우는 게 더 효과적이기 때문에... 글로 적진 않을 것ㅎㅎ

- 그리드 트랙 (Grid Track) : Grid의 행(Row) 또는 열(Column)
- 그리드 셀 (Grid Cell) : Grid의 한 칸. Grid 아이템 하나가 들어가는 “가상의 칸(틀)”이라고 생각하면 됨
- 그리드 라인(Grid Line) : Grid 셀을 구분하는 선
- 그리드 번호(Grid Number) : Grid 라인의 각 번호
- 그리드 갭(Grid Gap) : Grid 셀 사이의 간격

<br/>

### 🎇 부모 요소에 넣는 속성

- `display: grid;` : 이게 있어야 그리드를 쓸 수 있음. 당연함

#### 🚀 grid-template-columns/grid-template-rows

- `grid-template-columns` : 열의 배치를 지정해주는 속성
- `grid-template-rows` : 행의 배치를 지정해주는 속성

```js
.container {
	grid-template-columns: 200px 200px 500px;
	/* grid-template-columns: 1fr 1fr 1fr */
	/* grid-template-columns: repeat(3, 1fr) */
	/* grid-template-columns: 200px 1fr */
	/* grid-template-columns: 100px 200px auto */

	grid-template-rows: 200px 200px 500px;
	/* grid-template-rows: 1fr 1fr 1fr */
	/* grid-template-rows: repeat(3, 1fr) */
	/* grid-template-rows: 200px 1fr */
	/* grid-template-rows: 100px 200px auto */
}

// 굉장히 다양한 단위로 지정할 수 있다...
```

```js
grid-template-columns: 100px 2fr 1fr;
```

이런 식으로 혼종을 만들어낼 수도 있음

📌 **여기서 fr 이란?**
fraction, 숫자 비율대로 트랙의 크기를 나누겠다는 의미. `1fr 1fr 1fr` 라고 쓰면 1:1:1 의 비율로 칸을 나누게 됨

<br/>

### 🎇 자식 요소에 넣는 속성

#### 🚀 각 셀의 영역을 지정하는 속성

라인의 번호로 범위를 결정한다.

![](https://velog.velcdn.com/images/chaehe_3210/post/5c425b9a-bc57-4b60-9d05-79e9649e799d/image.png)

- grid-column-start
- grid-column-end
- grid-column
- grid-row-start
- grid-row-end
- grid-row

```css
.item:nth-child(1) {
  grid-column: 1 / 3;
  grid-row: 1 / 3;
}
```

👇 위의 css가 적용된 모습

![](https://velog.velcdn.com/images/chaehe_3210/post/898cc495-b4ea-44fd-a68d-e90c0ec74a03/image.png)

#### 🚀 영역 이름으로 그리드 정의

- grid-template-areas

![](https://velog.velcdn.com/images/chaehe_3210/post/f95d6927-4942-4f7e-9612-dff40c043248/image.png)

```css
grid-template-areas:
  "header header header"
  "   a    main    b   "
  "   .     .      .   "
  "footer footer footer";
```

```css
.header {
  grid-area: header;
}

.a {
  grid-area: a;
}

.main {
  grid-area: main;
}

.b {
  grid-area: b;
}

.footer {
  grid-area: footer;
}
```

보이는 그대로...
