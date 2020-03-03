## 리액트에서의 Styled Components

Styled Components는 몇 가지 **CSS-in-JS** 방식 중 하나로, 가장 인기가 많습니다. 자바스크립트 의존성 때문에 커맨드 라인에서 설치합니다.

{title="Command Line",lang="text"}
~~~~~~~
npm install styled-components
~~~~~~~

그리고 *src/App.js* 파일에서 Styled Components 라이브러리를 가져옵니다.

{title="src/App.js",lang="javascript"}
~~~~~~~
import React from 'react';
import axios from 'axios';
# leanpub-start-insert
import styled from 'styled-components';
# leanpub-end-insert
~~~~~~~

이름에서 알 수 있듯이 CSS-in-JS는 자바스크립트 파일에 CSS를 작성합니다. *src/App.js* 파일에서 첫 번째 스타일링 된 컴포넌트를 정의하세요.

{title="src/App.js",lang="javascript"}
~~~~~~~
const StyledContainer = styled.div`
  height: 100vw;
  padding: 20px;

  background: #83a4d4;
  background: linear-gradient(to left, #b6fbff, #83a4d4);

  color: #171212;
`;

const StyledHeadlinePrimary = styled.h1`
  font-size: 48px;
  font-weight: 300;
  letter-spacing: 2px;
`;
~~~~~~~

Styled Components에서는 자바스크립트 함수와 같이 자바스크립트 템플릿 리터럴(template literals)을 사용합니다. 백틱 사이의 모든 것을 인수(argument)로 볼 수 있고, `styled` 객체를 사용하여 필요한 모든 HTML 요소(예: div, h1)에 함수로 접근할 수 있습니다. 스타일로 함수를 호출하면 App 컴포넌트에서 사용할 수 있는 리액트 컴포넌트를 반환(return)합니다.

{title="src/App.js",lang="javascript"}
~~~~~~~
const App = () => {
  ...

  return (
# leanpub-start-insert
    <StyledContainer>
      <StyledHeadlinePrimary>My Hacker Stories</StyledHeadlinePrimary>
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
# leanpub-start-insert
    </StyledContainer>
# leanpub-end-insert
  );
};
~~~~~~~

이렇게 만든 리액트 컴포넌트는 일반적인 리액트 컴포넌트와 동일한 규칙을 따릅니다. 엘리먼트 태그 사이의 모든 것은 리액트 `children` prop으로 자동 전달됩니다. 이번에는 인라인 스타일(inline styles)을 사용하지 않고 전용 스타일로 Item 컴포넌트를 정의합니다. `StyledColumn`은 리액트 prop을 통해 동적으로 스타일이 적용됩니다.

{title="src/App.js",lang="javascript"}
~~~~~~~
const Item = ({ item, onRemoveItem }) => (
# leanpub-start-insert
  <StyledItem>
    <StyledColumn width="40%">
# leanpub-end-insert
      <a href={item.url}>{item.title}</a>
# leanpub-start-insert
    </StyledColumn>
    <StyledColumn width="30%">{item.author}</StyledColumn>
    <StyledColumn width="10%">{item.num_comments}</StyledColumn>
    <StyledColumn width="10%">{item.points}</StyledColumn>
    <StyledColumn width="10%">
      <StyledButtonSmall
# leanpub-end-insert
        type="button"
        onClick={() => onRemoveItem(item)}
      >
        Dismiss
# leanpub-start-insert
      </StyledButtonSmall>
    </StyledColumn>
  </StyledItem>
# leanpub-end-insert
);
~~~~~~~

가변적인 `width` prop은 스타일링 된 컴포넌트의 템플릿 리터럴에서 인라인 함수의 인수로 접근할 수 있습니다. 함수의 리턴 값은 문자열로 적용됩니다. 화살표 함수의 본문을 생략하면 즉시 반환할 수 있으므로 간결한 인라인 함수가 됩니다.

{title="src/App.js",lang="javascript"}
~~~~~~~
const StyledItem = styled.div`
  display: flex;
  align-items: center;
  padding-bottom: 5px;
`;

const StyledColumn = styled.span`
  padding: 0 5px;
  white-space: nowrap;
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;

  a {
    color: inherit;
  }

  width: ${props => props.width};
`;
~~~~~~~

CSS 중첩(nesting)과 같은 고급 기능은 기본적으로 Styled Components에서 사용할 수 있습니다. 중첩 요소에 접근할 수 있으며 `&` CSS 연산자를 사용하여 현재 요소를 선택할 수 있습니다.

