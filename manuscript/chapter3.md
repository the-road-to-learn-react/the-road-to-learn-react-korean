# API 실제 사용 Getting Real with an API

이번 장에서는 외부 API로 데이터를 요청하고 받아보겠습니다.

API에 잘 모른다면, 저자가 쓴 [아무도 나에게 API를 알려주지 않았다](https://www.robinwieruch.de/what-is-an-api-javascript/) 글을 읽어보길 바랍니다. 

[해커 뉴스(Hacker News)](https://news.ycombinator.com/) 플랫폼은 기술 주제 관련 우수 기사를 큐레이션 하는 플랫폼입니다. 이 책에서는 해커 뉴스 API를 사용해 플랫폼에서 인기 글 목록을 보여줄 것입니다. 해커 뉴스는 [데이터](https://github.com/HackerNews/API) 조회 API와 [검색](https://hn.algolia.com/api) API를 제공합니다. 먼저 사이트에서 API 명세서를 읽고 데이터 구조를 파악하길 바랍니다.

## 생명주기 메서드 Lifecycle Methods

API를 사용하기 전, 컴포넌트 생명주기 메서드에 대해 알아봅시다. 생명주기 메서드는 ES6 클래스 컴포넌트에서 사용할 수 있지만 비 상태 컴포넌트에서는 사용할 수 없습니다. 

이전 장에서 우리는 E6 클래스에 대해 배웠습니다. `render()` 외에 리액트 ES6 클래스 컴포넌트의 다른 메서드가 있습니다. 바로 생명주기 메서드입니다. 

우리는 이미 ES6 클래스 컴포넌트에서 사용하는 두 생명주기 메서드 `constructor()`와 `render()`를 알고 있습니다. 

컴포넌트 인스턴스가 만들어져 DOM에 삽입될 때 생성자가 호출됩니다. 컴포넌트가 인스턴스화 됩니다. 이 프로세스를 컴포넌트가 탑재됐다(mounting)고 말합니다. 

`render()`는 마운트 프로세스 중에도 호출되지만 컴포넌트가 업데이트될 때도 호출됩니다. 컴포넌트의 state와 props가 변경될 때마다 `render()`이 호출됩니다. 

아마 이전 장에서 두 메서드를 사용해봤을 겁니다. 그 외 더 많은 생명주기 메서드가 있습니다.

컴포넌트 마운트에는` componentWillMount()`와`componentDidMount()` 두 메서드가 있습니다. `constructor()`메서드가 호출되면, `componentWillMount()`메서드가 호출되고, 그 후 `render()`메서드가 호출되며, 마지막에 `componentDidMount()`메서드가 호출됩니다. 

마운트 프로세스에서 생명주기 메서드는 아래 순서대로 호출됩니다.

* `constructor()`
* `componentWillMount()`
* `render()`
* `componentDidMount()`


state나 props가 변경 시, 업데이트 프로세스가 시작되고, 아래와 같은 순서로 5개의 생명주기 메서드가 호출됩니다.

* `componentWillReceiveProps()`
* `shouldComponentUpdate()`
* `componentWillUpdate()`
* `render()`
* `componentDidUpdate()`

마지막으로 컴포넌트 마운트 해제가 되기 전에, `componentWillUnmount()` 메서드가 호출됩니다.

* `componentWillUnmount()`

처음부터 모든 생명주기 메서드를 사용하지 않아도 됩니다. 아직 많이 다뤄보지 않았기 때문에 익숙하지 않을 뿐입니다. 대규모 애플리케이션에도 `constructor()`과 `render()` 메서드만 사용하는 경우가 많습니다. 앞서 알아본 생명주기 메서드를 언제 사용해야 하는지 정리해봅시다. 

* **`constructor(props)`** - 컴포넌트 초기화 시 호출됩니다. 초기 컴포넌트 상태 및 클래스 메서드를 정의합니다. 

* **`componentWillMount()`** - `render()` 메서드 전에 호출됩니다. 컴포넌트의 두 번째 렌더링을 트리거하지 않아 컴포넌트 상태를 설정하기 적합합니다. `constructor()`을 사용해 초기 상태를 설정하는 것이 좋습니다. 

* **`render()`** - 컴포넌트의 출력을 반환하기 때문에 필수로 사용해야 합니다. props 및 상태를 입력을 가져오고 요소를 반환합니다. 

* **`componentDidMount()`** - 컴포넌트가 마운트 될 때 한 번만 호출됩니다. 이 메서드에서 API에서 데이터를 가져오기 위해 비동기 요청을 수행할 수 있습니다. 가져온 데이터는 내부 컴포넌트 상태에 저장되어 `render()` 메서드로 컴포넌트가 업데이트됩니다. 

* **`componentWillReceiveProps(nextProps)`** - 업데이트 생명주기 동안 호출됩니다. `nextProps`는 다음 props를, `this.props`로 이전 props를 조회해 서로 차이를 비교할 수 있습니다. `nextProps`를 컴포넌트의 state로 사용할 수 있습니다.
 
* **`shouldComponentUpdate(nextProps, nextState)`** - props 또는 state의 변경으로 컴포넌트가 업데이트될 때 호출됩니다. 성능 최적화를 위해 고도화된 리액트 애플리케이션에서 사용됩니다. 생명주기 메서드에서 반환하는 부울 값(boolean)에 컴포넌트와 모든 자식이 업데이트 주기에 렌더링 되거나 반대로 그렇지 않을 수 있습니다. 문제가 되는 특정 컴포넌트의 렌더링 생명주기 렌더링을 막을 수 있습니다. 명주기 메서드를 방지할 수 있습니다. 

* **`componentWillUpdate(nextProps, nextState)`** - `render()` 메서드 전에 바로 호출됩니다. `nextProps`는 다음 props를, `nextState`로 다음 state를 조회할 수 있습니다. `render()` 메서드가 실행되기 전에 마지막으로 상태를 업데이트할 수 있는 기회입니다. 이후에는 `setState()` 메서드를 더 이상 사용할 수 없습니다. nextProps를 state 값으로 사용하려면 `componentWillReceiveProps()` 메서드를 를 사용합니다. 

* **`componentDidUpdate(prevProps, prevState)`** - `render()` 메서드 후에 즉시 호출됩니다. DOM조작을 하거나 추가 비동기 요청을 수행할 수 있습니다. 

* **`componentWillUnmount()`** - 컴포넌트를 해체하기 전에 호출됩니다. 테스트를 초기화하는 작업을 수행할 수 있습니다.

`constructor()`와 `render()` 메서드는 이미 사용해봤습니다. ES6 클래스 컴포넌트에서 사용되는 생명주기입니다. `render()` 메서드는 컴포넌트 인스턴스를 반환하기 때문에 반드시 필요한 메서드입니다.

그 외 생명주기 메서드로 `componentDidCatch(error, info)`가 있습니다. 이 메서드는 [React 16](https://www.robinwieruch.de/what-is-new-in-react-16/)에서 도입되었으며 컴포넌트 에러를 캐치합니다. 예를 들어 목록을 표시하는 애플리케이션이 있다고 해봅시다. 외부 API 호출이 실패하여 state가 `null`로 표시되었습니다. 리스트가 비어있지 않고 값이 `null`이 때문에 filter과 map을 사용할 수 없습니다. 이 경우 오류가 발생하여 컴포넌트가 깨지고 전체 애플리케이션이 작동되지 않습니다. 이 때, `componentDidCatch()`로 오류를 포착하고 내부 상태에 저장하여 사용자에게 오류 메세지를 표시해줄 수 있습니다.

### 읽어보기

* [[리액트 공식문서] 리액트 생명주기](https://facebook.github.io/react/docs/react-component.html)
* [[리액트 공식문서] 리액트 생명주기 메서드와 상태 관리](https://facebook.github.io/react/docs/state-and-lifecycle.html)
* [[reactjs.org] 컴포넌트 에러 핸들링](https://reactjs.org/blog/2017/07/26/error-handling-in-react-16.html)

## 데이터 가져오기 Fetching Data

이제 해커 뉴스 API로 데이터를 가져올 준비가 끝났습니다. `componentDidMount()` 생명주기 메서드 안에 `fetch()` 자바스크립트 네이티브 API를 사용해 외부 API를 호출하겠습니다.
그 전에 요청할 API의 URL를 각 조각으로 분리하여 기본 매개 변수로 설정합시다.

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

ES6에서는 [템플릿 문자열](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Template_literals)로 문자열을 연결합니다. 이를 사용해 URL을 API 엔드 포인트에 연결합시다.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// ES6
const url = `${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${DEFAULT_QUERY}`;

// ES5
var url = PATH_BASE + PATH_SEARCH + '?' + PARAM_SEARCH + DEFAULT_QUERY;

console.log(url);
// output: https://hn.algolia.com/api/v1/search?query=redux
~~~~~~~~

이렇게 하면 나중에 URL 구성을 좀더 편하게 관리할 수 있습니다.

API 요청을 시작합시다. 전체 데이터가 한 번에 가져올 것인데, 각 단계는 이후에 설명하겠습니다.

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
    this.fetchSearchTopStories = this.fetchSearchTopStories.bind(this);
# leanpub-end-insert
    this.onSearchChange = this.onSearchChange.bind(this);
    this.onDismiss = this.onDismiss.bind(this);
  }

# leanpub-start-insert
  setSearchTopStories(result) {
    this.setState({ result });
  }

  fetchSearchTopStories(searchTerm) {
    fetch(`${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${searchTerm}`)
      .then(response => response.json())
      .then(result => this.setSearchTopStories(result))
      .catch(e => e);
  }

  componentDidMount() {
    const { searchTerm } = this.state;
    this.fetchSearchTopStories(searchTerm);
  }
# leanpub-end-insert

  ...
}
~~~~~~~~

이 코드 속에 정말 많은 일들이 일어났습니다. 코드를 더 작은 조각으로 나눌까 싶었지만 그렇다면 각 조각 간 관계를 파악하기 어렵다고 판단했습니다. 단계별로 자세히 설명하겠습니다.

첫째, 해커 뉴스 API의 데이터가 사용됨으로 이전의 샘플 데이터는 더 이상 필요없기 때문에 제거합니다. 컴포넌트 초기 상태는 결과 `result`가 비어있고, 검색어 `searchTerm`는 디폴트 값(`DEFAULT_QUERY`)으로 설정되어 있습니다. searchTerm은 Search 컴포넌트의 입력필드와 첫 번째 요청 시 사용됩니다.
                                                                                    
둘째, 컴포넌트가 마운트된 후 데이터를 가져오기 위해 `componentDidMount()` 생명주기 메서드를 사용합니다. 첫 번째 요청 시 로컬 상태의 기본 검색어가 사용됩니다. 기본 매개변수(`DEFAULT_QUERY`)값이 "redux"이기 때문에 "redux" 기사를 가져올 것입니다.

세 번째, 자바스크립트 네이티브 fetch API가 사용됩니다. ES6 템플릿 문자열로  `searchTerm`과 함께 URL를 만듭니다. 이 URL는 fetch API로 인자로 사용합니다. 응답은 JSON 데이터 구조로 변환하고 컴포넌트 내부 `result` 상태 값에 저장됩니다. 요청 중에 오류가 발생하면 함수는 `then()` 아닌 catch 블록으로 실행됩니다. 다음 장에서 오류 처리에 대해 설명합니다.

마지막으로 생성자에 컴포넌트 메서드를 바인딩 합니다.

이제 기존 샘플 데이터 대신 API로 가져온 데이터를 사용할 수 있습니다. 그러나 조심히 사용해야 합니다. 데이터 목록인 `result`는 [메타 정보와 인기 리스트가 같이 담겨 객체가 복잡합니다.](https://hn.algolia.com/api). `render()` 메서드에서 `console.log(this.state);`로 상태를 출력해 개발자 도구에서 확인해보세요.

다음으로 `result`를 사용해 렌더링 여부를 처리합시다. 초기에 `result` 값이 없는 경우 `null`을 반환합니다. API 요청이 성공하면 결과값이 내부 상태값에 저장되고 업데이트된 App 컴포넌트는 다시 렌더링됩니다.

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

컴포넌트 생명주기 동안 일어나는 일을 요약하면 다음과 같습니다. 컴포넌트는 생성자로 초기화된 후 렌더링 됩니다. 내부 상태 result 값이 null이므로 이 컴포넌트는 아무것도 보이지 않습니다. 그다음 `componentDidMount()` 메서드가 실행됩니다. 이 메서드에서 해커 뉴스 API를 통해 비동기로 데이터를 가져옵니다. 데이터가 도착하면 `setSearchTopStories()`에서 내부 컴포넌트 상태를 업데이트합니다. 이후 업데이트 생명주기가 시작됩니다. 컴포넌트는 `render()`메서드를 다시 실행하지만 이번에는 상태 result 값이 있기 때문에 나타납니다. 따라서 컴포넌트와 Table 컴포넌트가 렌더링 됩니다. 

대부분 브라우저가 지원하는 `fetch()`로 비동기 요청을 수행했습니다. *create-react-app*은 모든 브라우저를 지원하고 있습니다. `fetch()` 대신 노드 패키지인 [superagent](https://github.com/visionmedia/superagent) 또는 [axios](https://github.com/mzabriskie/axios)를 사용할 수 있습니다. 

다시 애플리케이션으로 돌아갑시다. 인기 글 목록이 보일 겁니다. 두 가지 버그가 생겼습니다. 첫째, "dismiss" 버튼입니다. 이 버튼은 새로 받은 객체를 식별하지 못하기 때문에 작동하지 않습니다. 둘째, 초기에 서버에서 검색한 기사 목록을 가져왔지만, 이후 다른 검색어를 요청하면 클라이언트에서 리스트를 필터링합니다. Search 컴포넌트에서 API로 result 객체를 가져오도록 수정해야 합니다. 다음 장에서 이 버그들을 해결하겠습니다.


### 읽어보기

* [[MDN] ES6 템플릿 문자열](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Template_literals)
* [[MDN] the native fetch API](https://developer.mozilla.org/en/docs/Web/API/Fetch_API)
* [[저자 블로그] 리액트에서 데이터 호출하기](https://www.robinwieruch.de/react-fetching-data/)

## ES6 전개 연산자 Spread Operators

`onDismiss()` 메서드는 요청받은 데이터 객체를 인식하지 못하기 때문에 "dismiss" 버튼이 작동하지 않습니다. 지금은 전체 글 목록만 알고 있을 뿐입니다. 목록 내 각 객체를 식별할 수 있게 변경해보겠습니다.

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

지금 `setState()` 메서드에서는 무슨 일이 일어날까요? `result`는 객체 안에 `hits` 프로퍼티를 조회해야 합니다. `hits`는 여러 프로퍼티 중 하나입니다. 조회된 아이템이 `result` 개체에서 제거되면 목록만 업데이트되고 다른 프로퍼티는 그대로 유지됩니다.

`result` 객체 내 `hits` 프로퍼티 값을 직접적으로 변경할 수 있겠지만, 이렇게 구현하지 않을 겁니다.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// 이렇게 하지 마세요. 
this.state.result.hits = updatedHits;
~~~~~~~~

리액트는 불변 데이터 구조가 원칙입니다. 객체를 변경하거나, 상태를 직접 변경해서는 안됩니다. 다른 방법은 새로운 객체를 생성해 어떤 객체도 변경하지 않아 불변 데이터 구조를 유지합니다. 항상 새 객체를 반환하고 객체를 변경하지 않도록 합니다. 

이를 위해 ES6 `Object.assign()` 메서드를 사용합니다. 첫 번째 인자는 타깃 객체이고, 나머지 인자는 소스 객체입니다. 이 객체를 병합해 타깃 객체에 병합합니다. 타깃 객체는 빈 객체가 될 수 있습니다. 소스 객체는 변경되지 않기 때문에 불변성 원칙을 고수합니다. 아래 코드를 작성해봅시다.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const updatedHits = { hits: updatedHits };
const updatedResult = Object.assign({}, this.state.result, updatedHits);
~~~~~~~~

동일한 프로퍼티 이름을 공유할 때, 후자 객체는 전자의 병합 객체를 덮어씁니다. 이제 `onDismiss()` 메서드를 다시 작성해보겠습니다.

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

문제는 해결되었지만, 앞에서 배운 전개 연산자로 더 간단한 코드로 작성할 수 있습니다. `...`은 배열 또는 객체의 모든 값을 복사합니다.

ES6 **배열** 스프레드 연산자를 사용하지 않아도 된다고 생각할 수 있습니다. 그러나 보다 명료하게 코드를 표현하기 위해 아래와 같이 작성할 수 있습니다.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const userList = ['Robin', 'Andrew', 'Dan'];
const additionalUser = 'Jordan';
const allUsers = [ ...userList, additionalUser ];

console.log(allUsers);
// output: ['Robin', 'Andrew', 'Dan', 'Jordan']
~~~~~~~~

`allUsers` 변수는 완전히 새로운 배열입니다. `userList` 변수와 `additionalUser`변수의 값은 그래로 유지됩니다. 이런 식으로 두 배열을 새 배열로 병합 할 수 있습니다.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const oldUsers = ['Robin', 'Andrew'];
const newUsers = ['Dan', 'Jordan'];
const allUsers = [ ...oldUsers, ...newUsers ];

console.log(allUsers);
// output: ['Robin', 'Andrew', 'Dan', 'Jordan']
~~~~~~~~

다음으로 객체 스프레드 연산자(object spread operator)를 알아봅시다. ES6이 아닌, 리액트 커뮤니티에서 이미 사용하고있는 [객체 스프레드 연산자(Object Rest/Spread Properties)](https://github.com/sebmarkbage/ecmascript-rest-spread)을 말합니다. *create-react-app* 역시 객체 스프레드 연산자를 도입했습니다.

ES6 배열 전개 연산자와 같지만 객체가 있다는 것이 다릅니다. 각 키(key)-값(value) 쌍을 새 객체에 복사합니다.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const userNames = { firstname: 'Robin', lastname: 'Wieruch' };
const age = 28;
const user = { ...userNames, age };

console.log(user);
// output: { firstname: 'Robin', lastname: 'Wieruch', age: 28 }
~~~~~~~~

배열 전개 예제와 같이 여러 객체를 펼칠 수 있습니다.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const userNames = { firstname: 'Robin', lastname: 'Wieruch' };
const userAge = { age: 28 };
const user = { ...userNames, ...userAge };

console.log(user);
// output: { firstname: 'Robin', lastname: 'Wieruch', age: 28 }
~~~~~~~~

결국 `Object.assign()` 메서드를 대체할 수 있습니다.

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

`onDismiss()`메서드가 잘 작동하는지 확인합시다. `onDismiss()`메서드는 result 목록에서 각 객체를 식별하고 목록에서 취소된 항목을 업데이트합니다.

### 읽어보기

* [[MDN] ES6 Object.assign()](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)
* [[MDN]ES6 배열 전개 연산자](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Spread_operator)

## 조건부 렌더링 Conditional Rendering

앞에서 조건부 렌더링에 대해 일부 설명했습니다. 조건부 렌더링은 하나 또는 다른 요소를 렌더링을 결정합니다. 요소를 렌더링 하거나 아무것도 렌더링 하지 않는 것을 말합니다. 우리가 잘 알고 있는 if-else 문으로 간단하게 작성할 수 있습니다. 

컴포넌트 내부 상태 `result` 객체 초기값은 `null`입니다. App 컴포넌트에 API에서 `result`가 도착하지 않으면 요소를 반환하지 않습니다. 이 것이 바로 조건부 렌더링입니다. 특정 조건에 대해 이전에`render()` 생명주기 메서드에서 반환하기 때문에 이미 조건부 렌더링에 해당합니다. App 컴포넌트는 아무것도 렌더링 하지 않거나 요소를 렌더링 하게 됩니다. 

한 단계를 더 나아가 봅시다. 독립적인 조건부 렌더링에서 `result`에 의존하는 컴포넌트인 Table도 함께 포함시킵니다. `result`가 없더라도 모든 내용이 표시되어야 합니다. 이번에는 삼항 조건 연산자(ternary operator)를 사용해보겠습니다.

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

마지막으로 논리 연산자 `&&`를 사용합시다. 자바스크립트에서 `true &&'Hello World'` 경우 'Hello World'로 평가되고, `false && 'Hello World'`경우 항상 거짓으로 평가됩니다.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const result = true && 'Hello World';
console.log(result);
// output: Hello World

const result = false && 'Hello World';
console.log(result);
// output: false
~~~~~~~~

리액트에서도 논리 연산자를 사용할 수 있습니다. 조건이 참이면, `&&` 논리 연산자 뒷부분 내용이 출력됩니다. 조건이 거짓이면 리액트는 표현식을 무시하고 건너뜁니다. Table 컴포넌트를 반환 또는 아무것도 반환하지 않게 조건부 렌더링에 만들어봅시다.

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

지금까지 조건부 렌더링 사용 방법에 대해 알아보았습니다. [저자의 블로그]( (https://www.robinwieruch.de/conditional-rendering-react/))에서 조건부 렌더링에 대한 더 많은 예제를 소개했습니다. 

마침내 애플리케이션에서 페치 한 데이터를 모두 볼 수 있습니다. 데이터 페치가 보류 중일 때를 제외하고 모든 항목이 표시됩니다. 요청이 결과를 해석하여 로컬 상태로 저장하면 `render()` 메서드가 다시 실행되고, 조건부 렌더링의 조건에 따라 Table 컴포넌트가 표시됩니다.


### 읽어보기

* [[리액트 공식문서] 리액트 조건 렌더링](https://facebook.github.io/react/docs/conditional-rendering.html)
* [[저자 블로그] 조건문 렌더링의 다양한 방법](https://www.robinwieruch.de/conditional-rendering-react/)

## Search 컴포넌트의 클라이언트와 서버 처리  Client-/Server-side Search

클라이언트에서 검색어에 따라 목록 필터링을 구현해보겠습니다. 검색어 기본 매개변수로 해커 뉴스 API에 요청하여 서버에서 검색한 후, 이에 대한 응답을 `componentDidMount()` 생명주기 메서드에서 처리할 것입니다. 

App 컴포넌트에서 `onSearchSubmit()` 메서드를 정의하겠습니다. 이 메서드는 Search 컴포넌트에서 검색을 실행할 때 해커 뉴스 API 요청 결과를 가져옵니다. `componentDidMount()` 생명주기 메서드에서 했던 것과 같습니다. 그러나 이번에는 검색어 기본 매개변수가 아닌 내부 상태 값을 사용합니다.

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
    this.fetchSearchTopStories = this.fetchSearchTopStories.bind(this);
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
    this.fetchSearchTopStories(searchTerm);
  }
# leanpub-end-insert

  ...
}
~~~~~~~~

Search 컴포넌트에 'Search' 버튼을 추가해야 합니다. 이 버튼은 검색 요청을 보내고, 해커 뉴스 API에서 데이터를 가져옵니다. 

먼저 Search 컴포넌트에 `onSearchSubmit()` 메서드를 전달합시다.

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

두 번째로 Search 컴포넌트에 버튼을 추가하겠습니다. 버튼은 `type="submit"`을 가지고, 폼은 `onSubmit()` 메서드를 전달하기 위해 `onSubmit()` 속성을 사용합니다. 자식 프로퍼티는 버튼 내용으로 사용하겠습니다.

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

Table 컴포넌트에서 클라이언트의 검색이 필요 없기 때문에 필터 기능을 제거합니다. `isSearched()` 함수도 더 이상 사용하지 않기 때문에 제거합니다. 비로소 검색 버튼을 클릭하면 해커 뉴스 API를 통해 결괏값을 가져옵니다.

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

해커 뉴스 기사를 검색할 수 있고 API가 완벽하게 작동하는지 확인해봅시다. 더 이상 클라이언트가 검색을 처리하지 않아야 합니다.

### 읽어보기

* [[리액트 공식문서] synthetic events in React](https://facebook.github.io/react/docs/events.html)

### 실습하기

* [해커 뉴스 API](https://hn.algolia.com/api)로 이것저것 실험해봅니다.

## 페이지 매김 데이터 가져오기 Paginated Fetch

반환되는 데이터 구조를 자세히 살펴보았나요? [해커 뉴스 API](https://hn.algolia.com/api)는 첫 번째 `hits` 목록만 반환합니다. 처음 응답 시, page 프로퍼티 값은 `0`으로 페이지 번호를 말합니다. 이 프로퍼티 값으로 목록을 가져옵니다. 검색어는 유지하고 다음 페이지 번호를 API에 전달하면 됩니다. 

페이지 매김 데이터를 처리할 수 있게 요청할 URL를 쪼개어 각 상수로 만들겠습니다. 이렇게 하면 URL를 구성 가능한 형태로 만들 수 있습니다.

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

이제 상수를 매개변수로 추가합시다.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const url = `${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${searchTerm}&${PARAM_PAGE}`;

console.log(url);
// output: https://hn.algolia.com/api/v1/search?query=redux&page=
~~~~~~~~

`fetchSearchTopStories()` 메서드는 두 번째 인자가 있는데, 이 두 번째 인자가 없으면 기본으로 `0`페이지를 전달합니다. 따라서 `componentDidMount()` 메서드와 `onSearchSubmit()` 메서드는 최초 요청 시, 첫 번째 페이지를 가져옵니다. 추가 요청이 있을 때마다 두 번째 인자를 통해 다음 페이지를 가져와야 합니다.

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
      .catch(e => e);
  }

  ...

}
~~~~~~~~

이제 `fetchSearchTopStories()` 메서드에서 API 응답을 받아 현재 페이지를 사용할 수 있습니다. 버튼에서 이 메서드를 사용해 `onClick` 버튼 핸들러로 더 많은 목록을 가져와야 합니다. 다음으로 버튼을 클릭하면 그다음 페이지의 데이터를 가져오게 만들어봅시다. `onClick()` 핸들러에서 현재 검색어와 다음 페이지 번호(현재 페이지 + 1)만 정의하면 됩니다.

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

`render()` 메서드에서 `result`가 없을 때 페이지 번호 기본값은 `0`입니다. `render()` 메서드는  `componentDidMount()` 생명주기 메서드에서 비동기로 데이터를 가져오기 전에 실행됨을 잊지 맙시다.

아직 한 가지 더 할 일이 남아있습니다. 다음 페이지의 데이터를 가져오지만, 이전 데이터를 덮어쓰게 되는 문제가 있습니다. 새 데이터를 정의하기보다 기존 상태 값에 새 결과 객체 목록을 추가하는 것이 바람직합니다.

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

`setSearchTopStories()`메서드에서 일어나는 일을 설명하겠습니다. 

첫째, 먼저 `result`에서 `hits`와 `page`를 얻습니다. 

둘째, 이미 `hits`가 있는지 확인합니다. `page` 번호가 0일 때는 `componentDidMount()`또는 `onSearchSubmit()`에서 새로운 검색 요청이 전달됩니다. `hits` 목록은 비어있지만, "More" 버튼을 클릭하면 0이 아닌 다음 페이지를 요청합니다. 이전 `hits`는 이미 저장되어있으므로 사용 가능합니다. 

셋째, 이전 `hits`에 새로운 `hits`이 덮어쓰기 되면 안 됩니다. 따라서 두 목록을 병합해야 합니다. 이를 위해 ES6 배열 전개 연산자를 사용합니다. 

넷째, `hits`와 `hits`를 컴포넌트 내부 상태로 업데이트합니다. 

마지막으로 "More" 버튼 클릭할 때 일부 목록만 가져오게 해봅시다. 각 요청에 따라 목록을 가져올 수 있게 API URL를 구성 가능한 형태로 만들겠습니다. 각 경로를 상수로 만들어 추가하면 됩니다.

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

이제 상수를 사용해 API URL를 조합하겠습니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
fetchSearchTopStories(searchTerm, page = 0) {
# leanpub-start-insert
  fetch(`${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${searchTerm}&${PARAM_PAGE}${page}&${PARAM_HPP}${DEFAULT_HPP}`)
# leanpub-end-insert
    .then(response => response.json())
    .then(result => this.setSearchTopStories(result))
    .catch(e => e);
}
~~~~~~~~

이전보다 더 많은 목록을 가져옵니다. 이번 장에서는 해커 뉴스 API를 통해 실제 데이터를 사용해봤습니다. 새 프로그래밍 언어나 라이브러리를 학습할 때, 외부 API를 사용하면 더 재미있게 배울 수 있을 겁니다. 저도 이렇게 배웠으니까요.

### 실습하기

* [해커 뉴스 API](https://hn.algolia.com/api)의 매개변수를 바꿔 API를 요청해봅니다.

## 클라이언트 캐시 Client Cache

해커 뉴스 API에 검색 요청을 보냅니다. 처음에는 "redux"를 검색하고, 그다음 "react"를, 그리고 다시 "redux" 검색을 할 수 있습니다. 총 세 번 요청을 보냅니다. "redux"를 두 번 검색하여 모든 데이터를 한 번에 가져오기 위해 비동기 왕복 여행(asynchronous roundtrip)을 합니다. 클라이언트 캐시에서 각 결과를 저장합니다. API 요청을 받으면 이미 결과가 있는지 확인합니다. 이미 캐시가 있으면 캐시를 사용합니다. 캐시가 없다면 데이터를 가져오기 위해 API를 요청합니다. 

각 결과마다 클라이언트 캐시를 가지려면, 컴포넌트 내부 상태에 `results` 하나가 아닌, 여러 `results`를 저장해야 합니다. `results` 객체는 키(key)가 검색어이고, 값(value)이 `hits`입니다. 각 API 결과는 키를 검색어로 개별로 저장됩니다.

현재 로컬 상태는 아래 코드와 비슷할 것입니다.

{title="Code Playground",lang="javascript"}
~~~~~~~~
result: {
  hits: [ ... ],
  page: 2,
}
~~~~~~~~

예를 들어, 두 번 API를 요청한다고 해봅시다. 첫 번째 검색어는 "redux"이고 두 번째는 "react"입니다. `results`객체는 아래와 같을 것입니다.

{title="Code Playground",lang="javascript"}
~~~~~~~~
results: {
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

첫째, 컴포넌트 초기 상태의 `result` 객체를 `results`로 변경합니다. 둘째, 각 `result`를 저장할 때 사용될 임시  `searchKey`를 정의합니다.

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

요청을 보내기 전에 `searchTerm`를 반영하는 `searchKey`를 설정해야 합니다. 왜 `searchTerm`을 사용하지 않을까요? 구현하기 전 충분히 이해를 하고 넘어가야 합니다. `searchTerm`은 Search 컴포넌트의 입력 필드에 입력을 할 때마다 그 값이 변경되는 변경 변수(fluctuant variable)입니다. 우리는 값이 변경되지 않는 고정 변수가 필요합니다. 캐시 안에 저장된 현재 `result`가 해당됩니다. 따라서 `render()` 메서드에 현재 `result`를 표시할 수 있습니다.

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

다음으로 `result`가 컴포넌트 내부 상태에 저장되기 위해 함수를 수정해야 합니다. `searchKey`로 각 `result`를 저장해야 합니다.

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

`searchKey`는 업데이트된 `hits`와 `page`를 `results`에 저장하기 위한 키로 사용됩니다. 

첫째, 컴포넌트 상태에서 `searchKey`를 검색해야 합니다. `searchKey`는 `componentDidMount()`와 `onSearchSubmit()`에 설정되어 있습니다.

둘째, 이전 `hits`는 새로 받은 `hits`와 합쳐져야 합니다. 하지만 이번에는 `searchKey` 키로 `results`의 이전 `hits`를 가져옵니다.

셋째, 새`result`는 상태 `results`로 매핑됩니다. `setState()`에 있는 `results`객체를 살펴봅시다.

{title="src/App.js",lang=javascript}
~~~~~~~~
results: {
  ...results,
  [searchKey]: { hits: updatedHits, page }
}
~~~~~~~~

아래 부분은 `searchKey`에 의해 업데이트된 `results`를 저장합니다. 값은 `hits`와 `page` 프로퍼티가 있는 객체입니다. `searchKey`는 검색어입니다. 이미 여러분은 `[searchKey]:...` 구문을 배웠습니다. ES6를 통해 계산된 프로퍼티 이름입니다. 이를 통해 객체에서 값을 동적으로 할당할 수 있습니다. 

윗부분은 객체 전개 연산자를 사용해 `searchKey`로 모든 `results`를 전파해야 합니다. 그렇지 않으면 이전에 저장한 모든 `results`가 손실됩니다. 

이제 검색어 별로 모든 `results`를 저장합시다. 먼저 캐시를 활성화합니다. 다음 단계는 변경되지 않는 `searchKey`에 따라 `results`를 검색합니다. 때문에 변경이 없는 변수로 `searchKey`를 사용했습니다. 그렇지 않으면 Search 컴포넌트를 사용할 때 값이 변경될 수 있으므로, 변경되는 `searchTerm`를 사용할 경우 검색이 중단되는 문제가 발생합니다.

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

`searchKey`로 찾은 결과가 없을 때, 목록이 비어있는 것이 초기 설정이기 때문에, Table 컴포넌트를 조건부 렌더링으로 처리할 수 있습니다. 이제 `searchTerm`이 아닌 `searchKey`를 "More"버튼에 전달하겠습니다. 이렇게 하지 않으면  `searchTerm`의 변경 값에 따라 페이지 매김 데이터를 가져오게 됩니다. Search 컴포넌트 내 입력 필드의`searchTerm` 프로퍼티는 유지합니다. 

검색 기능이 잘 작동해야 합니다. 해커 뉴스 API에서 받은 모든 결과를 저장합니다.

`onDismiss()` 메서드도 수정하겠습니다. 이 메서드는 `result` 객체를 다루는데, 여러 `results`를 다루게 해봅시다.


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

"Dismiss" 버튼이 잘 동작해야 합니다. 

그러나 각 검색 제출 시, API 요청을 방지하는 장치는 없습니다. 이미 `result`가 있는 경우 API가 요청되지 않도록 해야 합니다. 따라서 현재 캐시 함수는 완벽하지 않습니다. `results`를 캐시 하지만 실제 사용하지 않습니다. 캐시에서 `result`가 이미 있다면 API 요청을 보내지 않게 고쳐봅시다.


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

클라이언트는 검색어를 두 번 검색하게 되지만, 최종 API에는 한 번만 요청합니다. 페이지 매김 데이터도 같은 방법으로 캐시됩니다. 
`results`의 각 `result`에 대한 마지막 페이지를 항상 저장하기 때문입니다. 바로 이 접근 방법이 애플리케이션에 캐시를 도입하는 가장 좋은 방법입니다. 해커 뉴스 API로 페이지 매김 데이터를 제공한 덕분입니다.

## 오류 처리 Error Handling

해커 뉴스 API와 인터렉션을 위한 모든 준비가 끝났습니다. 개발 중 수없이 많은 오류를 만나게 됩니다. API 결과를 캐싱하고 페이지 매김 된 목록 기능을 시용하여 API에서 하위 목록을 끊임없이 가져올 수 있습니다. 그러나 아직 할 일이 남아 있습니다. 프로그램을 만들다 보면 수없이 많은 오류를 만나게 됩니다. 하지만 대부분 애플리케이션을 개발하면서 오류 처리를 간과하기도 합니다. 오류 처리가 무시한 개발은 정말 쉽습니다. 

이번 장에서는 API 요청 시, 효과적인 오류 처리 방법을 소개합니다. 이전 장에서 로컬 상태와 조건부 렌더링으로 오류 처리하는 방법을 배웠습니다. 기본적으로 오류는 리액트에서 또 다른 상태라고 볼 수 있습니다. 오류가 발생하면 로컬 상태로 저장하고 컴포넌트의 조건부 렌더링을 통해 보입니다. 이것이 전부입니다. App 컴포넌트에 작성하겠습니다. App 컴포넌트는 해커 뉴스 API로 데이터를 받는 곳이기 때문입니다.

첫째, 로컬 상태에 오류를 추가합시다. `error` 객체의 초기값은 `null`이 오류가 발생하면 그 값으로 설정됩니다.

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

둘째, 네이티브 fetch에서 catch 블록 안에 `setState()`메서드로 오류 객체를 로컬 상태로 저장합니다. API 요청이 성공하지 못하면 catch 블록이 실행됩니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  fetchSearchTopStories(searchTerm, page = 0) {
    fetch(`${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${searchTerm}&${PARAM_PAGE}${page}&${PARAM_HPP}${DEFAULT_HPP}`)
      .then(response => response.json())
      .then(result => this.setSearchTopStories(result))
# leanpub-start-insert
      .catch(e => this.setState({ error: e }));
# leanpub-end-insert
  }

  ...

}
~~~~~~~~

셋째, `render()` 내 로컬 상태 내 `error` 객체를 가져오고 조건문 렌더링으로 에러 메시지를 표시합니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
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

여기까지입니다. 오류 처기라 잘 작동하는지 테스트하려면 API URL을 존재하지 않는 URL로 바꿔서 테스트해보세요.

{title="src/App.js",lang=javascript}
~~~~~~~~
const PATH_BASE = 'https://hn.foo.bar.com/api/v1';
~~~~~~~~

애플리케이션 대신, 오류 메시지가 나타납니다. 원하는 곳에 조건부 렌더링으로 오류 메시지를 표시하면 됩니다. 이 경우 전체 앱이 보이지 않기 때문에, 사용자 경험에 문제가 있습니다. 그렇다면 Table 컴포넌트나 에러 메시지 둘 중 하나를 표시하면 어떨까요? 오류가 발생한 후에도 나머지 애플리케이션은 계속 표시됩니다.

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

URL을 기존 URL로 되돌리는 것을 잊지 마세요.

{title="src/App.js",lang=javascript}
~~~~~~~~
const PATH_BASE = 'https://hn.algolia.com/api/v1';
~~~~~~~~

애플리케이션이 잘 작동하는지 확인해봅시다. API 요청이 실패할 경우 오류 처리를 추가해야 합니다.

### 읽어보기

* [[reactjs.org] 리액트 컴포넌트 오류 처리](https://reactjs.org/blog/2017/07/26/error-handling-in-react-16.html)

{pagebreak}

앞으로 여러분은 리액트에서 API를 사용할 수 있습니다. 이번 장에서 배운 내용을 정리해봅시다.

* 리액트
  * ES6 클래스 컴포넌트의 생명주기 메소드와 사용 사례
  * componentDidMount() 메서드에서 API 호출
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
  * 클라이언트 및 서버 내 검색 기능
  * 데이터 페이지네이션
  * 클라이언트 렌더

실습 코드는 [깃허브 리퍼지토리](https://github.com/rwieruch/hackernews-client/tree/4.2)에서 확인할 수 있습니다.