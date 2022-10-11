### API : 응용 프로그램 프로그래밍 인터페이스

운영체제나 프로그래밍 언어가 제공하는 기능을 제어할 수 있게 만든 인터페이스를 뜻한다. 주로 파일 제어, 창 제어, 화상 처리, 문자 제어 등을 위한 인터페이스를 제공한다.
요점 : 서버에 데이터를 요청하고, 요청한 데이터를 받아오는 작업을 API 호출이라고 부름
![Api 설명](https://velog.velcdn.com/images/chaehe_3210/post/82b25d48-e520-4e1b-8812-e2788031688b/image.png)

#### 💡 늘 아리까리했던 비동기 처리를 왜 해야 되는가에 대한 개념

우리는 API 호출에 대한 응답을 **정확히 언제 받을지 알 수가 없음**.
요청한 데이터를 응답해줄 때까지의 시간은 인터넷 연결 속도, 서버의 부하 상태 등등으로 인해 예상할 수 없고 완전히 실패(rejected)할 때도 있다. 이러한 언제 끝날지 모르는 작업들을 모두 동기적으로 처리할 수는 없음. 그렇기 때문에 Promise 객체를 이용해서 비동기 호출을 하게 된다.

> tip : https://jsonplaceholder.typicode.com/
> 무료로 api 호출에 대해 더미 데이터를 응답해주는 서비스
> 별 조건 없이 무료로 공개하는 api를 **오픈 api**라고 함

<br/>

### fetch

```javascript
// fetch 기본 문법
let promise = fetch(url, [options]);
// url - 접근하고자 하는 URL
// options - 선택 매개변수, method 나 header 등을 지정 가능
```

`fetch()`를 호출하면 브라우저는 네트워크 요청을 보내고 프로미스가 반환된다. 반환되는 프로미스는 fetch를 호출하는 코드에서 사용된다.

```javascript
fetch("https://jsonplaceholder.typicode.com/posts")
  .then((res) => res.json())
  //첫번째 then에서 응답 받아온 데이터
  .then((json) => console.log(json));
```

위 코드가 일단은 기본 틀이라고 보면 될 것 같다.<br/>
먼저, 서버에서 응답 헤더를 받자마자 fetch 호출 시 반환받은 promise가 내장 클래스 Response의 인스턴스와 함께 이행 상태가 된다.<br/> 그리고 받아온 JSON 정보를 객체 형식으로 가져오는 작업을 한다(첫번째 then).<br/> 정보를 가져오게 되면(resolve) console.log(json)을 콘솔에 찍게 된다(대충 이런 흐름으로 이해했는데 틀렸으면 알려주세요...)

으으음.... 문서 이것저것 보고 있는데 와닿는 느낌이 잘 안 들어서 이전에 노마드코더 강의를 보면서 weather api 뽑아온 예제를 들고왔다.

```javascript
function onGeoOk(position) {
  const lat = position.coords.latitude;
  const lon = position.coords.longitude;
  const url = `https://api.openweathermap.org/data/2.5/weather?lat=${lat}&lon=${lon}&appid=${API_KEY}&units=metric`;
  fetch(url) //
    .then((response) => response.json())
    .then((data) => {
      const weather = document.querySelector("#weather span:first-child");
      const city = document.querySelector("#weather span:last-child");
      city.innerText = data.name;
      weather.innerText = `${data.weather[0].main} / ${data.main.temp}`;
    });
}
```

1. 날씨 정보를 불러와지고나면,
2. (첫번째 then)파라미터 response에 JSON 값을 넣고, JSON으로 불러와진 값을 분석해 자바스크립트 객체를 생성한다.
3. (두번째 then)객체가 성공적으로 생성이 되면 data라는 파라미터에 객체의 정보가 들어오게 되고, 이것을 이용해 data.name 이나 data.weather[0].main 과 같이 정보를 쏙쏙 빼먹을 수 있게 된다!!<br/>
   (틀린 정보 있으면 둥글게 알려주쉐욧)

#### async / await 활용해보기

```javascript
function getData() {
  fetch("https://jsonplaceholder.typicode.com/posts")
    .then((jsonResponse) => jsonResponse.json())
    .then((data) => console.log(data));
}

getData();
```

위의 함수를

```javascript
async function getData() {
  let rawResponse = await fetch("https://jsonplaceholder.typicode.com/posts");
  let jsonResponse = await rawResponse.json();
  console.log(jsonResponse);
}

getData();
```

이렇게 바꿀 수 있는 것 같다.
