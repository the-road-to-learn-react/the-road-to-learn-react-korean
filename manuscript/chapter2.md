# 리액트 기초 Basics in React

이번 장에서는 리액트 기초적인 내용을 알아볼 것입니다. 정적인 컴포넌트보다 동적인 컴포넌트와 인터렉션을 구현해보면서 컴포넌트를 선언과 재사용 가능한 컴포넌트에 대해 배우고, 마지막으로 컴포넌트에 숨결을 불어넣어 생명체로 만들어 봅시다.

## 컴포넌트 내부 상태 Internal component state

컴포넌트 내부 상태(Internal component state)는 컴포넌트에 저장된 상태(state)의 프로퍼티를 수정, 삭제, 저장할 수 있습니다. 다른 말로는 지역 상태(local state)라고도 합니다. ES6 클래스 생성자(class constructor)를 사용해 컴포넌트를 만들고 내부 컴포넌트 상태를 초기화할 수 있습니다. 생성자는 컴포넌트가 초기화될 때 한 번만 호출됩니다.

이제 클래스에 대해 알아봅시다.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

# leanpub-start-insert
  constructor(props) {
    super(props);
  }
# leanpub-end-insert

  ...

}
~~~~~~~~

ES6 클래스 컴포넌트에 생성자가 있는 경우, App 컴포넌트는 `Component`의 하위 클래스이기 때문에 반드시 `super();`를 호출하고, App 컴포넌트를 선언할 때 `extends Component`로 컴포넌트를 확장시킵니다. 이 부분은 ES6 클래스 컴포넌트 부분에서 좀더 알아보겠습니다.

또한 `super(props);`도 호출할 수 있습니다. 생성자에 접근하기 원한다면 생성자 내 `this.props`를 설정합니다. 이를 설정하지 않으면 생성자에서 `this.props`를 접근할 때 `undefined`로 출력됩니다. 이 부분은 리액트 props를 설명할 때 자세히 알아보겠습니다.

아래와 같이 list라는 배열을 만들고 이를 컴포넌트 초기 state로 만듭니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
const list = [
  {
    title: 'React',
    url: 'https://facebook.github.io/react/',
    author: 'Jordan Walke',
    num_comments: 3,
    points: 4,
    objectID: 0,
  },
  ...
];

class App extends Component {

  constructor(props) {
    super(props);

# leanpub-start-insert
    this.state = {
      list: list,
    };
# leanpub-end-insert
  }

  ...

}
~~~~~~~~

state는 `this`객체를 사용해 클래스에 바인딩됩니다. 따라서 전체 컴포넌트 내 로컬 상태값에 액세스 할 수 있게 됩니다. 때문에 `render()` 메서드에서 state를 사용할 수 있게 됩니다. 앞서 컴포넌트 내부에 list를 정의해 사용했지만, 컴포넌트 state에 정의하여 `render()` 메서드에 사용했습니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    return (
      <div className="App">
# leanpub-start-insert
        {this.state.list.map(item =>
# leanpub-end-insert
          <div key={item.objectID}>
            <span>
              <a href={item.url}>{item.title}</a>
            </span>
            <span>{item.author}</span>
            <span>{item.num_comments}</span>
            <span>{item.points}</span>
          </div>
        )}
      </div>
    );
  }
}
~~~~~~~~

컴포넌트 내부에서도 배열의 내 항목을 추가, 변경 제거를 할 수 있습니다. 컴포넌트 상태를 변경할 때마다 `render()` 메소드가 재실행됩니다. 이를 통해 쉽게 컴포넌트 상태를 변경하고 전달된 데이터가 올바르게 표시되는지 확인할 수 있습니다.

그러나 절대로 상태를 직접 변경해서는 안됩니다. 상태를 변경하려면 `setState()` 메소드를 사용합니다. 다음 장에서 `setState()` 메소드에 대해 알아보겠습니다. 

### 실습하기

* 컴포넌트에 state를 만들고 테스트합니다
  * 생성자에 초기 상태 값을 할당합니다.
  * `render()`로 state를 사용하고 표시합니다.
  
### 읽어보기 
* [[MDN] : ES6 클래스 생성자](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes#Constructor)

## ES6 객체 초기자 Object Initializer

자바스크립트 ES6는 좀더 간결해진 문법으로 객체 초기자를 작성합니다. 기존에는 아래와 같이 객체 정의를 해왔을 것입니다.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const name = 'Robin';

const user = {
  name: name,
};
~~~~~~~~

ES6에서는 개체 프로퍼티 이름과 변수 이름이 같다면, 축약해 정의할 수 있습니다.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const name = 'Robin';

const user = {
  name,
};
~~~~~~~~

리액트 애플리케이션도 동일합니다. state 프로퍼티 이름과 변수 이름이 동일하기 때문에 `list`로 선언하면 됩니다.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// ES5
this.state = {
  list: list,
};

// ES6
this.state = {
  list,
};
~~~~~~~~

ES6에서는 객체 메소드도 축약합니다.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// ES5
var userService = {
  getUserName: function (user) {
    return user.firstname + ' ' + user.lastname;
  },
};

// ES6
const userService = {
  getUserName(user) {
    return user.firstname + ' ' + user.lastname;
  },
};
~~~~~~~~

또한 ES6에서는 계산된 프로퍼티를 이름(computed property name)을 사용할 수 있다.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// ES5
var user = {
  name: 'Robin',
};

// ES6
const key = 'name';
const user = {
  [key]: 'Robin',
};
~~~~~~~~

계산된 프로퍼티 이름(computed property name)이 이해가 안될 수도 있습니다. 왜 필요할까요? 다음 장에서 키(key)를 사용해 객체에서 동적인 방식으로 값을 할당하는데 이 때 사용하기 위함입니다. 자바스크립트에서 룩업 테이블(lookup tables: 주어진 연산에 대해 미리 계산된 결과들의 배열)을 생성하는 것이 코드 가독성을 높여줍니다.

### 읽어보기
* [[MDN] ES6 객체 초기자 (object initializer)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Object_initializer)

### 실습하기

* ES6 객체 초기자를 생성해봅니다.


## 단방향 데이터 흐름 Unidirectional Data Flow

App 컴포넌트에 state가 있지만 그 값이 변동되지 않아 정적인 컴포넌트라 할 수 있습니다. state를 조작하여 동적인 컴포넌트를 만들어 봅시다.

각 배열의 항목마다 `취소` 버튼을 추가하고, 버튼을 클릭하면 각 항목이 삭제되도록 구현해봅시다.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    return (
      <div className="App">
        {this.state.list.map(item =>
          <div key={item.objectID}>
            <span>
              <a href={item.url}>{item.title}</a>
            </span>
            <span>{item.author}</span>
            <span>{item.num_comments}</span>
            <span>{item.points}</span>
# leanpub-start-insert
            <span>
              <button
                onClick={() => this.onDismiss(item.objectID)}
                type="button"
              >
                Dismiss
              </button>
            </span>
# leanpub-end-insert
          </div>
        )}
      </div>
    );
  }
}
~~~~~~~~

