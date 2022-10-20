## useState

### setState 가 state 를 변경시킬 수 있는 원리

state는 상수인데 어떻게 setState로 값을 변경 시키는 거지? < 애초에 이 개념이 아니었다. useState 개념 처음 봤을 때부터 아리까리하던 부분이었는데, 드디어 이해하게 됐다.

![](https://velog.velcdn.com/images/chaehe_3210/post/289b7949-049a-47c7-9dea-29b049bcf2ef/image.gif)

왜 첫번째 클릭엔 '실행될까?'가 안 뜨는지에 대한 원리를 알아보자...

![](https://velog.velcdn.com/images/chaehe_3210/post/efc0efcb-6bf3-47cf-a795-205ce2aa46a5/image.png)

useState가 정의되어 있는 곳까지 거슬러 올라가보면, useState 밖에 전역으로 선언된 `_value` 가 있다. useState를 통해 관리하는 '상태'가 바로 `_value` 이다. **`setState`는 자신이 선언된 위치에서 접근할 수 있는 `_value`를 변경하는 것이다.** (클로저)

**▶ 웹이 로딩되고 최초로 App함수가 호출**

1. useState 는 실행될 때 마다 초기값을 전달받지만,
2. 내부적으로 `_value`값이 undefined인지 확인해서,
3. 최초의 호출에만 초기값을 `_value`에 할당하고, 이후 초기값은 사용되지 않는다.
4. 이후 `_vlaue`와 그 값을 재할당하는 `setState` 함수를 배열에 담아 반환한다.

**▶ setState 호출**

5. 전달 받은 값을 react 모듈 상단의 `_value`에 할당한다.
6. 컴포넌트 리렌더링을 trigger 한다.

**▶ setState가 실행되어 리렌더링이 발생**

7. setState가 리렌더링을 트리거하며 **App 함수가 두 번째로 실행되었을 때**,
8. 다시 인수를 useState에 전달하며 호출한다.
9. useState는 내부적으로 value 값을 확인하고, undefined가 아닌 값이 할당되어 있기 때문에 초기값 할당문을 실행하지 않는다.
10. 이후 useState가 현재 시점의 value와 setState를 반환한다.
11. 두 번째 실행된 App 함수 내부에서 useState가 반환한 값을 비구조화 할당으로 추출해 변수에 할당한다.

> **setState 함수는 자신과 함께 반환된 변수를 변경시키는게 아니라(const!!)**, 다음 useState가 반환할 react 모듈의 `_value`를 변경시키고, 컴포넌트를 리렌더링 시키는 역할을 한다. 변경된 값은 useState가 가져온다!

**setState 호출 이후 로직에서도 state의 값은 이전과 동일하다는 점 유념!!!!**

여기까지의 출처 👇
https://velog.io/@yes3427/React-Side-Effect

요약하면, 처음 앱이 실행되었을 때 이니셜밸류(0)가 밸류에 할당이 된다(이전은 undefined였으니까). 그리고 그 밸류(0) 값을 state에 할당하게 된다. 그 후 첫번째 클릭 이벤트가 실행되면, setState 함수에 1이라는 값을 전달하게 되고, 함수 내부의 `_value` 재할당을 통해 값을 1로 업데이트하고 컴포넌트를 리렌더링한다.

**🚨 이때, 현재 state 값은 0이기 때문에 조건문은 실행되지 않는다.**

이제 두번째 클릭 이벤트를 실행하면, 현재 value의 값은 undefined가 아니기 때문에 setState로 값을 재할당하는 과정을 건너뛰고, 바로 1을 리턴하게 된다. state의 값이 1이기 때문에 콘솔로그도 실행되는 것....(내가 이해한 게 맞겠지?)
