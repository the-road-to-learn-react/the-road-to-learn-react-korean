# 6. 심화: 리액트 상태 관리

여러분은 기본적인 리액트 상태를 관리하고 다룰 수 있습니다. 이번 장에서는 리액트 상태 관리의 모범 사례와 적용 방법, 관련 라이브러리를 통해 상태 관리의 심화 내용을 살펴보겠습니다.

## 6.1 상태 끌어올리기

App 컴포넌트는 ES6 컴포넌트로 상태를 가지고 있으며, 클래스 메서드에서 상태와 로직을 처리합니다. Table 컴포넌트로 많은 프로퍼티를 props로 넘겼습니다. 이 props는 Table 컴포넌트에서만 사용합니다. 결론적으로 App 컴포넌트가 Table 컴포넌트가 사용하고 있는지 모릅니다. 

리스트 정렬 기능은 Table 컴포넌트에서만 사용됩니다. App 컴포넌트가 Table 컴포넌트를 모르기 때문에, 이 기능을 Table 컴포넌트로 옮길 수 있습니다. 특정 컴포넌트에 에서 다른 컴포넌트로 전달한 하위 상태를 리팩터링 하는 프로세스를 __상태 끌어올리기(lifting state)__ 라고 합니다. 우리는 App 컴포넌트에서 사용하지 않는 상태를 Table 컴포넌트로 옮기려고 합니다. 즉 상태는 상위 컴포넌트에서 하위 컴포넌트로 옮겨집니다. 

Table 컴포넌트의 상태와 클래스 메서드를 처리하기 위해서 ES6클래스 컴포넌트로 수정해야 합니다. 함수형 컴포넌트에서 ES6 클래스 컴포넌트로 리팩터링은 쉽습니다. 

현재 Table 컴포넌트는 비 상태 함수형 컴포넌트입니다.

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
    ...
  );
}
~~~~~~~~

Table 컴포넌트를 ES6 클래스 컴포넌트로 수정합시다.

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
class Table extends Component {
  render() {
    const {
      list,
      sortKey,
      isSortReverse,
      onSort,
      onDismiss
    } = this.props;

    const sortedList = SORTS[sortKey](list);
    const reverseSortedList = isSortReverse
      ? sortedList.reverse()
      : sortedList;

    return(
      ...
    );
  }
}
# leanpub-end-insert
~~~~~~~~

컴포넌트의 상태와 메서드를 처리하기 위해 생성자와 초기 상태를 추가합시다.

{title="src/App.js",lang=javascript}
~~~~~~~~
class Table extends Component {
# leanpub-start-insert
  constructor(props) {
    super(props);

    this.state = {};
  }
# leanpub-end-insert

  render() {
    ...
  }
}
~~~~~~~~

이제 App 컴포넌트의 정렬 기능에 해당하는 상태와 클래스 메서드를 Table 컴포넌트로 옮깁니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
class Table extends Component {
  constructor(props) {
    super(props);

# leanpub-start-insert
    this.state = {
      sortKey: 'NONE',
      isSortReverse: false,
    };

    this.onSort = this.onSort.bind(this);
# leanpub-end-insert
  }

# leanpub-start-insert
  onSort(sortKey) {
    const isSortReverse = this.state.sortKey === sortKey && !this.state.isSortReverse;
    this.setState({ sortKey, isSortReverse });
  }
# leanpub-end-insert

  render() {
    ...
  }
}
~~~~~~~~

App 컴포넌트에서 옮긴 상태와 `onSort()` 메서드를 삭제하는 것을 잊지 맙시다.

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
      isLoading: false,
    };

    this.setSearchTopStories = this.setSearchTopStories.bind(this);
    this.fetchSearchTopStories = this.fetchSearchTopStories.bind(this);
    this.onDismiss = this.onDismiss.bind(this);
    this.onSearchSubmit = this.onSearchSubmit.bind(this);
    this.onSearchChange = this.onSearchChange.bind(this);
    this.needsToSearchTopStories = this.needsToSearchTopStories.bind(this);
  }

  ...

}
~~~~~~~~

Table 컴포넌트 API를 더 가볍게 만들 수 있습니다. App 컴포넌트에서 전달된 props를 제거하기 위해, Table 컴포넌트에서 내부적으로 처리하게 만듭시다.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
# leanpub-start-insert
    const {
      searchTerm,
      results,
      searchKey,
      error,
      isLoading
    } = this.state;