`onDismiss()` 메소드는 아직 구현하지 않았습니다. 우선 버튼의 `onClick` 핸들러를 살펴봅시다. `onDismiss()` 메소드는 화살표 함수입니다. 각 `item`의  `objectID` 프로퍼티로 삭제될 항목을 식별한다. 다른 방법은 `onClick` 핸들러 밖에서 함수를 정의한 후  이 함수를 핸들러에 전달하는 것입니다. 다음 장에서 핸들러에 대해 배울 것입니다.

button 요소는 한 줄이 아닌 여러 줄로 작성되었습니다.여러 속성을 한 줄로 나열해 길게 코드를 작성하면 이해하기 어려워집니다. 들여쓰기와 적절한 줄바꿈으로 누가봐도 코드가 읽기 쉽게 작성해야 합니다. 필수사항은 아니지만 권장하는 코드 작성법입니다.

`onDismiss()` 함수를 구현하겠습니다. 항목을 식별하기 위해 ID가 필요합니다. 이 함수는 클래스에 바인딩되어있으므로 클래스 메서드입니다. 따라서 `onDismiss()`가 아니라 `this.onDismiss()`로 접근합니다. `this` 객체는 클래스 인스턴스이다. `onDismiss()`를 클래스 메소드로 정의하기 위해서는 생성자에서 바인딩을 해야합니다. 바인딩은 다음 장에서 알아보겠습니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  constructor(props) {
    super(props);

    this.state = {
      list,
    };

# leanpub-start-insert
    this.onDismiss = this.onDismiss.bind(this);
# leanpub-end-insert
  }

  render() {
    ...
  }
}
~~~~~~~~


이제 클래스 함수와 함수가 할 일을 정의합시다. 클래스 메소드는 아래와 같이 작성합니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  constructor(props) {
    super(props);

    this.state = {
      list,
    };

    this.onDismiss = this.onDismiss.bind(this);
  }

# leanpub-start-insert
  onDismiss(id) {
    ...
  }
# leanpub-end-insert

  render() {
    ...
  }
}
~~~~~~~~

클래스 메소드 내부에 일어나는 일을 차근차근 설명해봅시다. list에서 ID와 매칭되는 항목을 제거하고 업데이트된 list를 state에 저장합니다. 그 다음 `render()` 메소드가 재 실행되고 업데이트된 list가 브라우저에 표시됩니다. 제거된 항목은 더 이상 보이지 않아야 합니다.

자바스크립트 `filter()` 메소드를 사용해 배열의 요소를 삭제할 수 있습니다. filter 함수는 입력 기능을 사용합니다. 이 함수는 배열을 반복해 각 요소마다 액세스합니다. 이렇게하면 필터 조건에 따라 배열의 각 요소를 대조합니다. 일치하면 배열이 그대로 유지됩니다. 반대로 일치하지 않으면 배열에서 필터링 됩니다. 그러나 반환되는 배열은 원래 배열 값을 변경하지 않는 불변 데이터 구조입니다. 리액트의 관습을 따르고 있습니다.
 
{title="src/App.js",lang=javascript}
~~~~~~~~
onDismiss(id) {
# leanpub-start-insert
  const updatedList = this.state.list.filter(function isNotId(item) {
    return item.objectID !== id;
  });
# leanpub-end-insert
}
~~~~~~~~

`filter()` 메소드를 보다 간결한 구문으로 작성할 수 있습니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
onDismiss(id) {
# leanpub-start-insert
  function isNotId(item) {
    return item.objectID !== id;
  }

  const updatedList = this.state.list.filter(isNotId);
# leanpub-end-insert
}
~~~~~~~~

ES6 화살표 함수를 사용해 좀더 간결한 구문으로 작성합시다.

{title="src/App.js",lang=javascript}
~~~~~~~~
onDismiss(id) {
# leanpub-start-insert
  const isNotId = item => item.objectID !== id;
  const updatedList = this.state.list.filter(isNotId);
# leanpub-end-insert
}
~~~~~~~~

버튼 `onClick` 핸들러 안에 인라인으로 함수를 작성할 수 있지만 코드 가독성이 떨어집니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
onDismiss(id) {
# leanpub-start-insert
  const updatedList = this.state.list.filter(item => item.objectID !== id);
# leanpub-end-insert
}
~~~~~~~~

버튼을 클릭하면 각 항목이 없어지는 것을 볼 수 있습니다. 그러나 state는 업데이트 되지 않았습니다. 마지막에 `setState()` 클래스 메소드로 `this.state.list` 를 업데이트 합시다.
{title="src/App.js",lang=javascript}
~~~~~~~~
onDismiss(id) {
  const isNotId = item => item.objectID !== id;
  const updatedList = this.state.list.filter(isNotId);
# leanpub-start-insert
  this.setState({ list: updatedList });
# leanpub-end-insert
}
~~~~~~~~

애플리케이션을 재실행해 "닫기" 버튼을 클릭해 작동하는지 확인합니다. 지금까지 실습한 내용이 바로 리액트의  __단방향 데이터 흐름(unidirectional data flow)__ 입니다. `onClick()`을 사용해 뷰에서 액션을 트리거하면, 함수 또는 클래스 메소드가 내부 컴포넌트 상태를 수정하고 컴포넌트의 `render()`메소드가 재실행되어 뷰를 업데이트합니다.

![단방향 데이터 흐름으로 내부 상태를 업데이트 합니다.](images/set-state-to-render-unidirectional.png)

### 읽어보기

