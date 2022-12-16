## ✨ device-dependent event handler

> The onclick event handler (and click event) is triggered when the mouse is pressed and released when over a page element or when the Enter key is pressed while a keyboard-navigable element has focus. In these cases, onclick is a device independent event handler.

온클릭은 마우스를 눌렀다 놓거나 키보드가 가능한 요소에(탭 같은 거) 포커스가 있는 동안 Enter 키를 눌렀을 때 트리거됨. onclick은 **장치 의존성 이벤트 핸들러**임

📌 참고할 링크
[Accessible JavaScript : JavaScript Event Handlers](https://webaim.org/techniques/javascript/eventhandlers#onclick)
[Avoid the sole use of device-dependent event handlers](https://amp.levelaccess.net/public/standards/view_best_practice.php?violation_id=359)

위의 두 링크를 간단하게 요약하면,

`onClick`과 같은 이벤트 핸들러는 장치 독립적인 이벤트. 마우스나 키보드 탭 포커스 상태일 때 트리거가 된다. 근데 div와 같은 비제어 요소는 `tabindex`를 사용해도 제대로 트리거 안됨. 이런 마우스나 키보드 사용에 의존적인 이벤트들은 그 기기를 사용하지 않는 사용자층은 그 이벤트들을 제대로 사용할 수 없다는 말임. 그렇기 때문에, 이벤트를 트리거 하는 **유일한** 수단을 온클릭과 같은 장치 의존적인 이벤트로 처리해버리는 건 권장하지 않음!!!

```html
<a
  href="http://www.yahoo.com"
  onmouseover="window.status='Go to the
yahoo homepage':return true"
>
  Click here
</a>
```

이 코드는 `onmouseover`에만 의존하는 이벤트이기 때문에 접근성 문제가 생길 가능성이 있음

```html
<a
  href="http://www.yahoo.com"
  onMouseOver="window.status='Go to the
yahoo homepage':return true"
  onFocus="window.status='Go to the yahoo
homepage':return true"
>
  Yahoo Home Page
</a>
```

이렇게 마우스를 사용하지 못하는 사용자를 위한 방식으로 처리하는 게 좋음

| Event Handler | Alternative |
| ------------- | ----------- |
| OnClick       | OnKeyPress  |
| OnMouseDown   | OnKeyDown   |
| OnMouseUp     | OnKeyUp     |
| OnMouseOver   | OnFocus     |
| OnMouseOut    | OnBlur      |
| OnDblClick    | OnKeyDown   |

대안?이라기보다는 마우스/키보드 이벤트 핸들러를 같이 사용해주거나 조건을 달아서 관리해주는 게 좋은듯하다!

또, 키보드 이벤트 핸들러가 사용된 요소가 키보드 포커스를 가질 수 있는지도 확인해야 된다. 만약 아니라면 인식을 못하기 때문에 tabindex를 1 이상으로 설정해주자!
