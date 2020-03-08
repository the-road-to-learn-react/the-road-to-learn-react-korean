# 리액트와 스타일링

리액트 어플리케이션을 스타일링하는 데에는 여러 방법이 있고, 어떤 **스타일링 방식**이 가장 적절한지, 어떤 **접근**이 가장 효율적인지 등에는 여러 의견이 있습니다. 본문에서는 다양한 접근을 가볍게 한 번 다뤄보도록 하겠습니다. 찬성 의견도 있을 수 있고, 반대 의견도 있을 수 있겠지만, 각 개발자와 팀에게 무엇이 가장 잘 맞는지가 가장 중요합니다.

리액트 스타일링은 우선 기본적인 CSS를 다루는 것으로 시작하겠으나, 조금 더 나아가서는 CSS-in-CSS (CSS 모듈)과 CSS-in-JS (Styled Components) 방식을 다룰 예정입니다. CSS 모듈과 Styled Components는 각 접근 방식들 중의 두 가지 예시일 뿐입니다. 이에 더불어, 로고나 아이콘 등에 사용되는 SVG를 우리의 리액트 어플리케이션에 포함하는 방식도 다룰 예정입니다.

만약 버튼이나 대화창, 드롭다운 등의 일반적인 UI 컴포넌트를 직접 만들고 싶지 않다면, 이런 컴포넌트를 기본으로 제공하는 [UI 라이브러리](https://www.robinwieruch.de/react-libraries)를 사용하는 것도 좋은 방법입니다. 그러나, 리액트를 배우는 데에는 이런 솔루션을 활용하는 것보다는 직접 컴포넌트를 만들어보는 것이 더 도움이 됩니다. 이에, 우리도 이와 같은 라이브러리를 사용하지 않겠습니다.

후술할 스타일링 방법론과 SVG들은 `create-react-app`에 미리 설정되어 있습니다. 만약 Webpack 등의 빌드 툴을 활용하고 있다면, CSS나 SVG 파일을 불러오기 위해서 따로 설정을 해 줘야 할 수도 있습니다. 우리는 create-react-app을 사용할 것이기 때문에, 기존에 있던 파일들을 사용하면 됩니다.

## 리액트에서의 CSS

리액트에서 CSS를 사용하는 방법은 여러분이 이미 알고 있을 수도 있는, 기본 CSS 용법과 유사합니다. 각각의 웹 애플리케이션은 HTML 요소에 클래스 `class` 속성을 부여하고(리액트에서는 `className` 이라고 부릅니다), 별도의 CSS 파일에서 스타일링을 진행합니다.

{title="src/App.js",lang="javascript"}
~~~~~~~
const App = () => {
  ...

  return (
# leanpub-start-insert
    <div className="container">
      <h1 className="headline-primary">My Hacker Stories</h1>
# leanpub-end-insert

      <SearchForm
        searchTerm={searchTerm}
        onSearchInput={handleSearchInput}
        onSearchSubmit={handleSearchSubmit}
      />

      {stories.isError && <p>Something went wrong ...</p>}

      {stories.isLoading ? (
        <p>Loading ...</p>
      ) : (
        <List list={stories.data} onRemoveItem={handleRemoveStory} />
      )}
    </div>
  );
};
~~~~~~~

여기에서는 `<hr />` 태그가 없어졌는데, 이 선은 이제 CSS로 처리할 것이기 때문입니다. 우선 CSS 파일을 불러오도록 하겠습니다. 이는 create-react-app의 기본 설정에 포함되어 있습니다.

{title="src/App.js",lang="javascript"}
~~~~~~~
import React from 'react';
import axios from 'axios';

# leanpub-start-insert
import './App.css';
# leanpub-end-insert
~~~~~~~

이 CSS 파일은 우리가 App 컴포넌트에서 사용하고 있는 두 개의 (이후에 더 추가됩니다) CSS 클래스를 정의합니다. 여러분의 *src/App.css* 파일에서 이 클래스들을 다음과 같이 정의해주세요.

{title="src/App.css",lang="css"}
~~~~~~~
.container {
  height: 100vw;
  padding: 20px;

  background: #83a4d4; /* 구 버전 브라우저를 위한 대안입니다 */
  background: linear-gradient(to left, #b6fbff, #83a4d4);

  color: #171212;
}

.headline-primary {
  font-size: 48px;
  font-weight: 300;
  letter-spacing: 2px;
}
~~~~~~~

애플리케이션을 다시 시작하면 방금 작성한 스타일이 적용되는 것을 볼 수 있습니다. 다음으로는 Item 컴포넌트를 스타일링하도록 하겠습니다. `className` 속성을 가지고 있는 경우도 있지만, 지금은 다른 스타일링 방식을 사용해 보겠습니다.

{title="src/App.js",lang="javascript"}
~~~~~~~
const Item = ({ item, onRemoveItem }) => (
# leanpub-start-insert
  <div className="item">
    <span style={{ width: '40%' }}>
# leanpub-end-insert
      <a href={item.url}>{item.title}</a>
    </span>
# leanpub-start-insert
    <span style={{ width: '30%' }}>{item.author}</span>
    <span style={{ width: '10%' }}>{item.num_comments}</span>
    <span style={{ width: '10%' }}>{item.points}</span>
    <span style={{ width: '10%' }}>
# leanpub-end-insert
      <button
        type="button"
        onClick={() => onRemoveItem(item)}
# leanpub-start-insert
        className="button button_small"
# leanpub-end-insert
      >
        Dismiss
      </button>
    </span>
  </div>
);
~~~~~~~

여기에서 볼 수 있듯, HTML의 기본 요소인 `style` 속성을 사용할 수도 있습니다. JSX에서 스타일은 자바스크립트 객체로 전달될 수도 있습니다. 이것은 우리가 단순히 CSS에서 스타일을 정적으로 정의하는 것 뿐만 아니라, 자바스크립트 파일에서 유동적인 스타일 요소를 정의할 수도 있다는 것을 의미합니다. 이와 같은 방식은 빠르게 프로토타입을 제작할 때나 동적인 스타일 정의를 할 때 유용하며, **인라인 스타일(inline style)**이라고 부릅니다. 다만, 스타일링을 별도로 진행하는 것이 JSX 파일의 가독성을 높이기 때문에, 인라인 스타일은 유의하여 사용해야 합니다. 

*src/App.css* 파일에서 새로운 CSS 클래스를 정의합니다. 여기에서는 CSS의 기본 기능만 사용하도록 하겠습니다. 여러 다른 CSS 확장 프로그램 (예: Sass)에서 활용되는 고급 CSS 기능(예: 상속)들은 [선택적 환경설정](https://create-react-app.dev/docs/adding-a-sass-stylesheet/)에 포함되므로, 본 예시에서 다루지 않습니다.

{title="src/App.css",lang="css"}
~~~~~~~
.item {
  display: flex;
  align-items: center;
  padding-bottom: 5px;
}

.item > span {
  padding: 0 5px;
  white-space: nowrap;
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}

.item > span > a {
  color: inherit;
}
~~~~~~~

이전 컴포넌트에서의 버튼 스타일을 아직 정의하지 않았기 때문에, 여기에서 우리는 기본적인 버튼 스타일과 두 개의 더 구체적인 버튼 디자인 (작은 버튼과 큰 버튼)을 정의하도록 하겠습니다. 둘 중 하나는 이미 사용되었고, 나머지 하나는 앞으로 쓸 예정입니다.

{title="src/App.css",lang="css"}
~~~~~~~
.button {
  background: transparent;
  border: 1px solid #171212;
  padding: 5px;
  cursor: pointer;

  transition: all 0.1s ease-in;
}

.button:hover {
  background: #171212;
  color: #ffffff;
}

.button_small {
  padding: 5px;
}

.button_large {
  padding: 10px;
}
~~~~~~~

네이밍 컨벤션 ([CSS 가이드라인](https://developer.mozilla.org/en-US/docs/MDN/Contribute/Guidelines/Code_guidelines/CSS))은 리액트에서 사용되는 스타일링 방식과는 별개로 고려해야 합니다. 위에서 작성한 CSS 코드는 BEM 규칙을 따르기 때문에 클래스의 변형 형태는 언더스코어(`_`)로 표시합니다. 네이밍 컨벤션 자체는 여러분과 여러분의 팀에게 가장 적합한 것을 고르세요. 이제는 다음 컴포넌트로 넘어가겠습니다.

{title="src/App.js",lang="javascript"}
~~~~~~~
const SearchForm = ({ ... }) => (
# leanpub-start-insert
  <form onSubmit={onSearchSubmit} className="search-form">
# leanpub-end-insert
    <InputWithLabel ... >
      <strong>Search:</strong>
    </InputWithLabel>

    <button
      type="submit"
      disabled={!searchTerm}
# leanpub-start-insert
      className="button button_large"
# leanpub-end-insert
    >
      Submit
    </button>
  </form>
);
~~~~~~~

이와 유사하게, 우리는 `className`  속성을 리액트 컴포넌트의 prop으로 전달할 수도 있습니다. 예를 들면, SearchForm 컴포넌트에 동적인 스타일을 적용하기 위해 CSS 파일에 미리 정의되어 있는 여러 클래스를 `className` prop에 전달하는 방법을 사용할 수도 있습니다. 마지막으로는 InputWithLabel 컴포넌트를 스타일링합니다.

{title="src/App.js",lang="javascript"}
~~~~~~~
const InputWithLabel = ({ ... }) => {
  ...

  return (
    <>
# leanpub-start-insert
      <label htmlFor={id} className="label">
# leanpub-end-insert
        {children}
      </label>
      &nbsp;
      <input
        ref={inputRef}
        id={id}
        type={type}
        value={value}
        onChange={onInputChange}
# leanpub-start-insert
        className="input"
# leanpub-end-insert
      />
    </>
  );
};
~~~~~~~

In your *src/App.css* file, add the remaining classes:
*src/App.css* 파일에 나머지 클래스들을 추가합니다.

{title="src/App.css",lang="css"}
~~~~~~~
.search-form {
  padding: 10px 0 20px 0;
  display: flex;
  align-items: baseline;
}

.label {
  border-top: 1px solid #171212;
  border-left: 1px solid #171212;
  padding-left: 5px;
  font-size: 24px;
}

.input {
  border: none;
  border-bottom: 1px solid #171212;
  background-color: transparent;

  font-size: 24px;
}
~~~~~~~

이 경우에는 코드를 단순화시키기 위해 *src/App.css* 파일에서 label이나 input과 같은 HTML 요소들을 개별적으로 스타일링했습니다. 그러나 실제 애플리케이션을 개발할 때는 *src/index.css* 파일에서 전역적으로 이와 같은 요소들을 스타일링하는 것이 더 좋을 수도 있습니다. 리액트 컴포넌트는 여러 파일에 나눠져 있기 때문에, 스타일을 공유하는 것이 더더욱 중요해집니다.

지금까지 본 것은 우리가 대부분 이미 알고 있는 기본적인 CSS이고, 조금 더 동적인 활용을 위해서는 inline style을 사용합니다. 다만 Sass(Syntactically Awesome Style Sheets: 신택스가 멋진 스타일시트)와 같은 CSS 확장 기능을 활용하지 않는다면, 네이티브 CSS에서는 상속과 같은 기능을 제공하지 않으므로 inline style을 다루기는 번거로워질 수 있습니다.

### 실습하기

* [마지막 섹션의 소스 코드](https://codesandbox.io/s/github/the-road-to-learn-react/hacker-stories/tree/hs/CSS-in-React)를 확인하세요.
  * [마지막 섹션의 변경 사항](https://github.com/the-road-to-learn-react/hacker-stories/compare/hs/react-modern-final...hs/CSS-in-React?expand=1)을 확인하세요.
* [create-react-app에서의 CSS 스타일시트 활용](https://create-react-app.dev/docs/adding-a-stylesheet)에 대해 더 자세히 알아보세요.
* 상속과 같은 고급 CSS 기능을 활용하기 위해서는 [create-react-app에서의 Sass 활용](https://create-react-app.dev/docs/adding-a-sass-stylesheet)에 대해 더 자세히 알아보세요.
* App 컴포넌트에서 SearchForm 컴포넌트로 `className` prop을 전달해보세요. 값은 `button_small` 혹은 `button_large`로 설정하고, 이것은 버튼 요소의 `className`으로 지정해 주세요.