* [[리액트 공식문서] 리액트 state와 생명주기](https://facebook.github.io/react/docs/state-and-lifecycle.html)

## 바인딩 Bindings

리액트에서 ES6 클래스 컴포넌트를 사용할 때 자바스크립트 바인딩(binding) 개념을 잘 알고 있어야 합니다. 이전 장에서 생성자 안에 `onDismiss()` 클래스 메소드를 바인딩했습니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {
  constructor(props) {
    super(props);

    this.state = {
      list,
    };

    this.onDismiss = this.onDismiss.bind(this);
  }

  ...
}
~~~~~~~~

왜 처음부터 바인딩을 했을까요? 클래스 메소드는 클래스 인스턴스에 자동으로 `this`를 바인딩하지 않기 때문에 일일이 바인딩해야 합니다. 아래 ES6 클래스 컴포넌트를 예시로 들겠습니다.

{title="Code Playground",lang=javascript}
~~~~~~~~
class ExplainBindingsComponent extends Component {
  onClickMe() {
    console.log(this);
  }

  render() {
    return (
      <button
        onClick={this.onClickMe}
        type="button"
      >
        Click Me
      </button>
    );
  }
}
~~~~~~~~

위 컴포넌트 렌더링은 잘 되지만, 버튼을 클릭하면 개발자 콘솔 로그에 `undefined`이 출력됩니다. 리액트를 사용할 때 많이 실수하는 버그 중 하나입니다. 클래스 메소드에서`this.state`를 접근하고자 할 때 `this`가 `undefined`이면 접근할 수 없습니다. 따라서 클래스 메소드에서 `this`를 접근할 수 있게 하려면 클래스 메소드를 `this`에 바인딩 해야합니다.

아래 클래스 컴포넌트의 클래스 메서드는 클래스 생성자에서 올바르게 바인딩했습니다.

{title="Code Playground",lang=javascript}
~~~~~~~~
class ExplainBindingsComponent extends Component {
# leanpub-start-insert
  constructor() {
    super();

    this.onClickMe = this.onClickMe.bind(this);
  }
# leanpub-end-insert

  onClickMe() {
    console.log(this);
  }

  render() {
    return (
      <button
        onClick={this.onClickMe}
        type="button"
      >
        Click Me
      </button>
    );
  }
}
~~~~~~~~

버튼 구현 시, 클래스 인스턴스를 구체적으로 지정하기 위해 `this` 객체를 정의한 후, `this.state`에 접근하거나, `this.props`를 사용해 접근합니다.

클래스 메서드 바인딩은 어느 곳에서든지 사용할 수 있습니다. 클래스 메소드인 `render()`에서도 사용할 수 있습니다.

{title="Code Playground",lang=javascript}
~~~~~~~~
class ExplainBindingsComponent extends Component {
  onClickMe() {
    console.log(this);
  }

  render() {
    return (
      <button
# leanpub-start-insert
        onClick={this.onClickMe.bind(this)}
# leanpub-end-insert
        type="button"
      >
        Click Me
      </button>
    );
  }
}
~~~~~~~~


그러나 `render()` 메소드가 실행될 때마다 클래스 메소드를 바인드하기 때문에, 컴포넌트가 업데이트 될 때마다 실행되어 성능에 영향을 미칩니다. `render()` 메소드에서 바인딩하지 않는 편이 좋습니다. 생성자에서 클래스 메서드를 바인딩해 컴포넌트가 인스턴스화 될 때 처음에 한 번만 바인딩하는 것이 더 좋은 방법입니다.

이외에 생성자에서 바로 클래스 메서드의 할 일을 정의할 수도 있습니다.

{title="Code Playground",lang=javascript}
~~~~~~~~
class ExplainBindingsComponent extends Component {
  constructor() {
    super();

# leanpub-start-insert
    this.onClickMe = () => {
      console.log(this);
    }
# leanpub-end-insert
  }

  render() {
    return (
      <button
        onClick={this.onClickMe}
        type="button"
      >
        Click Me
      </button>
    );
  }
}
~~~~~~~~

이 방법 또한 앞으로 생성자에게 혼란을 줄 수 있기 때문에 사용하지 않는 것이 좋습니다. 생성자는 클래스의 모든 프로퍼티를 인스턴스화하기 위해서 존재합니다. 따라서 클래스 메소드의 로직은 생성자 외부에서 정의합니다.

{title="Code Playground",lang=javascript}
~~~~~~~~
class ExplainBindingsComponent extends Component {
  constructor() {
    super();

    this.doSomething = this.doSomething.bind(this);
    this.doSomethingElse = this.doSomethingElse.bind(this);
  }

  doSomething() {
    // do something
  }

  doSomethingElse() {
    // do something else
  }

  ...
}
~~~~~~~~

이외에 ES6 화살표 함수를 사용하면 클래스 메소드를 생성자 내부에 바인딩하지 않고도 자동 바인딩할 수 있습니다.

{title="Code Playground",lang=javascript}
~~~~~~~~
class ExplainBindingsComponent extends Component {
  onClickMe = () => {
    console.log(this);
  }

  render() {
    return (
      <button
        onClick={this.onClickMe}
        type="button"
      >
        Click Me
      </button>
    );
  }
}
~~~~~~~~

클래스 메소드 마다 생성자에 바인딩 처리를 하기 번거롭기 때문에 위 방법을 추천합니다. 그러나 리액트 공식 문서는 생성자 내부에 클래스 메소드를 바인딩할 것을 제안하고 있습니다. 따라서 우리도 공식 문서의 지침에 따라 생성자 내부에 클래스 메소드를 바인딩하겠습니다.

### 실습하기

* 여러가지 바인딩을 시도하며 콘솔에 `this`를 출력합니다.


## 이벤트 핸들러 Event Handler

이번 장에서는 이벤트 핸들러에 대해 알아봅시다. 각 항목의 버튼을 클릭하면 해당 항목을 삭제하는 기능을 구현해 볼 것입니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
...

<button
  onClick={() => this.onDismiss(item.objectID)}
  type="button"
>
  Dismiss
</button>

...
~~~~~~~~

위 코드가 복잡하게 보일지도 모르겠습니다. 클래스 메소드 인자를 전달해기 때문에 화살표 함수로 래핑했습니다. 일반적으로 `render()` 밖에서 이벤트 핸들러에 전달되는 함수를 만들어야 합니다. 애플리케이션이 구동되면 클래스 메서드가 즉시 실행되기 때문에 코드는 작동하지 않을 겁니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
...

<button
  onClick={this.onDismiss(item.objectID)}
  type="button"
>
  Dismiss
</button>

...
~~~~~~~~

`onClick={doSomething()}`일 경우, `doSomething()` 함수는 그 즉시 실행됩니다. 핸들러 표현식이 평가되는데, 그 결과를 함수로 반환하지 않기 때문에 버튼을 클릭해도 아무 일도 일어나지 않습니다. 따라서 `onClick={doSomething}`으로 수정하면 실행됩니다. 이를 `onDismiss()`클래스 메소드에 적용해봅시다.

`onClick={this.onDismiss}`로 고쳤겠지만 이 역시 문제가 있습니다. 삭제해야할 항목을 식별하는 `item.objectID`가 빠져있습니다. 함수의 인자를 추가합시다.
 
{title="src/App.js",lang=javascript}
~~~~~~~~
...

<button
  onClick={() => this.onDismiss(item.objectID)}
  type="button"
>
  Dismiss
</button>

...
~~~~~~~~

`map()` 메소드로 각 항목마다 액션을 실행하도록 아래 코드와 같이 수정합시다.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    return (
      <div className="App">
        {this.state.list.map(item => {
# leanpub-start-insert
          const onHandleDismiss = () =>
            this.onDismiss(item.objectID);
# leanpub-end-insert

          return (
            <div key={item.objectID}>
              <span>
                <a href={item.url}>{item.title}</a>
              </span>
              <span>{item.author}</span>
              <span>{item.num_comments}</span>
              <span>{item.points}</span>
              <span>
                <button
# leanpub-start-insert
                  onClick={onHandleDismiss}
# leanpub-end-insert
                  type="button"
                >
                  Dismiss
                </button>
              </span>
            </div>
          );
        }
        )}
      </div>
    );
  }
}
~~~~~~~~

이제 모든 요소의 핸들러로 함수가 전달되었습니다.

다른 예도 살펴보기 위해 아래 코드로 고쳐 봅시다.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    return (
      <div className="App">
        {this.state.list.map(item =>
            ...
            <span>
              <button
# leanpub-start-insert
                onClick={console.log(item.objectID)}
# leanpub-end-insert
                type="button"
              >
                Dismiss
              </button>
            </span>
          </div>
        )}
      </div>
    );
  }
}
~~~~~~~~