{title="src/App.js",lang="javascript"}
~~~~~~~
const StyledButton = styled.button`
  background: transparent;
  border: 1px solid #171212;
  padding: 5px;
  cursor: pointer;

  transition: all 0.1s ease-in;

  &:hover {
    background: #171212;
    color: #ffffff;
  }
`;
~~~~~~~

`styled` 함수에 다른 컴포넌트를 전달하여 새로운 버전의 컴포넌트를 만들 수도 있습니다. 이전에 정의된 StyledButton 컴포넌트의 모든 기본 스타일이 새로운 버튼에 전달됩니다.

{title="src/App.js",lang="javascript"}
~~~~~~~
const StyledButtonSmall = styled(StyledButton)`
  padding: 5px;
`;

const StyledButtonLarge = styled(StyledButton)`
  padding: 10px;
`;

const StyledSearchForm = styled.form`
  padding: 10px 0 20px 0;
  display: flex;
  align-items: baseline;
`;
~~~~~~~

StyledSearchForm 처럼 스타일링 된 컴포넌트는 그것의 기본 엘리먼트(`form`, `button`)가 실제 HTML로 출력됩니다. 원래의 HTML 속성(`onSubmit`, `type`, `disabled`)을 계속 사용할 수 있습니다.

{title="src/App.js",lang="javascript"}
~~~~~~~
const SearchForm = ({ ... }) => (
# leanpub-start-insert
  <StyledSearchForm onSubmit={onSearchSubmit}>
# leanpub-end-insert
    <InputWithLabel
      id="search"
      value={searchTerm}
      isFocused
      onInputChange={onSearchInput}
    >
      <strong>Search:</strong>
    </InputWithLabel>

# leanpub-start-insert
    <StyledButtonLarge type="submit" disabled={!searchTerm}>
# leanpub-end-insert
      Submit
# leanpub-start-insert
    </StyledButtonLarge>
  </StyledSearchForm>
# leanpub-end-insert
);
~~~~~~~

마지막으로 InputWithLabel 컴포넌트입니다. 아직 스타일을 정의하지 않았습니다.

{title="src/App.js",lang="javascript"}
~~~~~~~
const InputWithLabel = ({ ... }) => {
  ...

  return (
    <>
# leanpub-start-insert
      <StyledLabel htmlFor={id}>{children}</StyledLabel>
# leanpub-end-insert
      &nbsp;
# leanpub-start-insert
      <StyledInput
# leanpub-end-insert
        ref={inputRef}
        id={id}
        type={type}
        value={value}
        onChange={onInputChange}
      />
    </>
  );
};
~~~~~~~

같은 파일에 해당 스타일을 정의합니다.

{title="src/App.js",lang="javascript"}
~~~~~~~
const StyledLabel = styled.label`
  border-top: 1px solid #171212;
  border-left: 1px solid #171212;
  padding-left: 5px;
  font-size: 24px;
`;

const StyledInput = styled.input`
  border: none;
  border-bottom: 1px solid #171212;
  background-color: transparent;

  font-size: 24px;
`;
~~~~~~~

스타일링 된 컴포넌트가 있는 CSS-in-JS 방식은 스타일 정의의 초점을 실제 리액트 컴포넌트로 바꿉니다. Styled Components는 중간 CSS 파일 없이 리액트 컴포넌트로 정의된 스타일입니다. 자바스크립트에서 스타일링 된 컴포넌트가 사용되지 않으면 IDE/에디터가 알려줍니다. Styled Components는 애플리케이션의 프로덕션 준비 과정에서 다른 자바스크립트 에셋(assets)과 함께 자바스크립트 번들 파일로 묶입니다. CSS-in-JS 방식에서는 별도의 CSS 파일은 없고 자바스크립트만 있습니다. CSS-in-JS와 CSS-in-CSS 방식은(예: Styled Components와 CSS Modules)은 리액트 개발자에게 인기 있습니다. 여러분과 여러분의 팀에 가장 적합한 것을 사용하세요.

### 실습하기

* [마지막 장의 소스 코드](https://codesandbox.io/s/github/the-road-to-learn-react/hacker-stories/tree/hs/Styled-Components-in-React)를 확인하세요.
  * [마지막 장의 변경 사항](https://github.com/the-road-to-learn-react/hacker-stories/compare/hs/react-modern-final...hs/Styled-Components-in-React?expand=1)을 확인하세요.
* 리액트의 [Styled Components](https://www.robinwieruch.de/react-styled-components)에 대해 자세히 알아보세요.
  * Styled Components를 사용할 때, 보통 전역 스타일에 대한 *src/index.css* 파일이 없습니다. Styled Components에서 전역 스타일을 사용하는 방법을 알아보세요.