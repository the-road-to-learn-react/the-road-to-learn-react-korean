# 4. 컴포넌트 모듈 구성 · 테스트

이번 장은 규모가 큰 애플리케이션에서 코드 유지와 관리 방법을 알아보겠습니다. 예제를 통해 폴더와 파일 구성 방법과 조직화된 코드 작성법을 배우고, 마지막으로 테스트 코드 작성법을 살펴보겠습니다. 소프트웨어 개발에 있어 코드 품질 향상은 매우 중요합니다. 이번 장에서는 애플리케이션 개발은 잠시 중단합니다.

## 4.1 ES6 Import · Export 

ES6에서는 모듈(Modules)의 특정 기능을 가져오고(`import`) 내보낼 수 있습니다(`export`). 이 기능은 변수를 지정할 수 있는 함수, 클래스 컴포넌트, 상수에 해당됩니다. 모듈은 단일 파일이거나, index 파일이 있는 폴더 전체가 될 수 있습니다.

1장에서 *create-react-app*으로 부트스트래핑 한 후, 생성된 파일에 `import`과 `export`가 있는 것을 보았을 것입니다.

`import`와 `export`으로 여러 파일에 동일한 코드를 공유할 수 있습니다. ES6 이전에는 모듈 사용법에 관한 표준화가 없었으나, ES6 이후 `import`와 `export`로 표준화되었습니다.

지금까지 계속해서 한 파일에서 모든 코드를 작성했습니다. `import`와 `export`으로 코드를 분할시켜 여러 파일을 만들면 코드 재사용과 유지 관리가 편해집니다. 

객체 지향 프로그래밍의 캡슐화(encapsulation, 객체의 속성(data fields)과 행위(메서드, methods)를 하나로 묶고, 실제 구현 내용 일부를 외부에 감추어 은닉한다.) 개념을 생각하면 됩니다. 한 파일 내 모든 함수를 밖으로 내보내야 하는 것은 아닙니다. 여러 함수 중 일부를 정의해 사용하면 됩니다. API와 같이 모든 파일에서 사용 가능합니다. 하지만 밖으로 내보내진 함수만 다른 파일에서 가져올 수 있습니다.

예제를 통해 `import`와 `export` 사용법에 대해 알아보겠습니다. *file1.js*와 *file2.js*은 변수를 서로 공유합니다. 이와 같이 여러 파일에서 `import`와 `export`로 변수를 공유할 수 있습니다. 

`export`는 한 개 이상의 변수를 내보낼 수 있습니다.

{title="Code Playground: file1.js",lang="javascript"}
~~~~~~~~
const firstname = 'robin';
const lastname = 'wieruch';

export { firstname, lastname };
~~~~~~~~

`import`에 상대 경로를 지정하고 내보낸 변수를 가져올 수 있습니다.

{title="Code Playground: file2.js",lang="javascript"}
~~~~~~~~
import { firstname, lastname } from './file1.js';

console.log(firstname);
// 출력: robin
~~~~~~~~

또한 내보낸 변수를 다른 파일에서 객체로 가져올 수 있습니다.

{title="Code Playground: file2.js",lang="javascript"}
~~~~~~~~
import * as person from './file1.js';

console.log(person.firstname);
// 출력: robin
~~~~~~~~

`import`한 모듈에 별명을 만들 수 있습니다. 보통 여러 파일에 함수 이름이 중복될 경우, 별명을 만들어 가져옵니다.

{title="Code Playground: file2.js",lang="javascript"}
~~~~~~~~
import { firstname as foo } from './file1.js';

console.log(foo);
// 출력: robin
~~~~~~~~

마지막으로 `default`의 아래와 같은 경우 사용됩니다.

* 단일 함수를 가져오고 내보낼 때
* 모듈 API 내 주요 함수임을 강조하기 위해 
* 폴백 import 기능(fallback import functionality)을 위해

`export default`에서 `{}`는 생략됩니다.

{title="Code Playground: file1.js",lang="javascript"}
~~~~~~~~
const robin = {
  firstname: 'robin',
  lastname: 'wieruch',
};

export default robin;
~~~~~~~~

`import`할 때 `export`한 이름을 바꿀 수 있습니다.

{title="Code Playground: file2.js",lang="javascript"}
~~~~~~~~
import developer from './file1.js';

console.log(developer);
// 출력: { firstname: 'robin', lastname: 'wieruch' }
~~~~~~~~

또한 `export`와 `export default`를 함께 사용할 수 있습니다.

{title="Code Playground: file1.js",lang="javascript"}
~~~~~~~~
const firstname = 'robin';
const lastname = 'wieruch';

const person = {
  firstname,
  lastname,
};