위 코드에서는 브라우저에서 애플리케이션을 열었을 때 바로 실행되지만 버튼을 클릭하면 실행되지 않는 문제가 있습니다. 

다시 아래 코드로 수정해봅시다. 이제 버튼을 클릭 할 때만 실행됩니다. 
{title="src/App.js",lang=javascript}
~~~~~~~~
...

<button
# leanpub-start-insert
  onClick={function () {
    console.log(item.objectID)
  }}
# leanpub-end-insert
  type="button"
>
  Dismiss
</button>

...
~~~~~~~~

클래스 메소드 `onDismiss()`에서 화살표 함수를 작성했듯이 이번에도 화살표 함수로 간결하게 함수를 작성해봅시다.

{title="src/App.js",lang=javascript}
~~~~~~~~
...

<button
# leanpub-start-insert
  onClick={() => console.log(item.objectID)}
# leanpub-end-insert
  type="button"
>
  Dismiss
</button>

...
~~~~~~~~

대부분 리액트 입문자들은 이벤트 핸들러 부분에서 함수 사용 방법에 큰 어려움을 겪기 때문에, 이 부분을 자세히 설명하고자 했습니다. 마지막으로 ES6 화살표 함수로 `item` 객체가 `objectID` 프로퍼티에 액세스 가능하게 만들어봅시다.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {
  ...

  render() {
    return (
      <div className="App">
        {this.state.list.map(item =>
          <div key={item.objectID}>
            ...
            <span>
# leanpub-start-insert
              <button
                onClick={() => this.onDismiss(item.objectID)}
                type="button"
              >
                Dismiss
              </button>
# leanpub-end-insert
            </span>
          </div>
        )}
      </div>
    );
  }
}
~~~~~~~~

이벤트 핸들러 내 화살표 함수 사용은 성능 문제와 연관됩니다. 그 예로, `onDismiss()` 내 `onClick` 핸들러는 식별자를 전달하고자 다른 화살표 함수로 메소드를 래핑했습니다. 따라서 `render()`이 실행될 때마다, 핸들러는 고차 함수인 화살표 함수를 인스턴스화 됩니다. 애플리케이션 성능에 영향을 미칠 수 있지만, 실제로 그 효과를 파악할 수 없습니다. 1000개의 항목마다 이벤트 핸들러에 화살표 함수가 있다고 해봅시다. 성능에 미치지 않도록 생성자에서 메서드를 바인딩하는 할 수 있을 겁니다. 하지만 지금 이제 막 리액트를 시작하는 단계에서 성능 최적화까지 생각하는 것은 시기상조입니다. 우리는 리액트를 배우는 그 자체에 일단 집중하도록 합시다.

### 실습하기

* `onClick` 핸들러에서 여러가지 함수 사용법을 시도합니다.

## 폼과 이벤트와의 상호 작용

이번에는 폼과 이벤트 인터렉션을 추가해보자. 검색 기능을 만들어보자. 검색 필드 내 입력 값은 title 속성에 따라 목록을 필터링하는 데 사용된다.

제일 먼저, JSX에 입력 필드가 있는 폼을 정의한다.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    return (
      <div className="App">
# leanpub-start-insert
        <form>
          <input type="text" />
        </form>
# leanpub-end-insert
        {this.state.list.map(item =>
          ...
        )}
      </div>
    );
  }
}
~~~~~~~~

검색어에 따라 일시적으로 목록을 필터링 해야한다. 이를 위해 입력 필드의 값을 로컬 상태로 저장해야한다. 어떻게 로컬 상태에 접근해야할까?  리액트에서 **synthetic events**를 사용해 이벤트 페이로드에 액세스 할 수 있다.

입력 필드의 `onChange` 핸들러를 정의해보자.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    return (
      <div className="App">
        <form>
# leanpub-start-insert
          <input
            type="text"
            onChange={this.onSearchChange}
          />
# leanpub-end-insert
        </form>
        ...
      </div>
    );
  }
}
~~~~~~~~

함수는 컴포넌트에 바인딩되므로 클래스 메서드에 다시 바인딩된다. 메소드를 바인드하고 정의해야한다.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  constructor(props) {
    super(props);

    this.state = {
      list,
    };

# leanpub-start-insert
    this.onSearchChange = this.onSearchChange.bind(this);
# leanpub-end-insert
    this.onDismiss = this.onDismiss.bind(this);
  }

# leanpub-start-insert
  onSearchChange() {
    ...
  }
# leanpub-end-insert

  ...
}
~~~~~~~~

핸들러를 사용할 때, 콜백 함수에서 합성된 이벤트를 사용할 수 있다.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

# leanpub-start-insert
  onSearchChange(event) {
# leanpub-end-insert
    ...
  }

  ...
}
~~~~~~~~

이벤트는 해당 객체 내 필트 값을 가지고 있다. 따라서 `this.setState()`를 사용해 검색 조건으로 로컬 상태를 업데이트 할 수 있다.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  onSearchChange(event) {
# leanpub-start-insert
    this.setState({ searchTerm: event.target.value });
# leanpub-end-insert
  }

  ...
}
~~~~~~~~



{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  constructor(props) {
    super(props);

    this.state = {
      list,
# leanpub-start-insert
      searchTerm: '',
# leanpub-end-insert
    };

    this.onSearchChange = this.onSearchChange.bind(this);
    this.onDismiss = this.onDismiss.bind(this);
  }

  ...
}
~~~~~~~~

이제 입력 필드 값이 변경 될 때마다 내부 컴포넌트 상태에 입력 값을 저장할 수 있게 됐다.

리액트 컴포넌트의 로컬 상태 업데이트 방법에 대해 간단히 설명하겠다. `this.setState ()`를 사용하여 `searchTerm`을 업데이트하기 위해 전달할 리스트가 있다. 하지만 그렇지 않다. `this.setState()`는 얕은 병합입니다. 하나의 속성을 업데이트 할 때 상태 개체의 형제 속성을 유지합니다. 따라서 목록 상태는 이미 항목을 닫은 경우에도 'searchTerm` 속성을 업데이트 할 때 동일하게 유지됩니다.

앱으로 돌아가보자. 리스트는 아직 필터링되지 않았다. 일반적인 방버븡로 `searchTerm`을 기반으로 목록을 일시적으로 필터링하는데, . 모든 리스트 내 요소를 필터링해야한다. 그렇다면 어떻게 일시적인 필터링을 어떻게 구현해야할까? `render()` 에서 리스트를 맵핑하기 전에 filter 함수를 적용 할 수 있다. 필터는`searchTerm(검색어)` 가 항목의 title 속성과 일치하는지 대조한다. 이미 자바스크립트 내장 함수인 filter를 사용해봤으니, 다시 해보자. 필터 함수가 새로운 배열을 반환하여 함수를 편리한 방식으로 사용할 수 있고, map 함수 앞에 filter 함수를 같이 사용할 수 있다.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    return (
      <div className="App">
        <form>
          <input
            type="text"
            onChange={this.onSearchChange}
          />
        </form>
# leanpub-start-insert
        {this.state.list.filter(...).map(item =>
# leanpub-end-insert
          ...
        )}
      </div>
    );
  }
}
~~~~~~~~

