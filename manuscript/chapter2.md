# 리액트 기초 다지기 Basics in React

이번 장에서는 리액트 기초적인 내용을 알아볼 것입니다. 정적인 컴포넌트보다 동적인 컴포넌트와 인터렉션을 구현해보면서 컴포넌트를 선언과 재사용이 가능한 컴포넌트에 대해 배우고, 마지막으로 컴포넌트에 숨결을 불어넣어 생명체로 만들어 봅시다.

## 컴포넌트 내부 상태 Internal component state

컴포넌트 내부 상태(Internal component state)는 컴포넌트에 저장된 상태(state)의 프로퍼티를 수정, 삭제, 저장할 수 있습니다. 다른 말로는 로컬 상태(local state)라고도 합니다. ES6 클래스 생성자(class constructor)를 사용해 컴포넌트를 만들고 내부 컴포넌트 상태를 초기화할 수 있습니다. 생성자는 컴포넌트가 초기화될 때 한 번만 호출됩니다.

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

컴포넌트 내부에서도 배열의 내 항목을 추가, 변경 제거를 할 수 있습니다. 컴포넌트 상태를 변경할 때마다 `render()` 메서드가 재실행됩니다. 이를 통해 쉽게 컴포넌트 상태를 변경하고 전달된 데이터가 올바르게 표시되는지 확인할 수 있습니다.

그러나 절대로 상태를 직접 변경해서는 안됩니다. 상태를 변경하려면 `setState()` 메서드를 사용합니다. 다음 장에서 `setState()` 메서드에 대해 알아보겠습니다. 

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

ES6에서는 객체 메서드도 축약합니다.

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

각 배열의 항목마다 `닫기` 버튼을 추가하고, 버튼을 클릭하면 각 대상이 삭제되게 구현해봅시다.

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
                닫기
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

`onDismiss()` 메서드는 아직 구현하지 않았습니다. 우선 버튼의 `onClick` 핸들러를 살펴봅시다. `onDismiss()` 메서드는 화살표 함수입니다. 각 `item`의  `objectID` 프로퍼티로 삭제될 항목을 식별한다. 다른 방법은 `onClick` 핸들러 밖에서 함수를 정의한 후  이 함수를 핸들러에 전달하는 것입니다. 다음 장에서 핸들러에 대해 배울 것입니다.

button 요소는 한 줄이 아닌 여러 줄로 작성되었습니다.여러 속성을 한 줄로 나열해 길게 코드를 작성하면 이해하기 어려워집니다. 들여쓰기와 적절한 줄바꿈으로 누가봐도 코드가 읽기 쉽게 작성해야 합니다. 필수사항은 아니지만 권장하는 코드 작성법입니다.

`onDismiss()` 함수를 구현하겠습니다. 항목을 식별하기 위해 ID가 필요합니다. 이 함수는 클래스에 바인딩되어있으므로 클래스 메서드입니다. 따라서 `onDismiss()`가 아니라 `this.onDismiss()`로 접근합니다. `this` 객체는 클래스 인스턴스이다. `onDismiss()`를 클래스 메서드로 정의하기 위해서는 생성자에서 바인딩을 해야합니다. 바인딩은 다음 장에서 알아보겠습니다.

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


이제 클래스 함수와 함수가 할 일을 정의합시다. 클래스 메서드는 아래와 같이 작성합니다.

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

클래스 메서드 내부에 일어나는 일을 차근차근 설명해봅시다. list에서 ID와 매칭되는 항목을 제거하고 업데이트된 list를 state에 저장합니다. 그 다음 `render()` 메서드가 재 실행되고 업데이트된 list가 브라우저에 표시됩니다. 제거된 항목은 더 이상 보이지 않아야 합니다.

자바스크립트 `filter()` 메서드를 사용해 배열의 요소를 삭제할 수 있습니다. filter 함수는 입력 기능을 사용합니다. 이 함수는 배열을 반복해 각 요소마다 액세스합니다. 이렇게하면 필터 조건에 따라 배열의 각 요소를 대조합니다. 일치하면 배열이 그대로 유지됩니다. 반대로 일치하지 않으면 배열에서 필터링 됩니다. 그러나 반환되는 배열은 원래 배열 값을 변경하지 않는 불변 데이터 구조입니다. 리액트의 관습을 따르고 있습니다.
 
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

`filter()` 메서드를 보다 간결한 구문으로 작성할 수 있습니다.

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

