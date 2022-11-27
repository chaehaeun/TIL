## 반응형 정사각형

퍼블리셔로 일하던 때에 이거 못 만들어서 많이 애먹었다. 특정 비율대로 사각형이 줄었으면 하는데 어떻게 하는 건지 감이 안 오더라.

구글링으로 해결!
안 잊어버리게 TIL에 적어놓는다.

```
.content-wrap.etc > div {
  position: relative;
  width: 33%;
  border-radius: 5rem;
  transition: 0.3s ease-in;
}

.content-wrap.etc > div::after {
  display: block;
  content: "";
  padding-bottom: 100%;
}

.content-wrap.etc > div:hover {
  transform: scale(1.05);
}

.content-wrap.etc a {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
  border-radius: 50%;
}
```
