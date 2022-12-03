## ✨ 자잘한 vscode 팁, 단축키

### 🎇 한 줄 삭제하기

`shift` + `ctrl` + `k`

### 🎇 emmet 수식 평가 (커스텀)

`shift` + `alt` + `m` : emmet 수식 평가

### 🎇 emmet 약어로 래핑 (커스텀)

`shift` + `alt` + `w` : emmet 약어로 래핑

```html
ul.member>li*5>a[href='/']+span[aria-hidden='true']{:}

<ul class="member">
  <li><a href="/"></a><span aria-hidden="true">:</span></li>
  <li><a href="/"></a><span aria-hidden="true">:</span></li>
  <li><a href="/"></a><span aria-hidden="true">:</span></li>
  <li><a href="/"></a><span aria-hidden="true">:</span></li>
  <li><a href="/"></a><span aria-hidden="true">:</span></li>
</ul>

ul>li.class$*>a[href='/']{어디에붙을까}
<ul>
  <li class="class1">
    <a href="/">어디에붙을까11</a>
  </li>
  <li class="class2">
    <a href="/">어디에붙을까22</a>
  </li>
  <li class="class3">
    <a href="/">어디에붙을까33</a>
  </li>
  <li class="class4">
    <a href="/">어디에붙을까44</a>
  </li>
  <li class="class5">
    <a href="/">어디에붙을까55</a>
  </li>
</ul>
```