버튼을 클릭하면 각 항목이 없어지는 것을 볼 수 있습니다. 그러나 state는 업데이트 되지 않았습니다. 마지막에 `setState()` 클래스 메서드로 `this.state.list` 를 업데이트 합시다.
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

애플리케이션을 재실행해 "닫기" 버튼을 클릭해 작동하는지 확인합니다. 지금까지 실습한 내용이 바로 리액트의  __단방향 데이터 흐름(unidirectional data flow)__ 입니다. `onClick()`을 사용해 뷰에서 액션을 트리거하면, 함수 또는 클래스 메서드가 내부 컴포넌트 상태를 수정하고 컴포넌트의 `render()`메서드가 재실행되어 뷰를 업데이트합니다.

![단방향 데이터 흐름으로 내부 상태를 업데이트 합니다.](images/set-state-to-render-unidirectional.png)

### 읽어보기

* [[리액트 공식문서] 리액트 state와 생명주기](https://facebook.github.io/react/docs/state-and-lifecycle.html)

## 바인딩 Bindings

리액트에서 ES6 클래스 컴포넌트를 사용할 때 자바스크립트 바인딩(binding) 개념을 잘 알고 있어야 합니다. 이전 장에서 생성자 안에 `onDismiss()` 클래스 메서드를 바인딩했습니다.

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

왜 처음부터 바인딩을 했을까요? 클래스 메서드는 클래스 인스턴스에 자동으로 `this`를 바인딩하지 않기 때문에 일일이 바인딩해야 합니다. 아래 ES6 클래스 컴포넌트를 예시로 들겠습니다.

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

위 컴포넌트 렌더링은 잘 되지만, 버튼을 클릭하면 개발자 콘솔 로그에 `undefined`이 출력됩니다. 리액트를 사용할 때 많이 실수하는 버그 중 하나입니다. 클래스 메서드에서`this.state`를 접근하고자 할 때 `this`가 `undefined`이면 접근할 수 없습니다. 따라서 클래스 메서드에서 `this`를 접근할 수 있게 하려면 클래스 메서드를 `this`에 바인딩 해야합니다.

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

클래스 메서드 바인딩은 어느 곳에서든지 사용할 수 있습니다. 클래스 메서드인 `render()`에서도 사용할 수 있습니다.

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


그러나 `render()` 메서드가 실행될 때마다 클래스 메서드를 바인드하기 때문에, 컴포넌트가 업데이트 될 때마다 실행되어 성능에 영향을 미칩니다. `render()` 메서드에서 바인딩하지 않는 편이 좋습니다. 생성자에서 클래스 메서드를 바인딩해 컴포넌트가 인스턴스화 될 때 처음에 한 번만 바인딩하는 것이 더 좋은 방법입니다.

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

이 방법 또한 앞으로 생성자에게 혼란을 줄 수 있기 때문에 사용하지 않는 것이 좋습니다. 생성자는 클래스의 모든 프로퍼티를 인스턴스화하기 위해서 존재합니다. 따라서 클래스 메서드의 로직은 생성자 외부에서 정의합니다.

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

이외에 ES6 화살표 함수를 사용하면 클래스 메서드를 생성자 내부에 바인딩하지 않고도 자동 바인딩할 수 있습니다.

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

클래스 메서드 마다 생성자에 바인딩 처리를 하기 번거롭기 때문에 위 방법을 추천합니다. 그러나 리액트 공식 문서는 생성자 내부에 클래스 메서드를 바인딩할 것을 제안하고 있습니다. 따라서 우리도 공식 문서의 지침에 따라 생성자 내부에 클래스 메서드를 바인딩하겠습니다.

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

위 코드가 복잡하게 보일지도 모르겠습니다. 클래스 메서드 인자를 전달해기 때문에 화살표 함수로 래핑했습니다. 일반적으로 `render()` 밖에서 이벤트 핸들러에 전달되는 함수를 만들어야 합니다. 애플리케이션이 구동되면 클래스 메서드가 즉시 실행되기 때문에 코드는 작동하지 않을 겁니다.

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

`onClick={doSomething()}`일 경우, `doSomething()` 함수는 그 즉시 실행됩니다. 핸들러 표현식이 평가되는데, 그 결과를 함수로 반환하지 않기 때문에 버튼을 클릭해도 아무 일도 일어나지 않습니다. 따라서 `onClick={doSomething}`으로 수정하면 실행됩니다. 이를 `onDismiss()`클래스 메서드에 적용해봅시다.

`onClick={this.onDismiss}`로 고쳤겠지만 이 역시 문제가 있습니다. 삭제할 항목을 식별하는 `item.objectID`가 빠져있습니다. 함수 인자를 추가합시다.
 
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

`map()` 메서드로 각 항목마다 액션을 실행하도록 아래 코드와 같이 수정합시다.

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

모든 요소의 핸들러로 함수가 전달되었습니다.

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

다시 아래 코드로 수정해봅시다. 버튼을 클릭 할 때만 함수가 실행됩니다. 
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

클래스 메서드 `onDismiss()`에서 화살표 함수를 작성했듯이 이번에도 화살표 함수로 간결하게 함수를 작성해봅시다.

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

이벤트 핸들러 내 화살표 함수 사용은 성능 문제와 연관됩니다. 그 예로, `onDismiss()` 내 `onClick` 핸들러는 식별자를 전달하고자 다른 화살표 함수로 메서드를 래핑했습니다. 따라서 `render()`이 실행될 때마다, 핸들러는 고차 함수인 화살표 함수를 인스턴스화 됩니다. 애플리케이션 성능에 영향을 미칠 수 있지만, 실제로 그 효과를 파악할 수 없습니다. 1000개의 항목마다 이벤트 핸들러에 화살표 함수가 있다고 해봅시다. 성능에 미치지 않도록 생성자에서 메서드를 바인딩하는 할 수 있을 겁니다. 하지만 리액트를 시작하는 단계에서 성능 최적화까지 생각하는 것은 시기상조입니다. 우리는 리액트를 배우는 그 자체에 일단 집중하도록 합시다.

### 실습하기

* `onClick` 핸들러에서 여러가지 함수 사용법을 사용해봅니다.

## 폼과 이벤트 상호 작용 Interactions with Forms and Events

이번 장에서는 검색 기능을 구현해보면서 폼과 이벤트 인터렉션을 배워봅시다. 검색 필드의 입력된 검색어에 따라 목록을 필터링 합니다.

첫 번째 단계로, JSX에 입력 필드가 있는 폼을 정의합시다.

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

검색어를 입력할 때마다 즉시 리스트를 필터링해야 합니다. 입력 필드 값이 state에 저장되려면 어떻게 해야할까요? 리액트의  **통합적 이벤트(synthetic event)** 를 사용해 이벤트 페이로드에 접근할 수 있습니다.

다음으로 입력 필드의 `onChange` 핸들러를 정의합시다.

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

이 함수는 컴포넌트 그리고 다시 클래스 메서드에 바운드됨으로, 바인딩이 필요합니다.

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

요소에서 핸들러를 사용할 때 콜백 함수로 통합적 이벤트에 접근할 수 있습니다.

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

타켓 객체의 입력 필드 값을 `this.setState()`를 사용해 state를 업데이트합니다.

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

생성자에서 `searchTerm` 프로퍼티 초기 값을 설정하는 것을 잊지 말아야 합니다. 처음 입력 필드는 비어 있어야하므로 값은 빈 문자열이 됩니다.

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

이제 입력 필드 값이 변경될 때마다 state가 업데이트됩니다.

state 업데이트 과정을 간략히 정리해봅시다. `this.setState()`는 특정 프로퍼티가 업데이트 되어도 다른 state 프로퍼티는 그대로 유지됩니다. `searchTerm` 프로퍼티가 업데이트될 때 `list`는 변하지 않습니다.

App 컴포넌트를 다시 봅시다. `list`를 필터링하기 위해 `searchTerm`을 가지고 목록을 필터링합니다. 이전에 `render()` 메서드에서 `map()`메서드를 사용해 배열의 모든 요소를 접근하고 각 프로퍼티를 표시했었습니다. 이번에도 유사하게 과정에  `filter()` 메서드를 추가할 수 있습니다. `searchTerm`은 각 요소의 `title` 프로퍼티 값과 일치하는지 확인해 리스트를 필터링합니다. `map()` 메서드 앞에 `filter()` 메서드를 같이 사용하는 것이 일반적입니다.

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

좀 더 다른 방법으로 `filter()` 메서드를 다뤄봅시다. 클래스 컴포넌트 외부에서 `filter()` 메서드에 전달되는 인자를 정의합시다. 컴포넌트 상태에 직접 접근할 수 없어 `searchTerm` 프로퍼티를 접근할 수도 없습니다. 따라서 새로운 함수가 필요합니다`filter()`메서드에 `searchTerm`을 전달하고 조건을 확인하는 함수를 반환하는 함수를 만들어보겠습니다. 다른 말로 이 함수는 고차 함수(higher order function)라고 불립니다.


이 책에서는 고차 함수에 대해 다루지 않지만, 기본적인 내용을 아는 것이 좋습니다. 리액트는 고차원 컴포넌트(higher order components)를 다루기 때문에 고차 함수에 대해 알고 있어야 합니다. 아마 나중에 이 개념을 알게 될 것입니다. 이제 다시 `filter()` 메서드 부분을 봅시다. 제일 먼저 App 컴포넌트 외부에 고차 함수를 정의합니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
function isSearched(searchTerm) {
  return function(item) {
    // 조건에 따라 true 또는 false을 반환
  }
}
# leanpub-end-insert

class App extends Component {

  ...

}
~~~~~~~~

이 함수는 `searchTerm`을 취하여 다른 함수를 반환합니다. 모든 `filter()` 메서드가 이 함수를 입력으로 사용합니다. 반환된 함수는 `filter()` 메서드에 전달되어 각 개체에 접근할 수 있습니다. 반환된 함수는 조건에 따라 배열을 필터링합니다. 아래와 같이 조건을 추가해봅시다.


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

ES6 `include()` 메서드로 `searchTerm`과 `title`의 값이 일치하는지 확인합니다. 이 때 문자열을 모두 소문자로 바꿔야 합니다. 만약 'redux'와 'Redux'와 불일치하기 때문입니다. `filter()` 메서드는 새 배열을 반환하기 때문에 `this.state.list`는 변경되지 않습니다.

ES5의 경우 `indexOf()` 메서드로 배열의 인덱스 값을 조회할 수 있습니다.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// ES5
string.indexOf(pattern) !== -1

// ES6
string.includes(pattern)
~~~~~~~~

ES6 화살표 함수로 코드를 간결하게 만듭시다.

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

ES5와 ES6 중 어떤 것이 더 읽기 쉬운가요? 저는 ES6 화살표 함수를 선호합니다. 리액트는 함수형 프로그래밍과 함수를 반환하는 함수(고차 함수)를 많이 사용하기 때문에 되도록 화살표 함수를 사용하는 것이 좋습니다. 화살표 함수는 코드를 명확하게 표현하며 가독성을 높이기 때문입니다.

마지막으로 `isSearched()` 함수를 `filter()`메서드에 추가해 목록을 필터링하게 만듭시다. `this.state.searchTerm`를 전달하면, 반환된 함수에서 조건에 `title`과 일치하는지 확인하여 목록을 필터링합니다.

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

브라우저를 열어 구현한 검색 기능이 잘 작동되는지 확인합시다.

### 읽어보기

* [[리액트 공식문서] 리액트 이벤트](https://facebook.github.io/react/docs/handling-events.html)
* [[위키피디아] 고차 함수(higher order functions)](https://en.wikipedia.org/wiki/Higher-order_function)

## ES6 구조해체 Destructuring

ES6에서는 구조해체(destructing)로 객체와 배열의 프로퍼티에 쉽게 접근할 수 있습니다. ES5와 ES6 문법을 서로 비교해봅시다.

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

ES5에서는 객체 프로퍼티에 접근할 때마다 행이 추가되지만, ES6에서는 코드 한 줄이면 됩니다.
가독성을 높이기 위해 객체를 해체할 때 각 객체 프로퍼티마다 줄을 바꿔주는 것이 좋습니다.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const {
  firstname,
  lastname
} = user;
~~~~~~~~

배열에서도 구조해체가 가능합니다. 여러 행으로 작성하는 것이 좋습니다.

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

앞서 만든 App 컴포넌트에서도 state 객체가 동일한 방식으로 구조해체될 수 있음을 알 수 있을 겁니다. 코드가 짧고 간결해졌습니다. 

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

ES5 또는 ES6으로 작성 가능합니다. 

{title="Code Playground",lang="javascript"}
~~~~~~~~
// ES5
var searchTerm = this.state.searchTerm;
var list = this.state.list;

// ES6
const { searchTerm, list } = this.state;
~~~~~~~~

이 책에서는 ES6를 사용하기 때문에 ES6 문법에 친숙해지길 바랍니다.


### 읽기자료

*  [[MDN] ES6 객체구조](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)

## 제어되는 컴포넌트 Controlled Components

앞에서 우리는 이미 단방향 데이터 흐름에 대해 배웠습니다. 검색어 입력 필드 역시 단방향 데이터 흐름 규칙이 적용됩니다. 입력필드는 목록을 필터링하기 위해 `searchTerm`로 state를 업데이트합니다. state가 변경되면 `render()` 메서드가 재실행되고 `searchTerm`에 검색어가 있는지 목록에서 확인 후 필터링합니다.

그러나 우리가 놓친 것이 있습니다. HTML input 태그의 `value` 속성은 입력 필드에 표시된 값을 가집니다. 바로 `searchTerm`에 해당합니다. 그렇다면 리액트에서는 `value`를 활용하지 않아도 될까요?

그렇지 않습니다. `<input>`,`<textarea>`,`<select>` 와 같은 폼 요소는 HTML로 자신의 상태를 유지합니다. 외부에서 그 값이 변경되면 내부 값도 수정되기 때문에, 이러한 컴포넌트를 리액트에서는 **제어되지 않은 컴포넌트(uncontrolled component)** 라 부릅니다. 리액트에서는 제어되지 않는 컴포넌트를 **제어되는 컴포넌트(controlled components)** 로 만들어야 합니다. 리액트 컴포넌트는 초기화 시점 뿐만 아니라, 어떤 시점이라도 반드시 뷰의 state를 나타내야 합니다. 

그렇다면 이제 무엇을 하면 될까요? 입력 필드에 `searchTerm`을 주면 됩니다.

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

여기까지입니다. 입력 필드에도 단방향 데이터 흐름 규칙이 적용되었습니다. 내부 컴포넌트 상태는 입력 필드로 부터 단방향으로 데이터를 받습니다. 아직 전체 내부 상태 관리 및 단방향 데이터 흐름이라는 단어가 생소하게 느껴질 수 있습니다. 점차 익숙해지면, 자연스럽게 사용할 수 있을 겁니다. 리액트는 SPA에 단방향 데이터 흐름이 포함된 새로운 패턴을 처음 제시했습니다. 현재 SPA 프레임워크와 라이브러리 역시 리액트의 영향을 받아 단방향 데이터 흐름을 도입했습니다.

### 읽어보기

*  [[리액트 공식문서] 폼(Form)](https://facebook.github.io/react/docs/forms.html)

## 컴포넌트 분리 Split Up Components

어느새 App 컴포넌트가 매우 커졌습니다. 계속해서 기능을 추가하고 개발하다보면 점점 규모가 커질 겁니다. 드디어 이 컴포넌트를 작은 컴포넌트 덩어리로 분리 작업을 시작할 때 입니다.

아래와 같이 검색 입력 컴포넌트 `<Search />` 와 리스트를 나열하는 `<Table />` 두 개로 컴포넌트를 분리해봅시다.

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

다음 분리한 컴포넌트에 사용할 프로퍼티를 전달해야 합니다. App 컴포넌트의 state와 클래스 메서드를 전달합니다.

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

이제 App 컴포넌트 옆에 분리시킨 컴포넌트를 정의합시다. 이 컴포넌트 역시 ES6 컴포넌트로 작성합니다.

제일 먼저 Search 컴포넌트를 작성합니다.

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

다음으로 Table 컴포넌트를 작성합니다.

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

이제 하나였던 컴포넌트가 3개가 되었습니다. 이쯤에서 `props`가 필요하다는 생각이 드는 분도 있을 겁니다. `props`은 프로퍼티(properties)의 약자로 `this`를 사용해 클래스 인스턴스에 접근합니다. App 컴포넌트에서 전달된 모든 값을 가지게 됩니다. 컴포넌트가 또 하위 컴포넌트로 계속해서 프로퍼티를 전달할 수 있습니다.

 App 컴포넌트에서 뽑아낸 컴포넌트를 다른 컴포넌트에서도 재사용할 수 있습니다. 컴포넌트는 `props`의 통해 값을 가져오기 때문에, `props` 값을 바꿔 전달할 수 있습니다.

### 실습하기

* Search와 Table 컴포넌트로 컴포넌트를 분리한 것처럼 추가적으로 더 분리할 수 있는 컴포넌트 요소를 찾아봅니다.
  *  다음 장에서 실습 코드와 충돌이 생길 수 있으니 지금 바로 코딩을 하지 마세요.

## 구성가능한 컴포넌트 Composable Components

`prop` 객체 안을 접근할 수 있는 프로퍼티가 하나 더 있습니다. 바로 `children` prop입니다.

 `children` prop는 자기 자신이 무엇인지 어떤 것이 모르지만, 그 안에 자식 요소가 전달됨을 의미합니다.
Search 컴포넌트에 텍스트를 자식으로 전달해보겠습니다. 

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

 Search 컴포넌트는 children 객체를 구조해제할 수 있습니다. `children` props가 표시될 곳을 정합시다.

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

입력 필드 옆에 검색어가 보여야 합니다. Search 컴포넌트를 다른 곳에 사용할 때마다, 다른 텍스트를 사용할 수 있습니다. 텍스트를 자식으로 전달했습니다. 이외에도 요소와 요소 트리(다시 컴포넌트로 캡슐화할 수 있습니다)를 자식으로 전달할 수 있습니다. children 프로퍼티를 사용하면 여러 컴포넌트를 서로 끼워넣고 맞물려 사용할 수 있게 됩니다.

### 읽어보기

* [[리액트 공식문서] 리액트 컴포지션 모델](https://facebook.github.io/react/docs/composition-vs-inheritance.html)

## 재사용가능한 컴포넌트 Reusable Components

재사용가능한 컴포넌트와 구성가능한 컴포넌트를 사용해 컴포넌트 간 게층을 만들 수 잇습니다 이 것이 바로 리액트 뷰 레이어의 토대입니다. 마지막 장에서 재사용성(reusability)에 대해 설명하겠습니다. 지금 만든 Table과 Search 컴포넌트도 재사용할 수 있습니다. 물론 App 컴포넌트도 재사용할 수 있습니다. 컴포넌트는 다른 곳에서 다시 인스턴스를 만들 수 있기 때문입니다. 

재사용가능한 컴포넌트로 Button 컴포넌트를 만들어보겠습니다. 앞으로 Button은 더 많이 자주 사용되는 컴포넌트가 될 겁니다.

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

이미 HTML에 `button`요소가 있어 굳이 컴포넌트로 만들 이유가 없다고 느낄 수도 있습니다. 우리는 `button` 요소 대신에 `button` 컴포넌트를 사용할 것입니다. 이 컴포넌트는 type 속성인 `button`만 사용합니다. 사용하는 곳마다 다시 사용할 수 있도록 모든 내용을 다시 정의할 겁니다. 여기서 우리는 보다 장기적인 안목을 가지고 컴포넌트를 만들어야 합니다. 애플리케이션에 다양한 버튼이 있고 프로퍼티, 스타일, 동작이 모두 제각각일 경우가 있습니다. 컴포넌트가 없다면 모든 버튼을 일일이 리팩토링해야합니다. 대신 Button 컴포넌트는 단일 소스로, 컴포넌트만 수정하면 다른 버튼을 한꺼번에 리팩토링할 수 있습니다. 

이처럼 기존의 `button`요소를 대체해 `Button` 컴포넌트를 사용할 수 있습니다. 이미 Button 컴포넌트가 button 타입을 가지고 있기 때문에 type 속성은 생략합니다.

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

Button 컴포넌트는 props에 `className` 프로퍼티를 전달받습니다. `className`는 HTML 클래스에서 파생된 것으로 클래스명입니다. 아직 우리는 `className`을 전달하지 않았습니다. 보다 명확히 표현하기 위해 ES6 기능인 기본 매개 변수를 사용할 수 있습니다.

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

이제 Button 컴포넌트를 사용할 때, `className` 프로퍼티 값이 지정되지 않아도 `undefined` 가 표시되지 않고 빈 문자열로 표시될 겁니다.

### 읽어보기

* [[MDN] ES6 기본 매개변수](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Default_parameters)

## 컴포넌트 선언 Component Declarations

지금까지 4가지 ES6 클래스 컴포넌트를 만들어봤습니다. 이제 자신감이 생겼길 바랍니다. 이번 장에서 비상태 함수형 컴포넌트(Functional Stateless Components)를 만들어 보겠습니다. 다시 한번 앞에서 배운 리액트 컴포넌트 유형을 정리해봅시다. 

* **비상태 함수형 컴포넌트** 비상태 함수형 컴포넌트는 props를 입력으로 받고 JSX를 반환하는 함수입니다. 여기까지는 ES6 클래스 컴포넌트와 비슷합니다. 그러나 비상태 함수형 컴포넌트는 state가 없기 때문에 `this.state` 또는 `this.setState()`로 state에 액세서하거나 업데이트 할 수 없습니다. 또한 생명주기 메서드도 없습니다. 아직 생명주기 메서드에 대해 배우지 않았지만, 이미 생명주기 메서드인 `constructor()`과 `render()`을 사용했습니다. 생명주기동안 `constructor()`는 한번만 실행되는 반면, `render()`은 컴포넌트가 업데이트될 때마다 한번 실행됩니다. 비상태 함수형은 생명주기 메서드가 없음을 꼭 기억하길 바랍니다.

* **ES6 클래스 컴포넌트** 이미 앞에서 ES6 클래스 컴포넌트를 사용했습니다. 클래스 정의 시, `extends Component` 부분은 리액트 컴포넌트로부터 확장한다는 뜻합니다. `extend`는 리액트 컴포넌트 API인 생명주기 메서드를 컴포넌트에 연결합니다. 때문에`render()` 클래스 메서드를 사용할 수 있는 겁니다. 또한 `this.state`와 `this.setState()`를 사용해 ES6 클래스 컴포넌트에서 상태를 저장하고 조작할 수 있게 됩니다.

* **React.createClass:**  `React.createClas`은 리액트 구버전의 클래스 선언문으로 ES5 애플리케이션에서 사용합니다. 페이스북은 ES6을 사용함에 따라 더 이상 `React.createClass`를 지원하지 않는다고 [공고](https://facebook.github.io/react/blog/2015/03/10/react-v0.13.html)했으며, [리액트 15.5 버전에서 비추천 경고문구](https://facebook.github.io/react/blog/2017/04/07/react-v15.5.0.html)를 추가했습니다. 이 책 역시 이 선언문을 사용하지 않습니다.

따라서 `React.createClass`를 제외하고 비상태 함수형 컴포넌트 또는 ES6 클래스 컴포넌트 사용해 컴포넌트를 선언할 수 있습니다. 그렇다면 언제 비상태 함수형 컴포넌트를 사용해야할까요? 기본 원칙은 컴포넌트에 상태나 생명주기 메서드가 필요 없을 때 비상태 함수형 컴포넌트를 사용합니다. 일반적으로 컴포넌트를 만들 때, 처음 비상태 함수형 컴포넌트로 만들고 이후 state와 생명주기 메서드가 필요할 때 ES6 클래스로 리팩토링합니다. 

개발 중인 애플리케이션으로 돌아갑시다. App 컴포넌트는 state를 사용하기 때문에 ES6 클래스 컴포넌트가 되어야 합니다. 그러나 나머지 세 컴포넌트는 state가 없으며 `this.state` 또는 `this.setState()`를 사용하지 않습니다. 또한 생명주기 메서드도 없습니다. Search 컴포넌트를 비상태 컴포넌트로 만들어 봅시다. 이후 스스로 Table과 Button 컴포넌트도 비상태 컴포넌트를 리팩토링해봅시다.

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

기본적인 코드입니다. props는 함수 시그니처(function signature: 함수의 원형에 명시되는 매개변수 리스트)에 액서스할 수 있고 JSX를 반환합니다. 앞에서 ES6 구조해체를 배웠습니다. 가장 좋은 방법은 함수 시그니처로 props를 구조해제하는 것입니다.

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

함수형 비상태 컴포넌트를 ES6 화살표 함수로 바꿔 간결하게 작성해봅시다. 블록을 제거하고 return을 제거합니다.  
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

지금 만든 함수는 props를 입력으로 사용하고 JSX를 반환하는 경우에만 사용할 수 있습니다. 이들 사이에 *해야할 일*을 추가할 수 있습니다. 
{title="Code Playground",lang=javascript}
~~~~~~~~
const Search = ({ value, onChange, children }) => {

  // 해야할 일

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

그러나 지금 필요하지 않기 때문에 위 코드로 변경하지 않을 겁니다. 대부분이 하나의 함수가 많은 기능을 하도록 코드를 작성하기 때문에 , 되도록 블록 부분을 걷어내 함수의 입력과 출력을 집중하여 개발하는 것이 좋습니다. 

이번 장에서 가벼운 비상태 함수 컴포넌트를 만들었고, 내부 상태나 생명주기 메서드가 필요한 경우 ES6 클래스 컴포넌트로 리팩토링하면 된다는 것을 배웠습니다. 그리고 ES6로 리액트 컴포넌트를 간결하고 보기 좋게 만들어보았습니다.


### 실습하기

* Table과 Button컴포넌트를 비상태 함수 컴포넌트로 리팩토링합니다.
 
### 읽어보기
* [[리액트 공식문서] ES6 클래스 컴포넌트와 비상태 함수 컴포넌트](https://facebook.github.io/react/docs/components-and-props.html)

## 컴포넌트 스타일링 Styling Components

애플리케이션과 컴포넌트를 에쁘게 꾸며 봅시다. *src/App.css* 과 *src/index.css* 파일을 사용할 겁니다. 이 파일은 *create-react-app*를 사용하여 부트스트랩했기 때문에 이미 프로젝트 폴더에 있을 겁니다. CSS 파일은 *src/App.js* 및 *src/index.js*로 가져와야 합니다. 아래 CSS 코드를 그대로 사용할수 있지만 내가 원하는데로 자유롭게 고칠 수 있습니다.

먼저 전반적인 애플리케이션의 스타일링을 수정합시다.

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

이어서 App 파일에서 컴포넌트 스타일를 지정합니다.

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

이제 컴포넌트에 스타일이 적용되었습니다. HTML 속성인 `class` 대신에 JSX에서는 `className`을 사용합니다.

먼저 ES6 클래스 컴포넌트인 App 컴포넌트에 적용해봅시다.

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

이어서 비상태 함수형 컴포넌트인 Tab 컴포넌트에도 적용해봅시다.

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

CSS로 애플리케이션과 컴포넌트 스타일을 지정했습니다. 이제 좀 괜찮게 보일 겁니다. JSX문법은 HTML과 자바스크립트를 섞어 사용하기 때문에 CSS를 추가할 수도 있습니다. 이를 인라인 스타일(Inline Style)이라고 합니다. 스타일을 자바스크립트 객체로 만든 다음 스타일 속성에 전달합니다. 

인라인 스타일로 테이블의 열 가로 넓이를 조절해봅시다.

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


인라인 스타일을 적용해보았습니다. 다음으로 외부에 스타일 객체를 정의해 좀더 코드를 깔끔하게 만듭시다.

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

그리고 각 열에 적용하면 됩니다. 가로넓이가 10%인 `smallColumn` 객체에 해당되면 `<span style={smallColumn}>`이라고 수정합니다.

리액트의 스타일링 방법은 매우 다양합니다. 지금은 순수한 CSS와 인라인 스타일로 스타일을 정의했습니다. 이 정도로도 충분합니다.

앞으로 여러 리액트 컴포넌트 스타일링 라이브러리을 스스로 찾아보고 적용해보길 바랍니다. 그러나 입문 수준에서는 순수한 CSS와 인라인 스타일만을 사용할 것을 권장합니다.

* [[리액트 컴포넌트 스타일링 라이브러리] styled-components](https://github.com/styled-components/styled-components)
* [[리액트 컴포넌트 스타일링 라이브러리] CSS Modules](https://github.com/css-modules/css-modules)

{pagebreak}

마침내 기본적인 리액트 애플리케이션을 만들 수 있게 되었습니다! 지금까지 배운 내용을 정리해봅시다.

* React
  * `this.state`와 `setState()`를 사용해 컴포넌트 내부 상태 관리
  * 이벤트 핸들러로 함수 또는 클래스 메서드를 전달하는 방법
  * 폼과 이벤트 처리
  * 리액트의 단뱡향 데이터 흐름 원칙
  * 제어되지 않은 컴포넌트를 제어되는 컴포넌트로 만들기
  * 자식 컴포넌트와 재사용 가능한 컴포넌트 구현
  * ES6 클래스 컴포넌트 및 비상태 함수형 컴포넌트의 사용과 구현
  * 컴포넌트 스타일링 방법
* ES6
  * 클래스 메서드에 함수 바인딩 방법
  * 객체와 배열의 구조해체
  * 기본 매개변수
* 기본
  * 고차 함수


잠시 휴식시간을 가집시다. 학습한 내용을 되새기고 적용해보며 이것저것 만들어보며 테스트해보길 바랍니다. [리액트 공식 문서](https://facebook.github.io/react/docs/installation.html)에서 리액트에 관한 자세한 내용을 읽어보길 바랍니다.

실습 코드는 [공식 깃허브 리퍼지토리](https://github.com/rwieruch/hackernews-client/tree/4.2)에서 확인할 수 있습니다.