# leanpub-end-insert

    ...

    return (
      <div className="page">
        ...
# leanpub-start-insert
        <Table
          list={list}
          onDismiss={this.onDismiss}
        />
# leanpub-end-insert
        ...
      </div>
    );
  }
}
~~~~~~~~

이제 Table 컴포넌트 내부에서 `onSort()` 메서드와 상태를 사용할 수 있습니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
class Table extends Component {

  ...

  render() {
# leanpub-start-insert
    const {
      list,
      onDismiss
    } = this.props;

    const {
      sortKey,
      isSortReverse,
    } = this.state;
# leanpub-end-insert

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
# leanpub-start-insert
              onSort={this.onSort}
# leanpub-end-insert
              activeSortKey={sortKey}
            >
              Title
            </Sort>
          </span>
          <span style={{ width: '30%' }}>
            <Sort
              sortKey={'AUTHOR'}
# leanpub-start-insert
              onSort={this.onSort}
# leanpub-end-insert
              activeSortKey={sortKey}
            >
              Author
            </Sort>
          </span>
          <span style={{ width: '10%' }}>
            <Sort
              sortKey={'COMMENTS'}
# leanpub-start-insert
              onSort={this.onSort}
# leanpub-end-insert
              activeSortKey={sortKey}
            >
              Comments
            </Sort>
          </span>
          <span style={{ width: '10%' }}>
            <Sort
              sortKey={'POINTS'}
# leanpub-start-insert
              onSort={this.onSort}
# leanpub-end-insert
              activeSortKey={sortKey}
            >
              Points
            </Sort>
          </span>
          <span style={{ width: '10%' }}>
            Archive
          </span>
        </div>
        { reverseSortedList.map((item) =>
          ...
        )}
      </div>
    );
  }
}
~~~~~~~~

애플리케이션이 잘 작동하는지 확인하세요. 여러분은 가장 중요한 리팩터링을 해봤습니다. Table 컴포넌트 API가 내부적으로 정렬 기능을 처리하고, 관련 있는 상태와 메서드를 상위 컴포넌트에서 이동시켜 상위 컴포넌트를 가볍게 만들었습니다. 

하위에서 상위 컴포넌트로 상태를 이동할 수 있습니다. 하위 컴포넌트의 내부 상태를 처리한다고 가정해봅시다. 부모 컴포넌트에서도 그 상태가 표시되어야 한다면, 부모 컴포넌트로 상태를 옮겨야 합니다. 더 나아가 자식 컴포넌트에 형제 컴포넌트의 상태를 표시한다고 해봅시다. 이 역시 형제 컴포넌트의 상태를 부모 컴포넌트로 옮겨야 합니다. 부모 컴포넌트는 내부 상태를 처리하면 두 컴포넌트에 모두 표시됩니다.

### 읽어보기

