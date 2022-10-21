## useRef

리액트를 사용하는 프로젝트에서 DOM 을 직접 선택해야 하는 상황이 발생 할 때가 있다. (ex. 특정 엘리먼트의 크기를 가져와야 한다든지, 스크롤바 위치를 가져오거나 설정해야 된다든지, 또는 포커스를 설정해줘야 된다든지)

이럴 때 사용하는 것이 `useRef`이다.

```js
const txtArea = useRef();
```

```js
// 코드
const handleEdit = () => {
  if (localContent.length < 5) {
    txtArea.current.focus(); // 👈
    return;
  }
  if (window.confirm(`${id}번째 일기를 수정하시겠습니까?`)) {
    onEdit(id, localContent);
    toggleIsEdit();
  }
};

// DOM
<div className="content">
  {isEdit ? (
    <textarea
      value={localContent}
      ref={txtArea} // 👈 이런 식으로 DOM에 접근할 수 있다
      onChange={(e) => setLocalContent(e.target.value)}
    ></textarea>
  ) : (
    <>{content}</>
  )}
</div>;
```
