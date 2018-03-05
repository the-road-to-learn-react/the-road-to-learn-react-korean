# 3. 외부 API 사용하기

 3장에서는 외부 API를 요청하고 응답받은 데이터 결과를 보여주는 방법을 배웁니다. API(Application Programming Interface)를 사용하기 앞서, 컴포넌트 생명주기 메서드에 대해 알아봅니다. 아직 API에 대해 잘 모르고 있다면, 저자가 쓴 ["아무도 나에게 API를 알려주지 않았다"](https://www.robinwieruch.de/what-is-an-api-javascript/) 글을 먼저 읽고 오길 바랍니다. 

[해커 뉴스(Hacker News)](https://news.ycombinator.com/)는 와이 콤비네이터(Y Combinator, 미국 실리콘밸리를 대표하는 글로벌 액셀러레이터)가 운영하는 기술 분야 뉴스 큐레이션 플랫폼입니다. 해커 뉴스는 [데이터 조회 API](https://github.com/HackerNews/API)와 [검색 API](https://hn.algolia.com/api)를 제공합니다. 시작하기 전 해커 뉴스 API 명세서를 읽고 데이터 구조를 파악해두길 바랍니다.

## 3.1 생명주기 메서드
 
2장에서 사용한 ES6 클래스 컴포넌트 메서드 `constructor()`와 `render()`는 생명주기 메서드(Lifecycle Methods)입니다. 생명주기 메서드는 ES6 클래스 컴포넌트에서 사용할 수 있지만, 비 상태 컴포넌트에서는 사용할 수 없습니다.

컴포넌트 인스턴스가 만들어져 DOM에 삽입될 때, `constructor()` 생성자 메서드가 호출됩니다. 이 과정을 컴포넌트가 마운트(mount, 탑재)된다고 하여 '마운트 프로세스'라고 합니다.

`render()` 메서드는 마운트 프로세스 중에 호출되며, 컴포넌트 state와 props가 변경되어 컴포넌트가 업데이트 될 때마다 호출됩니다.

이 외에도 더 많은 생명주기 메서드가 있습니다. 생명주기 프로세스 별로 호출되는 생명주기 메서드에 대해 자세히 알아보겠습니다.

컴포넌트가 인스턴스화될 때, '마운트 프로세스'가 시작되어 아래 순서대로 메서드가 호출됩니다.

* `constructor()`
* `componentWillMount()`
* `render()`
* `componentDidMount()`

state나 props가 변경 시, '업데이트 프로세스'가 시작됩니다. 아래 순서대로 5개의 생명주기 메서드가 호출됩니다.

* `componentWillReceiveProps()`
* `shouldComponentUpdate()`
* `componentWillUpdate()`
* `render()`
* `componentDidUpdate()`

마지막으로 컴포넌트 마운트 해제 되기 전, `componentWillUnmount()` 메서드가 호출됩니다.

* `componentWillUnmount()`

처음부터 모든 생명주기 메서드를 한꺼번에 사용하지 않아도 됩니다. 대규모 애플리케이션에도 `constructor()`과 `render()` 메서드만 사용하는 경우가 많습니다. 앞으로 점점 생명주기 메서드에 익숙해질테니 걱정하지 맙시다. 다음으로 생명주기 메서드를 언제 어떻게 사용해야 하는지 알아봅시다. 

* **`constructor(props)`** 메서드는 컴포넌트 초기화 시 호출됩니다. 초기 컴포넌트 상태 및 클래스 메서드를 정의합니다. 

* **`componentWillMount()`** 메서드는 `render()` 메서드 전에 호출됩니다. 컴포넌트 두 번째 렌더링을 일으키지 않기 때문에 이 메서드에서 컴포넌트 상태를 설정할 수 있지만, `constructor()` 메서드에서 초기 상태값을 설정하는 것이 바람직합니다.

* **`render()`** 메서드는 props 및 state를 읽고 엘레먼트를 반환합니다. 컴포넌트 출력을 반환하기 때문에 반드시 사용해야 합니다.  

* **`componentDidMount()`** 메서드는 컴포넌트가 마운트 될 때 한 번만 호출됩니다. 일반적으로 이 메서드에서 비동기 API를 사용합니다. 응답받은 외부 데이터는 내부 컴포넌트 상태에 저장되어 컴포넌트가 업데이트 되면 `render()` 메서드가 실행됩니다.

* **`componentWillReceiveProps(nextProps)`** 메서드는 업데이트 생명주기에서 호출됩니다. 다음 props은 `nextProps`와 이전 props인 `this.props`의 차이를 비교합니다. `nextProps`를 컴포넌트 state로 사용할 수 있습니다.
 
* **`shouldComponentUpdate(nextProps, nextState)`** 메서드는 props 또는 state가 변경되어 컴포넌트가 업데이트될 때 호출됩니다. 고도화된 리액트 애플리케이션에서 성능 최적화를 고려할 때 주로 사용됩니다. 반환되는 부울 값(boolean)에 따라 컴포넌트와 모든 자식이 업데이트 주기 동안 렌더링 되거나, 반대로 렌더링되지 않게 처리할 수 있습니다. 문제가 되는 특정 컴포넌트의 생명주기 렌더링을 막을 수 있습니다.

* **`componentWillUpdate(nextProps, nextState)`** 메서드는 `render()` 메서드 전에 바로 호출됩니다. `nextProps`는 다음 props를, `nextState`는 다음 state를 조회합니다. `render()` 메서드가 실행되기 전, 상태를 업데이트할 수 있는 마지막 기회입니다. `render()` 메서드가 실행된 이후에는 `setState()` 메서드를 사용할 수 없습니다. `nextProps`를 state 값으로 사용하려면 `componentWillReceiveProps()` 메서드를 사용합니다. 

* **`componentDidUpdate(prevProps, prevState)`** 메서드는 `render()` 메서드 후에 즉시 호출됩니다. 이 메서드에서 DOM 조작을 하거나 추가 비동기 요청을 수행합니다.

* **`componentWillUnmount()`** 메서드는 컴포넌트 해체 전에 호출됩니다. 이 메서드에서 컴포넌트를 초기화합니다,

`constructor()`와 `render()` 메서드는 생명주기 메서드 중 가장 많이 사용되는 메서드입니다. `render()` 메서드는 컴포넌트 인스턴스를 반환하기 때문에 반드시 사용해야 합니다.

그 외 `componentDidCatch(error, info)` 메서드가 있습니다. [리액트 버전 16](https://www.robinwieruch.de/what-is-new-in-react-16/)에서 추가된 메서드로 컴포넌트 에러를 캐치합니다. 예를 들어 API 호출이 실패하여 리스트 상태 값이 `null`이 된 경우, `filter()`과 `map()` 메서드를 사용할 수 없습니다. 따라서 오류가 발생하여 컴포넌트가 깨지면 애플리케이션 전체가 작동하지 않게 됩니다. 이때, `componentDidCatch()`로 오류를 발견하여 오류 메시지를 내부 상태에 저장하고, 이 내용을 화면에 표시할 수 있습니다.

### 읽어보기

* [[리액트 공식 문서] 리액트 생명주기](https://reactjs.org/docs/react-component.html)
* [[리액트 공식 문서] 리액트 생명주기 메서드와 상태 관리](https://reactjs.org/docs/state-and-lifecycle.html)
* [[리액트 공식 문서] 컴포넌트 에러 핸들링](https://reactjs.org/blog/2017/07/26/error-handling-in-react-16.html)

## 3.2 검색 결과 데이터 가져오기

이번 절에서는 해커 뉴스 API로 외부 데이터를 가져오기 위해 `componentDidMount()` 생명주기 메서드 안에 자바스크립트 네이티브 API `fetch()` 메서드를 사용하겠습니다.

먼저 요청 URL을 상수(constants)와 기본 매개 변수(default parameters)로 분절해 구성합니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
import React, { Component } from 'react';
import './App.css';

# leanpub-start-insert
const DEFAULT_QUERY = 'redux';

const PATH_BASE = 'https://hn.algolia.com/api/v1';
const PATH_SEARCH = '/search';
const PARAM_SEARCH = 'query=';
# leanpub-end-insert

...
~~~~~~~~

ES6는 새로운 문자열 표기법인 [템플릿 리터럴(Template_literals)](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Template_literals)을 도입했습니다. 템플릿 리터럴을 사용하면 여러 줄로 이루어진 문자열을 쉽게 표현할 수 있습니다. 템플릿 리터럴로 URL 구성하여 API 엔드 포인트에 연결합니다.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// ES6
const url = `${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${DEFAULT_QUERY}`;

// ES5
var url = PATH_BASE + PATH_SEARCH + '?' + PARAM_SEARCH + DEFAULT_QUERY;

console.log(url);
// 출력: https://hn.algolia.com/api/v1/search?query=redux
~~~~~~~~

이런 방법으로 URL를 구성하면 관리하기 쉬워집니다.

이제 API 호출하여 모든 데이터를 한 번에 가져옵니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
...

class App extends Component {

  constructor(props) {
    super(props);

    this.state = {
# leanpub-start-insert
      result: null,
      searchTerm: DEFAULT_QUERY,
# leanpub-end-insert
    };

# leanpub-start-insert
    this.setSearchTopStories = this.setSearchTopStories.bind(this);
# leanpub-end-insert
    this.onSearchChange = this.onSearchChange.bind(this);
    this.onDismiss = this.onDismiss.bind(this);
  }

# leanpub-start-insert
  setSearchTopStories(result) {
    this.setState({ result });
  }

  componentDidMount() {
    const { searchTerm } = this.state;

    fetch(`${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${searchTerm}`)
      .then(response => response.json())
      .then(result => this.setSearchTopStories(result))
      .catch(error => error);
  }
# leanpub-end-insert

  ...
}
~~~~~~~~

이 코드 속에 정말 많은 일들이 일어났습니다. 각 단계별로 일어난 일을 설명하겠습니다.

첫째, 해커 뉴스 API를 사용하기 때문에 기존 샘플 데이터인 `list` 변수를 삭제했습니다. 컴포넌트 내부 초기 상태 `result`는 `null`로 값이 비어 있으며, 검색어 `searchTerm`는 `DEFAULT_QUERY`로 설정되어 있습니다. `searchTerm`은 첫 번째 API 요청할 때와 Search 컴포넌트에서 입력 필드 기본 값을 표시할 때 사용됩니다.
                                                       
둘째, 컴포넌트가 마운트된 후 데이터를 가져오기 위해 `componentDidMount()` 생명주기 메서드를 사용했습니다. 첫 번째 API 요청 시 검색어 초기 상태 값을 사용합니다. 기본 매개변수인 `DEFAULT_QUERY` 값이 "redux"임으로, "redux" 기사를 맨 처음 가져옵니다.

셋째, fetch API를 사용했습니다. ES6 템플릿 리터럴로 `searchTerm`과 함께 URL를 구성했습니다. 구성된 URL는 fetch API의 인자로 전달됩니다. 응답 데이터는 반드시 JSON 데이터 구조로 변환시켜야 합니다. 마지막으로 이 데이터를 컴포넌트 내부 상태 `result`에 저장합니다. 요청 중 오류가 발생하면 `then()`이 아닌 `catch()`가 실행됩니다. 다음 장에서 오류 처리를 다룹니다.

마지막으로 생성자에 컴포넌트 메서드인 `setSearchTopStories()` 를 바인딩 했습니다.

이제 API로 가져온 외부 데이터 리스트가 표시됩니다. 컴포넌트 내부 상태 `result`는 [메타 정보와 인기 리스트가 같이 담겨 객체가 복잡합니다.](https://hn.algolia.com/api) `render()` 메서드 안에서 `console.log(this.state);`로 상태를 출력하고, 개발자 도구를 열어 디버깅 해보면 데이터를 확인해 볼 수 있습니다.

다음으로 조건문에 따라 `result`의 렌더링 여부를 처리해봅시다. 먼저 컴포넌트 내부 상태 `result`의 값이 없는 경우, `null`을 반환하여 아무것도 렌더링되지 않게 합니다. API 요청이 성공하면 응답 데이터가 컴포넌트 내부 상태에 저장되고 App 컴포넌트는 업데이트되어 다시 렌더링 됩니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
# leanpub-start-insert
    const { searchTerm, result } = this.state;

    if (!result) { return null; }

# leanpub-end-insert
    return (
      <div className="page">
        ...
        <Table
# leanpub-start-insert
          list={result.hits}
# leanpub-end-insert
          pattern={searchTerm}
          onDismiss={this.onDismiss}
        />
      </div>
    );
  }
}
~~~~~~~~

컴포넌트 생명주기 동안 어떤 일이 일어날까요? 컴포넌트는 생성자에서 초기화된 후 렌더링 됩니다. 내부 상태 `result` 값이 `null` 이므로 아무것도 표시되지 않습니다. 이후 `componentDidMount()` 메서드가 실행됩니다. 이 메서드에서 해커 뉴스 API 요청에 따라 비동기로 데이터를 가져옵니다. 응답 데이터가 도착하면 `setSearchTopStories()` 메서드에서 내부 컴포넌트 상태를 업데이트합니다. 그다음부터 업데이트 생명주기가 시작됩니다. `render()` 메서드가 다시 실행되는데, 이번에는 내부 상태 `result`가 있기 때문에 리스트가 표시됩니다. App 컴포넌트와 Table 컴포넌트가 렌더링 됩니다.

대부분의 브라우저와 *create-react-app*는 `fetch()` 메서드를  지원합니다. `fetch()` 대신 노드 패키지인 [superagent(수퍼에이전트)](https://github.com/visionmedia/superagent) 또는 [axios(액시오스)](https://github.com/mzabriskie/axios) 라이브러리를 도입할 수 있습니다.

이 책은 자바스크립트 불리언 연산에서 축약 표기법을 사용합니다. `if (result === null)` 대신 축약 표기법인 `if (!result)`를 사용했습니다. 이후에도 `if (list.length === 0)` 대신 `if (!list.length)`을, `if (someString !== '')` 대신 `if (someString)`을 사용하겠습니다. 아직 축약 표기법에 대해 잘 모르고 있다면, 이 부분을 반드시 학습하고 돌아오길 바랍니다.

다시 애플리케이션으로 돌아갑시다. 기사 리스트가 보이는지 확인해보세요. 하지만 앞으로 고쳐야할 버그가 남아있습니다. 첫째, "Dismiss" 버튼입니다. 버튼을 클릭해도 작동하지 않습니다. 각 버튼에 해당되는 객체를 식별하지 못해 리스트가 변경되지 않습니다. 둘째, 검색 기능입니다. 초기 검색은 서버에서 데이터를 가져오지만, 이후에는 클라이언트에서 필터링된 리스트를 가져옵니다. Search 컴포넌트에서도 API 요청으로 서버에서 데이터를 가져오게 수정해야 합니다. 다음 장에서 차근차근 해결해보겠습니다.

### 읽어보기

* [[MDN] ES6 템플릿 리터럴](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Template_literals)
* [[MDN] the native fetch API](https://developer.mozilla.org/en/docs/Web/API/Fetch_API)
* [[저자 블로그] 리액트에서 데이터 가져오기](https://www.robinwieruch.de/react-fetching-data/)

## 3.3 ES6 전개 연산자

"dismiss" 버튼을 클릭하면 아무것도 작동하지 않습니다. `onDismiss()` 메서드는 해당 객체를 식별하지 못하고 있기 때문입니다. 리스트 내 각 객체를 식별할 수 있게 `onDismiss()` 메서드를 고쳐봅시다.

{title="src/App.js",lang=javascript}
~~~~~~~~
onDismiss(id) {
  const isNotId = item => item.objectID !== id;
# leanpub-start-insert
  const updatedHits = this.state.result.hits.filter(isNotId);
  this.setState({
    ...
  });
# leanpub-end-insert
}
~~~~~~~~

`setState()` 메서드 부분을 살펴봅시다. 내부 상태 `result`는 복잡한 객체입니다. 이 중 인기 기사 리스트는 `result` 객체 내 `hits` 프로퍼티에 해당합니다. 버튼을 클릭하면 `result` 객체에서 해당 아이템이 제거된 리스트가 업데이트되지만, 다른 프로퍼티는 그대로 유지됩니다.

리액트는 불변 데이터 구조 원칙에 따라 객체 또는 상태를 바로 변경할 수 없습니다. `result` 객체 내 `hits` 프로퍼티 값을 직접적으로 변경할 수 있더라도 아래와 같이 사용하면 안됩니다.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// 이렇게 하지 마세요. 
this.state.result.hits = updatedHits;
~~~~~~~~

올바른 접근법은 원 객체와 동일한 객체를 생성하여 그 어떤 객체도 변경하지 않고 그대로 유지하는 것입니다.   

이를 위해 ES6 `Object.assign()` 메서드를 사용할 수 있습니다. `Object.assign()`의 첫 번째 인자는 타깃 객체이고, 나머지 두 인자는 소스 객체입니다. 이 합쳐진 두 객체를 타깃 객체에서 다시 합칩니다. 타깃 객체는 빈 객체가 될 수 있습니다. 소스 객체는 변경되지 않기 때문에 불변성 원칙을 고수합니다.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const updatedHits = { hits: updatedHits };
const updatedResult = Object.assign({}, this.state.result, updatedHits);
~~~~~~~~

동일한 프로퍼티 이름을 공유할 때, 후자 객체는 전자의 병합 객체를 덮어씁니다. `onDismiss()` 메서드를 다시 작성하겠습니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
onDismiss(id) {
  const isNotId = item => item.objectID !== id;
  const updatedHits = this.state.result.hits.filter(isNotId);
  this.setState({
# leanpub-start-insert
    result: Object.assign({}, this.state.result, { hits: updatedHits })
# leanpub-end-insert
  });
}
~~~~~~~~

문제는 해결되었지만, 앞에서 배운 ES6 전개 연산자(spread operator)를 활용해 더 간단한 코드로 만들 수 있습니다. `...` 표시는 배열 또는 객체의 모든 값을 복사함을 뜻합니다.

아래 ES6 **배열** 스프레드 연산자(array spread operator) 예제 코드를 봅시다.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const userList = ['Robin', 'Andrew', 'Dan'];
const additionalUser = 'Jordan';
const allUsers = [ ...userList, additionalUser ];

console.log(allUsers);
// 출력: ['Robin', 'Andrew', 'Dan', 'Jordan']
~~~~~~~~

`allUsers` 변수는 새 배열입니다. `userList` 변수와 `additionalUser` 변수 값은 그대로 유지됩니다. 이런 방법으로 두 배열을 새 배열로 합칠 수 있습니다.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const oldUsers = ['Robin', 'Andrew'];
const newUsers = ['Dan', 'Jordan'];
const allUsers = [ ...oldUsers, ...newUsers ];

console.log(allUsers);
// 출력: ['Robin', 'Andrew', 'Dan', 'Jordan']
~~~~~~~~

이어서 객체 스프레드 연산자(object spread operator)를 알아봅시다. [객체 스프레드 연산자](https://github.com/sebmarkbage/ecmascript-rest-spread)는 ES6 이후 차기 자바스크립트를 위해 제안된 기능이지만, 리액트에 먼저 도입되었습니다. *create-react-app* 역시 객체 스프레드 연산자를 지원합니다.

기본적으로 ES6 배열 전개 연산자(array spread operator)와 같지만, 배열이 아닌 객체라는 점이 다릅니다. 각 키(key)-값(value) 쌍을 새 객체에 복사합니다.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const userNames = { firstname: 'Robin', lastname: 'Wieruch' };
const age = 28;
const user = { ...userNames, age };

console.log(user);
// 출력: { firstname: 'Robin', lastname: 'Wieruch', age: 28 }
~~~~~~~~

배열 전개와 같이 여러 객체를 펼칠 수 있습니다.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const userNames = { firstname: 'Robin', lastname: 'Wieruch' };
const userAge = { age: 28 };
const user = { ...userNames, ...userAge };

console.log(user);
// 출력: { firstname: 'Robin', lastname: 'Wieruch', age: 28 }
~~~~~~~~

따라서 `Object.assign()` 메서드를 대체할 수 있습니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
onDismiss(id) {
  const isNotId = item => item.objectID !== id;
  const updatedHits = this.state.result.hits.filter(isNotId);
  this.setState({
# leanpub-start-insert
    result: { ...this.state.result, hits: updatedHits }
# leanpub-end-insert
  });
}
~~~~~~~~

`onDismiss()` 메서드가 잘 작동하는지 다시 확인합니다. `hits` 리스트 내 각 객체를 식별하여 해당 아이템을 제외된 리스트가 업데이트되는지 확인합니다.

### 읽어보기

* [[MDN] ES6 Object.assign()](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)
* [[MDN]ES6 배열 전개 연산자](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Spread_operator)

## 3.4 조건부 렌더링

조건부 렌더링(conditional rendering)은 하나 또는 여러 컴포넌트와 엘레먼트의 렌더링 여부를 결정합니다. `if-else` 문 역시 가장 기본적인 조건부 렌더링 방법입니다. 3.2 절에서 조건부 렌더링을 다뤘습니다.

컴포넌트 내부 상태 `result` 초기값은 `null` 입니다. App 컴포넌트에 API 응답 데이터가 도착하지 않으면 아무것도 반환되지 않습니다. `render()` 생명주기 메서드 실행 전에 조건에 따라 반환 여부가 결정됩니다. App 컴포넌트는 초기에 아무것도 렌더링 되지 않다가, `result` 값이 업데이트 되면 다시 렌더링 하게 됩니다. 

한 단계 더 나아가 봅시다. 조건부 렌더링에 `result`에 의존하는  Table 컴포넌트를 포함하겠습니다. `result` 값이 없어도 기본적으로 모든 내용이 표시되어야 합니다. 이번에는 `if-else` 문이 아닌 삼항 조건 연산자(ternary operator)로 작성해봅시다.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
# leanpub-start-insert
    const { searchTerm, result } = this.state;
# leanpub-end-insert
    return (
      <div className="page">
        <div className="interactions">
          <Search
            value={searchTerm}
            onChange={this.onSearchChange}
          >
            Search
          </Search>
        </div>
# leanpub-start-insert
        { result
          ? <Table
            list={result.hits}
            pattern={searchTerm}
            onDismiss={this.onDismiss}
          />
          : null
        }
# leanpub-end-insert
      </div>
    );
  }
}
~~~~~~~~

마지막으로 논리 연산자 `&&`로 작성합니다. 자바스크립트에서 `true &&'Hello World'` 경우 'Hello World'로 평가되고, `false && 'Hello World'`경우 거짓으로 평가됩니다.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const result = true && 'Hello World';
console.log(result);
// 출력: Hello World

const result = false && 'Hello World';
console.log(result);
// 출력: false
~~~~~~~~

애플리케이션에도 적용해봅시다. 조건이 참이면, `&&` 논리 연산자 뒷부분 내용이 출력됩니다. 조건이 거짓이면 리액트는 표현식을 무시하고 이를 건너뜁니다. 이번에도 조건이 참이면 Table 컴포넌트를 반환하고 거짓이면 반환하지 않게 만들어봅시다.

{title="src/App.js",lang=javascript}
~~~~~~~~
{ result &&
  <Table
    list={result.hits}
    pattern={searchTerm}
    onDismiss={this.onDismiss}
  />
}
~~~~~~~~

이번 절에서는 리액트 조건부 렌더링 세 가지 방법을 배웠습니다. 저자가 쓴 ["조건부 렌더링"](https://www.robinwieruch.de/conditional-rendering-react/) 글에서 더 많은 예제 코드를 확인할 수 있습니다.

애플리케이션에서 가져온 데이터가 잘 보이는지 확인하세요. 요청 보류 중일 떄를 제외하고 모든 기사 리스트가 보일 것입니다. 요청 후 받은 응답 데이터를 로컬 상태로 저장하면 `render()` 메서드가 다시 실행되고, 조건부 렌더링의 조건에 따라 Table 컴포넌트가 표시됩니다.

### 읽어보기

* [[리액트 공식 문서] 리액트 조건 렌더링](https://reactjs.org/docs/conditional-rendering.html)
* [[저자 블로그] 조건문 렌더링의 다양한 방법](https://www.robinwieruch.de/conditional-rendering-react/)

## 3.5 Search 컴포넌트 클라이언트 · 서버 처리

Search 컴포넌트의 입력 필드는 클라이언트에서 리스트를 필터링하고 있습니다. `componentDidMount()` 메서드에서 기본 검색 용어 매개 변수로 얻은 첫 번째 API 응답 데이터 결괏값만 가지고 있는 상태입니다. 이번 절에서는 Search 컴포넌트가 검색어를 클라이언트가 아닌 해커 뉴스 서버에서 전달하도록 리팩터링하겠습니다.

Search 컴포넌트에서 검색 요청 시 해커뉴스 API 데이터를 가져오기 위해 App 컴포넌트의 `onSearchSubmit()` 메서드를 아래와 같이 정의하겠습니다. 

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  constructor(props) {
    super(props);

    this.state = {
      result: null,
      searchTerm: DEFAULT_QUERY,
    };

    this.setSearchTopStories = this.setSearchTopStories.bind(this);
    this.onSearchChange = this.onSearchChange.bind(this);
# leanpub-start-insert
    this.onSearchSubmit = this.onSearchSubmit.bind(this);
# leanpub-end-insert
    this.onDismiss = this.onDismiss.bind(this);
  }

  ...

# leanpub-start-insert
  onSearchSubmit() {
    const { searchTerm } = this.state;
  }
# leanpub-end-insert

  ...
}

~~~~~~~~

 `onSearchSubmit()` 메서드는 `componentDidMount()` 메서드과 동일하게 검색 API를 요청 합니다. 차이점은 기본값이 아닌 수정된 입력 검색어을 사용한다는 것입니다. 따라서 이 기능을 떼어 재사용 가능한 메서드로 만들 수 있습니다. 이를 위해 새로 `fetchSearchTopStories()` 메서드를 만들겠습니다.
 
{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  constructor(props) {
    super(props);

    this.state = {
      result: null,
      searchTerm: DEFAULT_QUERY,
    };

    this.setSearchTopStories = this.setSearchTopStories.bind(this);
# leanpub-start-insert
    this.fetchSearchTopStories = this.fetchSearchTopStories.bind(this);
# leanpub-end-insert
    this.onSearchChange = this.onSearchChange.bind(this);
    this.onSearchSubmit = this.onSearchSubmit.bind(this);
    this.onDismiss = this.onDismiss.bind(this);
  }

  ...

# leanpub-start-insert
  fetchSearchTopStories(searchTerm) {
    fetch(`${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${searchTerm}`)
      .then(response => response.json())
      .then(result => this.setSearchTopStories(result))
      .catch(error => error);
  }
# leanpub-end-insert

  componentDidMount() {
    const { searchTerm } = this.state;
# leanpub-start-insert
    this.fetchSearchTopStories(searchTerm);
# leanpub-end-insert
  }

  ...

  onSearchSubmit() {
    const { searchTerm } = this.state;
# leanpub-start-insert
    this.fetchSearchTopStories(searchTerm);
# leanpub-end-insert
  }

  ...
}
~~~~~~~~

다음으로 Search 컴포넌트에 "Search" 버튼을 추가합니다. 이 버튼을 클릭해 검색 요청을 보내고 해커 뉴스 API에서 응답 데이터를 가져와봅시다.

물론 입력 필드 값이 변경될 때마다 해커 뉴스 API에서 데이터를 가져오게 할 수도 있습니다. 이 경우 `onChange()` 핸들러를 사용할 수 있겠지만 구현이 꽤 복잡하기 때문에 이를 사용하지 않았습니다. 이제 하나씩 만들어보겠습니다.

첫째, Search 컴포넌트에 `onSearchSubmit()` 메서드를 전달합니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    const { searchTerm, result } = this.state;
    return (
      <div className="page">
        <div className="interactions">
          <Search
            value={searchTerm}
            onChange={this.onSearchChange}
# leanpub-start-insert
            onSubmit={this.onSearchSubmit}
# leanpub-end-insert
          >
            Search
          </Search>
        </div>
        { result &&
          <Table
            list={result.hits}
            pattern={searchTerm}
            onDismiss={this.onDismiss}
          />
        }
      </div>
    );
  }
}
~~~~~~~~

둘째, Search 컴포넌트에 버튼을 추가합니다. 버튼은 `type="submit"`을 가지고, 폼은 `onSubmit()` 메서드를 전달합니다. 버튼 내용은 자식 프로퍼티 `children`를 사용합니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
const Search = ({
  value,
  onChange,
  onSubmit,
  children
}) =>
  <form onSubmit={onSubmit}>
    <input
      type="text"
      value={value}
      onChange={onChange}
    />
    <button type="submit">
      {children}
    </button>
  </form>
# leanpub-end-insert
~~~~~~~~

Table 컴포넌트에서 클라이언트 검색 기능은 더 이상 필요없습니다. 따라서 `isSearched()` 함수를 삭제합니다. 이제 "Search" 버튼을 클릭하면 해커 뉴스 API 검색 결과를 가져올 것입니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    const { searchTerm, result } = this.state;
    return (
      <div className="page">
        ...
        { result &&
          <Table
# leanpub-start-insert
            list={result.hits}
            onDismiss={this.onDismiss}
# leanpub-end-insert
          />
        }
      </div>
    );
  }
}

...

# leanpub-start-insert
const Table = ({ list, onDismiss }) =>
# leanpub-end-insert
  <div className="table">
# leanpub-start-insert
    {list.map(item =>
# leanpub-end-insert
      ...
    )}
  </div>
~~~~~~~~

검색 버튼을 클릭하면 브라우저가 새로고침 되는데 이는 HTML 폼 전송 콜백에 대한 네이티브 브라우저 동작입니다. 리액트에서는 네이티브 브라우저 동작을 방지하기 위해 `preventDefault()` 이벤트 메서드를 자주 사용합니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
onSearchSubmit(event) {
# leanpub-end-insert
  const { searchTerm } = this.state;
  this.fetchSearchTopStories(searchTerm);
# leanpub-start-insert
  event.preventDefault();
# leanpub-end-insert
}
~~~~~~~~

이번 절에서는 검색어를 입력해 해커 뉴스 기사를 가져오는 기능을 구현했습니다.

### 읽어보기

* [[리액트 공식 문서] synthetic events in React](https://reactjs.org/docs/events.html)

### 실습하기

* [해커 뉴스 API](https://hn.algolia.com/api)로 이것저것 실험해봅니다.

## 3.6 페이지 매김 데이터 가져오기

3.5 절에서 반환된 데이터 구조를 자세히 살펴보았나요? [해커 뉴스 API](https://hn.algolia.com/api)를 통해 더 많은 인기 기사 리스트를 가져올 수 있습니다. 결괏값의 `page` 프로퍼티는 페이지 번호를 뜻합니다. 프로퍼티 값은 `0`으로 첫 번째 장입니다. 이번 절에서는 페이지 프로퍼티 값을 통해 많은 리스트를 가져오겠습니다. 검색어를 그대로 유지된 상태로 다음 페이지 번호를 API에 전달하면 됩니다. 

먼저 페이지 매김 데이터를 처리할 수 있게 기존 URL 구성을 수정합시다.

{title="src/App.js",lang=javascript}
~~~~~~~~
const DEFAULT_QUERY = 'redux';

const PATH_BASE = 'https://hn.algolia.com/api/v1';
const PATH_SEARCH = '/search';
const PARAM_SEARCH = 'query=';
# leanpub-start-insert
const PARAM_PAGE = 'page=';
# leanpub-end-insert
~~~~~~~~

그리고 이 상수들을 매개 변수로 추가해 URL를 구성합니다.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const url = `${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${searchTerm}&${PARAM_PAGE}`;

console.log(url);
// 출력: https://hn.algolia.com/api/v1/search?query=redux&page=
~~~~~~~~

`fetchSearchTopStories()` 메서드에서 페이지 번호를 뜻하는 두 번째 인자를 추가하겠습니다. 두 번째 인자가 없으면 기본값인 `0` 을 전달합니다. 따라서 `componentDidMount()` 메서드와 `onSearchSubmit()` 메서드는 최초 요청 시, 첫 번째 페이지를 가져옵니다. 추가 요청 시, 두 번째 인자를 통해 다음 페이지를 가져옵니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

# leanpub-start-insert
  fetchSearchTopStories(searchTerm, page = 0) {
    fetch(`${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${searchTerm}&${PARAM_PAGE}${page}`)
# leanpub-end-insert
      .then(response => response.json())
      .then(result => this.setSearchTopStories(result))
      .catch(error => error);
  }

  ...

}
~~~~~~~~

이제 `fetchSearchTopStories()` 메서드에서 API 응답을 받아 현재 페이지를 사용할 수 있습니다. "More" 버튼의 `onClick` 핸들러에서 이 메서드를 사용해 더 많은 리스트을 가져오게 만들겠습니다. 버튼을 클릭하면 그다음 페이지 데이터를 가져오게 됩니다. `onClick()` 핸들러에서 현재 검색어와 다음 페이지 번호(현재 페이지 + 1)를 정의하겠습니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    const { searchTerm, result } = this.state;
# leanpub-start-insert
    const page = (result && result.page) || 0;
# leanpub-end-insert
    return (
      <div className="page">
        <div className="interactions">
        ...
        { result &&
          <Table
            list={result.hits}
            onDismiss={this.onDismiss}
          />
        }
# leanpub-start-insert
        <div className="interactions">
          <Button onClick={() => this.fetchSearchTopStories(searchTerm, page + 1)}>
            More
          </Button>
        </div>
# leanpub-end-insert
      </div>
    );
  }
}
~~~~~~~~

`render()` 메서드에서 `result`가 없을 때 페이지 번호 기본값은 `0`입니다. 3.1 절에서 배웠듯이 `render()` 메서드는 `componentDidMount()` 생명주기 메서드에서 비동기로 데이터를 가져오기 전에 실행됩니다.

아직 할 일이 더 있습니다. 다음 페이지 데이터를 가져오지만, 이전 데이터를 덮어쓰게 되는 문제가 있습니다. 새 데이터를 정의하기보다 기존 상태 값에 새 결과 객체 리스트를 추가하는 것이 바람직한 방법입니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
setSearchTopStories(result) {
# leanpub-start-insert
  const { hits, page } = result;

  const oldHits = page !== 0
    ? this.state.result.hits
    : [];

  const updatedHits = [
    ...oldHits,
    ...hits
  ];

  this.setState({
    result: { hits: updatedHits, page }
  });
# leanpub-end-insert
}
~~~~~~~~

`setSearchTopStories()` 메서드에서 일어나는 일을 설명하겠습니다. 

첫째, 먼저 `result` 객체에서 프로퍼티 `hits`와 `page` 객체를 얻습니다. 

둘째, 이미 `hits`가 있는지 확인합니다. `page` 번호가 `0`일 때는 `componentDidMount()` 메서드 또는 `onSearchSubmit()` 메서드에서 새로운 검색 요청이 전달됩니다. `hits` 리스트가 비어있는 상태에서, "More" 버튼을 클릭하면 `0`이 아닌 다음 페이지를 요청합니다. 이전 `hits`는 저장되어 사용할 수 있습니다. 

셋째, 이전 `hits`에 새로운 `hits`를 덮어쓰기 하면 안 됩니다. 따라서 두 리스트를 병합 해야 합니다. 이를 위해 ES6 배열 전개 연산자를 사용합니다. 

넷째, `his`와 `page`를 컴포넌트 내부 상태로 업데이트합니다. 

마지막으로 "More" 버튼 클릭할 때 일부 리스트만 가져오게 만들어 보겠습니다. 다시 URL를 수정하겠습니다. 이전과 같은 방법으로 각 경로를 상수로 만들어 URL를 구성합니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
const DEFAULT_QUERY = 'redux';
# leanpub-start-insert
const DEFAULT_HPP = '100';
# leanpub-end-insert

const PATH_BASE = 'https://hn.algolia.com/api/v1';
const PATH_SEARCH = '/search';
const PARAM_SEARCH = 'query=';
const PARAM_PAGE = 'page=';
# leanpub-start-insert
const PARAM_HPP = 'hitsPerPage=';
# leanpub-end-insert
~~~~~~~~

이 상수를 조합해 URL를 만듭니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
fetchSearchTopStories(searchTerm, page = 0) {
# leanpub-start-insert
  fetch(`${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${searchTerm}&${PARAM_PAGE}${page}&${PARAM_HPP}${DEFAULT_HPP}`)
# leanpub-end-insert
    .then(response => response.json())
    .then(result => this.setSearchTopStories(result))
    .catch(error => error);
}
~~~~~~~~

이전보다 더 많은 리스트를 가져오게 되었습니다. 이번 절에서는 해커 뉴스 API를 통해 받은 실제 데이터를 사용해봤습니다. 새 프로그래밍 언어나 라이브러리를 학습할 때, 외부 API를 사용하면 더 재미있게 배울 수 있을 겁니다. 저도 이렇게 배웠으니까요.

### 실습하기

* [해커 뉴스 API](https://hn.algolia.com/api)의 매개변수를 바꿔 API를 요청해봅니다.

## 3.7 클라이언트 캐시

"Search" 버튼을 클릭하면 해커 뉴스 API에 요청을 보냅니다. 예를 들어 "redux"를 검색하고, 그다음 "react"를, 그리고 다시 "redux"를 입력해 검색 요청을 보냈다고 가정해봅시다. 총 세 번 검색 요청을 보냈습니다. 이 중 "redux"를 두 번 요청했는데, 두 번 모두 데이터를 가져오기 위해 비동기 왕복 여행(asynchronous roundtrip)을 거치게 됩니다. 이 경우 클라이언트 캐시(client-sided cache)를 도입하는 것이 좋습니다. 캐시(Cache)란 미리 만들어진 데이터를 임시로 저장하는 공간입니다. 우리가 구현할 내용은 다음과 같습니다. 새 API 요청을 받으면, 이전에 동일한 요청이 있는지 먼저 확인합니다. 이미 저장된 캐시가 있으면 이를 사용하고, 캐시가 없다면 새 데이터를 가져오기 위해 API를 요청합니다. 이제 시작해봅시다.

각 결과마다 클라이언트 캐시를 보유하려면, 컴포넌트 내부 상태에 `result` 하나 보다, 여러 `results`를 저장하는 것이 좋습니다. 이를 위해 `results` 객체는 키(key)가 검색어이고, 값(value)이 `hits`가 되게 만들겠습니다. API 결괏값은 검색어(key)에 매핑되어 저장되게 해봅시다.

현재 로컬 상태는 아래 코드와 비슷할 것입니다.

{title="Code Playground",lang="javascript"}
~~~~~~~~
result: {
  hits: [ ... ],
  page: 2,
}
~~~~~~~~

예를 들어, "redux" 그리고 "react"로 두 번 검색 요청을 보낸다면 `results` 객체는 아래와 같을 것입니다.

{title="Code Playground",lang="javascript"}
~~~~~~~~
results: {123
  redux: {
    hits: [ ... ],
    page: 2,
  },
  
  react: {
    hits: [ ... ],
    page: 1,
  },
  ...
}
~~~~~~~~

이제 `setState()` 메서드로 클라이언트 캐시를 구현해봅시다. 

첫째, 컴포넌트 내부 상태의 `result` 객체를 `results`로 변경합니다. 둘째, `searchKey`를 정의합니다. `searchKey`는 임시  키(key)로서 `result`를 저장하는데 쓰입니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  constructor(props) {
    super(props);

    this.state = {
# leanpub-start-insert
      results: null,
      searchKey: '',
# leanpub-end-insert
      searchTerm: DEFAULT_QUERY,
    };

    ...

  }

  ...

}
~~~~~~~~

API 요청을 보내기 전, `searchKey`를 설정해야 합니다. `searchKey`은 검색어인 `searchTerm`를 반영합니다. `searchTerm`가 아닌 `searchKey`를 사용하는 이유는 무엇일까요? `searchTerm`은 Search 컴포넌트에서 입력 필드에 검색어를 입력할 때마다 그 값이 변경되는 변경 변수(fluctuant variable)입니다. 값이 변경되지 않는 고정 변수가 필요합니다. 따라서 `searchKey`로 `results` 객체 내 해당하는 결과를 찾습니다. 이 변수는 캐시 내 현재 결과를 가리키는 포인터 역할을 합니다. 따라서 최종적으로 `render()` 메서드에 현재 결과가 표시됩니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
componentDidMount() {
  const { searchTerm } = this.state;
# leanpub-start-insert
  this.setState({ searchKey: searchTerm });
# leanpub-end-insert
  this.fetchSearchTopStories(searchTerm);
}

onSearchSubmit(event) {
  const { searchTerm } = this.state;
# leanpub-start-insert
  this.setState({ searchKey: searchTerm });
# leanpub-end-insert
  this.fetchSearchTopStories(searchTerm);
  event.preventDefault();
}
~~~~~~~~

다음으로 `searchKey`를 사용해 컴포넌트 내부 상태에 각 `result`를 저장하도록 수정하겠습니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  setSearchTopStories(result) {
    const { hits, page } = result;
# leanpub-start-insert
    const { searchKey, results } = this.state;

    const oldHits = results && results[searchKey]
      ? results[searchKey].hits
      : [];
# leanpub-end-insert

    const updatedHits = [
      ...oldHits,
      ...hits
    ];

    this.setState({
# leanpub-start-insert
      results: {
        ...results,
        [searchKey]: { hits: updatedHits, page }
      }
# leanpub-end-insert
    });
  }

  ...

}
~~~~~~~~

`searchKey`는 업데이트된 `hits`와 `page`를 `results`에 저장하기 위한 키(key)로 사용됩니다. 각 단계별로 자세히 설명하겠습니다.

첫째, 컴포넌트 내부 상태에서 `searchKey`를 받습니다. `searchKey`는 `componentDidMount()`와 `onSearchSubmit()` 메서드에서 설정했습니다.

둘째, `oldhits` 변수는 이미 검색 결과가 있는지 확인하기 위해 `searchKey` 에 해당하는 목록을 찾고 그 값을 저장합니다.

셋째, `updateHits` 변수는 총 결괏값을 전달하기 위해 이전 결과인 `oldhits`와 새 결과인 `hits`를 합칩니다.

 다음으로 `setState()`에 있는 `results` 객체를 살펴봅시다.

{title="src/App.js",lang=javascript}
~~~~~~~~
results: {
  ...results,
  [searchKey]: { hits: updatedHits, page }
}
~~~~~~~~

`  [searchKey]: { hits: updatedHits, page }
` 이 부분을 자세히 설명하겠습니다. `results` 객체에서 `searchKey`로 찾은 결과를 저장함을 말합니다. 키(key)는 `searchKey`는 검색어이며, 값(value)은 `hits`와 `page` 프로퍼티가 있는 객체입니다. 이전 절에서 `[searchKey]:...`와 같은 구문을 이미 배웠습니다. ES6의 계산된 프로퍼티 이름(ES6 computed property name)로 객체에서 값을 동적으로 할당할 때 사용됩니다.

`  ...results,` 부분은 객체 전개 연산자를 사용해 `searchKey`에 따른 모든 `results`를 전파합니다. 그렇지 않으면 이전에 저장된 모든 `results`가 손실됩니다. 

이제 모든 결과를 검색어로 저장하겠습니다. 먼저 캐시를 활성화입니다. 그리고 고정값인 `searchKey`에 따라 `results`를 검색합니다. `searchKey`를 사용하지 않으면 Search 컴포넌트를 사용할 때 그 값이 계속 변경됩니다. 따라서 변동값인 `searchTerm`를 사용할 경우 검색이 중단되는 문제가 발생합니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
# leanpub-start-insert
    const {
      searchTerm,
      results,
      searchKey
    } = this.state;

    const page = (
      results &&
      results[searchKey] &&
      results[searchKey].page
    ) || 0;

    const list = (
      results &&
      results[searchKey] &&
      results[searchKey].hits
    ) || [];

# leanpub-end-insert
    return (
      <div className="page">
        <div className="interactions">
          ...
        </div>
# leanpub-start-insert
        <Table
          list={list}
          onDismiss={this.onDismiss}
        />
# leanpub-end-insert
        <div className="interactions">
# leanpub-start-insert
          <Button onClick={() => this.fetchSearchTopStories(searchKey, page + 1)}>
# leanpub-end-insert
            More
          </Button>
        </div>
      </div>
    );
  }
}
~~~~~~~~

`searchKey`에 해당하는 결괏값을 찾지 못할 때, `list`는 비어있습니다. 이렇게 만들면 Table 컴포넌트를 조건부 렌더링으로 처리할 수 있습니다. "More" 버튼에 `searchTerm`를 `searchKey`로 수정했습니다. 이렇게 하지 않으면  `searchTerm`이 변경될 때마다  페이지 매김 데이터를 가져오게 됩니다. Search 컴포넌트 내 입력 필드 값이 변경될 때 마다 `searchTerm`에 저장됨을 잊지 마세요.

검색 기능이 잘 작동하는지, 해커 뉴스 API에서 받은 데이터가 잘 저장되는지 확인하세요.

추가로 `onDismiss()` 메서드도 개선하겠습니다. `result` 객체가 아닌, 여러 `results`를 다루도록 수정하겠습니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
  onDismiss(id) {
# leanpub-start-insert
    const { searchKey, results } = this.state;
    const { hits, page } = results[searchKey];
# leanpub-end-insert

    const isNotId = item => item.objectID !== id;
# leanpub-start-insert
    const updatedHits = hits.filter(isNotId);

    this.setState({
      results: {
        ...results,
        [searchKey]: { hits: updatedHits, page }
      }
    });
# leanpub-end-insert
  }
~~~~~~~~

"Dismiss" 버튼이 잘 동작하는지 확인하세요. 

하지만 캐시 기능이 완벽하게 구현되지 않았습니다. 검색 요청 시, API 요청을 막는 장치가 없습니다. 이미 동일한 결과가 있을 때 API 요청을 방지하는 검사를 하지 않습니다. `results`는 캐시에 저장되지만 이를 사용하지 않았습니다. 마지막으로 캐시에서 `results`를 사용할 수 있을 때, API 요청을 막도록 수정해봅시다.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  constructor(props) {

    ...

# leanpub-start-insert
    this.needsToSearchTopStories = this.needsToSearchTopStories.bind(this);
# leanpub-end-insert
    this.setSearchTopStories = this.setSearchTopStories.bind(this);
    this.fetchSearchTopStories = this.fetchSearchTopStories.bind(this);
    this.onSearchChange = this.onSearchChange.bind(this);
    this.onSearchSubmit = this.onSearchSubmit.bind(this);
    this.onDismiss = this.onDismiss.bind(this);
  }

# leanpub-start-insert
  needsToSearchTopStories(searchTerm) {
    return !this.state.results[searchTerm];
  }
# leanpub-end-insert

  ...

  onSearchSubmit(event) {
    const { searchTerm } = this.state;
    this.setState({ searchKey: searchTerm });
# leanpub-start-insert

    if (this.needsToSearchTopStories(searchTerm)) {
      this.fetchSearchTopStories(searchTerm);
    }

# leanpub-end-insert
    event.preventDefault();
  }

  ...

}
~~~~~~~~

이제 클라이언트는 동일한 검색어를 두 번 검색하더라도, 단 한번 API 요청을 보냅니다. 페이지 매김 데이터도 같은 방법으로 캐시됩니다. `result`에 항상 마지막 페이지가 저장되기 때문입니다. 

## 3.8 오류 처리 

이전 절에서 API 결과를 캐싱하고 페이지 매김 기능을 구현해 더 많은 기사 리스트를 계속 가져오게 만들었습니다. 그러나 제일 중요한 부분을 하지 않았습니다. 안타깝게도 많은 개발자들이 애플리케이션을 개발하면서 오류 처리(Error Handling)를 간과하고 있습니다. 오류 처리를 무시한 채 개발하는 것은 정말 쉽기 때문이지요.

이번 절에서는 API 요청 시, 효과적인 오류 처리 방법에 대해 알아보겠습니다. 이전 절에서 로컬 상태와 조건부 렌더링으로 오류 처리하는 방법을 배웠습니다. 기본적으로 오류는 리액트에서 또 다른 "상태(state)"라고 볼 수 있습니다. 오류가 발생하면 로컬 상태로 저장하고 컴포넌트의 조건부 렌더링로 오류 메시지를 표시할 수 있습니다. 우리는 App 컴포넌트의 오류 처리를 다루겠습니다. App 컴포넌트는 해커 뉴스 API로 데이터를 받는 곳이기 주요 컴포넌트이기 때문입니다. 그럼 이제 시작해봅시다.

첫째, 로컬 상태에 오류를 추가합니다. `error` 객체의 초기값은 `null`이고, 오류가 발생하면 오류 메시지 값으로 설정됩니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {
  constructor(props) {
    super(props);

    this.state = {
      results: null,
      searchKey: '',
      searchTerm: DEFAULT_QUERY,
# leanpub-start-insert
      error: null,
# leanpub-end-insert
    };

    ...
  }

...

}
~~~~~~~~

둘째, fetch API의 `catch()` 블록 안에 `setState()`메서드로 오류 객체를 로컬 상태로 저장합니다. API 요청이 실패하면 catch 블록이 실행됩니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  fetchSearchTopStories(searchTerm, page = 0) {
    fetch(`${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${searchTerm}&${PARAM_PAGE}${page}&${PARAM_HPP}${DEFAULT_HPP}`)
      .then(response => response.json())
      .then(result => this.setSearchTopStories(result))
# leanpub-start-insert
      .catch(error => this.setState({ error }));
# leanpub-end-insert
  }

  ...

}
~~~~~~~~