* [[리액트 공식 문서] 리액트 상태 옮기기](https://reactjs.org/docs/lifting-state-up.html)
* [[저자 블로그] Redux 사용 전 알아야 할 것들](https://www.robinwieruch.de/learn-react-before-using-redux/)

## 6.2 심화: `setState()`

지금까지 `setState()` 메서드로 컴포넌트 내부 상태를 관리했습니다. 전체 상태 중 부분적으로 업데이트할 객체를 함수에 전달했습니다.

{title="Code Playground",lang="javascript"}
~~~~~~~~
this.setState({ foo: bar });
~~~~~~~~

`setState()`는 객체 하나만 취하지 않습니다. 함수를 전달해 상태를 업데이트할 수 있습니다.

{title="Code Playground",lang="javascript"}
~~~~~~~~
this.setState((prevState, props) => {
  ...
});
~~~~~~~~

왜 이렇게 해야 할까요? 단일 객체가 아닌 함수를 사용할 때 문제가 되기 때문입니다. 바로 이전 상태 또는 props에 따라 상태를 업데이트하는 경우가 있습니다. 이때 함수를 사용하지 않으면 내부 상태로 인해 버그가 발생. `setState()` 메서드는 비동기입니다. 리액트는 `setState()` 메서드를 호출하고 이를 실행합니다. `setState()`가 호출될 때, 이전 state나 props를 변경할 수 있습니다.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const { fooCount } = this.state;
const { barCount } = this.props;
this.setState({ count: fooCount + barCount });
~~~~~~~~

`fooCount`과 `barCount`가 있다고 가정해봅시다. 그리고 `setState()` 메서드를 호출할 때 state나 props에 따라 어떤 곳이 변경된다고 합시다. 애플리케이션 규모가 커지면서 한 개 이상의 `setState()`가 호출해야 하는 일이 생깁니다. `setState()`는 비동기적으로 실행되기 때문에, 이전 상태 값을 의존할 수 있습니다. 

`setState()` 메서드는 state와 props를 처리하는 비동기 콜백 함수입니다. `setState()` 메서드는 비동기로 실행될 때 이전 state와 props를 변경합니다.

{title="Code Playground",lang="javascript"}
~~~~~~~~
this.setState((prevState, props) => {
  const { fooCount } = prevState;
  const { barCount } = props;
  return { count: fooCount + barCount };
});
~~~~~~~~

이제 문제를 해결해봅시다. `setState()` 메서드를 사용하는 곳을 수정해 state와 props를 의존하게 만들겠습니다. 나중에 다른 곳도 수정할 수 있습니다.

`setSearchTopStories()`메서드는 이전 상태를 의존함으로 `setState()`에서 함수를 사용할 수 있습니다. 지금 현재 코드는 아래와 같을 것입니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
setSearchTopStories(result) {
  const { hits, page } = result;
  const { searchKey, results } = this.state;

  const oldHits = results && results[searchKey]
    ? results[searchKey].hits
    : [];

  const updatedHits = [
    ...oldHits,
    ...hits
  ];

  this.setState({
    results: {
      ...results,
      [searchKey]: { hits: updatedHits, page }
    },
    isLoading: false
  });
}
~~~~~~~~

상태 값을 가져오지만 비동기로 이전 상태를 업데이트합니다. 이전 상태로 인해 버그가 발생하지 않게 함수를 만들어봅시다.

{title="src/App.js",lang=javascript}
~~~~~~~~
setSearchTopStories(result) {
  const { hits, page } = result;

# leanpub-start-insert
  this.setState(prevState => {
    ...
  });
# leanpub-end-insert
}
~~~~~~~~

여기서 구현한 블록 전체를 함수 안으로 옮길 수 있습니다. `this.state`을 `prevState`으로 바꾸면 됩니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
setSearchTopStories(result) {
  const { hits, page } = result;

  this.setState(prevState => {
# leanpub-start-insert
    const { searchKey, results } = prevState;

    const oldHits = results && results[searchKey]
      ? results[searchKey].hits
      : [];

    const updatedHits = [
      ...oldHits,
      ...hits
    ];

    return {
      results: {
        ...results,
        [searchKey]: { hits: updatedHits, page }
      },
      isLoading: false
    };
# leanpub-end-insert
  });
}
~~~~~~~~

한 가지 더 개선할 점이 있습니다. 코드 가독성을 높이기 위해 함수를 밖으로 빼낼 수 있습니다. 객체에 대한 함수의 장점입니다. 이 함수는 컴포넌트 외부로 빼낼 수 있지만, 고차 함수를 만들어 결과를 전달할 수 있게 해야 합니다. API에서 가져온 `result`를 가지고 상태를 업데이트하도록 말이지요.

{title="src/App.js",lang=javascript}
~~~~~~~~
setSearchTopStories(result) {
  const { hits, page } = result;
  this.setState(updateSearchTopStoriesState(hits, page));
}
~~~~~~~~

`updateSearchTopStoriesState()` 함수는 함수를 반환하는 고차 함수입니다. 고차 함수는 App 컴포넌트 외부에서 정의해도 됩니다. 함수 시그니처가 조금 달라졌다는 점만 유념합시다.

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
const updateSearchTopStoriesState = (hits, page) => (prevState) => {
  const { searchKey, results } = prevState;

  const oldHits = results && results[searchKey]
    ? results[searchKey].hits
    : [];

  const updatedHits = [
    ...oldHits,
    ...hits
  ];

  return {
    results: {
      ...results,
      [searchKey]: { hits: updatedHits, page }
    },
    isLoading: false
  };
};
# leanpub-end-insert

class App extends Component {
  ...
}
~~~~~~~~

`setState()` 메서드에서 객체 접근 함수는 잠재적인 버그를 수정해주고 코드의 가독성과 유지 보수성을 높여줍니다. 또한 App 컴포넌트 외부에서 테스트를 할 수 있습니다. 즉 밖으로 export 하여 테스트를 진행할 수 있습니다.

### 읽어보기

* [[리액트 공식 문서] 올바른 상태 사용법](https://reactjs.org/docs/state-and-lifecycle.html#using-state-correctly)

### 실습하기
* `setState()` 메서드에서 함수를 사용할 수 있게 리팩토링합니다.
  * 의존된 props 또는 state가 있는 경우에만
* 테스트를 재실행하고 업데이트되었는지 확인합니다.

## 6.3 상태 제어

규모가 큰 애플리케이션의 상태 관리는 매우 중요합니다. 리액트는 물론 많은 SPA 프레임워크가 상태 관리 문제가 있습니다. 최근 몇 년간 애플리케이션이 더 복잡해짐에 따라 상태 제어하는 문제가 대두되고 있습니다.

리액트는 다른 솔루션에 비해서 이미 큰 발전을 이뤘습니다. 단방향 데이터 흐름과 컴포넌트에서 상태를 관리하는 API가 반드시 필요합니다. 이를 통해 상태와 상태 변경 사항, 컴포넌트 레벨에서 특정 수준의 애플리케이션 레벨까지 쉽게 파악할 수 있게 됩니다.

이미 고도화된 애플리케이션의 경우 상태 변경을 파악하기가 어렵습니다. `setState()`를 통해 상태를 조작하다보면 버그가 생길 수 있습니다. 컴포넌트 간 필요한 상태를 공유하거나, 혹은 불필요한 상태를 삭제하기 위해서 상태를 여러번 옮겨야 합니다. 또한 형제 컴포넌트가 종속되어 있는 경우 상태를 옮겨야 할 경우가 생깁니다. 컴포넌트가 최상위 컴포넌트에서 아주 멀리 떨어져 있는 경우에도 전체 컴포넌트에 상태를 공유하기 위해 상태를 또 옮겨야 할 겁니다. 결론적으로 컴포넌트는 상태 관리가 문제입니다. 그러나 컴포넌트의 본래 목적은 UI를 보여주는 것입니다.

이러한 모든 이유를 해결하기 위해 상태 관리를 처리할 수 있는 독립형 솔루션이 있습니다. 이 솔루션은 리액트에만 허용된 것이 아니라 타 프레임워크에서도 사용가능합니다. 이러한 점이 바로 리액트 생태계를 풍부하고 강력하게 만들어 줍니다. 상태 관리 라이브러리인 [리덕스(Redux)](http://redux.js.org/docs/introduction/)와 [모브엑스(MobX)](https://mobx.js.org/)에 대해 들어봤을 겁니다. 둘 중 하나를 선택해 리액트 애플리케이션에 사용할 수 있습니다. [react-redux](https://github.com/reactjs/react-redux) 또는 [mobx-react](https://github.com/mobxjs/mobx-react) 라이브러리를 도입해 리액트 뷰 레이어에 통합시킬 수 있습니다.
 
리덕스와 모브엑스는 이 책에서 다루지 않습니다. 이 책을 마친 여러분들은 리액트와 그 생태계를 계속 탐험할 수 있는 방법을 스스로 깨우치게 될 것입니다. 이 책을 마친 후 리덕스 학습을 시작할 수 있습니다. 외부 상태 관리로 넘어가지 전에 저자의 ['Redux와 MobX에 대해 바로알기'](https://www.robinwieruch.de/redux-mobx-confusion/) 글을 참고하길 바랍니다. 외부 상태 관리 학습 방법에 대해 자세히 기술했습니다.

### 읽어보기

* [[저자 블로그] 외부 상태 관리와 학습 방법](https://www.robinwieruch.de/redux-mobx-confusion/)
* [[저자 두번째 출간 도서] 리액트 상태 다루기](https://roadtoreact.com/)

{pagebreak}

이번 장에서는 심화 수준의 리액트 상태 관리를 배웠습니다. 지금까지 학습한 내용을 정리해봅시다.

* 리액트
  * 쓰임새와 역할에 맞게 상하위 컴포넌트로 상태를 재배치합니다.
  * `setState()` 메서드에서 과거 상태로 인한 버그를 방지하기 위해 외부에서 별도 함수를 만들어 인자로 사용할 수 있습니다.
  * 상태 관리 라이브러리를 사용해 외부에서 상태 관리를 할 수 있습니다.
  
실습 코드는 [깃허브 리퍼지토리](https://github.com/the-road-to-learn-react/hackernews-client/tree/5.1)에서 확인할 수 있습니다.
