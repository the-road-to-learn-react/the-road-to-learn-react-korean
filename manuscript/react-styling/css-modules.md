## 리액트 CSS 모듈

CSS 모듈은 고급 **CSS-in-CSS**(.css파일에 작성한 CSS) 접근 방식입니다. CSS 파일은 그대로 유지되며  Sass와 같은 CSS 확장에도 적용할 수도 있습니다. 하지만 이것은 리액트 컴포넌트 안에서는 다르게 사용됩니다. CSS 모듈을 create-react-app에서 사용하기 위해서는 *src/App.css* 파일명을 *src/App.module.css*로 변경해야 합니다. 이 작업은 프로젝트 디렉토리의 command line에서 이루어 집니다:

{title="Command Line",lang="text"}
~~~~~~~
mv src/App.css src/App.module.css
~~~~~~~

이름이 변경된 *src/App.module.css*에서도 이전과 같이 첫 번째 CSS 클래스 정의로 시작합니다:


{title="src/App.module.css",lang="css"}
~~~~~~~
.container {
  height: 100vw;
  padding: 20px;

  background: #83a4d4; /* fallback for old browsers */
  background: linear-gradient(to left, #b6fbff, #83a4d4);

  color: #171212;
}

.headlinePrimary {
  font-size: 48px;
  font-weight: 300;
  letter-spacing: 2px;
}
~~~~~~~

상대 경로를 사용하여 *src/App.module.css* 파일을 다시 가져옵니다. 이번에는 사용자에게 있는 이름(여기서는 `styles`)을 자바스크립트 객체로 가져오시면 됩니다.

{title="src/App.js",lang="javascript"}
~~~~~~~
import React from 'react';
import axios from 'axios';

# leanpub-start-insert
import styles from './App.module.css';
# leanpub-end-insert
~~~~~~~

`className`을 CSS 파일에 매핑 된 문자열로 정의하는 대신 `styles` 객체에서 직접 CSS 클래스에 액세스하여 JSX 표현식의 자바스크립트를 사용하여 엘리먼트에 할당합니다.

{title="src/App.js",lang="javascript"}
~~~~~~~
const App = () => {
  ...

  return (
# leanpub-start-insert
    <div className={styles.container}>
      <h1 className={styles.headlinePrimary}>My Hacker Stories</h1>
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

`styles` 객체를 통해 여러 CSS 클래스를 element의 단일 `className` 속성에 추가하는 방법은 다양합니다. 여기서는 JavaScript 템플릿 리터럴을 사용합니다:

{title="src/App.js",lang="javascript"}
~~~~~~~
const Item = ({ item, onRemoveItem }) => (
# leanpub-start-insert
  <div className={styles.item}>
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
        className={`${styles.button} ${styles.buttonSmall}`}
# leanpub-end-insert
      >
        Dismiss
      </button>
    </span>
  </div>
);
~~~~~~~

우리는 또한 JSX에서 인라인 스타일을 더 동적인 스타일로 다시 추가 할 수 있습니다. CSS 중첩과 같은 고급 기능을 사용하기 위해 Sass와 같은 CSS 확장을 추가할 수도 있습니다. 하지만 우리는 CSS의 기존 특징들을 고수할 것입니다.

{title="src/App.module.css",lang="css"}
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

{title="src/App.module.css",lang="css"}
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

.buttonSmall {
  padding: 5px;
}

.buttonLarge {
  padding: 10px;
}
~~~~~~~

여기서는 css 섹션 에서의 `button_small` 및 `button_large`와 달리 BEM(Block Element Modifier) 네이밍 컨벤션으로의 전환이 있습니다. 만약 이전 네이밍 컨벤션이 유지 된다면 자바스크립트에서 객체에 밑줄 사용시 제한이 있기에  `styles['button_small']`을 사용하는 스타일에만 접근할 수 있습니다. 대시(`-`)로 정의된 클래스에 대해서도 동일한 단점이 적용됩니다. 우리는 `styles.buttonSmall`를 사용할 수 있습니다.(아래 파일을 참조하세요.).

{title="src/App.js",lang="javascript"}
~~~~~~~
const SearchForm = ({ ... }) => (
# leanpub-start-insert
  <form onSubmit={onSearchSubmit} className={styles.searchForm}>
# leanpub-end-insert
    <InputWithLabel ... >
      <strong>Search:</strong>
    </InputWithLabel>

    <button
      type="submit"
      disabled={!searchTerm}
# leanpub-start-insert
      className={`${styles.button} ${styles.buttonLarge}`}
# leanpub-end-insert
    >
      Submit
    </button>
  </form>
);
~~~~~~~
트
SearchForm 컴포넌도 스타일을 받습니다. JavaScript의 템플릿 리터럴을 통해 한 요소에 두 가지 스타일을 사용하기 위해서는 문자열을 사용해야 합니다. 한 가지 다른 방법은 커맨트 라인을 통해 프로젝트에 [classnames](https://github.com/JedWatson/classnames) 라이브러리를 설치하는 것입니다:

{title="src/App.js",lang="javascript"}
~~~~~~~
import cs from 'classnames';

...

// somewhere in a className attribute
className={cs(styles.button, styles.buttonLarge)}
~~~~~~~

[classnames](https://github.com/JedWatson/classnames) 라이브러리는 조건부 스타일도 제공합니다. 마침내 InputWithLabel 컴포넌트도 작성할 수 있게 되었습니다:

{title="src/App.js",lang="javascript"}
~~~~~~~
const InputWithLabel = ({ ... }) => {
  ...

  return (
    <>
# leanpub-start-insert
      <label htmlFor={id} className={styles.label}>
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
        className={styles.input}
# leanpub-end-insert
      />
    </>
  );
};
~~~~~~~

이제 *src/App.module.css* 파일에서 나머지 스타일을 완성하십시오:

{title="src/App.module.css",lang="css"}
~~~~~~~
.searchForm {
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

마지막 섹션과 동일한 주의사항이 있습니다: `input` 및 `label`과 같은 일부 스타일은 CSS 모듈보다 전역 *src/index.css* 파일에 작성하는게 더 효율적일 수 있습니다.

CSS 모듈은 다른 .css파일 안에 작성한 CSS와 마찬가지로 중첩과 같은 고급 CSS 기능에 Sass를 사용할 수 있습니다. CSS 모듈의 장점은 스타일이 정의되지 않을 때마다 자바스크립트에서 오류가 발생한다는 것입니다. 기존의 표준 CSS 접근 방식에서는 자바스크립트 및 CSS 파일의 스타일이 일치하지 않을 수 있습니다.

### Exercises:

* Confirm your [source code for the last section](https://codesandbox.io/s/github/the-road-to-learn-react/hacker-stories/tree/hs/CSS-Modules-in-React).
  * Confirm the [changes from the last section](https://github.com/the-road-to-learn-react/hacker-stories/compare/hs/react-modern-final...hs/CSS-Modules-in-React?expand=1).
* Read more about [CSS Modules in create-react-app](https://create-react-app.dev/docs/adding-a-css-modules-stylesheet).