셋째, `render()` 내 로컬 상태 내 `error` 객체를 가져오고 조건문 렌더링으로 오류 메시지를 표시합니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {
class App extends Component {

  ...

  render() {
    const {
      searchTerm,
      results,
      searchKey,
# leanpub-start-insert
      error
# leanpub-end-insert
    } = this.state;

    ...

# leanpub-start-insert
    if (error) {
      return <p>Something went wrong.</p>;
    }
# leanpub-end-insert

    return (
      <div className="page">
        ...
      </div>
    );
  }
}
~~~~~~~~

여기까지입니다. 오류 처리에 문제 없는지를 확인하려면 API URL을 존재하지 않는 URL로 바꿔서 테스트해보세요.

{title="src/App.js",lang=javascript}
~~~~~~~~
const PATH_BASE = 'https://hn.foo.bar.com/api/v1';
~~~~~~~~

애플리케이션 대신, 오류 메시지가 표시됩니다. 원하는 곳에 조건부 렌더링으로 오류 메시지를 표시하면 됩니다. 조건부 렌더링을 전체 앱으로 감싸게 되면 빈 화면만 보일 것입니다. 따라서 Table 컴포넌트 또는 오류 메시지 둘 중 하나를 표시하면 어떨까요? 오류가 발생해도 나머지 컴포넌트는 화면에 보이기 때문에 사용자에게 혼란을 주지 않을 것입니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    const {
      searchTerm,
      results,
      searchKey,
      error
    } = this.state;

    const page = (
      results &&
      results[searchKey] &&
      results[searchKey].page
    ) || 0;

    const list = (
      results &&
      results[searchKey] &&
      results[searchKey].hits
    ) || [];

    return (
      <div className="page">
        <div className="interactions">
          ...
        </div>
# leanpub-start-insert
        { error
          ? <div className="interactions">
            <p>Something went wrong.</p>
          </div>
          : <Table
            list={list}
            onDismiss={this.onDismiss}
          />
        }
# leanpub-end-insert
        ...
      </div>
    );
  }
}
~~~~~~~~

