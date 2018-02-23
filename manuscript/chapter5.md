# 심화: 컴포넌트 Advanced React Components

이번 장에서는 리액트 컴포넌트 심화 내용을 다룹니다. 고차 컴포넌트(higher order components)를 학습한 후 실제 구현을 해볼 것입니다. 리액트 고급 주제와 복잡한 인터렉션도 함께 알아봅시다.

## Ref와 DOM Refs and the DOM

종종 리액트에서 DOM 노드를 다룰 일이 있습니다. `ref` 속성으로 DOM 노드를 접근할 수 있습니다. 리액트의 단방향 데이터 흐름과 선언 방식에 위배되기 때문에 안티 패턴(anti pattern)이라고도 합니다. 검색 입력 필드를 구현했을 때, 잠시 소개헀습니다. 리액트 공식 문서에는 세 가지 사용 사례가 소개되어있습니다.

* DOM API (focus, media playback 등)을 사용하는 경우
* 명령형 노드 애니메이션을 호출하하는 경우
* DOM 노드가 필요한 라이브러리(예: [D3.js](https://d3js.org/))를 사용하는 경우

Search 컴포넌트를 예제로 살펴보겠습니다. 애플리케이션이 처음 렌더링 될 때 input 필드를 포커스(focus) 하기 위해 DOM API를 필요로 합니다. 이 부분에 대해 자세히 설명하겠지만 우리가 만들고 있는 애플리케이션에는 실질적인 도움은 되지 않으므로 이후 변경 사항은 생략하겠습니다. 

일반적으로 함수형 비 상태 컴포넌트와 ES6 클래스 컴포넌트 모두 `ref` 속성을 사용할 수 있습니다. 포커스 사용을 위해 생명주기 메서드가 필요합니다. ES6 클래스 컴포넌트와 함께 `ref` 속성을 사용하여 input 필드에 포커스 하게 만들어봅시다. 

먼저 함수형 비 상태 컴포넌트를 ES6 클래스 컴포넌트로 리팩터링 하겠습니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
class Search extends Component {
  render() {
    const {
      value,
      onChange,
      onSubmit,
      children
    } = this.props;

    return (
# leanpub-end-insert
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
# leanpub-start-insert
    );
  }
}
# leanpub-end-insert
~~~~~~~~

ES6 클래스 컴포넌트의 `this` 객체는 `ref` 속성으로 DOM 노드를 참조할 수 있습니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
class Search extends Component {
  render() {
    const {
      value,
      onChange,
      onSubmit,
      children
    } = this.props;

    return (
      <form onSubmit={onSubmit}>
        <input
          type="text"
          value={value}
          onChange={onChange}
# leanpub-start-insert
          ref={(node) => { this.input = node; }}
# leanpub-end-insert
        />
        <button type="submit">
          {children}
        </button>
      </form>
    );
  }
}
~~~~~~~~

이제 input 필드에 포커스를 주었고 마운트된 컴포넌트에 `this` 객체, 생명주기 메소드와 DOM API를 사용할 수 있습니다.


{title="src/App.js",lang=javascript}
~~~~~~~~
class Search extends Component {
# leanpub-start-insert
  componentDidMount() {
    this.input.focus();
  }
# leanpub-end-insert

  render() {
    const {
      value,
      onChange,
      onSubmit,
      children
    } = this.props;

    return (
      <form onSubmit={onSubmit}>
        <input
          type="text"
          value={value}
          onChange={onChange}
          ref={(node) => { this.input = node; }}
        />
        <button type="submit">
          {children}
        </button>
      </form>
    );
  }
}
~~~~~~~~

애플리케이션이 렌더링될 때 input 필드에 포커스를 주어야 합니다. 이를 위해 기본적으로 `ref` 속성을 사용합니다.

그러나 `this` 객체가 없는 함수형 비상태 컴포넌트는 `ref`를 어떻게 접근할 수 있을까요? 아래 함수형 비상태 컴포넌트를 살펴보겠습니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
const Search = ({
  value,
  onChange,
  onSubmit,
  children
}) => {
# leanpub-start-insert
  let input;
# leanpub-end-insert
  return (
    <form onSubmit={onSubmit}>
      <input
        type="text"
        value={value}
        onChange={onChange}
# leanpub-start-insert
        ref={(node) => input = node}
# leanpub-end-insert
      />
      <button type="submit">
        {children}
      </button>
    </form>
  );
}
~~~~~~~~

이제 DOM 요소에 접근할 수 있습니다. 포커스 예제의 경우 별다른 도움이 되지 않을 겁니다. 함수형 비 상태 컴포넌트에서 포커스를 트리거하는 생명주기 메서드가 없기 때문입니다. 그러나 가까운 미래에는 `ref` 속성을 가진 함수형 비 상태 컴포넌트가 가능할지도 모르겠습니다.

### 읽어보기

* [[리액트 공식문서] the ref attribute in general in React](https://reactjs.org/docs/refs-and-the-dom.html)
* [[리액트 공식문서] the usage of the ref attribute in React](https://www.robinwieruch.de/react-ref-attribute-dom-node/)

## 로딩 컴포넌트 Loading Component

애플리케이션으로 다시 돌아가 봅시다. 해커 뉴스 API로 검색 요청하는 동안 로딩 인디케이터를 보여주고 싶을 겁니다. 비동기 요청임으로 사용자에게 그동안 기다릴 것을 알려줘야 합니다. *src/App.js* 파일에서 재사용 가능한 Loading 컴포넌트를 만듭시다.

{title="src/App.js",lang=javascript}
~~~~~~~~
const Loading = () =>
  <div>Loading ...</div>
~~~~~~~~

다음 로딩 상태를 저장하는 프로퍼티가 필요합니다. 로딩 상태에 따라 컴포넌트가 보여집니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  constructor(props) {
    super(props);

    this.state = {
      results: null,
      searchKey: '',
      searchTerm: DEFAULT_QUERY,
      error: null,
# leanpub-start-insert
      isLoading: false,
# leanpub-end-insert
    };

    ...
  }

  ...

}
~~~~~~~~

`isLoading` 프로퍼티의 초기 값은 `false`입니다. App 컴포넌트가 마운트되기 전에는 로딩하지 않습니다.

요청할 때 로딩 상태 값을 `true`로 바꾸고, 요청이 성공되면 다시 `false`로 바꿉니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  setSearchTopStories(result) {
    ...

    this.setState({
      results: {
        ...results,
        [searchKey]: { hits: updatedHits, page }
      },
# leanpub-start-insert
      isLoading: false
# leanpub-end-insert
    });
  }

  fetchSearchTopStories(searchTerm, page = 0) {
# leanpub-start-insert
    this.setState({ isLoading: true });
# leanpub-end-insert

    fetch(`${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${searchTerm}&${PARAM_PAGE}${page}&${PARAM_HPP}${DEFAULT_HPP}`)
      .then(response => response.json())
      .then(result => this.setSearchTopStories(result))
      .catch(e => this.setState({ error: e }));
  }

  ...

}
~~~~~~~~

마지막으로 App 컴포넌트 안에 Loading 컴포넌트를 사용하겠습니다. 조건부 렌더링으로 로딩 상태에 따라 Loading 컴포넌트 또는 Button 컴포넌트 둘 중 하나를 보여줍니다. Button 컴포넌트는 더 많은 데이터를 가져오는 버튼입니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    const {
      searchTerm,
      results,
      searchKey,
      error,
# leanpub-start-insert
      isLoading
# leanpub-end-insert
    } = this.state;

    ...

    return (
      <div className="page">
        ...
        <div className="interactions">
# leanpub-start-insert
          { isLoading
            ? <Loading />
            : <Button
              onClick={() => this.fetchSearchTopStories(searchKey, page + 1)}>
              More
            </Button>
          }
# leanpub-end-insert
        </div>
      </div>
    );
  }
}
~~~~~~~~