이번에 다른 방법으로 filter 함수를 접근해보자. ES6 클래스 컴포넌트 외부에 filter 함수에 전달되는 인수를 정의해보자. 컴포넌트 상태에 접근할 수 없으므로 필터 조건을 평가하기 위해 `searchTerm` 속성에 접근 할 수도 없다. 그러므로 filter 함수에 `searchTerm`을 전달하고 조건을 평가할 수 있는 새로운 함수를 반환해야한다. 이를 고차함수 (higher order function)이라고 한다. 


고차원 함수에 대해 언급하지 않을 것이지만, 어느 정도 아는 것은 중요하다. 리액트는 고차원 컴포넌트(higher order components)를 다루기 때문에 고차 함수에 대해 알아야 한다. 나중에 이 개념을 알게 될 것이다. 이제 다시 filter 기능을 살펴보자. 먼저, App 컴포넌트 외부에서 고차 함수를 정의한다.

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
function isSearched(searchTerm) {
  return function(item) {
    // some condition which returns true or false
  }
}
# leanpub-end-insert

class App extends Component {

  ...

}
~~~~~~~~

이 함수는 `searchTerm`을 취하여 다른 함수를 반환한다. 모든 filter 함수가 이 함수를 입력으로 사용하기 때문이다 반환된 함수는 filter 함수에 전달되는 함수이기 때문에 항목 개체에 액세스 할 수 있다. 또한 반환된 함수는 함수에 정의된 조건에 따라 목록을 필터링한다. 아래와 같이 조건을 추가해보자.


{title="src/App.js",lang=javascript}
~~~~~~~~
function isSearched(searchTerm) {
  return function(item) {
# leanpub-start-insert
    return item.title.toLowerCase().includes(searchTerm.toLowerCase());
# leanpub-end-insert
  }
}

class App extends Component {

  ...

}
~~~~~~~~

조건은 들어오는`searchTerm` 패턴을 목록의 항목의 title 속성과 일치시키는 것이다. 자바스크립트 내장 함수인 `include`를 통해 할 수 있다. 패턴이 일치 할 때만 true를 반환하고 목록에 항목은 그대로 유지한다. 패턴이 일치하지 않으면 목록에서 항목이 제거됩니다. 패턴 매칭에 유의해야한다. 두 문자열 모두 소문자로 해야한다. 만약 검색어가 'redux'라면 목록 내 항목인 'Redux'와 불일치가 발생한다. 불변 리스트에 filter 함수를 사용해 새 목록을 반환하기 때문에 로컬 상태 내 원래 목록은 변경되지 않는다. 

한가지 언급해야 할 것이 남았다. 자바스크립트 built-in 기능에 대해 일 부 알아보았다. 이 기능은 ES6 기능이다. 자바스크립트 ES5에서는 어떻게 사용할까? `indexOf()` 함수를 사용해 목록의 항목 색인을 얻는다. 아이템이 목록에 있다면, `indexOf()`는 배열의 인덱스를 반환한다.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// ES5
string.indexOf(pattern) !== -1

// ES6
string.includes(pattern)
~~~~~~~~

ES6 화살표 함수로 간결하게 만들 수 있다.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// ES5
function isSearched(searchTerm) {
  return function(item) {
    return item.title.toLowerCase().includes(searchTerm.toLowerCase());
  }
}

// ES6
const isSearched = searchTerm => item =>
  item.title.toLowerCase().includes(searchTerm.toLowerCase());
~~~~~~~~

위의 두 함수 중 어떤 것이 더 읽기 쉽다고 생각하는가. 나는 두 번째 함수를 더 선호한다. 리액트에서는 함수형 프로그래밍을 주로 사용한다. 함수를 반환하는 함수(고차 함수)를 더 자주 사용한다. 자바스크립트  ES6에서는 화살표 함수를 사용해 보다 명확하게 표현할 수 있다.

마지막으로 `isSearched()`함수를 사용해 목록을 필터링한다. 로컬 상태에서 `searchTerm` 속성을 전달하면 필터 입력 함수를 반환하고 필터 조건을 기반으로 목록을 필터링합니다. 이후에는 필터링 된 목록을 매핑하여 각 목록 항목에 대한 요소를 표시한다.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    return (
      <div className="App">
        <form>
          <input
            type="text"
            onChange={this.onSearchChange}
          />
        </form>
# leanpub-start-insert
        {this.state.list.filter(isSearched(this.state.searchTerm)).map(item =>
# leanpub-end-insert
          ...
        )}
      </div>
    );
  }
}
~~~~~~~~

이제 검색 기능이 작동할 것이다. 브라우저에서 직접 사용해보자.

### 실습