오류 테스트를 위해 기존 URL를 수정했다면 원 상태로 되돌려야 합니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
const PATH_BASE = 'https://hn.algolia.com/api/v1';
~~~~~~~~

애플리케이션이 잘 작동하는지 확인하세요. 

### 읽어보기

* [[리액트 공식 문서] 리액트 컴포넌트 오류 처리](https://reactjs.org/blog/2017/07/26/error-handling-in-react-16.html)

{pagebreak}

## 3.9 Axios 라이브러리 사용

이전 절에서 fetch API로 해커 뉴스 데이터를 받아왔습니다. 하지만 모든 브라우저가 fetch API를 지원하는 것은 아닙니다. 특히 오래된 브라우저는 fetch API를 지원하지 않으며, 헤드리스 브라우저(Headless Browser)에서 애플리케이션을 테스트할 때, fetch API와 관련된 문제가 발생할 수 있습니다. 헤드리스 브라우저는 실제 애플리케이션이 구동되는 브라우저가 아닌 테스트용으로 테스트 자동화에 사용되는 브라우저입니다. 물론 구버전 브라우저(폴리필, polyfill)와 테스트([isomorphic-fetch](https://github.com/matthew-andrews/isomorphic-fetch))에서도 fetch API를 사용할 수 있는 방법이 있지만, 이 책에서는 이 내용을 다루지 않겠습니다.

이와 같이 브라우저 간 문제와 API의 한계를 해결하고자 기존 fetch API를 대체하는 라이브러리를 도입할 수 있습니다. 그 중 [axios](https://github.com/axios/axios)는 많은 리액트 개발자들이 사용하고 있는 비동기 API 라이브러리입니다. 이번 절에서는 axios 라이브러리 설치와 사용 방법, 웹 개발의 고질적인 문제점인 구버전 브라우저과 헤드리스 브라우저 테스트 해결 방법에 대해 알아보겠습니다.

fetch API를 axios로 바꿔봅시다.

첫째, 커맨드 라인에 axios를 설치합니다.

{title="Command Line",lang="text"}
~~~~~~~~
npm install axios
~~~~~~~~

둘째, App 컴포넌트 파일에 axios를 가져옵니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
import React, { Component } from 'react';
# leanpub-start-insert
import axios from 'axios';
# leanpub-end-insert
import './App.css';

...
~~~~~~~~

지금부터 `fetch()` 메서드 대신 `axios()` 메서드를 사용하겠습니다. 사용법은 fetch API와 거의 동일합니다. 인자는 URL이며 프로미스(promise)를 반환합니다. 반환된 응답을 JSON으로 변환하지 않아도 됩니다. axios 이 일을 하며. 자바스크립트에서 `data` 객체로 반환합니다. 따라서 반환된 데이터 구조 그대로 사용하면 됩니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  fetchSearchTopStories(searchTerm, page = 0) {
# leanpub-start-insert
    axios(`${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${searchTerm}&${PARAM_PAGE}${page}&${PARAM_HPP}${DEFAULT_HPP}`)
      .then(result => this.setSearchTopStories(result.data))
# leanpub-end-insert
      .catch(error => this.setState({ error }));
  }

  ...

}
~~~~~~~~

`axios()` 메서드는 디폴트로 HTTP GET 요청을 보냅니다. HTTP GET 요청을 좀더 명확하게 표현하려면 `axios.get()` 메서드를 사용합니다. 마찬가지로 HTTP POST 요청은 `axios.post()` 메서드를 사용합니다. 이처럼 axios는 간단한 코드만으로 원격 API 요청을 훌륭하게 수행합니다. 기존 fetch API는 코드가 복잡하며 프로미스(promise)도 처리해야 해서 불편했지만, axios로 개발이 편하고 쉽게 개발할 수 있습니다. 특정 브라우저와 헤드리스 브라우저 환경 문제 역시 해결됩니다. 

App 컴포넌트에서 해커 뉴스 API 요청 시, 고쳐야할 점이 하나 더 있습니다. 예를 들어 `componentDidMount()` 생명주기 메서드에서 API 요청을 하는 동안, 네비게이션을 클릭해 다른 페이지로 이동했다고 가정해봅시다. 그리고 `then()` 또는 `catch()` 에서 `this.setState()` 메서드를 사용했습니다. 컴포넌트 마운트가 해제되더라도 여전히 `componentDidMount()` 메서드에서는 보류 중인 요청이 존재합니다. 이 경우 브라우저 개발자 도구나 커맨드 라인에서 아래와 같은 경고 메시지가 표시됩니다.

{title="Command Line",lang="text"}
~~~~~~~~
Warning: Can only update a mounted or mounting component. This usually means you called setState, replaceState, or forceUpdate on an unmounted component. This is a no-op.
~~~~~~~~

이 경고 메시지를 없애기 위한 해결 방법은 컴포넌트가 마운트 해제될 때 API 요청을 중단하거나, 마운트 해제된 컴포넌트에서 `this.setState()` 메서드 호출을 방지하는 것입니다.
니다. 리액트 개발에서는 경고 메시지 없이 깨끗한 애플리케이션을 유지하는 것이 매우 중요합니다. promise에서 API 요청을 중단하게 처리하겠습니다. 아래 제가 소개하는 방법은 많은 개발자들이 따르고 있는 모범 사례입니다. 경고 메시지 내용을 해결할지, 그대로 내버려 둘지는 판단은 각자의 몫입니다. 이 책의 후반부에도 비슷한 경고 메시지가 나올 때마다 아래 방법으로 해결하면 됩니다.

해결 방법은 다음과 같습니다. 컴포넌트 생명주기 상태를 가리키는 클래스 필드인 `_isMounted`을 만듭니다. 초기값은 `false`입니다. 컴포넌트가 마운트 될 때 `true`로 변경되며, 컴포넌트가 마운트 해제될 때 다시 `false`로 변경됩니다. 이렇게 하면 컴포넌트 생명주기를 추적할 수 있습니다. 로컬 상태 아니기 때문에 `this.state`와 `this.setState()`와 무관합니다. 클래스 필드 값이 변경되어도 컴포넌트가 다시 렌더링 되지 않습니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {
# leanpub-start-insert
  _isMounted = false;
# leanpub-end-insert

  constructor(props) {
    ...
  }

  ...

  componentDidMount() {
# leanpub-start-insert
    this._isMounted = true;
# leanpub-end-insert

    const { searchTerm } = this.state;
    this.setState({ searchKey: searchTerm });
    this.fetchSearchTopStories(searchTerm);
  }

# leanpub-start-insert
  componentWillUnmount() {
    this._isMounted = false;
  }
# leanpub-end-insert

  ...

}
~~~~~~~~

마지막으로 처음부터 요청을 중단하지 않고, 컴포넌트가 마운트 해제되었는지 확인하여 `this.setState()` 메서드 호출을 피하도록 수정하겠습니다. 이제 경고 메시지가 사라질 것입니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  fetchSearchTopStories(searchTerm, page = 0) {
    axios(`${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${searchTerm}&${PARAM_PAGE}${page}&${PARAM_HPP}${DEFAULT_HPP}`)
# leanpub-start-insert
      .then(result => this._isMounted && this.setSearchTopStories(result.data))
      .catch(error => this._isMounted && this.setState({ error }));
# leanpub-end-insert
  }

  ...

}
~~~~~~~~

이번 절에서는 리액트에서 다른 라이브러리로 대체하는 방법을 배웠습니다. 자바스크립트에는 많은 라이브러리들이 있습니다. 개발 중 문제가 생기면, 여러 라이브러리를 탐색하고 적합한 솔루션을 도입해 해결합니다. 또한 마운트 해제된 컴포넌트에서 `this.setState()` 메서드 호출을 피하는 방법을 배웠습니다. 반대로 axios에서 API 요청 취소를 막는 방법도 있으니 직접 찾아보고 실습해보길 바랍니다. 

### 읽어보기

* [[저자 블로그] 왜 프레임워크가 중요한가](https://www.robinwieruch.de/why-frameworks-matter/)
* [[저자 리퍼지토리] 리액트 컴포넌트 대체 문법](https://github.com/rwieruch/react-alternative-class-component-syntax)

이번 장에서는 외부 API를 사용하는 방법을 배웠습니다. 지금까지 학습한 내용을 정리해봅시다.

* 리액트
  * ES6 클래스 컴포넌트의 생명주기 메소드와 사용 사례
  * `componentDidMount()` 메서드에서 API 호출
  * 조건부 렌더링
  * 폼 이벤트
  * 에러 핸들링
* ES6
  * 템플릿 문자열 구성
  * 전개 연산자로 불변 데이터 구조 작성
  * 프로퍼티 이름 계산
* 일반
  * 해커 뉴스 API 인터렉션
  * 네이티브 브라우저 fetch API
  * 클라이언트 및 서버 내 검색 기능 구현
  * 페이지네이션 데이터 호출
  * 클라이언트 렌더링
  * fetch 대신 axios 도입

실습 코드는 [깃허브 리퍼지토리](https://github.com/the-road-to-learn-react/hackernews-client/tree/5.1)에서 확인할 수 있습니다.