초기에 `componentDidMount()` 메서드에서 요청하기 때문에 애플리케이션을 시작할 때 Loading 컴포넌트가 표시됩니다. 리스트가 비어있기 때문에 Table 컴포넌트는 보이지 않습니다. 해커 뉴스 API에서 응답을 받으면 `result`가 표시되고 `isLoading` 상태는 `false`로 설정되며 Loading 컴포넌트는 사라지고 "More" 버튼이 나타납니다. 데이터를 한번 더 가져오면, 이 버튼이 사라지고 다시 Loading 컴포넌트가 보입니다.

### 실습하기

* "Loading ..."  텍스트 대신 [Font Awesome](http://fontawesome.io/) 라이브러리를 사용해 로딩 아이콘을 보여줍니다.

## 고차 컴포넌트 Higher Order Components
  
고차 컴포넌트(HOCs: Higher order components)는 리액트의 고급 주제입니다. HOCs는 고차 함수와 동일한 개념입니다. HOCs는 함수와 같이 입력, 선택적 인자를 받아 컴포넌트를 출력합니다. 반환된 컴포넌트는 입력된 컴포넌트로 JSX에서 사용됩니다. 

HOCs는 여러 사용 사례가 있습니다. 프로퍼티 준비, 상태 관리, 컴포넌트 표시 변경을 할 수 있습니다. 또는 조건부 렌더링에서 HOCs를 헬퍼로 사용할 수 있습니다. 예를 들어 List 컴포넌트가 있는데, 이 컴포넌트는 리스트를 반환하거나 리스트가 없거나 null일 경우 아무것도 반환하지 않는다고 합니다. HOCs는 리스트가 없을 때 렌더링 하지 못하게 할 수 있습니다. 이렇게 하면 List 컴포넌트는 리스트가 없을 경우를 체크하지 않아도 됩니다. 그저 List 컴포넌트는 리스트를 렌더링 하는 역할에만 충실하면 됩니다. 

컴포넌트를 입력으로 사용하고, 컴포넌트를 반환하는 간단한 HOC를 만들어봅시다. *src/App.js* 파일에서 작성하면 됩니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
function withFoo(Component) {
  return function(props) {
    return <Component { ...props } />;
  }
}
~~~~~~~~

관례로 HOC 변수명 앞에 `with` 접두어를 붙입니다. ES6 화살표 함수로 HOC를 간결하게 작성합시다.

{title="src/App.js",lang=javascript}
~~~~~~~~
const withFoo = (Component) => (props) =>
  <Component { ...props } />
~~~~~~~~

이 예제에서 입력 컴포넌트는 출력 컴포넌트와 동일하게 유지됩니다. 아무 일도 일어나지 않습니다. 동일한 컴포넌트 인스턴스를 렌더링하고 모든 props를 출력 컴포넌트로 전달합니다. 그러나 유용하지 않습니다. 출력 컴포넌트를 개선해봅시다. 출력 컴포넌트는 로딩 상태가 true일 때 Loading 컴포넌트를 보여주고, 그렇지 않을 때 입력 컴포넌트를 보여줘야 합니다. 조건부 렌더링에서 HOC을 사용하는 것이 좋습니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
const withLoading = (Component) => (props) =>
  props.isLoading
    ? <Loading />
    : <Component { ...props } />
# leanpub-end-insert
~~~~~~~~

로딩 프로퍼티에 따라 조건부 렌더링을 적용할 수 있습니다. 이 함수는 Loading 컴포넌트 또는 입력 컴포넌트를 반환합니다.

일반적으로 위 예제처럼 props와 같이 컴포넌트를 입력과 같이 전파하는 것이 매우 효과적입니다. 아래 코드를 보고 서로 차이점을 비교해보세요.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// props를 전달하기 전 구조 해체를 해야합니다.
const { foo, bar } = props;
<SomeComponent foo={foo} bar={bar} />

// 객체 전개 연산자를 사용해 모든 객체 프로퍼티를 전달합니다.
<SomeComponent { ...props } />
~~~~~~~~

`isLoading`를 포함한 모든 props를 객체를 전개해서 입력 컴포넌트에 전달했습니다. 그러나 입력 컴포넌트는 `isLoading` 프로퍼티를 신경 쓰지 않습니다. 이를 피하기 위해 ES6 구조 해체를 사용합니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
const withLoading = (Component) => ({ isLoading, ...rest }) =>
  isLoading
    ? <Loading />
    : <Component { ...rest } />
# leanpub-end-insert
~~~~~~~~

객체에서 프로퍼티 하나를 가져오지만 나머지 객체는 유지됩니다. 또한 여러 프로퍼티와 함께 사용됩니다. 이미 여러분들은 [[MDN] 구조 해체 할당](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment) 부분을 읽었을 겁니다.

이제 JSX에서 HOC를 사용해봅시다. 우리는 "More" 버튼 또는 Loading 컴포넌트 둘 중 하나를 보여줄 겁니다. Loading 컴포넌트는 HOC에 캡슐화되어있지만 입력 컴포넌트는 빠져있습니다. 입력 컴포넌트를 Button 컴포넌트로, 출력 컴포넌트는 ButtonWithLoading 컴포넌트로 추가합시다.

{title="src/App.js",lang=javascript}
~~~~~~~~
const Button = ({ onClick, className = '', children }) =>
  <button
    onClick={onClick}
    className={className}
    type="button"
  >
    {children}
  </button>

const Loading = () =>
  <div>Loading ...</div>

const withLoading = (Component) => ({ isLoading, ...rest }) =>
  isLoading
    ? <Loading />
    : <Component { ...rest } />

# leanpub-start-insert
const ButtonWithLoading = withLoading(Button);
# leanpub-end-insert
~~~~~~~~

마지막으로 ButtonWithLoading 컴포넌트가 loading 상태를 프로퍼티로 가져오게 만듭시다. HOC가 loading 프로퍼티를 사용하는 동안, 나머지 props 전체는 Button 컴포넌트로 전달됩니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    ...
    return (
      <div className="page">
        ...
        <div className="interactions">
# leanpub-start-insert
          <ButtonWithLoading
            isLoading={isLoading}
            onClick={() => this.fetchSearchTopStories(searchKey, page + 1)}>
            More
          </ButtonWithLoading>
# leanpub-end-insert
        </div>
      </div>
    );
  }
}
~~~~~~~~

