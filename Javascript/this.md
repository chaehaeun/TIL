# This

this 는 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수다. <br/> this를 통해 자신이 속한 객체 또는 자신이 생성할 인스턴스의 프로퍼티나 메서드를 참조할 수 있다.

### 자유로운 this

다른 언어를 사용하다 자바스크립트로 넘어온 개발자는 this는 항상 메서드가 정의된 객체를 참조할 것이라고 생각하기 쉽다. <br/>
하지만 자바스크립트에서의 `this`는 런타임에 따라 결정된다.<br/>
**메서드가 어디에서 정의되었는지에 상관없이 this는 '점 앞의' 객체(caller)가 무엇인가에 따라 자유롭게 결정된다.**

> #### ❓ this 바인딩?
>
> 바인딩이란 식별자와 값을 연결하는 과정을 의미한다. 예를 들어, 변수 선언은 변수 이름(식별자)과 확보된 메모리 공간의 주소를 바인딩하는 것이다. this 바인딩은 this와 this를 가리킬 객체를 바인딩하는 것이다!

this는 어디에서든 참조 가능하다. 전역에서도 함수 내부에서도 가능!

```js
console.log(this);
```

▶ **전역 컨텍스트**에서의 `this`는 브라우저에서는 window, 노드에서는 모듈을 가리킨다.( `globalThis` )

```js
function sayHi() {
  console.log(this);
}

sayHi(); // undefined
```

▶ 객체가 없어도 함수를 호출할 수 있다. 하지만 this는 객체의 프로퍼티나 메서드를 참조하기 위한 자기 참조 변수이므로 일반적으로 객체의 메서드 내부 또는 생성자 함수 내부에서만 의미가 있다.

위의 코드 같은 경우엔 느슨한 모드에서 `this`는 `글로벌this`를 가리킬 것이고, **엄격한 모드에서는 undefined를 출력하게 될 것이다.**

일반 함수 내부에서 this를 사용할 필요가 없기 때문에, 이런 식의 코드는 대개 실수로 작성된 경우가 많다고 한다. 별로 권장하지 않는 듯

## 함수 호출 방식과 this 바인딩

> #### 📃 렉시컬 스코프와 this 바인딩은 결정 시기가 다르다.
>
> 함수의 상위 스코프를 결정하는 방식인 렉시컬 스코프는 함수 정의가 평가되어 함수 객체가 생성되는 시점에 상위 스코프를 결정한다. 하지만 this는 함수 호출 시점에서 결정된다.

아래의 예제로 보자.

```js
let user = { name: "John" };
let admin = { name: "Admin" };

function sayHi() {
  alert(this.name);
}

// 별개의 객체에서 동일한 함수를 사용함
user.f = sayHi;
admin.f = sayHi;

// 'this'는 '점(.) 앞의' 객체를 참조하기 때문에
// this 값이 달라짐
user.f(); // John  (this == user)
admin.f(); // Admin  (this == admin)

admin["f"](); // Admin (점과 대괄호는 동일하게 동작함)
```

동일한 함수여도 함수가 호출된 시점(런타임)에 따라서 다른 값을 참조한 것을 알 수 있다.

여러가지 함수 호출 방식에서 this 바인딩이 어떻게 결정되는지 조오오금 더 자세하게 알아보자!

<br/>

### 메서드 호출

위에서 기술했듯이 메서드 내부의 this는 메서드를 호출한 객체(caller)에 바인딩 된다.

```js
const person = {
  name: "Lee",
  getName() {
    return this.name;
  },
};

// 메서드 getName을 호출한 객체는 person이다.
console.log(person.getName()); // Lee
```

여기서 확실하게 짚고 넘어가야 되는 것은, `person` 객체의 `getName` 프로퍼티가 가리키는 함수 객체는 `person` 객체에 포함된 것이 아니라 독립적으로 존재하는 별도의 객체이다. `getName` 프로퍼티가 함수의 객체를 가리키고 있을 뿐이다.

그렇기 때문에 `getNmae` 메서드는 일반 변수에 할당하여 일반 함수로 호출할 수도 있다.

```js
const person = {
  name: "Lee",
  getName() {
    return this.name;
  },
};

const anotherPerson = {
  name: "Kim",
};
// getName 메서드를 anotherPerson 객체의 메서드로 할당
anotherPerson.getName = person.getName;

// getName 메서드를 호출한 객체는 anotherPerson 이기 때문에
// 콘솔에 찍히는 값은 Kim 이다.
console.log(anotherPerson.getName()); // Kim

// getName 메서드를 변수에 할당
const getName = person.getName;

// getName 메서드를 일반 함수로 호출
console.log(getName()); // ' '
// 이때 일반 함수로 호출된 getName 함수 내부의 this.name은 globalThis가 된다.
```