export {
  firstname,
  lastname,
};

export default person;
~~~~~~~~

{title="Code Playground: file2.js",lang="javascript"}
~~~~~~~~
import developer, { firstname, lastname } from './file1.js';

console.log(developer);
// 출력: { firstname: 'robin', lastname: 'wieruch' }
console.log(firstname, lastname);
// 출력: robin wieruch
~~~~~~~~

`export`에서 바로 변수를 내보낼 수 있습니다.

{title="Code Playground: file1.js",lang="javascript"}
~~~~~~~~
export const firstname = 'robin';
export const lastname = 'wieruch';
~~~~~~~~

지금까지 ES6 모듈의 주요 기능을 살펴보았습니다. 이번 절에서는 짜임새 있는 코드로 구성하여 재사용 가능한 모듈 API 형태로 설계하는 방법을 배웠습니다. 다음 절에서 함수를 내보내고 가져와서 테스트하는 방법을 알아보겠습니다.

### 읽어보기

* [[MDN] ES6 import](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import)
* [[MDN] ES6 export](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export)

## 4.2 ES6 모듈 구성

그동안 *src/App.js* 파일의 컴포넌트를 모듈로 만들지 않고 같은 파일에 코드를 계속 작성한 이유가 궁금할 것입니다. 처음 리액트를 시작할 때는 한 파일에서 모든 코드를 작성하는 것이 좋습니다. 리액트 개발이 익숙해지고 애플리케이션 규모가 커지면 그때 컴포넌트를 모듈로 분할해야 합니다.

지금까지 만든 해커 뉴스 애플리케이션을 아래와 같은 모듈로 구성할 수 있습니다. 

{title="Folder Structure",lang="text"}
~~~~~~~~
src/
  index.js
  index.css
  App.js
  App.test.js
  App.css
  Button.js
  Button.test.js
  Button.css
  Table.js
  Table.test.js
  Table.css
  Search.js
  Search.test.js
  Search.css
~~~~~~~~

이 구성에서 파일명은 같지만 확장자는 다르다는 것을 알 수 있습니다. 같은 파일명으로 묶으면 좀더 체계적인 모듈 구조로 구성할 수 있습니다. 

{title="Folder Structure",lang="text"}
~~~~~~~~
src/
  index.js
  index.css
  App/
    index.js
    test.js
    index.css
  Button/
    index.js
    test.js
    index.css
  Table/
    index.js
    test.js
    index.css
  Search/
    index.js
    test.js
    index.css
~~~~~~~~

폴더 구조가 깨끗해졌지 않나요? `index.js`은 폴더의 첫 진입점을 나타냅니다. `index.js`는 네이밍 컨벤션으로 다른 파일명으로 바꿔도 무방합니다. 각 모듈은 컴포넌트가 정의된 자바스크립트 파일, 스타일 파일, 테스트 파일로 구성되어 있습니다.

다음으로 App 컴포넌트에 있는 상수를 꺼내 새 파일을 만들겠습니다. 해커 뉴스 API 요청 URL를 구성할 때 정의한 상수를 말합니다.

{title="Folder Structure",lang="text"}
~~~~~~~~
src/
  index.js
  index.css
# leanpub-start-insert
  constants/
    index.js
  components/
# leanpub-end-insert
    App/
      index.js
      test.js
      index..css
    Button/
      index.js
      test.js
      index..css
    ...
~~~~~~~~

