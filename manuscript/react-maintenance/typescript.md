## 리액트에서의 타입스크립트

자바스크립트와 리액트에서 타입스크립트를 활용하면 애플리케이션 개발에 좋은 점이 많습니다. 타입스크립트는 커맨드 라인이나 브라우저의 런타임이 아닌 IDE에서 컴파일할 때 타입(type) 오류가 발생합니다. 이는 자바스크립트 개발의 피드백 루프를 단축합니다. 개발 경험이 향상됨과 동시에, 모든 변수의 타입을 정의하므로 코드는 코드 자체로 문서화(self-documenting)가 되고 이해하기 쉬워집니다. 또한 코드 블록을 이동하거나 코드 베이스의 더 큰 리팩토링을 수행하기 훨씬 더 효율적입니다. 타입스크립트와 같은 정적 타입 언어(statically typed languages)는 자바스크립트처럼 동적 타입 언어(dynamically typed languages)보다 장점이 많아 유행하고 있습니다. [타입스크립트에 대해](https://www.typescriptlang.org/index.html) 더 알아보세요.

리액트에서 타입스크립트를 사용하기 위해, 커맨드 라인에서 애플리케이션에 타입스크립트와 해당 의존성(dependencies)을 설치하세요. 설치 과정에서 막히면 [create-react-app](https://create-react-app.dev/docs/adding-typescript/)의 공식 타입스크립트 설치 지침을 따르세요.

{title="Command Line",lang="text"}
~~~~~~~
npm install --save typescript @types/node @types/react
npm install --save typescript @types/react-dom @types/jest
~~~~~~~

그리고 모든 자바스크립트 파일(*.js*)을 타입스크립트 파일(*.tsx*)로 바꿉니다.

{title="Command Line",lang="text"}
~~~~~~~
mv src/index.js src/index.tsx
mv src/App.js src/App.tsx
~~~~~~~

커맨드 라인에서 개발 서버를 다시 시작하세요. 브라우저와 IDE에서 컴파일 오류가 발생할 수 있습니다. 동작하지 않는다면 에디터에 타입스크립트 플러그인이나 IDE의 확장 기능을 설치해보세요. 리액트에서 타입스크립트를 설정한 뒤, 사용자 정의 훅(custom hook)의 인수(arguments)에 타입을 지정하여 전체 *src/App.tsx* 파일에 대한 타입 안전성(type safety)을 높입니다.

{title="src/App.tsx",lang="javascript"}
~~~~~~~
const useSemiPersistentState = (
# leanpub-start-insert
  key: string,
  initialState: string
# leanpub-end-insert
) => {
  const [value, setValue] = React.useState(
    localStorage.getItem(key) || initialState
  );

  React.useEffect(() => {
    localStorage.setItem(key, value);
  }, [value, key]);

  return [value, setValue];
};
~~~~~~~

함수의 인수에 타입을 추가하는 것은 리액트보다는 자바스크립트와 관련한 것입니다. 함수에 자바스크립트의 문자열 원시 타입(primitives)인 인수 두 개를 전달하도록 지정합니다. 또한 함수가 `문자열`(상태)로 배열(`[]`)을 반환하게 하고, `상태 업데이트 함수`처럼 `값`을 사용해서 아무것도 반환하지 않도록 할 수도(`void`) 있습니다.

{title="src/App.tsx",lang="javascript"}
~~~~~~~
const useSemiPersistentState = (
  key: string,
  initialState: string
# leanpub-start-insert
): [string, (newValue: string) => void] => {
# leanpub-end-insert
  const [value, setValue] = React.useState(
    localStorage.getItem(key) || initialState
  );

  React.useEffect(() => {
    localStorage.setItem(key, value);
  }, [value, key]);

  return [value, setValue];
};
~~~~~~~

사용자 정의 훅의 타입 안정성을 높이기 위해서 함수 본문의 내부 리액트 훅에 타입을 정의하지는 않았습니다. 대부분 기본적으로 리액트 훅에 **타입 추론**(type inference)이 적용되기 때문입니다. 리액트 useState 훅의 *초기 상태*가 자바스크립트의 문자열 원시 타입이라면, 반환된 *현재 상태*는 문자열로 추론됩니다. 반환된 *상태 업데이트 함수*는 문자열을 인수로 사용하며 아무 것도 반환하지 않습니다.

{title="Code Playground",lang="javascript"}
~~~~~~~
const [value, setValue] = React.useState('React');
// value is inferred to be a string
// setValue only takes a string as argument
~~~~~~~

리액트 애플리케이션과 컴포넌트의 타입 안정성을 높이는 방법에는 여러 가지가 있습니다. 애플리케이션에서 리프(leaf) 컴포넌트의 prop과 상태(state)를 봅시다. 다음 예시에서 Item 컴포넌트는 prop으로 스토리 `item`과 콜백 핸들러 함수 `onRemoveItem`를 전달받습니다. 굳이 장황하게 코드를 써보면, 이전처럼 함수의 두 인수에 인라인으로 타입을 정의할 수 있습니다.

{title="src/App.tsx",lang="javascript"}
~~~~~~~
const Item = ({
  item,
  onRemoveItem,
# leanpub-start-insert
}: {
  item: {
    objectID: string;
    url: string;
    title: string;
    author: string;
    num_comments: number;
    points: number;
  };
  onRemoveItem: (item: {
    objectID: string;
    url: string;
    title: string;
    author: string;
    num_comments: number;
    points: number;
  }) => void;
}) => (
# leanpub-end-insert
  <div>
    ...
  </div>
);
~~~~~~~

코드에는 두 가지 문제가 있습니다. 장황하고 중복된 내용이지요. *src/App.js* 상단 컴포넌트 외부에 사용자 정의의 `Story` 타입을 정의하여 두 문제를 모두 해결해 봅시다.

{title="src/App.tsx",lang="javascript"}
~~~~~~~
# leanpub-start-insert
type Story = {
  objectID: string;
  url: string;
  title: string;
  author: string;
  num_comments: number;
  points: number;
};
# leanpub-end-insert

...

const Item = ({
  item,
  onRemoveItem,
# leanpub-start-insert
}: {
  item: Story;
  onRemoveItem: (item: Story) => void;
}) => (
# leanpub-end-insert
  <div>
    ...
  </div>
);
~~~~~~~

`item`은 `Story` 타입입니다. `onRemoveItem` 함수는 `Story` 타입의 `item`을 인수로 취하고 아무것도 반환하지 않습니다. 그다음 외부에서 Item 컴포넌트의 props를 정의하여 코드를 정리하세요.

{title="src/App.tsx",lang="javascript"}
~~~~~~~
# leanpub-start-insert
type ItemProps = {
  item: Story;
  onRemoveItem: (item: Story) => void;
};
# leanpub-end-insert

# leanpub-start-insert
const Item = ({ item, onRemoveItem }: ItemProps) => (
# leanpub-end-insert
  <div>
    ...
  </div>
);
~~~~~~~

지금까지 타입스크립트로 리액트 컴포넌트의 prop 타입을 정의하는 가장 보편적인 방법을 알아보았습니다. 이제 List 컴포넌트에서 prop에 동일한 방식으로 타입을 정의합니다.

{title="src/App.tsx",lang="javascript"}
~~~~~~~
type Story = {
  ...
};

# leanpub-start-insert
type Stories = Array<Story>;
# leanpub-end-insert

...

# leanpub-start-insert
type ListProps = {
  list: Stories;
  onRemoveItem: (item: Story) => void;
};
# leanpub-end-insert

# leanpub-start-insert
const List = ({ list, onRemoveItem }: ListProps) =>
# leanpub-end-insert
  list.map(item => (
    <Item
      key={item.objectID}
      item={item}
      onRemoveItem={onRemoveItem}
    />
  ));
~~~~~~~

`onRemoveItem` 함수는 `ItemProps`와 `ListProps`에서 두 번 타입이 선언되었습니다. 좀 더 확실하게 한다면 `OnRemoveItem`를 타입스크립트 타입으로 뽑아내어 독립적으로 정의하고, `onRemoveItem` prop 타입을 정의하는 두 부분에 재사용*할 수도* 있습니다. 그러나 컴포넌트를 다른 파일에 분할할수록 개발이 점점 어려워집니다. 그래서 이 부분에서는 중복을 유지합니다.

이미 `Story`와 `Stories` 타입이 있으므로 다른 컴포넌트에 맞춰 용도를 변경할 수 있습니다. `App` 컴포넌트의 콜백 핸들러에 `Story` 타입을 추가하세요.

{title="src/App.tsx",lang="javascript"}
~~~~~~~
const App = () => {
  ...

# leanpub-start-insert
  const handleRemoveStory = (item: Story) => {
# leanpub-end-insert
    dispatchStories({
      type: 'REMOVE_STORY',
      payload: item,
    });
  };

  ...
};
~~~~~~~

리듀서(reducer) 함수는 `state`와 `action` 타입을 관찰하는 것 외에도 `Story` 타입을 관리합니다. 객체와 객체의 형태 모두 리듀서 함수에 전달된 것을 알 수 있습니다.

{title="src/App.tsx",lang="javascript"}
~~~~~~~
# leanpub-start-insert
type StoriesState = {
  data: Stories;
  isLoading: boolean;
  isError: boolean;
};
# leanpub-end-insert

# leanpub-start-insert
type StoriesAction = {
  type: string;
  payload: any;
};
# leanpub-end-insert

# leanpub-start-insert
const storiesReducer = (
  state: StoriesState,
  action: StoriesAction
) => {
# leanpub-end-insert
  ...
};
~~~~~~~

`string`과 `any`(타입스크립트 **와일드카드**) 타입으로 정의한 `Action` 타입은 여전히 너무 광범위합니다. 액션을 구별할 수 없기 때문에 실제 타입의 안전성을 얻을 수 없습니다. 각 액션의 타입스크립트 타입을 **인터페이스**(interface)로 지정하고, 예시의 `StoriesAction`처럼 **유니온 타입**(union type)을 사용하는 것이 더 좋습니다.

{title="src/App.tsx",lang="javascript"}
~~~~~~~
# leanpub-start-insert
interface StoriesFetchInitAction {
  type: 'STORIES_FETCH_INIT';
}
# leanpub-end-insert

# leanpub-start-insert
interface StoriesFetchSuccessAction {
  type: 'STORIES_FETCH_SUCCESS';
  payload: Stories;
}
# leanpub-end-insert

# leanpub-start-insert
interface StoriesFetchFailureAction {
  type: 'STORIES_FETCH_FAILURE';
}
# leanpub-end-insert

# leanpub-start-insert
interface StoriesRemoveAction {
  type: 'REMOVE_STORY';
  payload: Story;
}
# leanpub-end-insert

# leanpub-start-insert
type StoriesAction =
  | StoriesFetchInitAction
  | StoriesFetchSuccessAction
  | StoriesFetchFailureAction
  | StoriesRemoveAction;
# leanpub-end-insert

const storiesReducer = (
  state: StoriesState,
  action: StoriesAction
) => {
  ...
};
~~~~~~~

스토리 상태와 현재 상태, 액션은 타입입니다. 이제 반환된 새로운 상태(추론된)의 타입은 안전합니다. 예를 들어 정의되지 않은 액션 타입을 가진 리듀서에게 액션을 보내면 타입 오류가 발생합니다. 또 스토리를 삭제하는 액션에 스토리가 아닌 것을 전달해도 타입 오류가 발생합니다.

반환된 List 컴포넌트에 대해서 App 컴포넌트의 반환문에 여전히 타입 안전성 문제가 있습니다. List 컴포넌트에 HTML을 감싸는 `div` 요소 또는 리액트 fragment를 사용하여 수정할 수 있습니다.

{title="src/App.tsx",lang="javascript"}
~~~~~~~
const List = ({ list, onRemoveItem }: ListProps) => (
# leanpub-start-insert
  <>
    {list.map(item => (
# leanpub-end-insert
      <Item
        key={item.objectID}
        item={item}
        onRemoveItem={onRemoveItem}
      />
# leanpub-start-insert
    ))}
  </>
# leanpub-end-insert
);
~~~~~~~

깃허브의 리액트에서의 타입스크립트 이슈에 따르면 이렇습니다. *"컴파일러의 한계로 함수 컴포넌트가 JSX 표현식이나 null 이외의 것을 반환할 수 없기 때문에 엘리먼트에 다른 타입을 지정할 수 없다는 난해한 오류 메시지가 표시됩니다."*

이제 이벤트와 콜백 핸들러가 있는 SearchForm 컴포넌트를 살펴봅시다.

{title="src/App.tsx",lang="javascript"}
~~~~~~~
# leanpub-start-insert
type SearchFormProps = {
  searchTerm: string;
  onSearchInput: (event: React.ChangeEvent<HTMLInputElement>) => void;
  onSearchSubmit: (event: React.FormEvent<HTMLFormElement>) => void;
};
# leanpub-end-insert

const SearchForm = ({
  searchTerm,
  onSearchInput,
  onSearchSubmit,
# leanpub-start-insert
}: SearchFormProps) => (
# leanpub-end-insert
  ...
);
~~~~~~~

보통 `React.ChangeEvent <HTMLInputElement>` 또는 `React.FormEvent <HTMLFormElement>` 대신 `React.SyntheticEvent`를 사용합니다. 다시 App 컴포넌트로 돌아가서 콜백 핸들러에 같은 타입을 적용합니다.

{title="src/App.tsx",lang="javascript"}
~~~~~~~
const App = () => {
  ...

  const handleSearchInput = (
# leanpub-start-insert
    event: React.ChangeEvent<HTMLInputElement>
# leanpub-end-insert
  ) => {
    setSearchTerm(event.target.value);
  };

  const handleSearchSubmit = (
# leanpub-start-insert
    event: React.FormEvent<HTMLFormElement>
# leanpub-end-insert
  ) => {
    setUrl(`${API_ENDPOINT}${searchTerm}`);

    event.preventDefault();
  };

  ...
};
~~~~~~~

InputWithLabel 컴포넌트만 남았습니다. 이 컴포넌트의 prop을 다루기 전에 리액트 useRef 훅의 `ref`를 살펴봅시다. 안타깝게도 반환 값은 추론되지 않습니다.

{title="src/App.tsx",lang="javascript"}
~~~~~~~
const InputWithLabel = ({ ... }) => {
# leanpub-start-insert
  const inputRef = React.useRef<HTMLInputElement>(null!);
# leanpub-end-insert

  React.useEffect(() => {
    if (isFocused && inputRef.current) {
      inputRef.current.focus();
    }
  }, [isFocused]);
~~~~~~~

반환된 `ref` 타입을 안전하게 만들었고, `focus` 메소드를 실행(읽기)만 하기 때문에 읽기 전용으로 타입을 지정했습니다. 리액트는 DOM 요소를 `current` 속성으로 설정합니다.

마지막으로 InputWithLabel 컴포넌트 prop의 타입 안전성을 확인합니다. 리액트의 특정 타입으로 `children` prop을 지정하고, 물음표로 **선택적 타입**임을 표시합니다.

{title="src/App.tsx",lang="javascript"}
~~~~~~~
# leanpub-start-insert
type InputWithLabelProps = {
  id: string;
  value: string;
  type?: string;
  onInputChange: (event: React.ChangeEvent<HTMLInputElement>) => void;
  isFocused?: boolean;
  children: React.ReactNode;
};
# leanpub-end-insert

const InputWithLabel = ({
  id,
  value,
  type = 'text',
  onInputChange,
  isFocused,
  children,
# leanpub-start-insert
}: InputWithLabelProps) => {
# leanpub-end-insert
  ...
};
~~~~~~~

`type`과 `isFocused` 속성은 모두 선택 사항입니다. 타입스크립트를 사용하면 이 속성들은 prop으로 전달하지 않아도 된다는 것을 컴파일러에 알릴 수 있습니다. `children` prop에 적용할 수 있는 타입스크립트 타입 정의 방법에는 여러 가지가 있으며, 그 중 리액트 라이브러리의 `React.ReactNode`가 가장 보편적입니다.

드디어 전체 리액트 애플리케이션에 타입스크립트를 적용했습니다. 이제 컴파일할 때에 타입 오류를 쉽게 찾을 수 있습니다. 리액트 애플리케이션에 타입스크립트를 활용할 때 함수의 인수에 타입을 정의하는 것부터 시작하세요. 이러한 함수는 바닐라 자바스크립트 함수, 사용자 정의 리액트 훅이나 리액트 함수 컴포넌트일 수 있습니다. 리액트를 사용할 때는 폼 엘리먼트와 이벤트, JSX의 특정 타입을 이해하는 것이 중요합니다.

### 실습하기

* [마지막 장의 소스 코드](https://codesandbox.io/s/github/the-road-to-learn-react/hacker-stories/tree/hs/TypeScript-in-React)를 확인하세요.
  * [마지막 장의 변경 사항](https://github.com/the-road-to-learn-react/hacker-stories/compare/hs/react-modern-final...hs/TypeScript-in-React?expand=1)을 확인하세요.
* [React + TypeScript Cheatsheet](https://github.com/typescript-cheatsheets/react-typescript-cheatsheet#reacttypescript-cheatsheets)를 살펴보세요. 이 장에서 보았던 가장 일반적인 사용 예시도 다룹니다. 당장 이해하지 못해도 상관 습니다.
* 계속해서 다음 장을 공부하면서 타입스크립트를 사용하여 타입을 없애거나 유지해보세요. 타입을 유지하는 경우, 컴파일 오류가 발생할 때마다 새 타입을 추가하세요.