테스트를 다시 실행하면, App 컴포넌트의 스냅샷 테스트가 실패함을 알 수 있습니다. 커맨드 라인에서 diff가 아래와 같이 보일 것입니다.

{title="Command Line",lang="text"}
~~~~~~~~
-    <button
-      className=""
-      onClick={[Function]}
-      type="button"
-    >
-      More
-    </button>
+    <div>
+      Loading ...
+    </div>
~~~~~~~~

잘못되었다고 판단되면 지금 바로 컴포넌트를 수정할 수 있습니다. 또는 스냅샷을 수락할 수 있습니다. Loading 컴포넌트를 만들었기 때문에 커맨드 라인에서 변경된 스냅샷 테스트를 수락할 수 있습니다. 

고차 컴포넌트는 리액트에서 고급 테크닉입니다. 고차 컴포넌트는 컴포넌트의 재사용성 향상, 강화된 추상화, 컴포넌트의 조합과 props, 상태 및 뷰 조작 등 여러 목적을 가지고 있습니다. 지금 모든 것을 이해하지 못한다고 해도 걱정하지 않길 바랍니다. 익숙해지기까지 시간이 걸립니다. 

저자가 쓴 [고차 컴포넌트 시작하기](https://www.robinwieruch.de/gentle-introduction-higher-order-components/) 글을 읽어보길 바랍니다. 고차 컴포넌트를 배우는 방법을 설명하고 함수형 프로그래밍과 조건부 렌더링 내 고차 컴포넌트 문제를 해결할 수 있는 방법을 소개했습니다.

### 읽어보기

* [[저자 블로그] 고차 컴포넌트 시작하기](https://www.robinwieruch.de/gentle-introduction-higher-order-components/)

### 실습하기
* HOC를 실험해봅니다.
* 그 외 더 많은 HOC 사용 사례를 생각해봅시다.
  * 직접 HOC를 구현해봅시다.

## 심화: 정렬 Advance Sorting

클라이언트와 서버 모두 검색 인터렉션을 다뤄봤습니다. Table 컴포넌트로 좀 더한층 심화된 인터렉션을 다뤄보겠습니다. Table 컴포넌트의 각 첫 행에 라벨을 추가하고 이를 활용해 정렬 기능을 만들어보겠습니다. 

저는 정렬 기능 구현 시, 직접 함수를 만드는 것보다 유틸리티 라이브러리를 사용하는 것을 선호합니다. 그중 가장 대표적인 유틸리티 라이브러리인 [로대쉬(Lodash)](https://lodash.com/)이며 다른 라이브러리를 사용해도 좋습니다. 먼저 로대쉬를 설치합시다.

{title="Command Line",lang="text"}
~~~~~~~~
npm install lodash
~~~~~~~~

그리고 *src/App.js* 파일에서 로대쉬의 정렬 함수를 가져옵니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
import React, { Component } from 'react';
import fetch from 'isomorphic-fetch';
# leanpub-start-insert
import { sortBy } from 'lodash';
# leanpub-end-insert
import './App.css';
~~~~~~~~

Table 컴포넌트에는 title, author, comments, points 열이 있습니다. 정렬 함수는 특정 프로퍼티에 따라 정렬된 리스트를 반환합니다. 또한 정렬되지 않는 리스트를 반환하는 기본 정렬 함수가 필요합니다. 기본 정렬 함수는 초기 상태 값이 됩니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
...

# leanpub-start-insert
const SORTS = {
  NONE: list => list,
  TITLE: list => sortBy(list, 'title'),
  AUTHOR: list => sortBy(list, 'author'),
  COMMENTS: list => sortBy(list, 'num_comments').reverse(),
  POINTS: list => sortBy(list, 'points').reverse(),
};
# leanpub-end-insert

class App extends Component {
  ...
}
...
~~~~~~~~

두 개의 정렬 함수가 역순으로 나열되었습니다. 처음 리스트를 정렬할 때, 댓글 수와 점수가 높은 순으로 정렬됩니다. 

`SORTS`객체로 정렬 함수를 사용합니다.

App 컴포넌트는 정렬 상태를 저장합니다 초기 상태는 초기 정렬 함수이며, 정렬되지 않은 입력 리스트를 반환합니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
this.state = {
  results: null,
  searchKey: '',
  searchTerm: DEFAULT_QUERY,
  error: null,
  isLoading: false,
# leanpub-start-insert
  sortKey: 'NONE',
# leanpub-end-insert
};
~~~~~~~~


다른 `sortKey`로 `AUTHOR` 키를 선택해봅시다, `SORTS` 객체의 `AUTHOR` 함수를 통해 리스트를 정렬합니다.

다음 App 컴포넌트에서 새로운 클래스 메서드를 만들어봅시다. 이 메서드는 `sortKey`로 정렬 함수를 찾아 리스트에 적용합니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  constructor(props) {

    ...

    this.needsToSearchTopStories = this.needsToSearchTopStories.bind(this);
    this.setSearchTopStories = this.setSearchTopStories.bind(this);
    this.fetchSearchTopStories = this.fetchSearchTopStories.bind(this);
    this.onSearchSubmit = this.onSearchSubmit.bind(this);
    this.onSearchChange = this.onSearchChange.bind(this);
    this.onDismiss = this.onDismiss.bind(this);
# leanpub-start-insert
    this.onSort = this.onSort.bind(this);
# leanpub-end-insert
  }

# leanpub-start-insert
  onSort(sortKey) {
    this.setState({ sortKey });
  }
# leanpub-end-insert

  ...

}
~~~~~~~~

그리고 Table 컴포넌트에 메서드와 `sortKey`을 전달합시다.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    const {
      searchTerm,
      results,
      searchKey,
      error,
      isLoading,
# leanpub-start-insert
      sortKey
# leanpub-end-insert
    } = this.state;

    ...

    return (
      <div className="page">
        ...
        <Table
          list={list}
# leanpub-start-insert
          sortKey={sortKey}
          onSort={this.onSort}
# leanpub-end-insert
          onDismiss={this.onDismiss}
        />
        ...
      </div>
    );
  }
}
~~~~~~~~