*src/constants/* 와 *src/components/* 폴더로 구분했습니다. 이제 *src/constants/index.js* 파일은 아래와 같을 것입니다.

{title="Code Playground: src/constants/index.js",lang="javascript"}
~~~~~~~~
export const DEFAULT_QUERY = 'redux';
export const DEFAULT_HPP = '100';
export const PATH_BASE = 'https://hn.algolia.com/api/v1';
export const PATH_SEARCH = '/search';
export const PARAM_SEARCH = 'query=';
export const PARAM_PAGE = 'page=';
export const PARAM_HPP = 'hitsPerPage=';
~~~~~~~~

이어서 *App/index.js* 파일에서 사용할 상수를 가져옵니다.

{title="Code Playground: src/components/App/index.js",lang=javascript}
~~~~~~~~
import {
  DEFAULT_QUERY,
  DEFAULT_HPP,
  PATH_BASE,
  PATH_SEARCH,
  PARAM_SEARCH,
  PARAM_PAGE,
  PARAM_HPP,
} from '../constants/index.js';

...
~~~~~~~~

*index.js* 네이밍 컨벤션을 사용하면 상대 경로에서 파일 이름을 생략할 수 있습니다.

{title="Code Playground: src/components/App/index.js",lang=javascript}
~~~~~~~~
import {
  DEFAULT_QUERY,
  DEFAULT_HPP,
  PATH_BASE,
  PATH_SEARCH,
  PARAM_SEARCH,
  PARAM_PAGE,
  PARAM_HPP,
# leanpub-start-insert
} from '../constants';
# leanpub-end-insert

...
~~~~~~~~

*index.js* 파일에 어떤 비밀이 있는 걸까요? 이 네이밍 컨벤션은 node.js에서 도입되었습니다. index 파일은 모듈의 진입점으로 외부 모듈에서*index.js* 파일을 사용해 모듈에서 퍼블릭(public) 코드를 가져올 수 있습니다. 예를 들어 아래와 같은 폴더 구조가 있다고 합시다.

{title="Folder Structure",lang="text"}
~~~~~~~~
src/
  index.js
  App/
    index.js
  Buttons/
    index.js
    SubmitButton.js
    SaveButton.js
    CancelButton.js
~~~~~~~~

*Buttons/* 폴더에는 여러 버튼 컴포넌트가 있습니다. 각 파일에는  `export default` 문으로 컴포넌트를 내보내고 *Buttons/index.js*에서 모든 버튼 컴포넌트들을 가져옵니다. 다시 *Buttons/index.js* 파일에서 가져온 모든 버튼 컴포넌트들을 내보냅니다. *index.js* 파일은 공용 모듈 API(public module API) 역할을 합니다.

{title="Code Playground: src/Buttons/index.js",lang="javascript"}
~~~~~~~~
import SubmitButton from './SubmitButton';
import SaveButton from './SaveButton';
import CancelButton from './CancelButton';

export {
  SubmitButton,
  SaveButton,
  CancelButton,
};
~~~~~~~~

*src/App/index.js*는 *index.js*의 퍼블릭 모듈 API를 통해서만 버튼을 가져올 수 있습니다.

{title="Code Playground: src/App/index.js",lang=javascript}
~~~~~~~~
import {
  SubmitButton,
  SaveButton,
  CancelButton
} from '../Buttons';
~~~~~~~~

이제부터 *index.js*가 아닌 다른 파일로 접근해 모듈을 가져오면 안됩니다. 캡슐화 규칙을 깨트리기 때문입니다.

{title="Code Playground: src/App/index.js",lang=javascript}
~~~~~~~~
// 이렇게 하지 마세요.
import SubmitButton from '../Buttons/SubmitButton';
~~~~~~~~

이번 절에서 캡슐화 규칙에 따라 코드를 리팩터링 하는 방법을 살펴보았습니다. 이 책을 모두 마친 후, 개인 숙제로 해보길 바랍니다.

### 실습하기

* 책을 다 마친 후 *src/App.js* 을 리팩토링하여 컴포넌트 모듈로 만듭니다.

## 4.3 Jest 스냅샷 테스트

이 책에서는 테스트를 깊게 다루지 않습니다. 그러나 프로그래밍에서 테스트는 필수이기 때문에 꼭 알아야 합니다. 테스트야 말로 코드의 품질을 높이고 모든 것이 문제없이 잘 작동하는 애플리케이션임을 보장하기 때문이지요. 

테스팅 피라미드(testing pyramid)에 대해 한 번쯤 들어봤을 것입니다. 테스팅 피라미드는 제일 위 꼭대기에서부터 종단 간 테스트(end-to-end test), 통합 테스트(integration test), 단위 테스트(unit test)로 구성됩니다. 테스트에 익숙하지 못한 분들을 위해 간략한 개념만 설명하겠습니다. 단위 테스트는 독립적이고 작은 코드 블록을 테스트합니다. 단위 테스트 함수는 단일 함수를 테스트합니다. 그러나 한 단위(unit)가 독립적일 경우 문제가 없지만, 여러 단위가 결합하는 경우 작동에 문제가 생길 수 있습니다. 이 경우, 단위들을 그룹으로 묶어 테스트를 진행해야 합니다. 이때 통합 테스트를 실시해 각 단위 그룹이 잘 작동하는지 확인합니다. 마지막으로 종단 간 테스트를 실시해 실제 사용자 시나리오 시물레이션 테스트를 진행합니다. 로그인 기능에 웹 자동화 테스트(Web Automation Testing)를 도입하는 것이 종단 간 테스트의 대표적인 예입니다.

그렇다면 각 유형별로 몇 개의 테스트가 필요할까요? 일반적으로 독립적인 함수일 경우 많은 단위 테스트가 필요합니다. 그에 비해 통합 테스트와 종단 간 테스트의 수는 적습니다. 일반적인 테스트 과정은 단위 테스트 실시 후 다음 단계로 정상적인 작동 여부를 확인하기 위해 몇 가지 통합 테스트를 실시하고, 마지막으로 최종 사용자 시나리오를 점검하기 위해 몇 가지 종단 간 테스트를 실시합니다.

그렇다면 리액트 애플리케이션에서 테스트를 어떻게 도입해야 할까요? 기본적인 리액트 테스트는 컴포넌트 테스트를 말하는데, 이는 단위 테스트와 스냅샷 테스트에 해당합니다. 이번 절에서는 [Jest(제스트)](https://facebook.github.io/jest/) 스냅샷 테스트(snapshot test) 코드 작성법을 배웁니다. 그리고 다음 절에서 다른 테스트 라이브러리인, Enzyme(엔자임) 테스트 코드 작성법을 배웁니다.

[Jest](https://facebook.github.io/jest/)는 페이스북에서 만든 자바스크립트 테스트 프레임워크로써 컴포넌트 테스트를 지원합니다. *create-react-app*에 Jest가 포함되어 있어 새로 설치할 필요가 없습니다. 

첫 번째 컴포넌트를 테스트하겠습니다. 그전에 *src/App.js*파일에서 테스트할 컴포넌트를 내보내야 합니다. 그렇게 해야 다른 파일에서 테스트할 수 있기 때문입니다. 4.1 절에서 이미 배운 내용입니다. `export`로 Button, Search, Table 컴포넌트를 내보냅니다. 

{title="src/App.js",lang=javascript}
~~~~~~~~
...

class App extends Component {
  ...
}

...

export default App;

# leanpub-start-insert
export {
  Button,
  Search,
  Table,
};
# leanpub-end-insert
~~~~~~~~

*create-react-app*에서 제공하는 *App.test.js* 파일이 바로 첫 번째 테스트 파일에 해당합니다. 이 테스트는 App 컴포넌트가 오류 없이 렌더링 되고 있는지를 확인합니다.
 
{title="src/App.test.js",lang=javascript}
~~~~~~~~
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';

it('renders without crashing', () => {
  const div = document.createElement('div');
  ReactDOM.render(<App />, div);
  ReactDOM.unmountComponentAtNode(div);
});
~~~~~~~~

"it" 문은 테스트 케이스를 말합니다. 구문 안에 테스트 내용이 있고, 테스트 시 성공 또는 실패 여부를 알려줍니다. "describe" 문으로 "it" 문 전체를 테스트 슈트(test suit)로 감쌀 수 있습니다. 테스트 슈트는 특정 컴포넌트의 "it" 문이 포함합니다. 나중에 "describe" 문에 대해 자세히 학습하길 바랍니다. 이들 모두 테스트 케이스를 분리하고 구성합니다.

커맨드 라인에서 *create-react-app* 테스트 스크립트를 사용해 테스트를 진행해보겠습니다. 모든 테스트 케이스에 대한 출력을 볼 수 있습니다. 아래 명령어를 실행해봅시다.

{title="Command Line",lang="text"}
~~~~~~~~
npm test
~~~~~~~~

**중요** App 컴포넌트 단위 테스트에서 오류가 발생하면, `componentDidMount()` 생명주기 메서드 안에 `fetchSearchTopStories()` 메서드에서 fetch 메서드가 문제가 있는 것입니다. 이 경우 아래 지시에 따라 해결합시다.

* 커맨드 라인에서 `npm install isomorphic-fetch` 명령어로 isomorphic-fetch 패키지를 설치합니다. 
* *App.js* 파일에서 isomorphic-fetch 모듈을 가져오기 위해 `import fetch from 'isomorphic-fetch';` 를 추가합니다.

이제 Jest 스냅샷 테스트를 작성하겠습니다. 이 테스트는 렌더링 된 컴포넌트 스냅샷 만들고 차기 스냅샷(future snapshot)과 비교하는 스냅샷을 실행합니다. 테스트에서 차기 스냅샷이 변경 여부를 알려줍니다. 컴포넌트를 의도적으로 수정했을 경우, 또는 수정을 원치 않은 경우에 스냅샷 변경을 수락할 수 있습니다. 렌더링 결과와 변경 사항(diff) 만을 가지고 테스트하기 때문에 단위 테스트를 잘 보완해줍니다. 의도 맞게  변경되었다면 변경된 스냅샷만 간단히 수락하면 되기 때문에 관리가 어렵지 않습니다.

Jest는 스냅샷을 폴더에 저장합니다. 이런 방법으로 차기 스냡샷과 차이를 비교하여 검증합니다. 또한 단일 폴더에 스냅샷을 공유할 수 있습니다.

스냅샷 테스트를 작성하기 전, `react-test-renderer` 유틸리티 라이브러리를 설치하겠습니다.

{title="Command Line",lang="text"}
~~~~~~~~
npm install --save-dev react-test-renderer
~~~~~~~~

이제 첫 번째 스냡샷 테스트로 App 컴포넌트를 확장하겠습니다. 먼저 노드 패키지에서 설치한 라이브러리를 가져오고, App 컴포넌트를 "it" 문으로 감싼 다음, 전체를 "describe" 문으로 감쌉니다. 이 경우 테스트 슈트는 App 컴포넌트를 가리킵니다.

{title="src/App.test.js",lang=javascript}
~~~~~~~~
import React from 'react';
import ReactDOM from 'react-dom';
# leanpub-start-insert
import renderer from 'react-test-renderer';
# leanpub-end-insert
import App from './App';

# leanpub-start-insert
describe('App', () => {
# leanpub-end-insert

  it('renders without crashing', () => {
    const div = document.createElement('div');
    ReactDOM.render(<App />, div);
  });

# leanpub-start-insert
});
# leanpub-end-insert
~~~~~~~~

이제 "test" 문으로 첫 번째 스냅샷 테스트를 작성하겠습니다.

{title="src/App.test.js",lang=javascript}
~~~~~~~~
import React from 'react';
import ReactDOM from 'react-dom';
import renderer from 'react-test-renderer';
import App from './App';

describe('App', () => {

  it('renders without crashing', () => {
    const div = document.createElement('div');
    ReactDOM.render(<App />, div);
    ReactDOM.unmountComponentAtNode(div);
  });

# leanpub-start-insert
  test('has a valid snapshot', () => {
    const component = renderer.create(
      <App />
    );
    let tree = component.toJSON();
    expect(tree).toMatchSnapshot();
  });

# leanpub-end-insert
});
~~~~~~~~

다시 테스트를 실행해 테스트가 성공 또는 실패했는지 확인하세요. 테스트가 성공해야 합니다. App 컴포넌트 내 렌더 내 출력을 변경하면 스냅샷 테스트가 실패합니다. 스냅샷을 업데이트하거나 App 컴포넌트를 확인할 수 있습니다.

`renderer.create()` 함수는 App 컴포넌트 스냅샷을 생성합니다. 가상으로 렌더딩된 스냅샷을 DOM을 저장합니다. 마지막으로 실행한 스냅샷과 일치하는지 확인합니다. 이러한 방법으로 DOM이 동일하게 유지되고 변경되지 않음을 확인합니다.

지금부터 Search, Button, Table 컴포넌트 테스트 코드를 작성하겠습니다. 

첫 번째로 Search 컴포넌트 테스트 코드를 작성하겠습니다.

{title="src/App.test.js",lang=javascript}
~~~~~~~~
import React from 'react';
import ReactDOM from 'react-dom';
import renderer from 'react-test-renderer';
# leanpub-start-insert
import App, { Search } from './App';
# leanpub-end-insert

...

# leanpub-start-insert
describe('Search', () => {

  it('renders without crashing', () => {
    const div = document.createElement('div');
    ReactDOM.render(<Search>Search</Search>, div);
  });

  test('has a valid snapshot', () => {
    const component = renderer.create(
      <Search>Search</Search>
    );
    let tree = component.toJSON();
    expect(tree).toMatchSnapshot();
  });

});
# leanpub-end-insert
~~~~~~~~

Search 컴포넌트 두 테스트는 App 컴포넌트와 유사합니다. 첫 번째 테스트는 Search 컴포넌트를 DOM에 렌더링하고 렌더링 프로세스 중 오류가 없는지 확인합니다. 오류가 있는 경우, expect, match, equal 등 검증문(assertion)이 없어도 테스트가 중단됩니다. 두 번째 스냅샷 테스트는 렌더링된 컴포넌트의 스냅샷을 저장하고 이전 스냅샷과 비교하여 실행합니다. 스냅샷이 변경되면 테스트가 실패합니다.

두 번째로 Button 컴포넌트 테스트 코드를 작성하겠습니다. Search 컴포넌트와 동일한 테스트 규칙이 적용됩니다.

{title="src/App.test.js",lang=javascript}
~~~~~~~~
...
# leanpub-start-insert
import App, { Search, Button } from './App';
# leanpub-end-insert

...

# leanpub-start-insert
describe('Button', () => {

  it('renders without crashing', () => {
    const div = document.createElement('div');
    ReactDOM.render(<Button>Give Me More</Button>, div);
  });

  test('has a valid snapshot', () => {
    const component = renderer.create(
      <Button>Give Me More</Button>
    );
    let tree = component.toJSON();
    expect(tree).toMatchSnapshot();
  });

});
# leanpub-end-insert
~~~~~~~~

마지막으로 Table 컴포넌트 테스트 코드를 작성하겠습니다. Table 컴포넌트 리스트를 렌더링하기 위해 props를 전달해야 합니다.

{title="src/App.test.js",lang=javascript}
~~~~~~~~
...
# leanpub-start-insert
import App, { Search, Button, Table } from './App';
# leanpub-end-insert

...

# leanpub-start-insert
describe('Table', () => {

  const props = {
    list: [
      { title: '1', author: '1', num_comments: 1, points: 2, objectID: 'y' },
      { title: '2', author: '2', num_comments: 1, points: 2, objectID: 'z' },
    ],
  };

  it('renders without crashing', () => {
    const div = document.createElement('div');
    ReactDOM.render(<Table { ...props } />, div);
  });

  test('has a valid snapshot', () => {
    const component = renderer.create(
      <Table { ...props } />
    );
    let tree = component.toJSON();
    expect(tree).toMatchSnapshot();
  });

});
# leanpub-end-insert
~~~~~~~~

이처럼 스냅샷 테스트 작성은 매우 간단합니다. 컴포넌트 출력이 변경하지 않는지만 확인하면 됩니다. 일단 출력을 변경하면 변경 사항을 수락할지 여부를 결정해야 합니다. 그렇지 않으면 원하는 출력이 아닐 경우, 컴포넌트를 수정해야 합니다.

### 실습하기

* `render()` 메서드에서 컴포넌트 리턴 값을 변경하면 스냅샷 테스트가 실패하는지 확인합니다.
  * 스냅샷 변경을 수락하거나 거부합니다.
* 앞으로 컴포넌트가 변경될 때마다 스냅샷 테스트 코드를 수정하여 최신 상태로 유지합니다.

### 읽어보기
* [[리액트 공식 문서] Jest](https://facebook.github.io/jest/docs/tutorial-react.html)

## 4.4 Enzyme 단위 테스트

[Enzyme(엔자임)](https://github.com/airbnb/enzyme)는 에어비앤비가 만든 테스트 유틸리티로 리액트 컴포넌트를 확인, 조작, 통과시킵니다. Enzyme은 스냅샷 테스트를 보완하여 단위 테스트를 수행합니다.

이제 Enzyme을 어떻게 사용하는지 알아보겠습니다. 

첫째, Enzyme은 *create-react-app*에서 제공하지 않기 때문에 별도로 설치해야 합니다. 리액트 확장판도 함께 설치합니다.

{title="Command Line",lang="text"}
~~~~~~~~
npm install --save-dev enzyme react-addons-test-utils enzyme-adapter-react-16
~~~~~~~~

둘째, Enzyme을 테스트 설정에 추가하고 리액트가 사용할 수 있게 어댑터를 초기화합니다.

{title="src/App.test.js",lang=javascript}
~~~~~~~~
import React from 'react';
import ReactDOM from 'react-dom';
import renderer from 'react-test-renderer';
# leanpub-start-insert
import Enzyme from 'enzyme';
import Adapter from 'enzyme-adapter-react-16';
# leanpub-end-insert
import App, { Search, Button, Table } from './App';

# leanpub-start-insert
Enzyme.configure({ adapter: new Adapter() });
# leanpub-end-insert
~~~~~~~~

Table 컴포넌트 "describe" 문에 첫 번째 단위 테스트를 작성하겠습니다. `shallow()` 메서드는 컴포넌트를 렌더링합니다. 리스트 내 아이템이 두 개임으로 Table 컴포넌트 내 엘레먼트가 두 개 있다고 가정하겠습니다. 이 검증문은 `table-row` 클래스에 두 엘레먼트가 있는지를 체크합니다. 

{title="src/App.test.js",lang=javascript}
~~~~~~~~
import React from 'react';
import ReactDOM from 'react-dom';
import renderer from 'react-test-renderer';
# leanpub-start-insert
import Enzyme, { shallow } from 'enzyme';
# leanpub-end-insert
import Adapter from 'enzyme-adapter-react-16';
import App, { Search, Button, Table } from './App';

...

describe('Table', () => {

  const props = {
    list: [
      { title: '1', author: '1', num_comments: 1, points: 2, objectID: 'y' },
      { title: '2', author: '2', num_comments: 1, points: 2, objectID: 'z' },
    ],
  };

  ...

# leanpub-start-insert
  it('shows two items in list', () => {
    const element = shallow(
      <Table { ...props } />
    );

    expect(element.find('.table-row').length).toBe(2);
  });
# leanpub-end-insert

});
~~~~~~~~

`shallow()` 메서드는 자식 컴포넌트가 없는 컴포넌트를 렌더링합니다. 이런 방식으로 하나의 컴포넌트를 전담해 테스트를 실시합니다.

Enzyme API에는 세 가지 렌더링 메서드가 있습니다. `shallow()`  메서드를 포함해 `mount()`와 `render()` 메서드가 있습니다. `mount()`와 `render()` 메서드는 부모 컴포넌트 및 하위 컴포넌트의 인스턴스를 인스턴스화합니다. `mount()` 메서드는 컴포넌트 생명주기 메서드를 테스트합니다. 렌더링 메서드 주요 사용 규칙은 다음과 같습니다.

* 제일 먼저 `shallow()` 테스트를 시작합니다. 
* `mount()` 메서드를 사용해 `componentDidMount()` 또는 `componentDidUpdate()` 메서드를 테스트합니다.
* 컴포넌트의 생명주기와 자식 컴포넌트를 테스트 시, `mount()`를 사용합니다.
* `render()` 메서드는 `mount()`보다 적은 오버헤드(overhead, 처리를 위해 투입되는 간접적인 처리 시간·메모리)로 자식 렌터링을 테스트할 경우, 생명주기 메서드와 무관할 경우에 사용합니다.

앞으로도 컴포넌트 테스트 코드를 계속 작성할 수 있습니다. 항상 테스트를 간단하고 유지 가능하게 만들어야 합니다. 그렇지 않으면 매번 컴포넌트를 변경할 때마다 리팩터링해야 합니다. 페이스북이 Jest에서 스냅샷 테스트를 도입한 이유도 이 때문입니다.

### 실습하기

* Button 컴포넌트에 대한 Enzyme 단위 테스트 코드를 작성해봅니다.
* 다음 절에서 단위 테스트를 최신 상태로 유지합니다.

### 읽어보기

* [enzyme과 API 렌더링](https://github.com/airbnb/enzyme)

## 4.5 PropTypes 컴포넌트 인터페이스

자바스크립트 타입 인터페이스인 [타입스크립트(TypeScript)](https://www.typescriptlang.org/) 또는 [플로우(Flow)](https://flowtype.org/)에 대해 한번 쯤 들어본 적이 있을 것입니다. 프로그래밍에서는 텍스트 입력 오류가 발생하기 쉽기 때문에 텍스트를 기반으로 유효성을 검사합니다. 프로그램이 실행되기 전 에디터와 유틸리티로 텍스트 오류를 체크합니다. 이로써 프로그램 품질을 견고하게 만들어 줍니다.

이 책에서는 컴포넌트 유형 체크를 위해 PropType(프롭타입)을 사용합니다.  타입스크립트와 플로우는 다루지 않습니다. 리액트는 타입 검사기가 포함되어 있습니다. PropType는 컴포넌트 인터페이스입니다. PropTypes 인터페이스는 부모 컴포넌트에서 하위 컴퍼넌트로 전달되는 모든 props의 유효를 검사합니다.

이번 절에서는 PropType을 사용해 모든 컴포넌트 타입을 안전하게 만들 수 있는 방법을 배웁니다. 다음 장에 변경될 내용은 생략하고 진행하겠습니다. 불필요한 코드 리팩터링이 추가로 필요하기 때문입니다. 다만 컴퍼넌트 인터페이스 타입을 안전하게 유지하기 위해 PropType는 계속 업데이트해야 합니다. 

먼저 PropType 패키지를 설치하겠습니다.

{title="Command Line",lang="text"}
~~~~~~~~
npm install prop-types
~~~~~~~~

*App.js* 파일에서 PropTypes를 가져옵니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
import PropTypes from 'prop-types';
# leanpub-end-insert
~~~~~~~~

Button 컴포넌트에 props 인터페이스를 지정하겠습니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
const Button = ({
  onClick,
  className = '',
  children,
}) =>
  <button
    onClick={onClick}
    className={className}
    type="button"
  >
    {children}
  </button>

# leanpub-start-insert
Button.propTypes = {
  onClick: PropTypes.func,
  className: PropTypes.string,
  children: PropTypes.node,
};
# leanpub-end-insert
~~~~~~~~
 
 함수 시그니처(function signature, 함수와 메서드 입력ㆍ출력을 정의) 에서 인자를 가져와 PropTypes을 지정합니다. 기본적인 PropTypes 정의는 다음과 같습니다.

* PropTypes.array
* PropTypes.bool
* PropTypes.func
* PropTypes.number
* PropTypes.object
* PropTypes.string

또한 렌더링 엘레먼트(노드, 문자열, 리액트 엘레먼트)를 정의하는 PropType는 다음과 같습니다.

* PropTypes.node
* PropTypes.element

Button 컴포넌트에서 `PropTypes.node`를 사용했습니다. [리액트 공식 문서](https://reactjs.org/docs/typechecking-with-proptypes.html#proptypes)에서 더 많은 PropType 정의를 확인할 수 있습니다.

Button 컴포넌트에 PropTypes 정의를 추가할 수 있습니다. 해당 값은 `null` 또는 `undefined`가 될 수 있습니다. 하지만 중요한 props라면 PropTypes을 정의하는 것이 바람직합니다. 또한 컴포넌트에 props 전달을 필수 사항으로 정의할 수 있습니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
Button.propTypes = {
# leanpub-start-insert
  onClick: PropTypes.func.isRequired,
# leanpub-end-insert
  className: PropTypes.string,
# leanpub-start-insert
  children: PropTypes.node.isRequired,
# leanpub-end-insert
};
~~~~~~~~

`className`의 기본값이 빈 문자열임으로 필수 사항은 아닙니다. 이번에는 Table 컴포넌트 PropType을 정의하겠습니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
Table.propTypes = {
  list: PropTypes.array.isRequired,
  onDismiss: PropTypes.func.isRequired,
};
# leanpub-end-insert
~~~~~~~~

Table 컴포넌트의 `list` props 객체 내 모든 프로퍼티를 정의하겠습니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
Table.propTypes = {
  list: PropTypes.arrayOf(
    PropTypes.shape({
      objectID: PropTypes.string.isRequired,
      author: PropTypes.string,
      url: PropTypes.string,
      num_comments: PropTypes.number,
      points: PropTypes.number,
    })
  ).isRequired,
  onDismiss: PropTypes.func.isRequired,
};
~~~~~~~~

이중 `objectID` 프로퍼티의 PropTypes 정의는 필수입니다. `objectID`에 의존하는 코드가 있기 때문입니다. 다른 프로퍼티는 화면에 표시만 됨으로 반드시 필요하지 않습니다. 더욱이 해커 뉴스 API가 정의된 프로퍼티 타입이 항상 유지되는지 확신할 수 없기 때문입니다.

PropTypes 이외에 prop의 기본값을 정의하는 default props이 있습니다. Button 컴포넌트를 다시 보겠습니다. `className` 프로퍼티는 컴포넌트 시그니처 안에 ES6 기본 매개변수를 가지고 있습니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
const Button = ({
  onClick,
  className = '',
  children
}) =>
  ...
~~~~~~~~

이 기본 매개변수를 default props으로 바꾸겠습니다.
{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
const Button = ({
  onClick,
  className,
  children
}) =>
# leanpub-end-insert
  <button
    onClick={onClick}
    className={className}
    type="button"
  >
    {children}
  </button>

# leanpub-start-insert
Button.defaultProps = {
  className: '',
};
# leanpub-end-insert
~~~~~~~~

ES6 기본 매개변수와 동일하게, default prop은 부모 컴포넌트가 값을 지정하지 않은 경우, 프로퍼티 기본 값이 설정됩니다. default prop을 평가한 후 PropType 타입 검사를 시작합니다.

테스트를 다시 실행해 커맨드 라인에서 컴포넌트 PropType 오류가 있는지를 확인합니다. 테스트에서 PropType을 `isRequired`로 정의했으나, 컴포넌트 내 모든 props를 정의하지 않았을 때 오류가 발생할 수 있습니다. 테스트 자체는 통과됩니다. 오류를 피하기 위해 테스트에서 필수 props를 모두 전달하도록 합니다.

### 실습하기

* Search 컴포넌트의 PropType를 정의합니다.
* 다음 장에서 컴포넌트를 추가, 수정할 때마다 PropType을 업데이트합니다.

### 읽어보기

* [[리액트 공식 문서] 리액트 PropTypes](https://reactjs.org/docs/typechecking-with-proptypes.html)

{pagebreak}

이번 장에서는 코드 구성과 테스트 코드 작성법을 배웠습니다! 지금까지 배운 내용을 정리해봅시다.

* 리액트
  * PropTypes는 컴포넌트 타입을 정의합니다.
  * Jest는 컴포넌트 스냅샷 테스트를 수행합니다.
  * Enzyme은 컴포넌트 단위 테스트를 수행합니다.
* ES6
  * `import`와 `export`로 코드를 구성합니다.
* 일반
  * 코드 조직화는 애플리케이션을 확장할 수 있는 좋은 방법입니다.

실습 코드는 [깃허브 리퍼지토리](https://github.com/the-road-to-learn-react/hackernews-client/tree/5.1)에서 확인할 수 있습니다.
