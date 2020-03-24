## 리액트 프로젝트 구조

지금까지 만든 파일은 여러 개의 컴포넌트가 한 개의 파일 안에 있었습니다. 이 컴포넌트들은 각자의 파일/폴더(모듈이라고도 불리는)로 나눠질 수 있습니다. 그럼, 왜 처음부터 *src/App.js*파일과 컴포넌트 파일을 분리하지 않았는지 의문이 들 겁니다. 모든 컴포넌트를 한 개의 파일에 두는 것이 배우기 쉽기 때문에 이런 구조로 진행했습니다. 그러나 이제 애플리케이션의 규모가 커졌기 때문에 이번 장에선 컴포넌트를 모듈로 나누는 방법을 배워 보겠습니다. 

리액트 프로젝트를 재구성하기 전에, [자바스크립트의 import(가져오기) 구문와 export(내보내기) 구문](https://www.robinwieruch.de/javascript-import-export)에 대해 알아봅시다. 파일 import와 export는 리액트를 공부하기 전 꼭 알아야 할 자바스크립트의 중요 개념입니다. 리액트 애플리케이션은 프로젝트를 진행해 가면서 자연스럽게 구조가 잡히기 때문에 올바른 구조란 것은 없습니다.

이제 프로세스에 대해 알아보기 위해 프로젝트의 폴더와 파일 리팩토링을 해보겠습니다. 이후엔 리액트 프로젝트를 재구성하는 옵션도 살펴봅니다. 이 과정은 간단히 하기 위해 *src/App.js* 파일에서 실습을 진행하도록 하겠습니다. 

커맨드 라인에서 프로젝트 내 *src/*폴더로 이동해 아래의 커맨드처럼 컴포넌트 전용 파일을 생성합니다. 

{title="Command Line",lang="text"}
~~~~~~~
cd src
touch List.js InputWithLabel.js SearchForm.js
~~~~~~~

*src/List.js* 파일과 공유하는 Item 컴포넌트를 제외한  *src/App.js*에 있던 모든 컴포넌트를 각 파일로 분리합니다. 이후 모든 파일에 import 구문으로 리액트를 불러오고, 만든 컴포넌트는 export 구문으로 내보내야합니다. 예를 들어, 아래의 *src/List.js* 파일이 있습니다.

{title="src/List.js",lang="javascript"}
~~~~~~~
# leanpub-start-insert
import React from 'react';
# leanpub-end-insert

const List = ({ list, onRemoveItem }) =>
  list.map(item => (
    <Item
      key={item.objectID}
      item={item}
      onRemoveItem={onRemoveItem}
    />
  ));

const Item = ({ item, onRemoveItem }) => (
  <div>
    <span>
      <a href={item.url}>{item.title}</a>
    </span>
    <span>{item.author}</span>
    <span>{item.num_comments}</span>
    <span>{item.points}</span>
    <span>
      <button type="button" onClick={() => onRemoveItem(item)}>
        Dismiss
      </button>
    </span>
  </div>
);

# leanpub-start-insert
export default List;
# leanpub-end-insert
~~~~~~~

List 컴포넌트만 Item 컴포넌트를 사용하기 때문에, Item 컴포넌트와 List 컴포넌트는 같은 파일에 보관해도 됩니다. 만약 다른 컴포넌트도 Item 컴포넌트를 사용하면 Item 컴포넌트 전용 파일을 따로 만드는 것이 좋습니다. *src/SearchForm.js* 파일의 SearchForm 컴포넌트는 반드시 InputWithLabel 컴포넌트를 import 구문으로 불러와야 합니다. Item 컴포넌트처럼 InputWithLabel 컴포넌트와 SearchForm을 같은 파일에 두어도 됩니다. 그러나 InputWithLabel 컴포넌트를 재사용하는 것이 목표이기 때문에 두 컴포넌트를 분리합니다. 이제 InputWithLabel 컴포넌트를 import 구문으로 불러와 사용할 수 있습니다.

{title="src/SearchForm.js",lang="javascript"}
~~~~~~~
# leanpub-start-insert
import React from 'react';
# leanpub-end-insert

# leanpub-start-insert
import InputWithLabel from './InputWithLabel';
# leanpub-end-insert

const SearchForm = ({
  searchTerm,
  onSearchInput,
  onSearchSubmit,
}) => (
  <form onSubmit={onSearchSubmit}>
    <InputWithLabel
      id="search"
      value={searchTerm}
      isFocused
      onInputChange={onSearchInput}
    >
      <strong>Search:</strong>
    </InputWithLabel>

    <button type="submit" disabled={!searchTerm}>
      Submit
    </button>
  </form>
);

# leanpub-start-insert
export default SearchForm;
# leanpub-end-insert
~~~~~~~

App 컴포넌트는 render 함수에서 사용되는 컴포넌트를 모두 import 구문으로 불러와야 합니다. InputWithLabel 컴포넌트는 SearchForm 컴포넌트에서만 사용되기 때문에 App 컴포넌트에서는 import하지 않아도 됩니다.

{title="src/App.js",lang="javascript"}
~~~~~~~
import React from 'react';
import axios from 'axios';

# leanpub-start-insert
import SearchForm from './SearchForm';
import List from './List';
# leanpub-end-insert

...

const App = () => {
  ...
};

export default App;
~~~~~~~

이제 다른 컴포넌트에서 사용되는 컴포넌트들은 각자 전용 파일을 갖고 있습니다. Item 컴포넌트와 List 컴포넌트처럼 하나의 컴포넌트에서만 사용되는 경우엔 두 컴포넌트를 같은 파일에 두어도 됩니다. 만약 InputWithLabel처럼 재사용되는 컴포넌트는 컴포넌트 전용 파일을 만듭니다. 이처럼 파일/폴더 계층을 구성하는 방법은 다양합니다. 대표적인 방법은 모든 컴포넌트들을 각자 전용 파일을 갖는 것입니다.  

{title="Project Structure",lang="text"}
~~~~~~~
- List/
-- index.js
- SearchForm/
-- index.js
- InputWithLabel/
-- index.js
~~~~~~~

폴더마다 있는 *index.js*는 컴포넌트의 정보를 담고 있는 파일입니다. 컴포넌트의 정보만 담고 있는 *index.js*와 달리 같은 폴더 안에 있는 다른 파일들은 스타일링, 테스팅, 타입 등의 역할을 맡고 있습니다.

{title="Project Structure",lang="text"}
~~~~~~~
- List/
-- index.js
-- style.css
-- test.js
-- types.js
~~~~~~~

만약 CSS 파일이 필요 없는 CSS-in-JS를 사용한다면,  styled component 전용 *style.js* 파일을 만들 수 있습니다. 

{title="Project Structure",lang="text"}
~~~~~~~
- List/
-- index.js
# leanpub-start-insert
-- style.js
# leanpub-end-insert
-- test.js
-- types.js
~~~~~~~

프로젝트가 커지면서 프로젝트 구조를 **기능-지향 폴더 구조**에서 **도메인-지향 폴더 구조**로 바꿔야 할 수도 있습니다.  도메인 특화 컴포넌트는 범용 *shared/* 폴더로 공유합니다.

{title="Project Structure",lang="text"}
~~~~~~~
- Messages.js
- Users.js
- shared/
-- Button.js
-- Input.js
~~~~~~~

폴더 구조가 점점 복잡해지면, 컴포넌트 별로 폴더를 분리하고 역할에 따라 파일을 만들 수 있습니다.

{title="Project Structure",lang="text"}
~~~~~~~
- Messages/
-- index.js
-- style.css
-- test.js
-- types.js
- Users/
-- index.js
-- style.css
-- test.js
-- types.js
- shared/
-- Button/
--- index.js
--- style.css
--- test.js
--- types.js
-- Input/
--- index.js
--- style.css
--- test.js
--- types.js
~~~~~~~

리액트 프로젝트의 구조를 구성하는 방법은 프로젝트의 크기, 폴더 복잡도, 폴더 중첩도 그리고 폴더의 역할에 따라  다양합니다.  그렇기 때문에 파일/폴더 구조에는 정답이 없습니다.

시간이 지나면서 프로젝트 구조에 대한 필요 사항도 점점 늘어납니다. 그래도 모든 소스들을 한 파일에 두는 것이 편하다면 그렇게 두셔도 괜찮습니다. 단, 중첩 폴더가 너무 많아지면 위치 파악 어려워지니 중첩 폴더의 남발은 지양합니다.

### 읽어보기

* [지난 장의 소스코드](https://codesandbox.io/s/github/the-road-to-learn-react/hacker-stories/tree/hs/React-Folder-Structure) 확인하기
  * [지난 장과 달라진 점](https://github.com/the-road-to-learn-react/hacker-stories/compare/hs/react-modern-final...hs/React-Folder-Structure?expand=1) 알아보기
* [자바스크립트 import 및 export 구문](https://www.robinwieruch.de/javascript-import-export)에 대해 더 읽어보기
* 현재 폴더 구조가 괜찮으면 그대로 두어도 괜찮습니다. 다음 섹션에서는 폴더 구조는 건드리지 않고 *src/App.js* 파일만 사용한 실습을 합니다.