Table 컴포넌트는 리스트 정렬을 담당합니다. `sortKey` 로 `SORT` 함수 중 하나를 취해 리스트를 입력으로 전달합니다. 정렬된 리스트는 유지됩니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
const Table = ({
  list,
  sortKey,
  onSort,
  onDismiss
}) =>
# leanpub-end-insert
  <div className="table">
# leanpub-start-insert
    {SORTS[sortKey](list).map(item =>
# leanpub-end-insert
      <div key={item.objectID} className="table-row">
        ...
      </div>
    )}
  </div>
~~~~~~~~

이론적으로 리스트는 특정 함수에 의해 정렬됩니다. 기본 정렬은 `NONE`이기 때문에 아직 정렬이 되지 않았습니다. `sortKey`를 바꾸기 위해 `onSort()` 메서드를 실행하지 않았습니다.  Sort 컴포넌트로 열 헤더를 기반으로 정렬할 수 있게 합시다. Table 안에 Sort 컴포넌트를 사용해 확장해봅시다.

{title="src/App.js",lang=javascript}
~~~~~~~~
const Table = ({
  list,
  sortKey,
  onSort,
  onDismiss
}) =>
  <div className="table">
# leanpub-start-insert
    <div className="table-header">
      <span style={{ width: '40%' }}>
        <Sort
          sortKey={'TITLE'}
          onSort={onSort}
        >
          Title
        </Sort>
      </span>
      <span style={{ width: '30%' }}>
        <Sort
          sortKey={'AUTHOR'}
          onSort={onSort}
        >
          Author
        </Sort>
      </span>
      <span style={{ width: '10%' }}>
        <Sort
          sortKey={'COMMENTS'}
          onSort={onSort}
        >
          Comments
        </Sort>
      </span>
      <span style={{ width: '10%' }}>
        <Sort
          sortKey={'POINTS'}
          onSort={onSort}
        >
          Points
        </Sort>
      </span>
      <span style={{ width: '10%' }}>
        Archive
      </span>
    </div>