[이미지 출처](https://poiemaweb.com/js-this)
![](https://velog.velcdn.com/images/chaehe_3210/post/48891e0b-8f40-4e49-8159-055ab6065710/image.png)

<br/>

### 화살표 함수

화살표 함수는 일반 함수와 달리 고유한 `this`를 가지지 않는다. 화살표 함수에서 `this`를 참조하면, 화살표 함수가 아닌 외부 함수에서 `this`값을 가져온다.<br/>
다른 말로, 화살표 함수 안의 `this`는 동적으로 정해지지 않는다는 것이다.

아래 예시에서 함수 `arrow()`의 `this`는 외부 함수 `user.sayHi()`의 `this`가 된다.

```js
let user = {
  firstName: "보라",
  sayHi() {
    let arrow = () => alert(this.firstName);
    arrow();
  },
};

user.sayHi(); // 보라
```

별개의 `this`가 만들어지는 건 원하지 않고, 외부 컨텍스트에 있는 `this`를 이용하고 싶은 경우 화살표 함수가 유용하다.

이런 특징은 객체의 메서드 안에서 동일 객체의 프로퍼티를 대상으로 순회를 하는데 사용할 수 있다.

```js
let group = {
  title: "1모둠",
  students: ["보라", "호진", "지민"],

  showList() {
    this.students.forEach((student) => alert(this.title + ": " + student));
  },
};

group.showList();
```

예시의 `forEach`에서 화살표 함수를 사용했기 때문에 화살표 함수 본문에 있는 `this.title`은 화살표 함수 바깥에 있는 메서드인 `showList`가 가리키는 대상과 동일해진다. 즉 `this.title`은 `group.title`과 같다.

위의 예시에서 화살표 함수 대신 일반 function 함수를 사용했다면 에러가 발생했을 것이다.(this가 undefined기 때문)

<br/>

### 생성자 함수 호출

생성자 함수 내부의 this에는 생성자 함수가 (미래에) 생성할 인스턴스가 바인딩 된다.

```js
function Circle(radius) {
  //생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

const circle1 = new Circle(5);
const circle2 = new Circle(10);

console.log(circle1.getDiameter()); // 10
console.log(circle2.getDiameter()); // 20
```

<br/>

### bind()

추가해야지 해야지 하고 있다가 영영 안할 것 같았던...bind 함수가 갑자기 인강에서 튀어나와서 개념 한 번 보고 가려고 한다.

```js
let boundFunc = func.bind(context);
```

함수를 사전에 구성할 수 있게 해주는 메서드. (즉시 호출되지는 않는다고 한다.)

```js
let user = {
  firstName: "John",
};

function func() {
  alert(this.firstName);
}

let funcUser = func.bind(user);
funcUser(); // John
```

boundFunc를 호출하면 this가 고정된 func를 호출하는 것과 동일한 효과이다. 실행이 예정된 함수에서 this 예약어를 사용하게 한다는 말... 혼란스러운 것을 보니 this가 헷갈리고 있는 것 같네 복습하러 가겠습니다.

#### 부분 적용

아래에 작성할 예시에 사용된 적용 방식이다.

bind 함수는 this 뿐 아니라 **인수도 바인딩이 가능**하다.

```js
let bound = func.bind(context, [arg1], [arg2], ...);
```

```js
function mul(a, b) {
  return a * b;
}

let double = mul.bind(null, 2);

alert(double(3)); // = mul(2, 3) = 6
alert(double(4)); // = mul(2, 4) = 8
alert(double(5)); // = mul(2, 5) = 10
```

mul.bind(null, 2)를 호출하면 새로운 함수 double이 만들어진다. double엔 컨텍스트가 null, 첫 번째 인수는 2인 mul의 호출 결과가 전달된다.** 추가 인수는 ‘그대로’ 전달됨.**

잘 쓰이는 예시는 아니지만 알아두면 유용할 것 같다.

👇 이제 강의 예시로 보겠음

```js
// 커스텀훅
// .... sendTaskRequest 함수의 일부
      applyData(data);
    } catch (err) {
      setError(err.message || "Something went wrong!");
    }
    setIsLoading(false);
//


// app.js
const NewTask = (props) => {
  const { isLoading, error, sendRequest: sendTaskRequest } = useHttp();

  const createTask = (taskText, taskData) => {
    const generatedId = taskData.name; // firebase-specific => "name" contains generated id
    const createdTask = { id: generatedId, text: taskText };

    props.onAddTask(createdTask);
  };

  const enterTaskHandler = async (taskText) => {
    sendTaskRequest(
      {
        url: "https://httppractice-a67a8-default-rtdb.asia-southeast1.firebasedatabase.app/tasks.json",
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: { text: taskText },
      },
      createTask.bind(null, taskText)
    );
  };

  return (
    <Section>
      <TaskForm onEnterTask={enterTaskHandler} loading={isLoading} />
      {error && <p>{error}</p>}
    </Section>
  );
};
```

createTask의 첫번째 인수를 인풋에서 받아온 taskText로 고정시키고, 실제로 호출된 커스텀훅에서 받아온 데이터는 createTask의 두번째 인수로 들어가진다.