* [[리액트 공식문서]리액트 이벤트](https://facebook.github.io/react/docs/handling-events.html)에 대해 읽어본다.
* [고차 함수(higher order functions)](https://en.wikipedia.org/wiki/Higher-order_function)에 대해 읽어본다.

## ES6 구조해체 Destructuring

ES6에서는 디스트럭팅(destructing 구조해체)으로 객체와 배열의 속성에 쉽게 액서스할 수 있다. ES5와 ES6를 함께 비교해보자.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const user = {
  firstname: 'Robin',
  lastname: 'Wieruch',
};

// ES5
var firstname = user.firstname;
var lastname = user.lastname;

console.log(firstname + ' ' + lastname);
// output: Robin Wieruch

// ES6
const { firstname, lastname } = user;

console.log(firstname + ' ' + lastname);
// output: Robin Wieruch
~~~~~~~~

ES5에서 객체 속성에 액서스 할 때마다 행을 추가되지만, ES6에서는 코드 한 줄로 끝난다.
가독성을 높이기 위해 객체를 해체할 때 각 객체 속성마다 구분하여 줄을 바꿔주는 것이 좋다.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const {
  firstname,
  lastname
} = user;
~~~~~~~~

배열에서도 구조해체를 할 수 있다. 여러 행으로 작성하는 것이 코드를 읽기 더 편하게 해준다.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const users = ['Robin', 'Andrew', 'Dan'];
const [
  userOne,
  userTwo,
  userThree
] = users;

console.log(userOne, userTwo, userThree);
// output: Robin Andrew Dan
~~~~~~~~

앞서 만든 App 컴포넌트에서 로컬 상태 객체가 동일한 방식으로 디스트럭팅될 수 있음을 알 수 있을 것이다 filter와 map 코드를 축약할 수 있다.

{title="src/App.js",lang=javascript}
~~~~~~~~
  render() {
# leanpub-start-insert
    const { searchTerm, list } = this.state;
# leanpub-end-insert
    return (
      <div className="App">
        ...
# leanpub-start-insert
        {list.filter(isSearched(searchTerm)).map(item =>
# leanpub-end-insert
          ...
        )}
      </div>
    );
~~~~~~~~

ES5 또는 ES6 방법으로 작성할 수 있다. 

{title="Code Playground",lang="javascript"}
~~~~~~~~
// ES5
var searchTerm = this.state.searchTerm;
var list = this.state.list;

// ES6
const { searchTerm, list } = this.state;
~~~~~~~~

이 책에서는 ES6를 사용하기 때문에 ES6 문법에 친숙해지길 바란다.


### 실습

*  [[영문] MDN : ES6 destructuring](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment) 에 대해 읽어본다.

## 제어된 컴포넌트(Controlled Components)

앞 장에서 리액트의 단방향 데이터 흐름에 대해 이미 배웠다. 입력 필드에도 같은 법칙이 적용된다.이 필드는 목록을 필터링하기 위해`searchTerm`을 사용하여 로컬 상태를 업데이트한다. 상태가 변경되면`render ()`메소드가 다시 실행되고 로컬 상태에서 최근의`searchTerm`을 사용하여 필터 조건을 적용한다.

그러나 잊어버린 것이 있지 않은가? HTML input 태그에는 `value` 속성이 있다. value 속성은 input 필드에 표시된 값을 가진다. 이 경우 `searchTerm` 속성이 된다. 그러나 리액트에서는 필요로 하지 않는 것처럼 보인다.

그렇지 않다. `<input>`,`<textarea>`,`<select>` 와 같은 폼 요소는 일반 HTML로 자신의 상태를 유지한다. 누군가 외부에서 변경되면 값을 내부적으로 수정한다. 리액트에서는 **제어되지 않은 컴포넌트(uncontrolled componen)**라고 부른다. 그 자가 스스로 상태를 처리하기 때문이다.  리액트에서는 이러한 요소를 **제어된 컴포넌트(controlled components)**로 만들어야한다.

무엇을 하면 될까? input 필드의 값 속성만 설정하면 된다. 이미 값이 `searchTerm` 상태 속성에 저장되어 있다. 이를 활용하면 되지 않을까?

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    const { searchTerm, list } = this.state;
    return (
      <div className="App">
        <form>
          <input
            type="text"
# leanpub-start-insert
            value={searchTerm}
# leanpub-end-insert
            onChange={this.onSearchChange}
          />
        </form>
        ...
      </div>
    );
  }
}
~~~~~~~~

여기까지다. 입력 필드의 단방향 데이터 흐름 루프가 포함되었다. 내부 컴포넌트 상태는 입력 필드로 부터 단방향으로 데이터를 받는다. 아직 전체 내부 상태 관리 및 단방향 데이터 흐름이 생소할 수 있다. 점차 익숙해지면, 리액트에서 자연스럽게 사용할 수 있을 것이다. 리액트는 SPA(단일 페이지 애플리케이션: Single Page Applications)에 단방향 데이터 흐름이 포함된 새로운 패턴을 도입했다. 현재 많은 프레임워크와 라이브러리에서 단방향 데이터 흐름을 채택했다.

### 실습

*  [[영문] 리액트 공식문서 - forms](https://facebook.github.io/react/docs/forms.html)에 대해 읽어본다.

## 컴포넌트 분리

You have one large App component now. It keeps growing and can become confusing eventually. You can start to split it up into chunks of smaller components.

Let's start to use a component for the search input and a component for the list of items.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    const { searchTerm, list } = this.state;
    return (
      <div className="App">
# leanpub-start-insert
        <Search />
        <Table />
# leanpub-end-insert
      </div>
    );
  }
}
~~~~~~~~

You can pass those components properties which they can use themselves. In the case of the App component it needs to pass the properties managed in the local state and its class methods.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    const { searchTerm, list } = this.state;
    return (
      <div className="App">
# leanpub-start-insert
        <Search
          value={searchTerm}
          onChange={this.onSearchChange}
        />
        <Table
          list={list}
          pattern={searchTerm}
          onDismiss={this.onDismiss}
        />
# leanpub-end-insert
      </div>
    );
  }
}
~~~~~~~~

Now you can define the components next to your App component. Those components will be ES6 class components as well. They render the same elements like before.

The first one is the Search component.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {
  ...
}

# leanpub-start-insert
class Search extends Component {
  render() {
    const { value, onChange } = this.props;
    return (
      <form>
        <input
          type="text"
          value={value}
          onChange={onChange}
        />
      </form>
    );
  }
}
# leanpub-end-insert
~~~~~~~~

The second one is the Table component.

{title="src/App.js",lang=javascript}
~~~~~~~~
...

# leanpub-start-insert
class Table extends Component {
  render() {
    const { list, pattern, onDismiss } = this.props;
    return (
      <div>
        {list.filter(isSearched(pattern)).map(item =>
          <div key={item.objectID}>
            <span>
              <a href={item.url}>{item.title}</a>
            </span>
            <span>{item.author}</span>
            <span>{item.num_comments}</span>
            <span>{item.points}</span>
            <span>
              <button
                onClick={() => onDismiss(item.objectID)}
                type="button"
              >
                Dismiss
              </button>
            </span>
          </div>
        )}
      </div>
    );
  }
}
# leanpub-end-insert
~~~~~~~~

Now you have three ES6 class components. Perhaps you have noticed the `props` object that is accessible via the class instance by using `this`. The props, short form for properties, have all the values you have passed to the components when you used them in your App component. That way, components can pass properties down the component tree.

By extracting those components from the App component, you would be able to reuse them somewhere else. Since components get there values by using the props object, you can pass every time different props to your components when you use them somewhere else. These components became reusable.

### Exercises:

* figure out further components that you could split up as you have done with the Search and Table components
  * but don't do it now, otherwise you will run into conflicts in the next chapters

## Composable Components

There is one more little property which is accessible in the props object: the `children` prop. You can use it to pass elements to your components from above, which are unknown to the component itself, but make it possible to compose components into each other. Let's see how this looks like when you only pass a text (string) as a child to the Search component.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    const { searchTerm, list } = this.state;
    return (
      <div className="App">
# leanpub-start-insert
        <Search
          value={searchTerm}
          onChange={this.onSearchChange}
        >
          Search
        </Search>
# leanpub-end-insert
        <Table
          list={list}
          pattern={searchTerm}
          onDismiss={this.onDismiss}
        />
      </div>
    );
  }
}
~~~~~~~~

Now the Search component can destructure the children property from the props object. Then it can specify where the children should be displayed.

{title="src/App.js",lang=javascript}
~~~~~~~~
class Search extends Component {
  render() {
# leanpub-start-insert
    const { value, onChange, children } = this.props;
# leanpub-end-insert
    return (
      <form>
# leanpub-start-insert
        {children} <input
# leanpub-end-insert
          type="text"
          value={value}
          onChange={onChange}
        />
      </form>
    );
  }
}
~~~~~~~~

The "Search" text should be visible next to your input field now. When you use the Search component somewhere else, you can choose a different text if you like. After all, it is not only text that you can pass as children. You can pass an element and element trees (which can be encapsulated by components again) as children. The children property makes it possible to weave components into each other.

### Exercises:

* read more about [the composition model of React](https://facebook.github.io/react/docs/composition-vs-inheritance.html)

## Reusable Components

Reusable and composable components empower you to come up with capable component hierarchies. They are the foundation of React's view layer. The last chapters mentioned the term reusability. You can reuse the Table and Search components by now. Even the App component is reusable, because you could instantiate it somewhere else again.

Let's define one more reusable component, a Button component, which gets reused more often eventually.

{title="src/App.js",lang=javascript}
~~~~~~~~
class Button extends Component {
  render() {
    const {
      onClick,
      className,
      children,
    } = this.props;

    return (
      <button
        onClick={onClick}
        className={className}
        type="button"
      >
        {children}
      </button>
    );
  }
}
~~~~~~~~

It might seem redundant to declare such a component. You will use a `Button` component instead of a `button` element. It only spares the `type="button"`. Except for the type attribute you have to define everything else when you want to use the Button component. But you have to think about the long term investment here. Imagine you have several buttons in your application, but want to change an attribute, style or behavior for the button. Without the component you would have to refactor every button. Instead the Button component ensures to have only one single source of truth. One Button to refactor all buttons at once. One Button to rule them all.

Since you already have a button element, you can use the Button component instead. It omits the type attribute, because the Button component specifies it.

{title="src/App.js",lang=javascript}
~~~~~~~~
class Table extends Component {
  render() {
    const { list, pattern, onDismiss } = this.props;
    return (
      <div>
        {list.filter(isSearched(pattern)).map(item =>
          <div key={item.objectID}>
            <span>
              <a href={item.url}>{item.title}</a>
            </span>
            <span>{item.author}</span>
            <span>{item.num_comments}</span>
            <span>{item.points}</span>
            <span>
# leanpub-start-insert
              <Button onClick={() => onDismiss(item.objectID)}>
                Dismiss
              </Button>
# leanpub-end-insert
            </span>
          </div>
        )}
      </div>
    );
  }
}
~~~~~~~~

The Button component expects a `className` property in the props. The `className` attribute is another React derivate for the HTML attribute class. But we didn't pass any `className` when the Button was used. In the code it should be more explicit in the Button component that the `className` is optional.

Therefore, you can use the default parameter which is a JavaScript ES6 feature.

{title="src/App.js",lang=javascript}
~~~~~~~~
class Button extends Component {
  render() {
    const {
      onClick,
# leanpub-start-insert
      className = '',
# leanpub-end-insert
      children,
    } = this.props;

    ...
  }
}
~~~~~~~~

Now, whenever there is no `className` property specified when using the Button component, the value will be an empty string instead of `undefined`.

### 실습

* [ES6 기본 파라미터](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Default_parameters)에 대해 읽어본다.

## 컴포넌트 선언

지금까지 4가지 ES6 클래스 컴포넌트를 만들었다. 여러분도 할 수 있다. ES6 클래스 컴포넌트의 대안으로 비상태 컴포넌트 함수에 대해 알아보자.컴포넌트를 리팩토링하기 전에 리액트의 여러 컴포넌트 유형에 대해 소개하겠다.

* **비상태 함수형 컴포넌트(Functional Stateless Components)** These components are functions which get an input and return an output. The input are the props. The output is a component instance thus plain JSX. So far it is quite similar to an ES6 class component. However, functional stateless components are functions (functional) and they have no local state (stateless). You cannot access or update the state with `this.state` or `this.setState()` because there is no `this` object. Additionally, they have no lifecycle methods. You didn't learn about lifecycle methods yet, but you already used two: `constructor()` and `render()`. Whereas the constructor runs only once in the lifetime of a component, the `render()` class method runs once in the beginning and every time the component updates. Keep in mind that functional stateless component have no lifecycle methods, when you arrive at the lifecycle methods chapter later on.

* **ES6 Class Components:** You already used this type of component declaration in your four components. In the class definition, they extend from the React component. The `extend` hooks all the lifecycle methods, available in the React component API, to the component. That way you were able to use the `render()` class method. Additionally, you can store and manipulate state in ES6 class components by using `this.state` and `this.setState()`.

* **React.createClass:** The component declaration was used in older versions of React and still in JavaScript ES5 React applications. But [Facebook declared it as deprecated](https://facebook.github.io/react/blog/2015/03/10/react-v0.13.html) in favor of JavaScript ES6. They even added a [deprecation warning in version 15.5](https://facebook.github.io/react/blog/2017/04/07/react-v15.5.0.html). You will not use it in the book.

So basically there are only two component declarations left. But when to use functional stateless components over ES6 class components? A rule of thumb is to use functional stateless components when you don't need local state or component lifecycle methods. Usually you start to implement your components as functional stateless components. Once you need access to the state or lifecycle methods, you have to refactor it to an ES6 class component. In our application, we started the other way around for the sake of learning React.

Let's get back to your application. The App component uses internal state. That's why it has to stay as an ES6 class component. But the other three of your ES6 class components are stateless. They don't need access to `this.state` or `this.setState()`. Even more, they have no lifecycle methods. Let's refactor together the Search component to a stateless functional component. The Table and Button component refactoring will remain as your exercise.

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
function Search(props) {
  const { value, onChange, children } = props;
  return (
    <form>
      {children} <input
        type="text"
        value={value}
        onChange={onChange}
      />
    </form>
  );
}
# leanpub-end-insert
~~~~~~~~

That's basically it. The props are accessible in the function signature and the return value is JSX. But you can do more code wise in a functional stateless component. You already know the ES6 destructuring. The best practice is to use it in the function signature to destructure the props.

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
function Search({ value, onChange, children }) {
# leanpub-end-insert
  return (
    <form>
      {children} <input
        type="text"
        value={value}
        onChange={onChange}
      />
    </form>
  );
}
~~~~~~~~

But it can get better. You know already that ES6 arrow functions allow you to keep your functions concise. You can remove the block body of the function. In a concise body an implicit return is attached thus you can remove the return statement. Since your functional stateless component is a function, you can keep it concise as well.

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
const Search = ({ value, onChange, children }) =>
  <form>
    {children} <input
      type="text"
      value={value}
      onChange={onChange}
    />
  </form>
# leanpub-end-insert
~~~~~~~~

The last step was especially useful to enforce only to have props as input and JSX as output. Nothing in between. Still, you could *do something* in between by using a block body in your ES6 arrow function.

{title="Code Playground",lang=javascript}
~~~~~~~~
const Search = ({ value, onChange, children }) => {

  // do something

  return (
    <form>
      {children} <input
        type="text"
        value={value}
        onChange={onChange}
      />
    </form>
  );
}
~~~~~~~~

But you don't need it for now. That's why you can keep the previous version without the block body. When using block bodies, people often tend to do too many things in the function. By leaving the block body out, you can focus on the input and output of your function.

Now you have one lightweight functional stateless component. Once you would need access to its internal component state or lifecycle methods, you would refactor it to an ES6 class component. In addition you saw how JavaScript ES6 can be used in React components to make them more concise and elegant.

### Exercises:

* refactor the Table and Button component to stateless functional components
* read more about [ES6 class components and functional stateless components](https://facebook.github.io/react/docs/components-and-props.html)

## 컴포넌트 스타일링

애플리케이션과 컴포넌트에 기본적인 스타일을 추가해보자. *src/App.css* 과 *src/ index.css* 파일을 다시 사용할 수 있다. 이 파일은 *create-react-app*를 사용하여 부트스트랩했으므로 이미 프로젝트 폴더 내에 있어야한다. CSS 파일은 *src/App.js* 및 src/ index.js*파일로 가져와야 한다. 아래 이미 복사붙여넣을 수 있는 CSS를 준비했지만 자기가 원하는데로 자유롭게 사용할 수 있다.


먼저, 전반적인 애플리케이션의 스타일링을 고쳐보자.

{title="src/index.css",lang="css"}
~~~~~~~~
body {
  color: #222;
  background: #f4f4f4;
  font: 400 14px CoreSans, Arial,sans-serif;
}

a {
  color: #222;
}

a:hover {
  text-decoration: underline;
}

ul, li {
  list-style: none;
  padding: 0;
  margin: 0;
}

input {
  padding: 10px;
  border-radius: 5px;
  outline: none;
  margin-right: 10px;
  border: 1px solid #dddddd;
}

button {
  padding: 10px;
  border-radius: 5px;
  border: 1px solid #dddddd;
  background: transparent;
  color: #808080;
  cursor: pointer;
}

button:hover {
  color: #222;
}

*:focus {
  outline: none;
}
~~~~~~~~

둘째, App 파일에서 컴포넌트 스타일을 지정하자.

{title="src/App.css",lang="css"}
~~~~~~~~
.page {
  margin: 20px;
}

.interactions {
  text-align: center;
}

.table {
  margin: 20px 0;
}

.table-header {
  display: flex;
  line-height: 24px;
  font-size: 16px;
  padding: 0 10px;
  justify-content: space-between;
}

.table-empty {
  margin: 200px;
  text-align: center;
  font-size: 16px;
}

.table-row {
  display: flex;
  line-height: 24px;
  white-space: nowrap;
  margin: 10px 0;
  padding: 10px;
  background: #ffffff;
  border: 1px solid #e3e3e3;
}

.table-header > span {
  overflow: hidden;
  text-overflow: ellipsis;
  padding: 0 5px;
}

.table-row > span {
  overflow: hidden;
  text-overflow: ellipsis;
  padding: 0 5px;
}

.button-inline {
  border-width: 0;
  background: transparent;
  color: inherit;
  text-align: inherit;
  -webkit-font-smoothing: inherit;
  padding: 0;
  font-size: inherit;
  cursor: pointer;
}

.button-active {
  border-radius: 0;
  border-bottom: 1px solid #38BB6C;
}
~~~~~~~~

이제 일부 컴포넌트에 스타일을 사용할 수 있다. HTML 속성 `class` 대신에 `className`을 사용해한다.

먼저 App ES6 클래스 컴포넌트에 적용해보자.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    const { searchTerm, list } = this.state;
    return (
# leanpub-start-insert
      <div className="page">
        <div className="interactions">
# leanpub-end-insert
          <Search
            value={searchTerm}
            onChange={this.onSearchChange}
          >
            Search
          </Search>
# leanpub-start-insert
        </div>
# leanpub-end-insert
        <Table
          list={list}
          pattern={searchTerm}
          onDismiss={this.onDismiss}
        />
# leanpub-start-insert
      </div>
# leanpub-end-insert
    );
  }
}
~~~~~~~~

두 번째, 상태가 없는 함수(stateless function)인 Tab 컴포넌트에 적용해보자.

{title="src/App.js",lang=javascript}
~~~~~~~~
const Table = ({ list, pattern, onDismiss }) =>
# leanpub-start-insert
  <div className="table">
# leanpub-end-insert
    {list.filter(isSearched(pattern)).map(item =>
# leanpub-start-insert
      <div key={item.objectID} className="table-row">
# leanpub-end-insert
        <span>
          <a href={item.url}>{item.title}</a>
        </span>
        <span>{item.author}</span>
        <span>{item.num_comments}</span>
        <span>{item.points}</span>
        <span>
          <Button
            onClick={() => onDismiss(item.objectID)}
# leanpub-start-insert
            className="button-inline"
# leanpub-end-insert
          >
            Dismiss
          </Button>
        </span>
# leanpub-start-insert
      </div>
# leanpub-end-insert
    )}
# leanpub-start-insert
  </div>
# leanpub-end-insert
~~~~~~~~

Now you have styled your application and components with basic CSS. It should look quite decent. As you know, JSX mixes up HTML and JavaScript. Now one could argue to add CSS in the mix as well. That's called inline style. You can define JavaScript objects and pass them to the style attribute of an element.

Let's keep the Table column width flexible by using inline style.

{title="src/App.js",lang=javascript}
~~~~~~~~
const Table = ({ list, pattern, onDismiss }) =>
  <div className="table">
    {list.filter(isSearched(pattern)).map(item =>
      <div key={item.objectID} className="table-row">
# leanpub-start-insert
        <span style={{ width: '40%' }}>
          <a href={item.url}>{item.title}</a>
        </span>
        <span style={{ width: '30%' }}>
          {item.author}
        </span>
        <span style={{ width: '10%' }}>
          {item.num_comments}
        </span>
        <span style={{ width: '10%' }}>
          {item.points}
        </span>
        <span style={{ width: '10%' }}>
          <Button
            onClick={() => onDismiss(item.objectID)}
            className="button-inline"
          >
            Dismiss
          </Button>
        </span>
# leanpub-end-insert
      </div>
    )}
  </div>
~~~~~~~~

The style is inlined now. You could define the style objects outside of your elements to make it cleaner.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const largeColumn = {
  width: '40%',
};

const midColumn = {
  width: '30%',
};

const smallColumn = {
  width: '10%',
};
~~~~~~~~

After that you would use them in your columns: `<span style={smallColumn}>`.

In general, you will find different opinions and solutions for style in React. You used pure CSS and inline style now. That's sufficient to get started.

I don't want to be opinionated here, but I want to leave you some more options. You can read about them and apply them on your own. But if you are new to React, I would recommend to stick to pure CSS and inline style for now.

* [styled-components](https://github.com/styled-components/styled-components)
* [CSS Modules](https://github.com/css-modules/css-modules)

{pagebreak}

이제 여러분은 기초적인 리액트 애플리케이션을 만들 수 있게 됐다! 배운 내용을 정리하자.

* React
  * `this.state`와 `setState()`를 사용해 컴포넌트 내부 상태를 관리한다.
  * 함수 또는 클래스 메소드를 사용해 요소 핸들러로 전달한다.
  * use forms and events in React to add interactions
  * unidirectional data flow is an important concept in React
  * embrace controlled components
  * compose components with children and reusable components
  * usage and implementation of ES6 class components and functional stateless components
  * approaches to style your components
* ES6
  * functions that are bound to a class are class methods
  * destructuring of objects and arrays
  * default parameters
* 기본
  * 고차 함수


잠시 휴식시간을 가지자. 학습한 내용을 되새기고 적용해보자. 작성한 소스로 이것저것 테스트해보자. [리액트 공식 문서] (https://facebook.github.io/react/docs/installation.html)에서 자세한 내용을 확인할 수 있다.

[공식 저장소](https://github.com/rwieruch/hackernews-client/tree/4.2)에서 코드를 확인할 수 있다.