# leanpub-end-insert
    {SORTS[sortKey](list).map(item =>
      ...
    )}
  </div>
~~~~~~~~

Sort 컴포넌트는 특정 `sortKey`와 `onSort()`메서드를 갖고 있습니다. 내부에서 특정 키를 설정하기 위해  `sortKey` 메서드를 호출합니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
const Sort = ({ sortKey, onSort, children }) =>
  <Button onClick={() => onSort(sortKey)}>
    {children}
  </Button>
~~~~~~~~

보시다시피 Sort 컴포넌트는 공통인 Button 컴포넌트를 다시 사용했습니다. `onSort()`메서드에서 버튼 클릭시 전달된 각각의 `sortKey`가 설정됩니다. 열 헤더를 클릭할 때 리스트가 정렬되는지 확인해봅시다.

좀더 예쁘게 꾸며야할 부분이 있습니다. 지금있는 헤더 버튼이 보기 좋지 않습니다. Sort 컴포넌트 옆에 `className`을 지정하겠습니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
const Sort = ({ sortKey, onSort, children }) =>
# leanpub-start-insert
  <Button
    onClick={() => onSort(sortKey)}
    className="button-inline"
  >
# leanpub-end-insert
    {children}
  </Button>
~~~~~~~~

이제 멋지게 보입니다. 다음 목표는 역 정렬을 구현하는 것입니다. Sort 컴포넌트를 두 번 클릭하면 리스트가 역순으로 정렬 됩니다. 먼저 불리언 값을 사용해 `isSortReverse` 상태를 정의하겠습니다. `isSortReverse`가 `true`일 경우 역순으로 정렬되며, 반대로 `false`일 경우 정렬되지 않습니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
this.state = {
  results: null,
  searchKey: '',
  searchTerm: DEFAULT_QUERY,
  error: null,
  isLoading: false,
  sortKey: 'NONE',
# leanpub-start-insert
  isSortReverse: false,
# leanpub-end-insert
};
~~~~~~~~

