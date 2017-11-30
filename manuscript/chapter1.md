# 리액트 시작하기

이번 장은 리액트 기초적인 내용을 다룬다. 아직도 리액트를 왜 배워야하는지 모르겠다면 이번 장을 통해 그 답을 찾을 수 있을 것이다. 리액트 애플리케이션 설치를 시작으로 리액트의 세계에 첫 발을 담궈 보자. JSX와 ReactDOM를 배우고 첫 번째 리액트 컴포넌트를 만들어 보자.

## 안녕, 내 이름은 리액트

**왜 리액트를 배워야 할까?** 최근 몇 년 간 단일 페이지 애플리케이션([SPA: Single Page Application](https://en.wikipedia.org/wiki/Single-page_application))이 각광받고 있다. Angular, Ember 및 Backbone 등 자바스크립트 프레임워크의 등장은 바닐라 자바스크립트(Vanilla JavaScript: 타 라이브러리나 프레임워크 사용 없이 순수한 자바스크립트로 개발하는 것을 말함)와 jQuery를 사용하지 않고도 최신 웹 응용 프로그램을 구축할 수 있게 만들었다. 이외에도 SPA 프레임워크는 매우 다양하다. Angular 2010, Backbone 2010, Ember 2011 등 배포된 대부분 SPA 프레임워크는 1세대이다.

리액트는 2013년 페이스북이 처음 공표했다. 리액트는 SAP 프레임워크가 아닌, 뷰 라이브러리(View Library)이다. 여기서 뷰(View)란 [MVC](https://de.wikipedia.org/wiki/Model_View_Controller) (Model–View–Controller, MVC는 소프트웨어 공학에서 사용되는 소프트웨어 디자인 패턴을 말함) 패턴의 'V'를 말한다. 뷰는 브라우저 내 특정 컴포넌트를 보여준다. 그러나 리액트를 이루는 전체는 하나의 단일 페이지 애플리케이션을 제작할 수 있게 해준다.

그렇다면 수많은 1세대 SPA 프레임워크 중 리액트를 선택해야하는 이유는 무엇일까? 1세대 프레임워크는 한 번에 많은 것을 해결하려고 노력했지만, 리액트는 뷰 레이어를 만드는 역할만 한다. 리액트는 프레임워크가 아닌 라이브러리이다. 뷰는 컴포넌트로서 다른 컴포넌트와 함께 계층구조를 이룬다.

리액트 장점은 여러 복잡한 기능을 추가하기전 오롯이 뷰 레이어에 집중하여 개발할 수 있다는 것이다. SPA를 집이라고 한다면 리액트는 집을 짓는 벽돌과 같다. 복잡한 애플리케이션을 개발할 때 한 벽돌씩 만드는 작업이 필요하다 두 가지 장점이 있다. 

단계별로 하나씩 벽돌을 쌓는 법을 배울 수 있다. 한꺼번에 모두 이해하는 것을 걱정할 필요가 없다. 처음부터 모든 구조를 갖춘 프레임워크와 다르다. 리액트는 그 중 첫 번째이며 더 많은 벽돌들과 연결된다.

모든 벽돌은 상호 교환이 가능하다. 때문에 리액트 생태계는 혁신적이다. 여러 솔루션들이 서로 경쟁하고 있으며 각자 상황에 맞게 가장 적합한 솔루션을 선택하면 된다.

SPA 프레임 워크의 1세대는 이미 상용화 단계에 이르렀고 더욱 견고했지만, 여전히 리액트는 혁신적이다. [페이스북은 물론이거니와, 에어비앤비, 넷플릭스](https://github.com/facebook/react/wiki/Sites-Using-React) 등 선도적인 테크 회사들이 리액트를 채택해 플랫폼 개발을 하고 있다. 그들 모두가 리액트와 생태계에 만족하고 있으며 리액트의 미래에 투자하고 있다.

리액트는 최근 웹 애플리케이션을 개발하는데 가장 좋은 선택이 될 것이다. 뷰 레이어만 제공하지만, [그러나 리액트 생태계는 모든 프레임워크와 서로 상호 교환 가능하다는 철학을 가지고 있다.](https://www.robinwieruch.de/essential-react-libraries-framework/) 리액트는 간결한 API, 놀라운 생태계, 훌륭한 커뮤니티를 갖추고 있다. ["왜 나는 앵귤러(Angular)에서 리액트로 옮겼는가](https://www.robinwieruch.de/reasons-why-i-moved-from-angular-to-react/)" 블로그에서 리액트를 선택한 개인적인 경험을 작성했다. 굳이 타 프레임워크나 라이브러리가 아닌, 리액트를 선택해 개발하는 이유를 스스로에게 묻는 것이 매우 중요하다. 결국 모든 사람들이 다음 해 리액트 행보에 대해 궁금해 할 것이다.  


### 읽어보기

* [[영문] 왜 나는 앵귤러에서 리액트로 옮겼는가](https://www.robinwieruch.de/reasons-why-i-moved-from-angular-to-react/)
* [[영문] 유연한 리액트 생태계](https://www.robinwieruch.de/essential-react-libraries-framework/)

## 준비사항

이전에 SPA 프레임워크나 라이브러리 사용 경험이 있다면 이미 웹 개발 기초 지식이 있을 것이다. 이제 막 웹 개발에 입문했다면 리액트를 배우기 전에 HTML, CSS, 자바스크립트 ES5를 잘 다룰 수 있어야 한다. 이 책에서 자바스크립트 ES6로 점차적으로 변환할 것이다.[공식 슬랙 그룹](https://slack-the-road-to-learn-react.wieruch.com/)에 가입해 동료들을 만나고 서로에게 도움을 주길 바란다.

### 코드에디터, 터미널

개발 환경은 어떻게 갖춰야할까? 코드에디터나 IDE, 터미널(terminal : 또는 커맨드라인(command line)라고도 함)이 필요하다. [개발 환경 설정 가이드](https://www.robinwieruch.de/developer-setup/)를 참고하라. MacOS을 대상으로 작성했으나 타 운영체제에 적용하여도 무방하다. 이미 개발 환경 설정 방법에 관한 수 많은 가이드 문서가 있기 때문에 사용 중인 운영체제에 알맞는 가이드를 쉽게 찾을 수 있을 것이다.

git와 깃헙(GitHub)을 사용하여 책에서 실습한 내용을 깃헙 저장소에 커밋하여 프로젝트를 계속해서 진행할 수 있다. 자세한 내용은 [GIT 가이드](https://www.robinwieruch.de/git-essential-commands/)를 읽기 바란다. git 사용은 의무 사항이 아니다. 웹 개발 초보자인 경우 모든 것을 한꺼번에 배우려고 하면 매우 부담스러울 수 있다. 때문에 초보자는 git과 깃헙을 쓰지말고 본문 내용에 집중하기 바란다.

### Node, NPM 설치

마지막으로 [노드(node) 및 npm] (https://nodejs.org/en/)을 설치해야한다. 라이브러리를 관리를 위해 사용된다. 이 책에서는 npm(노드 패키지 관리자: node package manager)을 통해 외부 노드 패키지를 설치한다. 노드 패키지는 라이브러리 또는 전체 프레임워크가 될 수 있다.

터미널에서 노드와 npm의 버전을 확인할 수 있다. 터미널에 출력되지 않으면 먼저 노드와 npm를 설치해야한다. 이 책을 쓰는 동안 현재 노드 버전은 아래와 같다. 


{title="Command Line",lang="text"}
~~~~~~~~
node --version
*v8.3.0
npm --version
*v5.5.1
~~~~~~~~

## node, npm

이 장에서는 node와 npm에서 약간의 충돌 과정이 있울 수 있다. 완벽하진 않지만 필요한 모든 도구를 얻을 수 있다. 이미 패키지 설치와 관리가 익숙하다면 이번 장을 건너뛰이번 장을 건너뛸 수 있다.

**노드 패키지 관리자** (npm) 명령어로 외부 **노드 패키지**를 로컬에 설치한다. 이들 패키지는 유틸리티 함수, 라이브러리 또는 프레임워크 세트 등이다. 패키지는 애플리케이션에 종속된다. 패키지는 전역 노드 패키지 폴더 또는 프로젝트 내 지역 폴더에 설치할 수 있다.

전역 노드 패키지는 모든 터미널에서 액세스 할 수 있으므로 전역 디렉토리 내 한 번만 설치해야한다. 아래 명령어로 글로벌 패키지를 설치해보자.

{title="Command Line",lang="text"}
~~~~~~~~
npm install -g <package>
~~~~~~~~

`-g` 플래그는 npm에게 패키지를 전역 설치하도록 지시한다. 지역 패키지는 응용 프로그램 내에서 사용된다. 리액트는 라이브러리로서 응용 프로그램에서 사용할 수 있는 로컬 패키지이다. 아래 명령어로 패키지를 설치하자.

{title="Command Line",lang="text"}
~~~~~~~~
npm install <package>
~~~~~~~~

리액트 설치는 아래와 같이 한다. :

{title="Command Line",lang="text"}
~~~~~~~~
npm install react
~~~~~~~~

설치된 패키지는 생성된 *node_modules/* 폴더에 저장되며 종속된 *package.json* 파일에 패키지 리스트가 나열된다.

 *package.json* 파일이 있어야만 npm 명령어로 *node_modules/* 폴더를 초기화할 수 있다. 아래 명령어로 새 지역 패키지를 설치한다.
 
{title="Command Line",lang="text"}
~~~~~~~~
npm init -y
~~~~~~~~

`-y` 표시는 *package.json*의 기본값을 초기화하는 단축기다. 플래그를 사용하지 않으면 파일을 구성하는 방법을 결정해야 한다. npm 프로젝트를 초기화 한 후에는`npm install <package>`를 통해 새 패키지를 설치하는 것이 좋다.

*package.json*에 대해 더 알아보자. *package.json*는 노드 패키지 전체 파일을 공유하지 않고도 다른 개발자와 프로젝트를 공유 할 수 있다. 이 파일 내 프로젝트에 사용된 노드 패키지 리스트가 모두 적혀 있있는데, 이러한 패키지를 종속성(dependencies)이라 부른다. 패키지 종속성없어도 프로젝트를 다운 받을 수 있다. 종속성은 *package.json*에 적혀있다. 프로젝트를 복사하고 `npm install`을 사용해 모든 패키지를 간단하게 설치할 수 있다. `npm install` 스크립트는 *package.json* 파일에 나열된 모든 의존성을 취하여 *node_modules/* 폴더에 설치한다. 배운 npm 명령을 한 번 더 입력해보자.

npm 명령어 하나가 더 남았다. :

{title="Command Line",lang="text"}
~~~~~~~~
npm install --save-dev <package>
~~~~~~~~

`--save-dev` 명령어는 노드 패키지가 개발(development)환경에서 사용되는 것을 말한다. 애플리케이션 배포(production) 시, 개발 환경과 달리 사용하지 않는 노드 패키지가 있다. 애플리케이션 테스트를 위한 노드 패키지가 그 예다. npm을 통해 해당 패키지를 설치할 수 있지만, 실제 프로덕션 환경에서는 제외해야 한다. 테스트는 개발 프로세스 중에만 수행되고, 실제 배포된 환경에서는 제외된다. 이를 위해 `--save-dev`를 사용된다.

앞으로 더 많은 npm 명령어를 다루게 될 것이지만 지금은 이 정도로 충분하다.

### 실습

* npm 프로젝트 설치하기
  * `mkdir <folder_name>`를 입력해 새 폴더 생성한다.
  * `cd <folder_name>` 생성한 폴더 내로 들어간다.
  * `npm init -y` 또는 `npm init` 을 실행한다.
  * `npm install react`를 입력해 리액트를 전역 패키지로 설치한다.
  * *package.json* 파일과 *node_modules/* 폴더가 생성되었는지 확인한다.
  * *react* 노드 패키지를 제거하고 다시 설치하는 방법을 스스로 찾아본다.

* [npm 문서](https://docs.npmjs.com/)를 읽어본다.

## 설치

리액트 애플리케이션 설치를 위한 몇 가지 방법이 있다.

첫 번째는 CDN을 사용하는 것이다. CDN이란 [콘텐츠 전송 네트워크(Content_delivery_network)](https://en.wikipedia.org/wiki/Content_delivery_network)를 말한다. 많은 회사들이 CDN를 사용해 라이브러리를 제공하고 있다. 번들링된 리액트 라이브러리는 단 *react.js* 파일 뿐이기 때문에 리액트도 라이브러리다. CDN을 별도로 호스팅하거나 응용 프로그램 내에 설치할 수 있다.

CDN을 사용해 리액트를 시작해보자. HTML 파일 내 `<script>` 인라인 태그로 CDN url을 작성한다. *react*와 *react-dom* 두 라이브러리 url을 추가한다.


{title="Code Playground",lang="javascript"}
~~~~~~~~
<script crossorigin src="https://unpkg.com/react@16/umd/react.development.js"></script>
<script crossorigin src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>
~~~~~~~~

npm으로 노드 패키지 설치할 때도 왜 CDN을 사용해야할까? 

애플리케이션 내 *package.json* 파일이 있다면, npm으로 *react* 및 *react-dom*을 설치하면 된다. 사전에 *npm init -y* 명령어를 입력해 * package.json*을 초기화해야한다. npm 한 줄로 여러 노드 패키지를 한 번에 설치해보자.

{title="Command Line",lang="text"}
~~~~~~~~
npm install react react-dom
~~~~~~~~

이 방법은 npm으로 관리 중인 현재 애플리케이션에 리액트를 추가할 때 사용한다.

안타깝게도 이 것이 전부는 아니다. JSX(리액트 문법)과 자바스크립트 ES6로 만든 애플리케이션은 [바벨(babel)](http://babeljs.io/)을 사용한다. 바벨은 각기 다른 브라우저 사양에서도 자바스크립트 ES6와 JSX를 사용할 수 있게 변환해준다. 모든 브라우저가 ES6 문법을 해석할 수 없다. 바벨 설치를 위해 많은 환경 설정과 도구가 필요하다 떄문에 리액트 초심자는 바벨 환경구성하는데 중압감을 느낄 수 있다.

이러한 이유로, 페이스북에서는  zero-configuration React 솔루션인 *create-react-app* 만들었다. 다음 장에서는 이 부트스트래핑 툴로 애플리케이션을 설치하는 방법을 다룰 것이다.

### 실습

* read more about [React installations](https://facebook.github.io/react/docs/installation.html)

## 제로 구성 설치

본 책에서는 [create-react-app](https://github.com/facebookincubator/create-react-app)으로 애플리케이션을 부트스트래핑한다. 2016년 페이스북이 제안한 리액트 제로 구성 설치 스타터 킷(Zero-Configuration Setup Starter Kit)이다. [한 조사](https://twitter.com/dan_abramov/status/806985854099062785)에 따르면 96% 이상 넘는 사람들이 리액트 초보자들에게 create-react-app을 추천한다고 말했다.

*create-react-app*에서는 도구 구현과 구성이 백그라운드에서 진행되며 애플리케이션 구현에 중점을 둔다.

시작하려면 글로벌 노드 패키지에 패키지를 설치해야한다. 이후에는 새로운 애플리케이션을 부트스트랩하려면 명령어를 입력해 실행하면 된다.

{title="Command Line",lang="text"}
~~~~~~~~
npm install -g create-react-app
~~~~~~~~

*create-reaction-app* 버전을 확인해 성공적으로 패키지가 설치되었는지 확인할 수 있다.


{title="Command Line",lang="text"}
~~~~~~~~
create-react-app --version
*v1.4.1
~~~~~~~~

이제 첫 번째 리액트 애플리케이션을 부트스트래핑해보자. 애플리케이션 이름을 *hackernews*라 하겠다. 물론 다른 이름을 사용해도 된다. 부트스트랩에는 몇 초가 걸린다. 그 후 폴더 안으로 이동한다.

{title="Command Line",lang="text"}
~~~~~~~~
create-react-app hackernews
cd hackernews
~~~~~~~~

이제 코드에디터에서 애플리케이션을 열 수 있다. 폴더 내 아래와 같이 *create-react-app* 구조가 보일 것이다.

{title="Folder Structure",lang="text"}
~~~~~~~~
hackernews/
  README.md
  node_modules/
  package.json
  .gitignore
  public/
    favicon.ico
    index.html
  src/
    App.css
    App.js
    App.test.js
    index.css
    index.js
    logo.svg
~~~~~~~~

각 파일과 폴더 단위가 무엇을 지칭하는 지 알아보자. 처음부터 모든 것을 이해하지 않아도 된다.

* **README.md:** .md 확장자는 파일이 마크다운(markdown) 파일이다. Markdown은 일반 텍스트와 함께 간단한 마크업 언어로 작성된다. 많은 소스 코드 프로젝트에는 *README.md* 파일에 프로젝트 설명 및 설치 등 안내 사항을 담고 있다. 깃헙 저장소 페이지에 진입하면 초기화면에  *README.md*이 먼저 보인다. *create-react-app*을 설치한 후 바로 깃헙에 프로젝트를 올린다면 *README.md*는 공식 [create-react-app 깃헙 저장소](https://github.com/facebookincubator/create-react-app)와 동일하다.

* **node_modules/:** 이 폴더에는 npm을 통해 설치되었던 모든 노드 패키지가 들어있다. 이미 *create-react-app*를 사용했으므로 몇 개의 노드 모듈이 설치되어 있어야 한다. 보통 이 폴더를 절대로 건드리지 않는다. npm 명령어를 사용해 패키지를 설치 및 제거한다.

* **package.json:** 이 파일에는 노드 패키지 종속성 및 기타 프로젝트 구성 목록을 포함한다.

* **.gitignore:** 이 파일은 git을 사용할 때 원격 git 저장소에 추가가 되지 않아야할 모든 파일과 폴더를 나타낸다. 예를 들어 *node_module*와 같이 로컬 프로젝트에만 있어야 한다. 종속성 폴더 전체를 올리지 않고도 공유된 *package.json* 파일만으로 종속성을 설치할 수 있다.

* **public/:** 배포를 위해 프로젝트를 빌드 할 때에  필요한 모든 파일이 들어 있다. 프로젝트 빌드 할 때, *src/* 폴더 내 모든 코드는 몇 개의 파일로 묶여 *public* 폴더에 배치된다.

따라서 위에서 언급된 파일 및 폴더를 건드릴 필요가 없다. 초기에는 필요한 모든 것이 *src/* 폴더에 있다. 중요한 파일은 *src/App.js* 로 리액트 컴포넌트가 있다. 현재 이 파일 전체가 애플리케이션을 이루고 있으나, 리액트 컴포넌트를 여러 파일로 분절하여 나눠 유지 관리할 수 있다.

이외에 테스트를 위한 *src/App.test.js* 파일과 리액트의 진입점이라 볼 수 있는 *src/index.js* 파일이 보일 것이다. 다음 장에서 두 파일이 어떤 것인지 알게 될 것이다. 또한 애플리케이션과 컴포넌트 스타일을 지정하는 *src/index.css* 및 *src/App.css* 파일이 있다. 현재는 기본 스타일만 적용되어 있다.

* *create-reaction-app* 애플리케이션은 npm 프로젝트이다. npm을 사용하여 프로젝트에 노드 패키지를 설치하고 제거 할 수 있다. 또한 npm 명령어를 사용할 수 있다.

{title="Command Line",lang="text"}
~~~~~~~~
// Runs the application in http://localhost:3000
npm start

// Runs the tests
npm test

// Builds the application for production
npm run build
~~~~~~~~

스크립트 명령어는 * package.json *에 정의되어 있다. 보일러플레이트 리액트 애플리케이션이 부트스트래핑된다. 이제 아래 실습을 통해 브라우저에서 애플리케이션이 구동되는 것을 확인해보자.

### 실습

* `npm start`을 실행해 브라우저에서 구동 중인 애플리케이션 확인한다.
* `npm test`를 실행해본다.
* *public/*폴더 내 어떤 내용이 있는지 확인하고, `npm run build`스크립트를 실행해 폴더에 파일이 추가되었는지 확인한다. (추가된 파일을 다시 제거할 수 있고 프로젝트에 영향을 주지 않는다.)
* 폴더 구조가 익숙해지도록 한다.
* 파일 내용이 익숙해지도록 한다.
* npm 스크립트 및 create-react-app에 대한 [자세한 내용](https://github.com/facebookincubator/create-react-app)을 읽어본다.


## JSX 들어가기

이제 JSX에 대해 알아보자. JSX는 리액트 구문이다. 앞서 언급했듯이 *create-react-app*로 애플리케이션을 부트스트래핑한다. 애플리케이션을 구현하는 기본적인 파일이 제공된다. 소스 코드를 살펴보자. *src/App.js* 파일을 먼저 만들어보자.

{title="src/App.js",lang=javascript}
~~~~~~~~
import React, { Component } from 'react';
import logo from './logo.svg';
import './App.css';

class App extends Component {
  render() {
    return (
      <div className="App">
        <header className="App-header">
          <img src={logo} className="App-logo" alt="logo" />
          <h1 className="App-title">Welcome to React</h1>
        </header>
        <p className="App-intro">
          To get started, edit <code>src/App.js</code> and save to reload.
        </p>
      </div>
    );
  }
}

export default App;
~~~~~~~~


import/export 및 클래스 선언을 먼저 하려고 서두르지 않아도 된다. 이 기능은 자바스크립트 ES6 기능이다. 다음 장에서 이들에 대해 더 자세히 알아볼 것이다.

`App`파일 내 **리액트 ES6 클래스 컴포넌트**를 볼 수 있다. 이 파일에서 바로 컴포넌트를 선언한다.
 일반적으로 컴포넌트를 선언한 후에는 애플리케이션 내 어느 곳이든지 다시 요소사용될 수 있다. 컴포넌트는 다른 말로 인스턴스를 사용한다. 다시 말하자면 컴포넌트가 인스턴스화 된다.

리턴하는 **요소(element) **는`render()`메소드에서 지정한다. 요소는 컴포넌트를 이룬다. 컴포넌트(componet), 인스턴스(Instance), 요소(element) 간의 차이를  알고 있다면 이해가 쉬울 것이다.

이제 `App` 컴포넌트가 인스턴스화 된 것을 볼 수 있다. 인스턴스가 되지 않았다면, 브라우저에서 렌더링 된 결과를 볼 수 없게 된다. App 컴포넌트는 선언되었지만, 다른 곳에 재사용되지 않았다. JSX문법인 `<App/>`를 사용하면 어느 컴포넌트에서나 인스턴스화하여 재사용할 수 있다.   

`render()` 블록 내 내용은 HTML과 비슷해보이지만 JSX 문법으로 작성한다. JSX를 사용하면 HTML과 자바스크립트를 결합해 쓸 수 있다. HTML과 자바스크립트를 분리해 사용해왔던 분들에게는 혼란스러울 수 있다. 때문에 JSX에서 기본 HTML을 처음 사용하도록 한다. 이제 `App` 파일에 있는 모든 내용을 지우자.


{title="src/App.js",lang=javascript}
~~~~~~~~
import React, { Component } from 'react';
import './App.css';

class App extends Component {
  render() {
    return (
      <div className="App">
        <h2>Welcome to the Road to learn React</h2>
      </div>
    );
  }
}

export default App;
~~~~~~~~

이제 자바스크립트 없이 `render()` 메소드로 HTML를 반환했다. "리액트 배움의 길에 오신 것을 환영합니다"라는 새 변수를 만들고, 이 변수를 JSX 문법인 중괄호(`{}`) 안에 넣어보자.

{title="src/App.js",lang=javascript}
~~~~~~~~
import React, { Component } from 'react';
import './App.css';

class App extends Component {
  render() {
# leanpub-start-insert
    var helloWorld = '리액트에 오신 여러분을 환영합니다';
# leanpub-end-insert
    return (
      <div className="App">
# leanpub-start-insert
        <h2>{helloWorld}</h2>
# leanpub-end-insert
      </div>
    );
  }
}

export default App;
~~~~~~~~

다시 `npm start` 명령어를 실행해 애플리케이션을 시작해보자.

아마 `className` 속성(attribute)이란 것을 눈치챘을 것이다. 
404/5000
이것은 HTML의 표준`class` 속성에 영향을 ㅂ다았다. 기술적인 이유로 JSX는 몇 가지 내부 HTML 속성을 대체해야했다. 리액트 공식문서에서 [지원하는 HTML 속성](https://facebook.github.io/react/docs/dom-elements.html)을 확인 할 수 있다. 모두 카멜케이스(camelCase) 표기법을 따른다. 앞으로 리액트를 배우면서 JSX 특정 속성을 좀더 살펴볼 것이다.

### 실습

* JSX 안에 새 변수를 만들고 렌더링한다.
  * 객체를 사용해 유저의 성과 이름을 나타낸다.
  * JSX 내 유저 속성을 렌더링한다. 
* [JSX](https://facebook.github.io/react/docs/introducing-jsx.html)에 대해 읽어본다. 
* [리액트 컴포넌트, 요소, 인스턴스](https://facebook.github.io/react/blog/2015/12/18/react-components-elements-and-instances.html)에 대해 읽어본다.

## ES6 const, let

앞에서 `var` 문을 사용해 변수 `helloWorld`를 선언했다. 자바스크립트 ES6의 변수 선언은 `const`또는 `let`으로 한다. ES6에서는 `var` 사용이 극히 드물다.

`const`로 선언된 변수는 다시 할당하거나 선언 할 수 없다. 수정되거나 변경될 수 없다. 불변 데이터 구조(immutable data structures)이다. 데이터 구조가 정의되면, 다시 변경할 수 없다.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// not allowed
const helloWorld = 'Welcome to the Road to learn React';
helloWorld = 'Bye Bye React';
~~~~~~~~

반면 `let`은 변경 가능하다.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// allowed
let helloWorld = 'Welcome to the Road to learn React';
helloWorld = 'Bye Bye React';
~~~~~~~~

변수를 다시 할당해야 할 때 사용할 수 있다.

그러나, `const` 사용에 주의해야한다. `const`로 선언된 변수는 수정할 수 없다. 그러나 변수가 배열이나 객체일 경우, 갖고 있는 변수는 수정할 수 있다. 보유하고 있는 값은 변경할 수 없다.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// allowed
const helloWorld = {
  text: 'Welcome to the Road to learn React'
};
helloWorld.text = 'Bye Bye React';
~~~~~~~~

그럼 어느 상황에서 `let`과 `const`를 사용해야할까? 사용법에 대한 의견은 서로 분분하다. 최대한 `const`를 사용하려고 노력하는 것이 좋다. 객체와 배열의 값이 변경 될 수 있더라도 데이터 구조를 변경하지 않으려는 것을 말한다. 변수를 수정하려면 `let`을 사용할 수 있다.

불변성은 리액트와 그 생태계가 따르고 있다. 그래서 변수를 정의 할 때 `const`가 우선시 되어야 한다. 객체가 복잡해지면 내부 값을 수정해야되는 경우도 있으니 주의해라. 

응용 프로그램에서는`const` 대신`var`을 사용해야한다.

{title="src/App.js",lang=javascript}
~~~~~~~~
import React, { Component } from 'react';
import './App.css';

class App extends Component {
  render() {
# leanpub-start-insert
    const helloWorld = 'Welcome to the Road to learn React';
# leanpub-end-insert
    return (
      <div className="App">
        <h2>{helloWorld}</h2>
      </div>
    );
  }
}

export default App;
~~~~~~~~

### 실습

* [ES6 const](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const)에 더 읽어본다.
* [ES6 let](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let)에 대해 읽어본다.
* 불변 데이터 구조에 대해 더 조사한다.
  * 일반적으로 프로그래밍에서 말하는 불편 데이터 구조란 무엇인지 알아본다.
  * 리액트와 생테계에서 사용되는 이유에 대하 알아본다.

## ReactDOM

App 컴포넌트를 다루기 전에 어디에 사용되고 있는지 확인해 보자. 리액트의 첫 진입점이라 할 수 있는 *src/index.js* 파일에 있다는 것을 확인할 수 있을 것이다.

{title="src/index.js",lang=javascript}
~~~~~~~~
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
import './index.css';

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
~~~~~~~~

대게 `ReactDOM.render()`는 HTML 내 DOM 노드를 JSX로 대체한다. 이 때문에 타 애플리케이션에 리액트가 쉽게 통합될 수 있는 것이다. 애플리케이션 내 `ReactDOM.render()`를 여러 번 사용할 수 있다. 원하는 곳마다 JSX 구문, 단일 리액트 컴포넌트, 다중 리액트 컴포넌트, 또는 전체 응용 프로그램을 부트스트랩 할 수 있습니다. 그러나 일반 리액트 애플리케이션은 관용적으로 전체 컴포넌트 트리를 부트스트랩하기 위해 `ReactDOM.render()`를 한 번만 사용한다.

`ReactDOM.render()`에는 두 개의 인자가 필요하다. 첫 번째 인자는 렌더링된 JSX, 두 번째 인자는 리액트 애플리케이션이 HTML에 들어갈 위치를 지정한다. `id='root'`에 들어가게 된다. *public/index.html* 파일을 열어 이 id 속성을 확인할 수 있다. 

`ReactDOM.render()`실행되면 이미 App 컴포넌트를 사용하고 있다. 그러나 되도록 가능한 간단한 JSX로 만들어 전달하는 것이 좋다. 컴포넌트를 인스턴스화 시킬 필요는 없다.

{title="Code Playground",lang=javascript}
~~~~~~~~
ReactDOM.render(
  <h1>안녕 리액트</h1>,
  document.getElementById('root')
);
~~~~~~~~

### 실습

* *public/index.html*을 열어 HTML 안에 리액트 애플리케이션 들어가는 곳을 찾아본다.
* [리액트 공식문서 - 리액트 내 렌더링되는 요소](https://facebook.github.io/react/docs/rendering-elements.html)에 관해 더 읽어본다

## Hot Module Replacement

개발자로서 개발 경험을 향상하기 위해 *src/index.js*에서 할 수 있는 일 한 가지가 있다. 옵션사항이기 때문에 리액트 초심자라면 이 부분을 꼭 알고 있을 필요는 없다.

*create-react-app*은 소스 코드 변경 시, 자동으로 브라우저를 새로고침해준다는 장점을 가지고 있다. *src/App.js* 파일 내 `helloWorld`를 다른 변수로 변경하라. 브라우저가 새로고침되어야 된다. 그러나 더 좋은 방법이 있다.

Hot Module Replacement(HMR)란 브라우저 내 애플리케이션을 재실행하기 위한 도구이다. 브라우저는 페이지 새로고침을 수행하지 않는다. *create-reaction-app*에서 쉽게 활성화 할 수 있습니다. *src/index.js*에서 진입점이 리액트를 가리키려면 아래와 같이 약간의 설정을 추가해야한다.

{title="src/index.js",lang=javascript}
~~~~~~~~
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
import './index.css';

ReactDOM.render(
  <App />,
  document.getElementById('root')
);

# leanpub-start-insert
if (module.hot) {
  module.hot.accept();
}
# leanpub-end-insert
~~~~~~~~

위 코드가 전부다. *src/App.js* 파일에서 `helloWorld` 변수를 다른 것으로 바꿔보자. 브라우저는 페이지 새로고침되지 않지만, 애플리케이션이 재실행되어 올바른 결과가 출력된다. HMR은 여러 장점을 가지고 있다.

`console.log()`문으로 코드를 디버깅한다고 생각해보자. 코드를 수정해도 브라우저 페이지가 새로고침되지 않기 떄문에 개발자 콘솔에 그대로 남아있다. 디버깅 목적이라면 편리하다.

애플리케이션이 성장하면서 페이지 새로고침은 생산성을 지연시킨다. 페이지가 로드될 떄까지 기다려야된다. 대규모 애플리케이션에서 페이지를 다시 로드하는데 몇 초 이상이 걸릴 수 있다. HMR은 이러한 단점을 해결한다.

가장 큰 장점은 HMR로 애플리케이션 상태를 유지할 수 있다는 것이다. 애플리케이션에 3단계의 대화창이 있다고 가정해보자. 다이얼로그 창 기능이라고 생각하면 된다. HMR이 없다면 소스 코드를 변경할 때마다 브라우저 페이지가 새로고침된다. 창을 다시 열어 1 단계에서 3 단계로 이동시켜야하는 번거로운 일을 해야된다. HMR을 사용하면 다이얼로그 창은 3 단계 상태로 유지된다. 소스코드가 수정되어도 애플리케이션 내 상태가 유지된다. 애플리케이션 자체는 다시 실행되지만 페이지는 새로고침되지 않는다.

### 실습

* *src/App.js* 소스코드를 수정해 HMR이 어떻게 실행되는지 확인한다.
* 댄 애브라몹(Dan Abramov)의 [리액트 라이브 : Hot Reloading으로 시간 여행 떠나기](https://www.youtube.com/watch?v=xsSnOQynTHs) 초반 10분간 시청한다.

## JSX 내 복잡한 자바스크립트

App 컴포넌트로 다시 돌아가보자 지금까지 JSX에서 초기 변수를 렌더했다. 이제 리스트 내의 원소를 렌더링 해보자 처음 리스트는 샘플 데이터이지만 나중에 외부 [API] (https://www.robinwieruch.de/what-is-an-api-javascript/)를 통해 데이터를 가져오기도 한다. 나중에 이 부분을 해보게되면 점점 재밌어 질 것이다.

먼저 리스트 내 목록을 정의하자.

{title="src/App.js",lang=javascript}
~~~~~~~~
import React, { Component } from 'react';
import './App.css';

# leanpub-start-insert
const list = [
  {
    title: 'React',
    url: 'https://facebook.github.io/react/',
    author: 'Jordan Walke',
    num_comments: 3,
    points: 4,
    objectID: 0,
  },
  {
    title: 'Redux',
    url: 'https://github.com/reactjs/redux',
    author: 'Dan Abramov, Andrew Clark',
    num_comments: 2,
    points: 5,
    objectID: 1,
  },
];
# leanpub-end-insert

class App extends Component {
  ...
}
~~~~~~~~

샘플 데이터는 나중에 API에서 가져올 데이터로 교체할 수 있다. 리스트 항목은 제목(title), url, 작성사(author)가 있다. 그외에 또한 식별자(objectID), 기사의 인기도를 나타내는 점수(point), 댓글 수(num_comments)가 있다.

이제 JSX에서  자바스크립트 내장함수인 `map`을 사용해 리스트 내 항목을 출력하는 반복문을 만들어보자. JSX에서 자바스크립트 표현식을 캡슐화하기 위해 중괄호(`{}`)를 사용한다.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {
  render() {
    return (
      <div className="App">
# leanpub-start-insert
        {list.map(function(item) {
          return <div>{item.title}</div>;
        })}
# leanpub-end-insert
      </div>
    );
  }
}

export default App;
~~~~~~~~

JSX 내 HTML과 자바스크립트를 함께 사용하는 것은 매우 강력하다. `map`를 사용하면 특정 항목을 다른 항목으로 변환해 출력할 수 있다. 이번에는 `map`를 사용해 HTML 태그와 섞어 변환해 볼 것이다.

지금까지 각 항목의 `title`만 보여졌다. 이제 모든 항목의 속성을 표시해보자.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {
  render() {
    return (
      <div className="App">
# leanpub-start-insert
        {list.map(function(item) {
          return (
            <div>
              <span>
                <a href={item.url}>{item.title}</a>
              </span>
              <span>{item.author}</span>
              <span>{item.num_comments}</span>
              <span>{item.points}</span>
            </div>
          );
        })}
# leanpub-end-insert
      </div>
    );
  }
}

export default App;
~~~~~~~~

이제 `map()` 함수로 JSX 내 반복구문을 간단하게 처리할 수 있다는 것을 알 수 있을 것이다. 각 항목은 HTML `<span>` 태그로 감쌌다. url은 a 태그의 href 속성과 함께 사용했다.

리액트는 각 항목을 완벽히 표시했다. 그러나 아직 모든 것이 끝난 것은 아니다 한 가지 도우미를 추가해 잠재요소를 포용하고 다른 속성을 축추가해 React에 대한 한 가지 도우미를 추가하여 잠재력을 완전히 포용하고 성능을 향상시켜야 한다. 바로 각 리스트 요소마다 `key` 속성을 추가해야한다. 이렇게 해야 리액트는 리스트내 항목이 변경될 떄마다 수정 및 제거된 항목을 식별 할 수 있게 된다. 샘플 데이터 리스트 항목에는 식별자(`objectID`)가 있어 이를 `key`로 지정하면 된다. 

{title="src/App.js",lang=javascript}
~~~~~~~~
{list.map(function(item) {
  return (
# leanpub-start-insert
    <div key={item.objectID}>
# leanpub-end-insert
      <span>
        <a href={item.url}>{item.title}</a>
      </span>
      <span>{item.author}</span>
      <span>{item.num_comments}</span>
      <span>{item.points}</span>
    </div>
  );
})}
~~~~~~~~

키 속성 값은 고유하며 안정적인 식별자를 사용해야한다. 배열의 인덱스값은 고정값이 아니기 떄문에 사용하지 말아야 한다. 목록의 순서가 변경되면, 리액트는 항목을 식별하기 어렵게 되기 떄문이다.

{title="src/App.js",lang=javascript}
~~~~~~~~
// 이렇게 사용하면 안된다
{list.map(function(item, key) {
  return (
    <div key={key}>
      ...
    </div>
  );
})}
~~~~~~~~

이제 두 배열 내 모든 항목이 보인다. 앱을 시작해 브라우저를 열어 변경된 내용을 확인해보자.

### 실습하기

* [리액트 리스트(lists)와 키(keys)](https://facebook.github.io/react/docs/lists-and-keys.html)에 관해 읽어본다.
* [자바스크립트 기본 내장 배열 함수](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) 내용을 복습한다.
* JSX 내 자바스크립트 표현식(expression)을 작성해본다.

## ES6 Arrow Functions

JavaScript ES6 introduced arrow functions. An arrow function expression is shorter than a function expression.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// function expression
function () { ... }

// arrow function expression
() => { ... }
~~~~~~~~

But you have to be aware of its functionalities. One of them is a different behavior with the `this` object. A function expression always defines its own `this` object. Arrow function expressions still have the `this` object of the enclosing context. Don't get confused when using `this` in an arrow function.

There is another valuable fact about arrow functions regarding the parenthesis. You can remove the parenthesis when the function gets only one argument, but have to keep them when it gets multiple arguments.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// allowed
item => { ... }

// allowed
(item) => { ... }

// not allowed
item, key => { ... }

// allowed
(item, key) => { ... }
~~~~~~~~

However, let's have a look at the `map` function. You can write it more concisely with an ES6 arrow function.

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
{list.map(item => {
# leanpub-end-insert
  return (
    <div key={item.objectID}>
      <span>
        <a href={item.url}>{item.title}</a>
      </span>
      <span>{item.author}</span>
      <span>{item.num_comments}</span>
      <span>{item.points}</span>
    </div>
  );
})}
~~~~~~~~

Additionally, you can remove the *block body*, meaning the curly braces, of the ES6 arrow function. In a *concise body* an implicit return is attached. Thus you can remove the return statement. That will happen more often in the book, so be sure to understand the difference between a block body and a concise body when using arrow functions.

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
{list.map(item =>
# leanpub-end-insert
  <div key={item.objectID}>
    <span>
      <a href={item.url}>{item.title}</a>
    </span>
    <span>{item.author}</span>
    <span>{item.num_comments}</span>
    <span>{item.points}</span>
  </div>
# leanpub-start-insert
)}
# leanpub-end-insert
~~~~~~~~

Your JSX looks more concise and readable now. It omits the function statement, the curly braces and the return statement. Instead a developer can focus on the implementation details.

### 읽어보기

* read more about [ES6 arrow functions](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Arrow_functions)

## ES6 Classes

자바스크립트 ES6는 클래스를 도입했다. A class is commonly used in object-oriented programming languages. JavaScript was and is very flexible in its programming paradigms. You can do functional programming and object-oriented programming side by side for their particular use cases.

Even though React embraces functional programming, for instance with immutable data structures, classes are used to declare components. They are called ES6 class components. React mixes the good parts of both programming paradigms.

Let's consider the following Developer class to examine a JavaScript ES6 class without thinking about a component.

{title="Code Playground",lang="javascript"}
~~~~~~~~
class Developer {
  constructor(firstname, lastname) {
    this.firstname = firstname;
    this.lastname = lastname;
  }

  getName() {
    return this.firstname + ' ' + this.lastname;
  }
}
~~~~~~~~

A class has a constructor to make it instantiable. The constructor can take arguments to assign it to the class instance. Additionally a class can define functions. Because the function is associated with a class, it is called a method. Often it is referenced as a class method.

The Developer class is only the class declaration. You can create multiple instances of the class by invoking it. It is similar to the ES6 class component, that has a declaration, but you have to use it somewhere else to instantiate it.

Let's see how you can instantiate the class and how you can use its methods.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const robin = new Developer('Robin', 'Wieruch');
console.log(robin.getName());
// output: Robin Wieruch
~~~~~~~~

React uses JavaScript ES6 classes for ES6 class components. You already used one ES6 class component.

{title="src/App.js",lang=javascript}
~~~~~~~~
import React, { Component } from 'react';

...

class App extends Component {
  render() {
    ...
  }
}
~~~~~~~~

The App class extends from `Component`. Basically you declare the App component, but it extends from another component. What does extend mean? In object-oriented programming you have the principle of inheritance. It is used to pass over functionalities from one class to another class.

The App class extends functionality from the Component class. To be more specific, it inherits functionalities from the Component class. The Component class is used to extend a basic ES6 class to a ES6 component class. It has all the functionalities that a component in React needs to have. The render method is one of these functionalities that you have already used. You will learn about other component class methods later on.

The `Component` class encapsulates all the implementation details of a React component. It enables developers to use classes as components in React.

The methods a React `Component` exposes is the public interface. One of these methods has to be overridden, the others don't need to be overridden. You will learn about the latter ones when the book arrives at lifecycle methods in a later chapter. The `render()` method has to be overridden, because it defines the output of a React `Component`. It has to be defined.

이제 자바스크립트 ES6 클래스 기초와 이를 리액트 컴포넌트로 사용하는 방법을 배웠다. 컴포넌트 메소드는 리액트 생명주기에 대해 배울 때 자세히 알아볼 것이다.

### 실습

* [ES6 클래스](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes)에 대해 더 자세히 읽어본다.
{pagebreak}

이제 여러분은 나만의 리액트 애플리케이션을 부트스트래핑을 할 수 잇다! 이번 장에서 배운 내용을 복습해보자.

* React
  * create-react-app bootstraps a React application
  * JSX mixes up HTML and JavaScript to define the output of React components in their render methods
  * components, instances and elements are different things in React
  * `ReactDOM.render()` is an entry point for a React application to hook React into the DOM
  * built-in JavaScript functionalities can be used in JSX
    * map can be used to render a list of items as HTML elements
* ES6
  * variable declarations with `const` and `let` can be used for specific use cases
    * use const over let in React applications
  * arrow functions can be used to keep your functions concise
  * classes are used to define components in React by extending them

이 시점에서 잠시 휴식 시간을 가지자. 학습 내용을 되새기고 스스로 적용해보자. 지금까지 작성한 소스 코드를 테스트 학습 내용을 내부화하고 독자적으로 적용하십시오. 지금까지 작성한 소스 코드로 이것저것 시험해보자.

[공식 깃헙 저장소](https://github.com/rwieruch/hackernews-client/tree/4.1)에서 소스코드를 참고하라.
