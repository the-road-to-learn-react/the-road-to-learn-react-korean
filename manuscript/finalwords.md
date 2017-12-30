# 마치는 글

드디어 이 책의 마지막 장이다. 이 책을 읽는 모든 분들이 읽는 내내 즐겁길 바라며 리액트를 알아가는데 큰 도움이 되었길 바란다. 이 책이 마음에 들었다면 리액트를 배우고자 하는 친구들에게 추천해주길 바란다. 물론 무료로 알려줘야 한다.

**이 책을 마친 후 무엇을 해야할까?** 이 책을 다 마쳤다면 직접 더 많은 기능을 추가해 애플리케이션을 확장하거나 자신의 리액트 프로젝트를 시작할 수 있다. 다른 책, 강좌 또는 튜토리얼을 배우기 전에 스스로 나만의 프로젝트를 만들어봐야 한다. 일주일동안 작업해보고 배포한 애플리케이션이나 깃헙 리퍼지토리를 저자의 [트위터](https://twitter.com/rwieruch) 또는 [Github](https://github.com/rwieruch)으로 알려주길 바란다. 이 책을 통해 독자들이 어떤 것을 만들었는지 무척 궁금하다. 그리고 더 많은 독자들과 함께 기쁜 마음으로 공유하고자 한다.

애플리케이션 내 여러 기능을 추가해보고 싶다면, 앞으로 공부할 방향에 대해 제안하고자 한다.

* **state 관리** 지금까지 `this.setState()`와 `this.state`를 사용하여 로컬 컴포넌트 상태를 관리하고 액세스했다. 완벽한 시작이다. 규모가 큰 애플리케이션에서는 [리액트 로컬 컴포넌트 state의 한계](https://www.robinwieruch.de/learn-react-before-using-redux/)가 있다. 때문에 [Redux 또는 MobX](https://www.robinwieruch.de/redux-mobx-confusion/)와 같은 상태 관리 라이브러리를 사용할 수 있다. [React to React](https://roadtoreact.com/) 온라인 강좌 사이트 내 "리액트 State 다루기(Taming the State in React)" 강좌에서 리액트 내 로컬 state, Redux, Mobx를 다룬다. 이 과정은 전자책이 제공되며, 소스 코드와 화상 채팅도 모두 참여할 것을 권장한다. 이 책이 마음에 들면, 리액트 State 다루기 강좌를 확인해보기 바란다. 

* **샘플 프로젝트** 기본적인 리액트를 배운 후 새로운 내용으로 넘어가지 전에 프로젝트를 진행하면서 배운 내용을 적용하는 것이 좋다. 리액트로 tic-tac-toe 게임이나 간단한 계산기를 만들어 볼 수 있다. 리액트로 재미있는 것들을 만들 수 있는 튜토리얼이 많이 있다. 저자의 [페이지 매김 및 무한 스크롤 목록](https://www.robinwieruch.de/react-paginated-list/), [트위터 목록 표시](https://www.robinwieruch.de/react-svg-patterns/), [React 내 Stripe 결제연동](https://www.robinwieruch.de/react-express-stripe-payment/)을 참고하기 바란다. 작은 규모의 리액트 애플리케이션을 만들어 보면서 리액트에 익숙해지길 바란다.

* **코드 구성** 책을 읽는 중에 코드 구성에 대한 내용을 다뤘다. 아직 시도해보지 않았다면 지금 해보길 바란다. 컴포넌트를 구조화 된 파일 및 폴더 (모듈)로 구성합니다. 코드 분할, 재사용 가능성, 유지 보수성 및 모듈 API 디자인의 원칙을 이해하고 배우는 데 도움이 될 것이다.

* **테스트** 이 책은 테스트의 맛만 보여줬다. 테스트에 대해 잘 모른다면, 리액트 애플리케이션의 컨텍스트에서 단위 테스트 및 통합 테스트 개념을 깊이 파고들 수 있다. 구현 단계에서 Enzyme와 Jest로 단위테스트와 스냅샷 테스트를 지속적으로 진행하는 것이 좋다.

* **비동기 요청** 내부 fetch API 대신 타 라이브러리([superagent](https://github.com/visionmedia/superagent) 또는 [axios](https://github.com/mzabriskie/axios) 등)를 사용해 비동기 요청을 수행할 수 있다. 비동기 요청을 위한 완벽한 솔루션은 없다. 그러나 리액트를 중심으로 다른 라이브러리나 솔루션 등 블록을 교환해보면 [리액트의 유연성이 얼마나 강력한지](https://www.robinwieruch.de/reasons-why-i-moved-from-angular-to-react/) 알 수 있을 것이다. 일반적인 프레임 워크는 한 가지의 솔루션을 사용한다. [flexible ecosystem like React](https://www.robinwieruch.de/essential-react-libraries-framework/) 에서는 솔루션을 교환할 수 있다.

* **Routing** [react-router](https://github.com/ReactTraining/react-router)를 사용해 라우팅을 구현할 수 있다. 지금까지 만들어본 애플리케이션은 단일 페이지만 있었다. React Router는 여러 URL에서 여러 페이지를 가질 수 있도록 도와준다. 애플리케이션에 라우팅을 도입하면 웹 서버에 다음 페이지를 가져 오도록 요청하지 않아도 된다. 라우터는 클라이언트에서 모든 것을 처리한다.

* **Type 검사** 이 책에서는 PropType을 사용하여 컴포넌트 인터페이스를 정의했다. PropType를 사용해 버그를 방지하는 것이 좋다. 그러나 PropTypes는 런타임에서만 체크한다. 한 단계 더 나아가 컴파일 타임에 정적 Type 검사를 도입 할 수 있다. 그 중 [TypeScript](https://www.typescriptlang.org/)는 유명한 접근 방법이다. 이외에 [Flow](https://flowtype.org/)를 사용하기도 한다. 보다 강력한 애플리케이션을 만들기 위해 Flow를 사용하는 것이 좋다.


* **Webpack, Babel** 이 책에서는 *create-react-app*로 애플리케이션을 시작했다. 어느 정도 리액트에 익숙해지면 다른 툴들을 배우고 싶을 것이다. *create-react-app*가 없이 프로젝트를 초기 설정할 수 있다. [Webpack과 Babel](https://www.robinwieruch.de/minimal-react-webpack-babel-setup/)을 사용해 최소한의 설정을 따르는 것이 좋다. 이후에는 더 많은 도구를 직접 적용할 수 있다. 예를 들어, [ESLint](https://www.robinwieruch.de/react-eslint-webpack-babel/)를 사용해 애플리케이션 통합 코드 스타일을 따를 수 있다.

* **React Native** [React Native](https://facebook.github.io/react-native/)은 모바일 애플리케이션아 개발 가능하다. 리액트에서 학습한 내용을 토대로 iOS와 안드로이드 애플리케이션을 만들어 볼 수 있다. 리액트에 대해 이미 잘 알고 있다면  React Native에서 배울 것이 아주 많지 않다. React와 React Native 모두 원칙은 같다. 익숙한 웹 UI가 아닌 모바일 UI를 새로 접하게 될 것이다. 

웹 개발 및 소프트웨어 엔지니어링에 대한 더 흥미로운 주제를 찾으려면 [저자 웹 사이트](https://www.robinwieruch.de/)를 방문하길 바란다. [구독신청](https://www.getrevue.co/profile/rwieruch)을 하면 매달 업데이트 소식을 받을 수 있고, [후원자](https://www.patreon.com/rwieruch)가 되어 저자의 컨텐츠 제작을 지원할 수 있다. 이외에 [React to React](https://roadtoreact.com/) 온라인 강좌를 통해 리액트 생태계와 관련된 심화 과정을 제공한다. 강좌에 방문하길 바란다! 

이 책이 여러분의 마음에 들었다면, 리액트를 알았으면 하는 사람이 누구인지 생각해보는 시간을 가졌으면 한다. 그 사람에게 이 책을 나눈다면, 나에게 정말 큰 도움이 될 것이다. 이 책은 다른 사람들과 함께 나누고자 출판되었따. 더 많은 사람들이 내 책을 읽고 피드백을 주면서 책 내용 또한 개선될 것이다.

리액트 배움의 길을 읽어주신 여러분들에게 감사드리며,

로빈