`onSort()` 메서드에서 리스트가 역순으로 정렬되었는지 평가할 수 있습니다. `sortKey` 상태값이 입력된 `sortKey`와 같으며 반대 상태가 `true`로 설정되지 않을 경우 정렬하게 됩니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
onSort(sortKey) {
# leanpub-start-insert
  const isSortReverse = this.state.sortKey === sortKey && !this.state.isSortReverse;
  this.setState({ sortKey, isSortReverse });
# leanpub-end-insert
}
~~~~~~~~

다시 Table 컴포넌트에 역정렬된 props를 전달합니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    const {
      searchTerm,
      results,
      searchKey,
      error,
      isLoading,
      sortKey,
# leanpub-start-insert
      isSortReverse
# leanpub-end-insert
    } = this.state;

    ...

    return (
      <div className="page">
        ...
        <Table
          list={list}
          sortKey={sortKey}
# leanpub-start-insert
          isSortReverse={isSortReverse}
# leanpub-end-insert
          onSort={this.onSort}
          onDismiss={this.onDismiss}
        />
        ...
      </div>
    );
  }
}
~~~~~~~~

Table 컴포넌트에 데이터를 계산하는 화살표 함수 블록 본문이 있어야 합니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
const Table = ({
  list,
  sortKey,
  isSortReverse,
  onSort,
  onDismiss
}) => {
  const sortedList = SORTS[sortKey](list);
  const reverseSortedList = isSortReverse
    ? sortedList.reverse()
    : sortedList;

  return(
# leanpub-end-insert
    <div className="table">
      <div className="table-header">
        ...
      </div>
# leanpub-start-insert
      {reverseSortedList.map(item =>
# leanpub-end-insert
        ...
      )}
    </div>
# leanpub-start-insert
  );
}
# leanpub-end-insert
~~~~~~~~

