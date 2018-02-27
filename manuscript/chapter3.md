# API 실제 사용 / Getting Real with an API

이번 장에서는 외부 API로 데이터를 요청하고 응답받은 데이터 리스트 결과를 보여주는 방법을 배우고 구현해보겠습니다. 

아직 API에 대해 잘 모른다면, 저자의 ['아무도 나에게 API를 알려주지 않았다'](https://www.robinwieruch.de/what-is-an-api-javascript/) 글을 먼저 읽고 오길 바랍니다. 

[해커 뉴스(Hacker News)](https://news.ycombinator.com/)는 기술 주제 관련 우수 기사를 큐레이션하는 사이트입니다. 이번 장부터 해커 뉴스에서 제공하는 API를 사용해 인기 기사 리스트를 보여주는 사이트를 만들어 볼 것입니다. 해커 뉴스는 [데이터 조회 API](https://github.com/HackerNews/API)와 [검색 API](https://hn.algolia.com/api)를 제공하고 있습니다. 시작하기 전 해커 뉴스 API 명세서를 읽고 데이터 구조를 파악 해두길 바랍니다.

## 생명주기 메서드 / Lifecycle Methods

외부 API를 사용하기 전, 컴포넌트 생명주기 메서드(Lifecycle Methods)에 대해 알아보겠습니다. 생명주기 메서드는 ES6 클래스 컴포넌트에서 사용하지만 비 상태 컴포넌트에서는 사용할 수 없습니다. 

이전 장에서 우리는 E6 클래스에 대해 배웠습니다. ES6 클래스 컴포넌트에서는 `render()` 외에도 다른 메서드가 있습니다. 바로 생명주기 메서드입니다.

우리는 이미 ES6 클래스 컴포넌트의 생명주기 메서드인 `constructor()` 와 `render()` 메서드를 사용해보았기 때문에 친숙하게 느껴질 것입니다. 

컴포넌트 인스턴스가 만들어져 DOM에 삽입될 때 생성자가 호출됩니다. 즉 컴포넌트가 인스턴스화 되는데, 이 프로세스를 컴포넌트가 탑재됐다(mounting)고 말합니다.

`render()` 메서드는 마운트 프로세스 중에도 호출되지만 컴포넌트가 업데이트될 때도 호출됩니다. 컴포넌트의 state와 props가 변경될 때마다 `render()` 메서드가 호출됩니다. 

본격적으로 각 생명주기 메서드에 대해 자세히 알아보겠습니다.

컴포넌트 마운트에는` componentWillMount()`와`componentDidMount()` 두 메서드가 있습니다. `constructor()`메서드가 호출되면, `componentWillMount()`메서드가 호출되고, 그 후 `render()` 메서드가 호출되며, 마지막에 `componentDidMount()`메서드가 호출됩니다. 

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

처음부터 모든 생명주기 메서드를 사용하지 않아도 됩니다. 대규모 애플리케이션에도 `constructor()`과 `render()` 메서드만 사용하는 경우가 많습니다. 앞으로 점차 익숙해질 것입니다. 다음으로 생명주기 메서드를 언제 어떻게 사용해야 하는지 알아봅시다. 

* **`constructor(props)`** 메서드는 컴포넌트 초기화 시 호출됩니다. 초기 컴포넌트 상태 및 클래스 메서드를 정의합니다. 

* **`componentWillMount()`** 메서드는 `render()` 메서드 전에 호출됩니다. 컴포넌트의 두 번째 렌더링을 트리거하지 않아 컴포넌트 상태를 설정하기 적합합니다. `constructor()`을 사용해 초기 상태를 설정하는 것이 좋습니다. 

* **`render()`** 메서드는 컴포넌트의 출력을 반환하기 때문에 반드시 사용해야 하는 메서드입니다. props 및 상태를 입력을 가져오고 요소를 반환합니다. 

* **`componentDidMount()`** 메서드는 컴포넌트가 마운트 될 때 한 번만 호출됩니다. 대부분 이 메서드 안에서 API를 통해 외부 데이터를 조회하고 가져오기 위한 비동기 요청을 보냅니다. 가져온 데이터는 내부 컴포넌트 상태에 저장되어 `render()` 메서드로 컴포넌트가 업데이트됩니다.

* **`componentWillReceiveProps(nextProps)`** 메서드는 업데이트 생명주기 동안 호출됩니다. `nextProps`는 다음 props를, `this.props`로 이전 props를 조회해 서로 차이를 비교합니다. `nextProps`를 컴포넌트의 state로 사용할 수 있습니다.
 
* **`shouldComponentUpdate(nextProps, nextState)`** 메서드는 props 또는 state가 변경되어 컴포넌트가 업데이트될 때 호출됩니다. 고도화된 리액트 애플리케이션에서 성능 최적화를 고려할 때 주로 사용됩니다. 이 메서드에서 반환하는 부울 값(boolean)에 따라 컴포넌트와 모든 자식이 업데이트 주기 동안 렌더링 되거나, 반대로 렌더링되지 않게 처리할 수 있습니다. 즉 문제가 되는 특정 컴포넌트의 생명주기 렌더링을 막을 수 있습니다.

* **`componentWillUpdate(nextProps, nextState)`** 메서드는 `render()` 메서드 전에 바로 호출됩니다. `nextProps`는 다음 props를, `nextState`로 다음 state를 조회합니다. `render()` 메서드가 실행되기 전에 상태를 업데이트할 수 있는 마지막 기회입니다. `render()` 메서드가 실행된 이후에는 `setState()` 메서드를 더 이상 사용할 수 없습니다. nextProps를 state 값으로 사용하려면 `componentWillReceiveProps()` 메서드를 사용합니다. 

* **`componentDidUpdate(prevProps, prevState)`** 메서드는 `render()` 메서드 후에 즉시 호출됩니다. DOM 조작을 하거나 추가 비동기 요청을 수행할 수 있습니다. 

* **`componentWillUnmount()`** 메서드는 컴포넌트를 해체하기 전에 호출됩니다. 테스트에서 초기화 작업을 수행할 수 있습니다.

`constructor()`와 `render()` ES6 클래스 컴포넌트에서 가장 많이 사용되는 생명주기 메서드입니다. `render()` 메서드는 컴포넌트 인스턴스를 반환하기 때문에 반드시 사용해야 합니다.

그 외 생명주기 메서드로 `componentDidCatch(error, info)`가 있습니다. 이 메서드는 [리액트 16](https://www.robinwieruch.de/what-is-new-in-react-16/)에서 처음 도입되었으며 컴포넌트 에러를 캐치합니다. 예를 들어 리스트를 보여주는 애플리케이션이 있다고 가정해봅시다. 외부 API 호출이 실패하여 state가 `null`로 표시되었습니다. 리스트가 비어있지 않고 값이 `null`이 때문에 filter과 map을 사용할 수 없습니다. 이 경우 오류가 발생하여 컴포넌트가 깨지고 전체 애플리케이션이 작동하지 않습니다. 이때, `componentDidCatch()`로 오류를 포착하고 오류 내용을 내부 상태에 저장하여 사용자에게 오류 메시지를 보여줄 수 있습니다.

### 읽어보기

* [[리액트 공식문서] 리액트 생명주기](https://reactjs.org/docs/react-component.html)
* [[리액트 공식문서] 리액트 생명주기 메서드와 상태 관리](https://reactjs.org/docs/state-and-lifecycle.html)
* [[리액트 공식문서] 컴포넌트 에러 핸들링](https://reactjs.org/blog/2017/07/26/error-handling-in-react-16.html)

## 데이터 가져오기 / Fetching Data

준비는 마쳤으니, 해커 뉴스 API로 데이터를 가져와봅시다. `componentDidMount()` 생명주기 메서드 안에 자바스크립트 네이티브 API인 `fetch()` 메서드를 사용해 외부 API를 호출합시다.

그전에 요청할 URL을 분절하여 상수(constants)와 기본 매개 변수(default parameters) 형태로 만들겠습니다.

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

ES6에서는 [템플릿 문자열](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Template_literals)로 문자열과 문자열을 연결합니다. 우리는 템플릿 문자열로 구성한 URL을 API 엔드 포인트에 연결할 것입니다.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// ES6
const url = `${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${DEFAULT_QUERY}`;

// ES5
var url = PATH_BASE + PATH_SEARCH + '?' + PARAM_SEARCH + DEFAULT_QUERY;

console.log(url);
// 출력: https://hn.algolia.com/api/v1/search?query=redux
~~~~~~~~

이렇게 하면 URL 구성 관리하기 편하다는 것을 알 수 있을 것입니다.

API 요청을 보내볼 차례입니다. 전체 데이터를 한꺼번에 가져올 것인데, 각 단계는 아래에 설명하겠습니다.

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

이 코드 속에 정말 많은 일들이 일어났습니다. 코드를 더 작은 조각으로 나눌까 싶었지만 그렇게 되면 각 조각 간 관계를 파악하기 어렵다고 판단했습니다. 이제 각 단계별로 일어난 일을 설명하겠습니다.

첫째, 해커 뉴스 API의 데이터가 사용됨으로 이전의 샘플 데이터는 더 이상 필요없기 때문에 제거했습니다. 컴포넌트 초기 상태는 결과 `result`가 비어있고, 검색어 `searchTerm`는 디폴트 값(`DEFAULT_QUERY`)으로 설정되어 있습니다. searchTerm은 Search 컴포넌트의 입력필드와 첫 번째 요청 시 사용됩니다.                                                        
둘째, 컴포넌트가 마운트된 이후에 데이터를 가져오기 위해 `componentDidMount()` 생명주기 메서드를 사용했습니다. 첫 번째 요청 시 로컬 상태의 기본 검색어가 사용됩니다. 기본 매개변수(`DEFAULT_QUERY`)값이 "redux"이기 때문에 "redux" 기사를 가져올 것입니다.

셋째, 자바스크립트 네이티브 fetch API를 사용했습니다. ES6 템플릿 문자열로 검색어인 `searchTerm` 변수와 함께 URL를 구성했습니다. 이 URL는 fetch API의 인자로 사용합니다. 응답받은 결과를 JSON 데이터 구조로 변환시키고, 마지막으로 컴포넌트 내부 상태 `result`에 저장됩니다. 요청 중 오류가 발생하면 `then()` 아닌 `catch()` 문이 실행됩니다. 오류 처리는 다음 장에서 배워볼 것입니다.

마지막으로 생성자에 컴포넌트 메서드인 `setSearchTopStories()` 를 바인딩 합니다.

이제 기존 샘플 데이터 대신 API로 가져온 외부 데이터를 사용할 수 있습니다. 데이터 리스트인 `result`는 [메타 정보와 인기 리스트가 같이 담겨 객체가 복잡하기 때문에](https://hn.algolia.com/api) 사용에 유의해야 합니다. `render()` 메서드 안에서 `console.log(this.state);`로 상태를 출력하여 개발자 도구에서 데이터 리스트를 확인 해보세요.

다음으로 조건에 따라 `result`의 렌더링 여부를 처리합시다. 먼저 초기에 `result` 값이 없는 경우 `null`을 반환합니다. API 요청이 성공하면 결괏값이 내부 상태 값에 저장되고 업데이트된 App 컴포넌트가 다시 렌더링 됩니다.

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

컴포넌트 생명주기 동안 일어나는 일을 설명해보겠습니다. 컴포넌트는 생성자로 초기화된 후 렌더링 됩니다. 내부 상태 `result` 값이 `null` 이므로 이 컴포넌트에 아무것도 표시되지 않습니다. 이후 `componentDidMount()` 메서드가 실행됩니다. 이 메서드에서 해커 뉴스 API 요청에 따라 비동기로 데이터를 가져옵니다. 데이터가 도착하면 `setSearchTopStories()` 메서드에서 내부 컴포넌트 상태를 업데이트합니다. 그다음부터 업데이트 생명주기가 시작됩니다. 컴포넌트는 `render()` 메서드를 다시 실행하지만, 이번에는 내부 상태 `result`에 값이 있기 때문에 리스트가 표시됩니다. 즉 App 컴포넌트와 Table 컴포넌트가 렌더링 됩니다. 

우리는 대부분 브라우저가 지원하는 `fetch()`로 비동기 요청을 수행했습니다. *create-react-app*은 모든 브라우저를 지원합니다. `fetch()` 대신 노드 패키지인 [superagent](https://github.com/visionmedia/superagent) 또는 [axios](https://github.com/mzabriskie/axios)를 사용할 수 있습니다. 

이 책은 자바스크립트 축약 표기법을 사용합니다. 이전 예제 코드 본 `if (!result)`는 `if (result === null)`과 같습니다. 예를 들어, `if (list.length === 0)` 대신 `if (!list.length)`을 사용을, `if (someString !== '')` 대신 `if (someString)`을 사용하겠습니다. 아직 축약 표기법에 대해 잘 모른다면 이 부분을 학습하고 돌아오길 바랍니다.

다시 애플리케이션으로 돌아갑시다. 인기 글 리스트가 보일 겁니다. 두 가지 버그가 생겼습니다. 첫째, "dismiss" 버튼입니다. 이 버튼은 새로 받은 객체를 식별하지 못하기 때문에 작동하지 않습니다. 둘째, 초기에 서버에서 검색한 기사 리스트를 가져왔지만, 이후 다른 검색어를 요청하면 클라이언트에서 리스트를 필터링합니다. Search 컴포넌트에서 API로 `result` 객체를 가져오도록 수정해야 합니다. 다음 장에서 이 문제들을 해결하겠습니다.


### 읽어보기

* [[MDN] ES6 템플릿 문자열](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Template_literals)
* [[MDN] the native fetch API](https://developer.mozilla.org/en/docs/Web/API/Fetch_API)
* [[저자 블로그] 리액트에서 데이터 호출하기](https://www.robinwieruch.de/react-fetching-data/)

## ES6 전개 연산자 / Spread Operators

"dismiss" 버튼을 클릭하면 아무것도 작동하지 않을 것입니다. 버튼의 `onDismiss()` 메서드는 요청받은 데이터 객체를 식별하지 못하고 있기 때문입니다. 지금은 전체 글 리스트만 조회할 수 있습니다. 리스트 내 각 객체를 식별할 수 있게 `onDismiss()` 메서드를 고쳐보겠습니다.

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

`setState()` 메서드 부분부터 설명하겠습니다. 인기 기사 리스트를 조회하려면 `result` 객체 내 `hits` 프로퍼티를 가져와야 합니다. 선택된 아이템은 `result` 개체에서 제거되며 리스트만 업데이트되고 다른 프로퍼티는 그대로 유지됩니다. 

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
// 출력: ['Robin', 'Andrew', 'Dan', 'Jordan']
~~~~~~~~

`allUsers` 변수는 완전히 새로운 배열입니다. `userList` 변수와 `additionalUser`변수의 값은 그대로 유지됩니다. 이런 식으로 두 배열을 새 배열로 병합 할 수 있습니다.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const oldUsers = ['Robin', 'Andrew'];
const newUsers = ['Dan', 'Jordan'];
const allUsers = [ ...oldUsers, ...newUsers ];

console.log(allUsers);
// 출력: ['Robin', 'Andrew', 'Dan', 'Jordan']
~~~~~~~~

다음으로 객체 스프레드 연산자(object spread operator)를 알아봅시다. ES6이 아닌, 리액트 커뮤니티에서 이미 사용하고 있는 [객체 스프레드 연산자(Object Rest/Spread Properties)](https://github.com/sebmarkbage/ecmascript-rest-spread)을 말합니다. *create-react-app* 역시 객체 스프레드 연산자를 도입했습니다.

ES6 배열 전개 연산자와 같지만 객체가 있다는 것이 다릅니다. 각 키(key)-값(value) 쌍을 새 객체에 복사합니다.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const userNames = { firstname: 'Robin', lastname: 'Wieruch' };
const age = 28;
const user = { ...userNames, age };

console.log(user);
// 출력: { firstname: 'Robin', lastname: 'Wieruch', age: 28 }
~~~~~~~~

배열 전개 예제와 같이 여러 객체를 펼칠 수 있습니다.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const userNames = { firstname: 'Robin', lastname: 'Wieruch' };
const userAge = { age: 28 };
const user = { ...userNames, ...userAge };

console.log(user);
// 출력: { firstname: 'Robin', lastname: 'Wieruch', age: 28 }
~~~~~~~~

따라서 `Object.assign()` 메서드를 대체해 사용할 수 있습니다.

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

`onDismiss()` 메서드가 잘 작동하는지 확인하세요. `onDismiss()` 메서드는 `hits` 리스트의 각 객체를 식별하여 해당 아이템을 제외된 리스트를 업데이트합니다.

### 읽어보기

* [[MDN] ES6 Object.assign()](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)
* [[MDN]ES6 배열 전개 연산자](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Spread_operator)

## 조건부 렌더링 / Conditional Rendering

이전 장에서 조건부 렌더링에 대해 간략하게 설명했습니다. 조건부 렌더링은 하나 또는 여러 컴포넌트와 요소의 렌더링 여부를 결정합니다. 특정 요소를 렌더링 하거나 렌더링 하지 게 처리할 수 있습니다. 앞에서 `if-else` 문으로 간단하게 작성했습니다.

컴포넌트 내부 상태 `result` 초기값은 `null` 입니다. App 컴포넌트에 API에서 `result`가 도착하지 않으면 아무것도 반환되지 않습니다. 이 것이 바로 조건부 렌더링입니다. `render()` 생명주기 메서드 전에 특정 조건에 따라 먼저 반환되었습니다. 따라서 App 컴포넌트는 아무것도 렌더링 되지 않다가, `result` 값이 업데이트 되면 렌더링 하게 됩니다. 

한 단계 더 나아가 봅시다. `result`에 의존하는 컴포넌트인 Table도 조건부 렌더링에 포함시키고, `result` 값이 없어도 모든 내용을 렌더링하게 처리하겠습니다. 이번에는 삼항 조건 연산자(ternary operator)를 사용해봅시다.

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

마지막으로 논리 연산자 `&&`를 사용해봅시다. 자바스크립트에서 `true &&'Hello World'` 경우 'Hello World'로 평가되고, `false && 'Hello World'`경우 항상 거짓으로 평가됩니다.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const result = true && 'Hello World';
console.log(result);
// 출력: Hello World

const result = false && 'Hello World';
console.log(result);
// 출력: false
~~~~~~~~

마찬가지로 리액트에서도 논리 연산자를 사용할 수 있습니다. 조건이 참이면, `&&` 논리 연산자 뒷부분 내용이 출력됩니다. 조건이 거짓이면 리액트는 표현식을 무시하고 이를 건너뜁니다. Table 컴포넌트를 반환 또는 아무것도 반환하지 않게 조건부 렌더링을 작성하겠습니다.

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

지금까지 조건부 렌더링 사용 방법에 대해 알아보았습니다. [저자의 블로그 글](https://www.robinwieruch.de/conditional-rendering-react/)에서 조건부 렌더링에 대한 더 많은 예제를 확인할 수 있습니다. 

애플리케이션에서 페치한 데이터 리스트를 모두 볼 수 있을 것입니다. 데이터 페치가 보류 중일 때를 제외하고 모든 기사 리스트가 표시됩니다. 전달받은 데이터를 해석해 로컬 상태로 저장하면 `render()` 메서드가 다시 실행되고, 조건부 렌더링의 조건에 따라 Table 컴포넌트가 표시되게 구현해봤습니다.

### 읽어보기

* [[리액트 공식문서] 리액트 조건 렌더링](https://reactjs.org/docs/conditional-rendering.html)
* [[저자 블로그] 조건문 렌더링의 다양한 방법](https://www.robinwieruch.de/conditional-rendering-react/)

## Search 컴포넌트의 클라이언트와 서버 처리 / Client-/Server-side Search

이번 장에서는 입력된 검색어에 따라 리스트가 정렬되게 만들어보겠습니다. 이 기능은 모두 클라이언트에서 처리됩니다. 첫 번째 API 요청은 `componentDidMount()` 메서드에서 기본 검색어 매개 변수를 사용해 요청을 보내겠습니다.

Search 컴포넌트에서 검색 제출 시, 해커 뉴스 API를 통해 데이터를 가져오게 하겠습니다. App 컴포넌트의 `onSearchSubmit()` 메서드를 아래와 같이 정의합시다. 

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

 `onSearchSubmit()` 메서드 역시 `componentDidMount()` 생명주기 메서드와 같이 API 페치 기능이 필요합니다. 차이점은 검색어 기본값이 아닌 수정된 입력 검색어를 사용한다는 것입니다. 따라서 이 기능을 떼어 재사용 가능한 메서드로 만들 수 있습니다. 이를 위해 `fetchSearchTopStories()` 메서드를 만들어 두 메서드 모두 사용할 수 있게 만듭시다. 
 
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

다음으로 Search 컴포넌트에 'Search' 버튼을 추가하겠습니다. 이 버튼을 클릭해 검색 요청을 보내고 해커 뉴스 API에서 데이터를 가져와봅시다.

물론 입력 필드 값이 변경 될 때마다 해커 뉴스 API에서 데이터를 가져오게 할 수도 있습니다. 이를 위해 `onClick()` 핸들러가 아닌 `onChange()` 핸들러를 사용할 수 있겠지만 구현이 꽤 복잡하기 때문에 사용하지 않겠습니다. 각 단계별로 하나씩 따라해봅시다.

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

둘째, Search 컴포넌트에 버튼을 추가합니다. 버튼은 `type="submit"`을 가지고, 폼은 `onSubmit()` 메서드를 전달하기 위해 `onSubmit()` 속성을 사용합니다. 자식 프로퍼티는 버튼 내용으로 사용하겠습니다.

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

Table 컴포넌트에서 클라이언트의 검색이 필요 없기 때문에 필터 기능을 제거하겠습니다. `isSearched()` 함수도 더 이상 사용하지 않기 때문에 제거합니다. 비로소 검색 버튼을 클릭하면 해커 뉴스 API를 통해 결괏값을 가져옵니다.

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

입력된 검색어로 해커 뉴스 기사를 검색할 수 있고 API가 완벽하게 작동하는지 확인해봅시다. 클라이언트 내 검색 처리가 없어야 합니다.

### 읽어보기

* [[리액트 공식문서] synthetic events in React](https://reactjs.org/docs/events.html)

### 실습하기

* [해커 뉴스 API](https://hn.algolia.com/api)로 이것저것 실험해봅니다.

## 페이지 매김 데이터 가져오기 / Paginated Fetch

반환되는 데이터 구조를 자세히 살펴보았나요? [해커 뉴스 API](https://hn.algolia.com/api)는 첫 번째 `hits` 리스트만 반환합니다. 처음 응답 시, `page` 프로퍼티 값은 `0`으로 페이지 번호를 말합니다. 이 프로퍼티 값으로 리스트을 가져옵니다. 검색어는 유지하고 다음 페이지 번호를 API에 전달하면 됩니다. 

앞에서 만든 URL 수정하겠습니다 이번에도 URL을 구성 가능한 형태로 만들겠습니다. 페이지 매김 데이터를 처리할 수 있게 요청할 URL를 쪼개어 각 상수로 만듭니다.

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

그리고 이 상수들을 매개변수로 추가해 URL를 구성합니다.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const url = `${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${searchTerm}&${PARAM_PAGE}`;

console.log(url);
// 출력: https://hn.algolia.com/api/v1/search?query=redux&page=
~~~~~~~~

`fetchSearchTopStories()` 메서드는 페이지 번호를 뜻하는 두 번째 인자를 추가하겠습니다. 두 번째 인자가 없으면 기본으로 `0` 값을 전달합니다. 따라서 `componentDidMount()` 메서드와 `onSearchSubmit()` 메서드는 최초 요청 시, 첫 번째 페이지를 가져옵니다. 추가 요청 시, 두 번째 인자를 통해 다음 페이지를 가져옵니다.

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

이제 `fetchSearchTopStories()` 메서드에서 API 응답을 받아 현재 페이지를 사용할 수 있습니다. 버튼에서 이 메서드를 사용해 `onClick` 버튼 핸들러로 더 많은 리스트을 가져와야 합니다. 다음으로 버튼을 클릭하면 그다음 페이지의 데이터를 가져오게 만들어봅시다. `onClick()` 핸들러에서 현재 검색어와 다음 페이지 번호(현재 페이지 + 1)만 정의하면 됩니다.

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

`render()` 메서드에서 `result`가 없을 때 페이지 번호 기본값은 `0`입니다. `render()` 메서드는 `componentDidMount()` 생명주기 메서드에서 비동기로 데이터를 가져오기 전에 실행됨을 잊지 맙시다. 

아직 한 가지 더 할 일이 남아있습니다. 다음 페이지의 데이터를 가져오지만, 이전 데이터를 덮어쓰게 되는 문제가 있습니다. 새 데이터를 정의하기보다 기존 상태 값에 새 결과 객체 리스트를 추가하는 것이 바람직합니다.

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

둘째, 이미 `hits`가 있는지 확인합니다. `page` 번호가 `0`일 때는 `componentDidMount()` 메서드 또는 `onSearchSubmit()` 메서드에서 새로운 검색 요청이 전달됩니다. `hits` 리스트는 비어있지만, "More" 버튼을 클릭하면 `0`이 아닌 다음 페이지를 요청합니다. 이전 `hits`는 이미 저장되어있으므로 사용 가능합니다. 

셋째, 이전 `hits`에 새로운 `hits`를 덮어쓰기 하면 안 됩니다. 따라서 두 리스트를 병합 해야 합니다. 이를 위해 ES6 배열 전개 연산자를 사용합니다. 

넷째, `hits`와 `hits`를 컴포넌트 내부 상태로 업데이트합니다. 

마지막으로 "More" 버튼 클릭할 때 일부 리스트만 가져오게 해봅시다. 각 요청에 따라 리스트를 가져올 수 있게 다시 URL를 수정하겠습니다. 각 경로를 상수로 만들어 URL를 구성합시다.

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

이제 상수를 사용해 URL를 조합합시다.

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

이전보다 더 많은 리스트를 가져오게 되었습니다. 이번 장에서는 해커 뉴스 API를 통해 실제 데이터를 사용해봤습니다. 새 프로그래밍 언어나 라이브러리를 학습할 때, 외부 API를 사용하면 더 재미있게 배울 수 있을 겁니다. 저도 이렇게 배웠으니까요.

### 실습하기

* [해커 뉴스 API](https://hn.algolia.com/api)의 매개변수를 바꿔 API를 요청해봅니다.

## 클라이언트 캐시 / Client Cache

"Search" 버튼을 클릭하면 해커 뉴스 API에 요청을 보냅니다. 예를 들어 "redux"를 검색하고, 그다음 "react"를, 그리고 다시 "redux"를 입력해 검색 요청을 보냈다고 가정해봅시다. 총 세 번 검색 요청을 보냈습니다. 이 중 "redux"를 두 번 요청했는데, 두 번 모두 데이터를 가져오기 위해 비동기 왕복 여행(asynchronous roundtrip)을 거치게 됩니다. 이 경우 클라이언트 캐시(client-sided cache)를 도입하는 것이 좋습니다. 캐시(Cache)란 미리 만들어진 데이터를 임시로 저장하는 공간입니다. 우리가 구현할 내용은 다음과 같습니다. 새 API 요청을 받으면, 이전에 동일한 요청이 있는지 먼저 확인합니다. 이미 캐시가 있으면 캐시를 사용하고, 캐시가 없다면 데이터를 가져오기 위해 API를 요청합니다. 이제 시작해보겠습니다.

각 결과마다 클라이언트 캐시를 보유하려면, 컴포넌트 내부 상태에 `result` 하나 보다, 여러 `results`를 저장하는 것이 좋습니다. `results` 객체는 키(key)가 검색어이고, 값(value)이 `hits`가 되게 하겠습니다. API 결괏값은 검색어(key)에 매핑되어 저장되게 만들겠습니다.

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

이제 `setState()` 메서드로 클라이언트 캐시를 구현하겠습니다. 첫째, 컴포넌트 내부 상태의 `result` 객체를 `results`로 변경합니다. 둘째, `searchKey`를 정의합니다. `searchKey`는 임시  키(key)로서 `result`를 저장하는데 쓰입니다.

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

다음으로 `searchKey`를 사용해 컴포넌트 내부 상태에 각 `result`를 저장하도록 수정합시다.

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

첫째, 컴포넌트 내부 상태에서 `searchKey`를 검색합니다. `searchKey`는 `componentDidMount()`와 `onSearchSubmit()`에 설정되어 있습니다.

둘째, 이전 `hits`는 새로 받은 `hits`와 합쳐집니다. 하지만 이번에는 `searchKey` 키로 `results`의 이전 `hits`를 가져옵니다.

셋째, 새`result`는 상태 `results`로 매핑됩니다. `setState()`에 있는 `results`객체를 살펴봅시다.

{title="src/App.js",lang=javascript}
~~~~~~~~
results: {
  ...results,
  [searchKey]: { hits: updatedHits, page }
}
~~~~~~~~

아래 부분은 `searchKey`에 의해 업데이트된 `results`를 저장합니다. 값은 `hits`와 `page` 프로퍼티가 있는 객체입니다. `searchKey`는 검색어입니다. 이미 여러분은 `[searchKey]:...` 구문을 배웠습니다. ES6를 통해 계산된 프로퍼티 이름인데, 이를 가지고 객체에서 값을 동적으로 할당할 수 있습니다. 

윗부분은 객체 전개 연산자를 사용해 `searchKey`로 모든 `results`를 전파해야 합니다. 그렇지 않으면 이전에 저장한 모든 `results`가 손실됩니다. 

이제 검색어 별로 모든 `results`를 저장하게 만들겠습니다. 먼저 캐시를 활성화합니다. 그리고 변경되지 않는 `searchKey`에 따라 `results`를 검색합니다. `searchKey`를 사용하지 않으면 Search 컴포넌트를 사용할 때 값이 변경될 수 있습니다. 값이 변경되는 `searchTerm`를 사용할 경우 검색이 중단되는 문제가 발생합니다.

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

`searchKey`에 해당된 결과가 없을 때, `list`는 비어있는 배열로 설정했습니다. 때문에 Table 컴포넌트를 조건부 렌더링으로 처리할 수 있습니다. 이제 `searchTerm`이 아닌 `searchKey`를 "More" 버튼에 전달하겠습니다. 이렇게 하지 않으면  `searchTerm`의 변경 값에 따라 페이지 매김 데이터를 가져오게 됩니다. Search 컴포넌트 내 입력 필드의`searchTerm` 프로퍼티는 유지합니다. 

검색 기능이 잘 작동하는지 확인하세요. 해커 뉴스 API에서 받은 데이터를 잘 저장되는지 확인하세요.

`onDismiss()` 메서드도 개선해야 합니다. 현재 이 메서드는 `result` 객체가 아닌, 여러 `results`를 다루도록 수정하겠습니다.

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

그러나 검색 요청 시, API 요청을 막는 장치가 없습니다. 이미 동일한 결과가 있을 때 API 요청을 방지하는 검사를 하지 않습니다. 따라서 캐시 기능은 아직 완벽하지 않습니다. `results`는 캐시에 저장되지만 이를 사용하지 않았습니다. 마지막으로 캐시에서 `results`를 사용할 수 있을 때, API 요청을 막도록 수정하겠습니다.

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

이제 클라이언트는 동일한 검색어를 두 번 검색하더라도, 최종적으로 API 요청은 한 번만 요청하게 됩니다. 각 결과의 마지막 페이지가 `results`에 저장하기 때문에 페이지 매김 데이터도 같은 방법으로 캐시됩니다.

## 오류 처리 / Error Handling

해커 뉴스 API와 인터렉션을 위한 모든 준비가 끝났습니다. 앞 장에서 API 결과를 캐싱했으며 페이지 매김 된 리스트 기능을 구현해 API를 통해 더 많은 기사 리스트를 계속 가져왔습니다. 그러나 아직 할 일이 더 남아 있습니다. 프로그램을 만들다 보면 수없이 많은 오류를 만나게 됩니다. 하지만 대부분 애플리케이션을 개발하면서 오류 처리를 간과할 것입니다. 오류 처리를 무시하고 개발하는 것은 정말 쉽기 때문이지요.

이번 장에서는 API 요청 시, 효과적인 오류 처리 방법에 대해 알아보겠습니다. 이전 장에서 로컬 상태와 조건부 렌더링으로 오류 처리하는 방법을 배웠습니다. 기본적으로 오류는 리액트에서 또 다른 상태라고 볼 수 있습니다. 오류가 발생하면 로컬 상태로 저장하고 컴포넌트의 조건부 렌더링을 통해 표시됩니다. App 컴포넌트에 오류를 처리하겠습니다. App 컴포넌트는 해커 뉴스 API로 데이터를 받는 곳이기 때문입니다. 그럼 이제 시작해봅시다.

첫째, 로컬 상태에 오류를 추가합니다. `error` 객체의 초기값은 `null`이 오류가 발생하면 그 값으로 설정됩니다.

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

둘째, fetch API의 `catch()` 블록 안에 `setState()`메서드로 오류 객체를 로컬 상태로 저장합니다. API 요청이 성공하지 못하면 catch 블록이 실행됩니다.

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

셋째, `render()` 내 로컬 상태 내 `error` 객체를 가져오고 조건문 렌더링으로 에러 메시지를 표시합니다.

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

여기까지입니다. 오류 처기라 잘 작동하는지 테스트하려면 API URL을 존재하지 않는 URL로 바꿔서 테스트해보세요.

{title="src/App.js",lang=javascript}
~~~~~~~~
const PATH_BASE = 'https://hn.foo.bar.com/api/v1';
~~~~~~~~

애플리케이션 대신, 오류 메시지가 나타납니다. 원하는 곳에 조건부 렌더링으로 오류 메시지를 표시하면 됩니다. 이 경우 전체 앱이 보이지 않습니다. 그렇다면 Table 컴포넌트 또는 오류 메시지 둘 중 하나를 표시하면 어떨까요? 오류가 발생해도 나머지 애플리케이션은 표시되기 때문에 사용자에게 혼란을 주지 않습니다.

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

URL을 원래대로 되돌리는 것을 잊지 마세요.

{title="src/App.js",lang=javascript}
~~~~~~~~
const PATH_BASE = 'https://hn.algolia.com/api/v1';
~~~~~~~~

애플리케이션이 잘 작동하는지 확인하세요. 

### 읽어보기

* [[리액트 공식문서] 리액트 컴포넌트 오류 처리](https://reactjs.org/blog/2017/07/26/error-handling-in-react-16.html)

{pagebreak}

## Fetch 대신 Axios 사용하기 / Axios instead of Fetch

이전 장에서 fetch API로 해커 뉴스 데이터를 받아왔습니다. 일반 브라우저에서는 fetch API를 사용할 수 있으나, 모든 브라우저가 지원하는 것은 아닙니다. 특히 오래된 브라우저는 fetch API를 지원하지 않습니다. 특히 헤드리스 브라우저(Headless Browser)에서 애플리케이션을 테스트할 때, fetch API와 관련된 문제가 발생할 수 있습니다. 헤드리스 브라우저는 실제 애플리케이션이 구동되는 브라우저가 아닌 테스트용으로 테스트 자동화에 사용되는 브라우저입니다. 물론 구버전 브라우저(폴리필)와 테스트([isomorphic-fetch](https://github.com/matthew-andrews/isomorphic-fetch))에서 fetch API를 사용할 수 있는 방법이 있지만, 이 책에서는 이 내용을 다루지 않겠습니다.

이처럼 브라우저 간 문제와 API의 한계를 해결하고자 기존 fetch API를 대체해 [액시오스(axios)](https://github.com/axios/axios) 라이브러리를 도입할 수 있습니다. axios는 많은 리액트 개발자들이 사용하고 있는 비동기 API 라이브러리입니다. 이번 장에서는 axios 라이브러리를 도입하는 방법과 웹 개발의 고질적인 문제점인 구버전 브라우저 헤드리스 브라우저 테스트의 해결 방법에 대해 알아보겠습니다.

먼저 fetch API를 axios로 바꾸겠습니다.

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

지금부터 `fetch()` 메서드 대신 `axios()` 메서드를 사용하겠습니다. 사용법은 fetch API와 거의 동일합니다. 인자로 URL을 가져와 프로미스(promise)를 반환합니다. 반환된 응답을 JSON으로 변환할 필요가 없습니다. axios는 자바스크립트에서 `data` 객체로 결과를 래핑 합니다. 따라서 반환된 데이터 구조를 그대로 사용하면 됩니다.

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

이번 장에서 fetch 대신 axios를 사용했습니다. `axios()` 메서드는 디폴트로 HTTP GET 요청을 보냅니다. 좀더 명확하게 표현하려면 `axios.get()` 메서드를 사용할 수 있습니다. HTTP POST 요청은 `axios.post()` 메서드를 사용합니다. 이와같이 axios는 간단한 코드로 원격 API 요청을 훌륭하게 수행합니다. 기존 fetch API는 복잡했고 프로미스의 단점을 처리해야 했기 때문에, axios를 사용하는 것이 훨씬 좋습니다. 앞으로 애플리케이션 테스트도 배울 것입니다. axios를 도입하면 특정 브라우저나 헤드리스 브라우저 환경 문제를 해결할 수 있으니 더 편하고 쉽게 개발할 수 있습니다.

App 컴포넌트에서 해커 뉴스 API 요청 시, 개선할 점이 하나 더 있습니다. 페이지가 브라우저에서 처음 렌더링 될 때 컴포넌트가 마운트됩니다.  예를 들어 `componentDidMount()` 생명주기 메서드에서 컴포넌트가 요청을 시작하지만, 애플리케이션에서 다른 네비게이션이 있어 다른 페이지로 이동한다고 가정해봅시다. 앱 컴포넌트가 마운트 해제되었지만 여전히 `componentDidMount()` 메서드에서는 보류 중인 요청이 존재합니다. 따라서 `then()` 또는 `catch()`에서 `this.setState()` 메서드를 사용해야 합니다. 이 경우 브라우저 개발자 도구나 커맨드 라인에서 아래와 같은 경고가 표시됩니다.

{title="Command Line",lang="text"}
~~~~~~~~
Warning: Can only update a mounted or mounting component. This usually means you called setState, replaceState, or forceUpdate on an unmounted component. This is a no-op.
~~~~~~~~

이 경고 문구를 삭제해야 합니다. 해결 방법은 컴포넌트가 마운트 해제될 때 요청을 중단하거나, 마운트 해제된 컴포넌트에서 `this.setState()` 메서드 호출을 방지하는 것입니다. 리액트 개발에서는 경고 메시지가 없게 깨끗한 애플리케이션을 유지하는 것이 매우 중요합니다. 현재 promise API에서 요청이 중단되지 않기 때문에 별도로 처리해야 합니다. 아래 제가 소개하는 방법은 많은 개발자들이 따르고 있는 모범 사례입니다. 마운트 되지 않은 컴포넌트 때문에 나타나는 경고 메시지 내용을 해결할지, 그대로 내버려 둘지는 개발자가 판단해야할 문제입니다. 이 책의 후반부에도 비슷한 경고 메시지가 나올 때마다 아래 방법으로 해결하면 됩니다.

해결법은 다음과 같습니다. 컴포넌트 생명주기 상태를 가리키는 클래스 필드인   `_isMounted`을 만들겠습니다. 컴포넌트가 초기화될 때, 초기값은 `false`입니다. 컴포넌트가 마운트 될 때 `true`로 변경되며, 컴포넌트가 마운트 해제될 때 다시 `false`로 변경됩니다. 이렇게 하면 컴포넌트 생명주기를 추적할 수 있게 됩니다. 리액트 로컬 상태에 의존하지 않고 컴포넌트 인스턴스에서 직접 접근할 수 있어 `this.state`와 `this.setState()`로 저장 및 수정된 로컬 상태와 관련이 없습니다. 따라서 클래스 필드 값이 변경되어도 컴포넌트는 다시 렌더링 되지 않습니다.

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

마지막으로 요청 자체를 중단하지 않고 컴포넌트가 이미 마운트 해제되었다면 `this.setState()` 메서드 호출을 피할 수 있습니다. 이를 통해 경고 메시지를 피할 수 있습니다.

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

이번 장에서는 리액트에서 다른 라이브러리로 대체하는 방법을 배웠습니다. 자바스크립트 생태계에는 많은 라이브러리들이 있습니다. 개발 중 문제가 생기면, 여러 라이브러리를 탐색하고 적합한 솔루션을 도입해 해결할 수 있습니다. 이외에 마운트 해제된 컴포넌트에서 `this.setState()` 메서드 호출을 피하는 방법을 배웠습니다. axios 라이브러리를 더 깊이 파고 들면, 첫 번째 API 요청 취소를 막는 방법을 알게 될 것입니다. 추후 이 내용에 대해 스스로 찾아보고 학습하길 바랍니다.

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