리스트가 역순으로 잘 정렬됩니다.

마지막으로 사용자 경험 향상을 위해 개선할 사항이 남았습니다. 지금 어떤 열을 기준으로 정렬했는지 알 수가 없습니다. 사용자가 올바르게 인식하려면 어떻게 해야할까요? 바로 시각적인 효과를 줄 수 있습니다.

각 Sort 컴포넌트는 특정 `sortKey`를 가져옵니다. 이 키로 현재 선택된 정렬을 식별하는 사용할 수 있습니다. 컴포넌트 내부 상태의 `sortKey`를 선택된 정렬 키로 Sort 컴포넌트로 전달합시다.

{title="src/App.js",lang=javascript}
~~~~~~~~
const Table = ({
  list,
  sortKey,
  isSortReverse,
  onSort,
  onDismiss
}) => {
  const sortedList = SORTS[sortKey](list);
  const reverseSortedList = isSortReverse
    ? sortedList.reverse()
    : sortedList;

  return(
    <div className="table">
      <div className="table-header">
        <span style={{ width: '40%' }}>
          <Sort
            sortKey={'TITLE'}
            onSort={onSort}
# leanpub-start-insert
            activeSortKey={sortKey}
# leanpub-end-insert
          >
            Title
          </Sort>
        </span>
        <span style={{ width: '30%' }}>
          <Sort
            sortKey={'AUTHOR'}
            onSort={onSort}
# leanpub-start-insert
            activeSortKey={sortKey}
# leanpub-end-insert
          >
            Author
          </Sort>
        </span>
        <span style={{ width: '10%' }}>
          <Sort
            sortKey={'COMMENTS'}
            onSort={onSort}
# leanpub-start-insert
            activeSortKey={sortKey}
# leanpub-end-insert
          >
            Comments
          </Sort>
        </span>
        <span style={{ width: '10%' }}>
          <Sort
            sortKey={'POINTS'}
            onSort={onSort}
# leanpub-start-insert
            activeSortKey={sortKey}
# leanpub-end-insert
          >
            Points
          </Sort>
        </span>
        <span style={{ width: '10%' }}>
          Archive
        </span>
      </div>
      {reverseSortedList.map(item =>
          ...
      )}
    </div>
  );
}
~~~~~~~~

이제 Sort 컴포넌트에서 `sortKey`와 `activeSortKey`를 근거로 정렬이 활성화되었는지 알 수 있습니다. 시각적인 효과를 주기 위해 Sort 컴포넌트에 `className` 속성을 지정합시다.

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
const Sort = ({
  sortKey,
  activeSortKey,
  onSort,
  children
}) => {
  const sortClass = ['button-inline'];

  if (sortKey === activeSortKey) {
    sortClass.push('button-active');
  }

  return (
    <Button
      onClick={() => onSort(sortKey)}
      className={sortClass.join(' ')}
    >
      {children}
    </Button>
  );
}
# leanpub-end-insert
~~~~~~~~

`sortClass`를 정의하는 방법이 꽤 좋아보지지 않습니다. classnames 라이브러리를 사용해서 이를 해결해보겠습니다.

classnames 라이브러리를 설치합시다.

{title="Command Line",lang="text"}
~~~~~~~~
npm install classnames
~~~~~~~~

이어서 *src/App.js* 파일에서 라이브러리를 선언해 가져옵니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
import React, { Component } from 'react';
import fetch from 'isomorphic-fetch';
import { sortBy } from 'lodash';
# leanpub-start-insert
import classNames from 'classnames';
# leanpub-end-insert
import './App.css';
~~~~~~~~

classname 라이브러리를 사용해 `className` 컴포넌트를 조건부 클래스로 정의합시다.

{title="src/App.js",lang=javascript}
~~~~~~~~
const Sort = ({
  sortKey,
  activeSortKey,
  onSort,
  children
}) => {
# leanpub-start-insert
  const sortClass = classNames(
    'button-inline',
    { 'button-active': sortKey === activeSortKey }
  );
# leanpub-end-insert

  return (
# leanpub-start-insert
    <Button
      onClick={() => onSort(sortKey)}
      className={sortClass}
    >
# leanpub-end-insert
      {children}
    </Button>
  );
}
~~~~~~~~

테스트를 실행하면 스냅샷 테스트 뿐만 아니라 Table 컴포넌트의 유닛 테스트가 실패한 것도 확인해야 합니다. 컴포넌트를 다시 변경했기 때문에, 스냅샷 테스트를 수락할 수 있습니다만, 유닛 테스트를 수정해야 합니다. *src/App.test.js* 파일에서 아래와 같이 Table 컴포넌트 부분에 `sortKey`과 `isSortReverse` 불리언을 추가합시다.

{title="src/App.test.js",lang=javascript}
~~~~~~~~
...

describe('Table', () => {

  const props = {
    list: [
      { title: '1', author: '1', num_comments: 1, points: 2, objectID: 'y' },
      { title: '2', author: '2', num_comments: 1, points: 2, objectID: 'z' },
    ],
# leanpub-start-insert
    sortKey: 'TITLE',
    isSortReverse: false,
# leanpub-end-insert
  };

  ...

});
~~~~~~~~

Table 컴포넌트에 확장된 prop가 있기 때문에 한번 더 Table 컴포넌트 스냅샷 테스트 실패를 승인해야할 수도 있습니다.

드디어 어려운 정렬 기능을 모두 완성했습니다.

### 실습하기

* [Font Awesome](http://fontawesome.io/) 라이브러리를 사용해 정렬 아이콘을 추가하기
  * [화살표 위](https://fontawesome.com/icons/arrow-up?style=solid) 또는 [화살표 아래](https://fontawesome.com/icons/arrow-down?style=solid) 아이콘을 정렬 레이블 제목 옆에 추가

### 읽어보기
* [classnames 라이브러리](https://github.com/JedWatson/classnames)

{pagebreak}

이번 장에서는 리액트에서 컴포넌트 고급 테크닉을 배웠습니다. 지금까지 학습한 내용을 정리해봅시다.

* 리액트
  * ref 속성으로 DOM 노드를 접근할 수 있습니다.
  * 고차 컴포넌트는 고급 컴포넌트을 위한 일반적인 테크닉입니다.
  * 리액트에서 어려운 인터렉션을 구현했습니다.
  * 조건부 클래스명을 위해 `classnames` 라이브러리를 사용했습니다.
* ES6
  * 객체와 배열을 분리하기 위해 구조 해체를 했습니다.
  
실습 코드는 [공식 깃허브 리퍼지토리](https://github.com/the-road-to-learn-react/hackernews-client/tree/5.1)에서 확인할 수 있습니다.