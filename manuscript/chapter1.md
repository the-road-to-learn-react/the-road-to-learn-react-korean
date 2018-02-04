# 리액트 시작하기 Introduction to React

리액트의 기본적인 내용에 대해 알아봅시다. 아직도 리액트를 배워야하는 이유를 모르겠다면 이번 장을 마칠 때 쯤, 그 답을 찾을 수 있을 것이라 확신합니다. 리액트 애플리케이션 설치를 시작으로 리액트의 세계에 첫 발을 내딛어봅시다. JSX와 ReactDOM를 배우고 첫 번째 리액트 컴포넌트를 만들어 봅시다.

## 안녕, 내 이름은 리액트 Hi, my name is React.

**왜 리액트를 배워야 할까요?** 최근 몇 년 간 단일 페이지 애플리케이션([SPA: Single Page Application](https://en.wikipedia.org/wiki/Single-page_application))이 각광받고 있습니다. Angular, Ember 및 Backbone 등 자바스크립트 프레임워크의 등장은 바닐라 자바스크립트(Vanilla JavaScript: 타 라이브러리나 프레임워크 사용 없이 순수한 자바스크립트로 개발하는 것을 말함)와 jQuery를 사용하지 않고도 최신 웹 응용 프로그램을 구축할 수 있게 되었습니다. 이외에도 SPA 프레임워크의 종류는 매우 다양합니다. Angular 2010, Backbone 2010, Ember 2011 등 배포된 대부분 SPA 프레임워크는 1세대입니다.

리액트는 페이스북이 만들고 2013년 공개한 라이브러리입니다. 리액트는 SAP 프레임워크가 아닌, 뷰 라이브러리(View Library)입니다. 여기서 뷰(View)란 [MVC](https://de.wikipedia.org/wiki/Model_View_Controller) (Model–View–Controller, MVC는 소프트웨어 공학에서 사용되는 소프트웨어 디자인 패턴을 말함) 패턴의 'V'를 지칭합니다. 뷰는 브라우저 내 특정 컴포넌트를 보여주는 역할을 하지만, 리액트로 단일 페이지 애플리케이션을 제작할 수 있습니다.

그렇다면 수많은 1세대 SPA 프레임워크 중 리액트를 선택해야하는 이유는 무엇일까요? 1세대 프레임워크는 한 번에 많은 테스크를 해결하려고 노력했지만, 리액트는 뷰 레이어를 만드는 역할만 합니다. 앞서 말했듯이 리액트는 프레임워크가 아닌 라이브러리입니다. 뷰는 컴포넌트로서 다른 컴포넌트와 함께 계층구조를 이룹니다.

리액트 장점은 여러 복잡한 기능을 추가하기전 오롯이 뷰 레이어에 집중하여 개발할 수 있다는 점입니다. SPA를 집이라고 한다면 리액트는 집을 짓기 위해 필요한 벽돌 한장과 같습니다. 복잡한 애플리케이션을 개발할 때 한 벽돌씩 만들고 쌓는 작업이 필요합니다. 이 작업은 두 가지 장점이 있습니다.

먼저 단계별로 하나씩 벽돌을 쌓는 법을 배울 수 있습니다. 처음부터 한꺼번에 모든 것을 공부하지 않아도 됩니다. 시작부터 모든 구조를 갖춘 프레임워크와 다릅니다. 제일 먼저 리액트를 먼저 배우고 그 이후 다른 벽돌(기술)을 배우며 더 많은 벽돌들과 연결시키는 작업을 통해 점점 넓고 깊게 배울 수 있습니다.

모든 벽돌은 상호 교환이 가능합니다. 이 것이 리액트 생태계가 혁신적이다라고 평가받는 이유입니다. 여러 솔루션들이 서로 경쟁하고 있고 각자 개발 상황에 맞는 적합한 솔루션을 선택하면 됩니다.

SPA 프레임 워크의 1세대는 이미 상용화 단계에 이르렀고 더욱 견고해졌습니다. 리액트는 여전히 혁신적입니다. [페이스북은 물론 에어비앤비, 넷플릭스](https://github.com/facebook/react/wiki/Sites-Using-React) 등 선도적인 테크 회사들이 리액트를 도입해 플랫폼 개발을 하고 있습니다. 그들 모두가 리액트와 생태계에 만족하고 있으며 리액트의 미래에 투자하고 있습니다.

최신 웹 애플리케이션 개발을 위해 리액트는 가장 좋은 선택이 될 겁니다. 리액트 자체는 뷰 레이어의 역할을 하지만 [거의 모든 프레임워크와 서로 상호 교환 가능합니다.](https://www.robinwieruch.de/essential-react-libraries-framework/) 리액트는 간결한 API, 놀라운 생태계, 훌륭한 커뮤니티를 갖추고 있습니다. ["왜 나는 앵귤러(Angular)에서 리액트로 옮겼는가](https://www.robinwieruch.de/reasons-why-i-moved-from-angular-to-react/)" 블로그에서 리액트를 선택한 개인적인 경험을 나누었습니다. 그러나 리액트를 배우기 이전에 다른 프레임워크나 라이브러리가 아닌, 리액트를 선택한 이유를 스스로에게 물어보기 바랍니다. 본인이 리액트를 사용해야하는 이유를 잘 알고 있어야, 더 많은 사람들이 앞으로 리액트의 발전과 행보에 관심을 가지게 될 것이니까요.


### 읽어보기

* [[저자 블로그] 왜 나는 앵귤러에서 리액트로 옮겼는가](https://www.robinwieruch.de/reasons-why-i-moved-from-angular-to-react/)
* [[저자 블로그] 유연한 리액트 생태계](https://www.robinwieruch.de/essential-react-libraries-framework/)

## 준비사항 Requirements

이전에 SPA 프레임워크나 라이브러리 사용 경험이 있다면 어느 정도 웹 개발 기초 지식이 있을 것입니다. 이제 막 웹 개발에 입문했다면 리액트를 배우기 전에 HTML, CSS, 자바스크립트 ES5를 잘 다룰 수 있어야 합니다. 이 책은 자바스크립트 ES6를 사용합니다. [공식 슬랙 그룹](https://slack-the-road-to-learn-react.wieruch.com/)에 가입해 동료들을 만나고 서로에게 도움을 주길 바랍니다.

### 코드에디터, 터미널

개발 환경은 어떻게 갖춰야할까요? 코드에디터나 IDE, 터미널(terminal : 또는 커맨드라인(command line)라고도 함)이 필요합니다. [개발 환경 설정 가이드](https://www.robinwieruch.de/developer-setup/)를 참고하길 바랍니다. MacOS을 대상으로 작성했으나 타 운영체제에 적용하여도 무방합니다. 이미 개발 환경 설정 방법에 관한 수 많은 가이드 문서가 있기 때문에 사용 중인 운영체제에 알맞는 가이드를 쉽게 찾을 수 있을 것입니다.

git와 깃허브(GitHub)을 사용하여 책에서 실습한 내용을 깃헙 저장소에 커밋하여 프로젝트를 계속해서 진행할 수 있습니다. 자세한 내용은 [GIT 가이드](https://www.robinwieruch.de/git-essential-commands/)를 읽어보고 실습해보길 바랍니다. git 사용은 의무 사항이 아닙니다. 웹 개발 초보자인 경우 모든 것을 한꺼번에 배우려고 하면 매우 부담스러울 수 있습니다. 초보자는 git과 깃허브를 쓰지말고 본문 내용에 집중하기 바랍니다.

### Node, NPM 

마지막으로 [노드(node) 및 npm](https://nodejs.org/en/) 설치가 필요합니다. 이 책에서는 npm(노드 패키지 관리자: node package manager)을 통해 외부 노드 패키지를 설치합니다. 노드 패키지는 라이브러리 또는 전체 프레임워크가 될 수 있습니다.

터미널에서 노드와 npm의 버전을 확인할 수 있습니다. 터미널에 출력되지 않으면 먼저 노드와 npm를 설치가 안된 것입니다.

{title="Command Line",lang="text"}
~~~~~~~~
node --version
*v8.3.0
npm --version
*v5.5.1
~~~~~~~~

## node, npm

node와 npm이 충돌 될 수 있습니다. 이미 패키지 설치와 관리가 익숙하다면 이 부분을 건너 뛰어도 좋습니다.

**노드 패키지 관리자** (npm) 명령어로 외부 **노드 패키지**를 로컬에 설치합니다. 이들 패키지는 유틸리티 함수, 라이브러리 또는 프레임워크 세트 등 입니다. 패키지는 애플리케이션에 종속됩니다. 패키지는 전역 노드 패키지 폴더 또는 프로젝트 내 지역 폴더에 설치될 수 있습니다.

전역 노드 패키지는 모든 터미널에서 액세스 할 수 있으므로 전역 디렉토리 내 한 번만 설치합니다. 아래 명령어로 글로벌 패키지를 설치해봅시다.

{title="Command Line",lang="text"}
~~~~~~~~
npm install -g <package>
~~~~~~~~

`-g` 플래그는 npm에게 패키지를 전역 패키지에 설치하도록 지시합니다. 지역 패키지는 개발 중인 애플리케이션 내에서만 사용됩니다. 리액트는 라이브러리로서 애플리케이션을 만들 수 있는 지역 패키지에 해당됩니다. 아래 명령어로 패키지를 설치합니다.

{title="Command Line",lang="text"}
~~~~~~~~
npm install <package>
~~~~~~~~

아래 명령어로 리액트를 설치합니다. 

{title="Command Line",lang="text"}
~~~~~~~~
npm install react
~~~~~~~~

설치된 패키지는 생성된 *node_modules/* 폴더에 저장되고 의존성 파일인 *package.json*에 패키지 리스트가 나열됩니다.

 *package.json* 파일이 있어야만 npm 명령어로 *node_modules/* 폴더를 초기화할 수 있습니다. 아래 명령어로 새 지역 패키지를 설치합니다.
 
{title="Command Line",lang="text"}
~~~~~~~~
npm init -y
~~~~~~~~

`-y` 표시는 *package.json*를 초기화하는 단축키입니다. 플래그 표시(`-`) 사용하지 않으면 파일구성 방법을 결정해야 해야합니다. npm 프로젝트 초기화 후 `npm install <package>`를 통해 새 패키지를 설치하는 것이 좋습니다.

*package.json*에 대해 더 알아봅시다. *package.json*는 노드 패키지 전체 파일을 공유하지 않고도 다른 개발자와 프로젝트를 공유 할 수 있습니다. 이 파일 내 프로젝트에 사용된 노드 패키지 리스트가 모두 적혀 있는데, 이러한 패키지를 종속성(dependencies)이라 부릅니다. 패키지 종속성이 없어도 프로젝트를 다운 받을 수 있습니다. 종속성은 *package.json*에 작성됩니다. 프로젝트를 복사한 후 `npm install`명령어로 모든 패키지가 쉽게 설치됩니다. `npm install` 스크립트는 *package.json* 파일에 나열된 모든 의존성을 취하여 *node_modules/* 폴더에 설치합니다.

npm 명령어 하나가 더 남았습니다.

{title="Command Line",lang="text"}
~~~~~~~~
npm install --save-dev <package>
~~~~~~~~

`--save-dev` 명령어는 노드 패키지가 개발 환경에서 사용되는 것을 말합니다. 애플리케이션 배포 환경에서, 개발 환경과 달리 사용하지 않는 노드 패키지가 있습니다. 예를 들어 테스트를 위한 노드 패키지가 그렇습니다. npm을 통해 해당 패키지를 설치할 수 있지만, 실제 제품이 운영되는 환경에서는 제외시켜야 합니다. 테스트는 개발 프로세스 중에만 수행되고, 실제 배포 환경에서는 제외됩니다. 이를 위해 `--save-dev` 명령어를 사용합니다.

앞으로 더 많은 npm 명령어를 다루게 될 것입니다. 지금은 이 정도로도 충분합니다.

### 실습하기

* npm 프로젝트를 설치합니다
  * `mkdir <folder_name>`를 입력해 새 폴더 생성합니다.
  * `cd <folder_name>` 생성한 폴더 내로 들어 갑니다.
  * `npm init -y` 또는 `npm init` 을 실행합니다.
  * `npm install react`를 입력해 리액트를 전역 패키지로 설치합니다.
  * *package.json* 파일과 *node_modules/* 폴더가 생성되었는지 확인합니다.
  * *react* 노드 패키지를 제거하고 다시 설치하는 방법을 스스로 찾아봅니다.

### 읽어보기

* [npm 공식문서](https://docs.npmjs.com/)

## 설치하기

리액트는 CDN 또는 npm 명령어로 설치할 수 있습니다.

### CDN
CDN이란 [콘텐츠 전송 네트워크(Content_delivery_network)](https://en.wikipedia.org/wiki/Content_delivery_network)를 말합니다. 많은 회사들이 CDN를 사용해 라이브러리를 제공하고 있습니다. 번들링된 리액트 라이브러리는 단 *react.js* 파일 뿐이기 때문에 리액트도 라이브러라라고 할 수 있습니다. CDN을 별도로 호스팅하거나 응용 프로그램 내에 설치하여 사용 가능합니다.

CDN을 사용해 리액트를 시작하기 위해 HTML 파일 내 `<script>` 인라인 태그로 CDN url을 작성합니다. *react*와 *react-dom* 두 라이브러리 url을 추가합니다.


{title="Code Playground",lang="javascript"}
~~~~~~~~
<script crossorigin src="https://unpkg.com/react@16/umd/react.development.js"></script>
<script crossorigin src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>
~~~~~~~~

### npm
npm으로 리액트를 설치하는 방법입니다. 애플리케이션 내 *package.json* 파일이 있다면, npm 명령어로 *react* 및 *react-dom*을 설치할 수 있습니다. 제일 먼저  *npm init -y* 명령어를 입력해 *package.json*을 초기화합니다. npm 명령어 한 줄로 여러 노드 패키지를 한 번에 설치해봅시다.

{title="Command Line",lang="text"}
~~~~~~~~
npm install react react-dom
~~~~~~~~

이와 같은 방법은 현재 개발 중인 애플리케이션에 리액트를 추가할 때 사용됩니다.

설치로 모든 준비가 끝난 것이 아닙니다. JSX(리액트 문법)과 자바스크립트 ES6로 만든 애플리케이션은 [바벨(babel)](http://babeljs.io/)을 사용합니다. 모든 브라우저가 ES6 문법을 해석할 수 없기 때문에, 바벨을 통해 자바스크립트 ES6와 JSX를 ES5로 변환해야 합니다. 바벨 설치를 위해 많은 환경 설정과 도구가 필요하기 때문에, 리액트 초심자에게 바벨 사용은 또 다른 장벽이 될 수 있습니다.

이러한 이유로 페이스북에서는 제로 구성 설치(zero-configuration) 솔루션인 *create-react-app* 패키지를 만들었습니다. 우리는 이 패키지로 리액트 애플리케이션을 만들어 볼 것입니다.

### 읽어보기

* [[리액트 공식문서] 리액트 설치](https://facebook.github.io/react/docs/installation.html)

## 제로 구성 설치 Zero-Configuration Setup

이 책에서는 [create-react-app](https://github.com/facebookincubator/create-react-app)으로 애플리케이션을 부트스트래핑합니다. 부트스트래핑(bootstrapping)이라는 뜻은 애플리케이션을 최초 생성하여 브라우저에서 실행하는 과정을 말합니다. create-react-app은 2016년 페이스북이 제안한 리액트 제로 구성 설치 스타터 킷(Zero-Configuration Setup Starter Kit)입니다. [한 조사](https://twitter.com/dan_abramov/status/806985854099062785)에 따르면 96% 이상의 개발자들이 리액트 입문자의 경우 create-react-app를 사용하여 리액트 개발할 것을 추천했습니다.

*create-react-app*는 리액트 개발 도구와 환경 설정이 이미 세팅되어 있어, 우리는 오롯이 애플리케이션 구현에만 신경쓰면 됩니다. 

시작을 위해 전역 노드 패키지에 패키지를 설치합니다.

{title="Command Line",lang="text"}
~~~~~~~~
npm install -g create-react-app
~~~~~~~~

*create-reaction-app* 버전을 확인해 성공적으로 패키지가 설치되었는지 확인해봅시다.

{title="Command Line",lang="text"}
~~~~~~~~
create-react-app --version
*v1.4.1
~~~~~~~~

첫 번째 리액트 애플리케이션을 시작해봅시다.  앞으로 새로운 리액트 애플리케이션을 부트스트랩할 때마다 `create-react-app <name>` 명령어를 입력하면 됩니다.

우리가 만들 애플리케이션은 해커 뉴스 플랫폼임으로 프로젝트 이름을 *hackernews*라 합시다. 물론 다른 이름을 사용해도 괜찮습니다. 부트스트랩에는 몇 초가 걸립니다. 완료 후에 생성된 폴더 안으로 들어갑니다.

{title="Command Line",lang="text"}
~~~~~~~~
create-react-app hackernews
cd hackernews
~~~~~~~~

코드에디터에서 애플리케이션을 열어 봅시다. 디렉토리에 아래와 같은 *create-react-app* 구조가 보일 것입니다.

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

각 파일과 폴더 단위가 무엇을 지칭하는지 알아봅시다. 처음부터 모든 것을 이해하지 않아도 됩니다.

* **README.md:** .md 확장자는 파일이 마크다운(markdown) 파일입니다. 일반 텍스트와 함께 간단한 마크업 언어로 작성합니다. 대부분의 프로젝트에는 *README.md* 파일에 프로젝트 설명 및 설치 방법 등 안내 사항이 작성되어 있습니다. 깃허브 리퍼지토리 페이지의 첫 화면에  *README.md*을 보았을 겁니다. *create-react-app*을 설치한 후 바로 깃헙에 프로젝트를 올린다면 *README.md*는 공식 [create-react-app 깃허브 리퍼지토리](https://github.com/facebookincubator/create-react-app)와 동일할 겁니다.

* **node_modules/:** 이 폴더에는 npm을 통해 설치되었던 모든 노드 패키지를 포함합니다. 이미 *create-react-app*를 사용했으므로 이미 여러 개의 노드 모듈이 설치되어있어야 합니다. 이 폴더를 건드리지 말아야 합니다. npm 명령어를 사용해 패키지를 설치 및 제거만 하도록 합니다.

* **package.json:** 이 파일에는 노드 패키지 종속성 및 기타 프로젝트 구성 목록을 포함합니다.

* **.gitignore:** 이 파일은 git을 사용할 때, 원격 git 리퍼지토리에 제외시킬 모든 파일과 폴더를 나타낸다. 예를 들어 *node_module*은 같이 로컬에만 있어야 합니다. 종속성 폴더 전체를 올리지 않고도 공유된 *package.json* 파일만으로 종속성을 설치할 수 있습니다.

* **public/:** 배포를 위해 프로젝트를 빌드 시 필요한 모든 파일을 포함합니다. 프로젝트 빌드 할 때, *src/* 폴더 내 모든 코드는 몇 개의 파일로 묶여 *public* 폴더에 배치됩니다.

따라서 위에서 언급된 파일 및 폴더를 건드릴 필요가 없습니다. 우리가 필요한 모든 것은 *src/* 폴더에 있습니다. 제일 중요한 파일은 *src/App.js*로 이 파일이 바로 리액트 컴포넌트입니다. 현재 이 파일이 전체 애플리케이션을 이루고 있지만, 리액트 컴포넌트를 여러 파일로 분절하여 나눠 유지 관리할 수 있습니다.

이외에 테스트를 위한 *src/App.test.js* 파일과 리액트의 진입점이라 볼 수 있는 *src/index.js* 파일이 보일 겁니다. 이어 다음 장에서 두 파일이 어떤 것인지 알게 될 것입니다. 또한 애플리케이션과 컴포넌트 스타일을 지정하는 *src/index.css* 및 *src/App.css* 파일이 있습니다. 현재는 기본 스타일만 적용되어 있습니다.

* *create-reaction-app* 애플리케이션은 npm 프로젝트입니다. npm을 사용하여 프로젝트에 노드 패키지를 설치하고 제거할 수 있습니다.

{title="Command Line",lang="text"}
~~~~~~~~
// http://localhost:3000 에서 애플리케이션 실행
npm start

// 테스트 실행
npm test

// 배포를 위한 애플리케이션 빌드
npm run build
~~~~~~~~

*package.json*에 스트립트 명령어가 정의되어 있습니다. 리액트 애플리케이션을 부트스트리팽 했으니 브라우저에서 애플리케이션이 구동되는 것을 확인해봅시다.

### 실습하기

* `npm start`을 실행해 브라우저에서 애플리케이션을 확인합니다.
* `npm test`를 실행합니다.
* *public/* 폴더 내 어떤 내용이 있는지 확인하고, `npm run build` 스크립트를 실행해 폴더에 어떤 파일이 추가되었는지 확인합니다. (추가된 파일은 프로젝트에 영향을 주지 않으므로 제거할 수 있습니다.)
* 폴더 구조에 익숙해지도록 합니다.
* 파일 내용이 익숙해지도록 합니다.

### 읽어보기
* [create-react-app 깃헙 리퍼지토리](https://github.com/facebookincubator/create-react-app)


## JSX 들어가기 Introduction to JSX

리액트 구문인 JSX에 대해 알아봅시다. *create-react-app*로 애플리케이션을 부트스트래핑을 하면 애플리케이션이 이미 구현되어 있습니다. *src/App.js* 소스 코드를 살펴봅시다.

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

자바스크립트 ES6 기능인 import/export 및 클래스 선언에 대해 알기 위해 조급해할 필요가 없습니다. 앞으로 차근차근 배워볼 것입니다.

`App.js` 파일에 **리액트 ES6 클래스 컴포넌트(React ES6 Class Component)** 를 볼 수 있습니다. 이 파일에서  컴포넌트를 선언합니다.

 컴포넌트를 선언한 후, 컴포넌트는 요소(element)로서 애플리케이션의 어느 곳이든지 재 사용될 수 있습니다. 컴포넌트가 인스턴스화되기 때문에, 다른 말로 컴포넌트의 인스턴스를 사용한다고 말합니다.
 
 `render()`메서드에서 **요소(element)** 가 리턴(return)됩니다. 여러 요소가 모여 컴포넌트 전체를 구성합니다. 컴포넌트(componet), 인스턴스(Instance), 요소(element) 간의 용어와 각 쓰임의 차이를 알고 있다면 이해가 쉬울 것입니다.

이제 `App` 컴포넌트가 인스턴스화 된 것을 볼 수 있습니다. 인스턴스가 되지 않았다면, 브라우저에서 렌더링된 결과를 볼 수 없습니다. App 컴포넌트는 선언되었지만, 다른 곳에 재사용되지 않았습니다. JSX문법인 `<App/>`를 사용하면 어느 컴포넌트에서나 인스턴스화하여 재사용할 수 있습니다.   

`render()` 블록 안에 내용은 HTML과 비슷해보일 겁니다만, JSX 문법으로 작성되었습니다. JSX를 사용하면 HTML과 자바스크립트를 결합해 쓸 수 있습니다. HTML과 자바스크립트를 따로 사용했다면 혼란스러울 겁니다. 먼저 JSX에서 기본적인 HTML 요소를 작성해봅시다. 먼저 `App` 컴포넌트의 모든 내용을 지우고 아래와 같이 작성합니다.


{title="src/App.js",lang=javascript}
~~~~~~~~
import React, { Component } from 'react';
import './App.css';

class App extends Component {
  render() {
    return (
      <div className="App">
        <h2>리액트에 오신 여러분을 환영합니다</h2>
      </div>
    );
  }
}

export default App;
~~~~~~~~

이제 자바스크립트없이 `render()`로 HTML를 반환했습니다. 새 변수를 만들고 "리액트에 오신 여러분을 환영합니다"라는 값을 주고, 이 변수를 JSX 문법인 중괄호(`{}`) 안에 넣습니다.

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

다시 `npm start` 명령어를 실행해 애플리케이션을 시작합니다.

아마 `className`가 속성(attribute)임을 눈치챘을 겁니다. `className`은 HTML 표준 `class`속성에 영향을 받았습니다. 기술적인 이유로 JSX는 몇 가지 내부 HTML 속성으로 대체했습니다. 리액트 공식문서에서 [지원하는 HTML 속성](https://facebook.github.io/react/docs/dom-elements.html)을 확인할 수 있습니다.  HTML 속성은 카멜케이스(camelCase) 표기법을 따릅니다. 앞으로 JSX 특정 속성을 좀더 살펴볼 것입니다.

### 실습하기

* JSX 안에 새 변수를 만들고 렌더링합니다.
  * 객체를 사용해 유저의 성과 이름을 나타냅니다.
  * JSX 내 유저 속성을 렌더링합니다. 
  
### 읽어보기  
* [[리액트 공식문서] JSX](https://facebook.github.io/react/docs/introducing-jsx.html)에 대해 읽어봅니다. 
* [[리액트 공식문서] 리액트 컴포넌트, 요소, 인스턴스](https://facebook.github.io/react/blog/2015/12/18/react-components-elements-and-instances.html)에 대해 읽어봅니다.

## ES6 const, let

앞에서 `var` 문을 사용해 변수 `helloWorld`를 선언했습니다. 자바스크립트 ES6 변수 선언은 `const`또는 `let`을 사용합니다. ES6에서는 `var` 사용하는 일은 극히 드뭅니다.

`const`로 선언된 변수를 다시 할당하거나 선언할 수 없습니다. 불변 데이터 구조(immutable data structures)이기 때문에 데이터 구조가 정의된 이후, 변경할 수 없습니다.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// 불가능
const helloWorld = '리액트에 오신 여러분을 환영합니다';
helloWorld = '안녕 리액트';
~~~~~~~~

반면 `let`은 변경 가능해 변수를 다시 할당할 수 있습니다.


{title="Code Playground",lang="javascript"}
~~~~~~~~
// 가능
let helloWorld = '리액트에 오신 여러분을 환영합니다';
helloWorld = '안녕 리액트';
~~~~~~~~

기본적으로 `const`로 선언된 변수는 변경할 수 없습니다. 그러나 변수가 배열이나 객체일 경우 변경 가능합니다.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// 가능
const helloWorld = {
  text: '리액트에 오신 여러분을 환영합니다'
};
helloWorld.text = '안녕 리액트';
~~~~~~~~

그렇다면 어느 상황에서 `let`과 `const`를 사용해야 할까요? 사용법에 대한 의견은 서로 분분합니다만, 가능하면 최대한 `const`를 기본적으로 사용하는 것이 좋습니다. 객체와 배열의 값이 변경되더라도 데이터 구조를 변경되지 않으려도 의도를 반영하기 때문입니다. 변수가 수정된다면 `let`을 사용합니다.

리액트 생태계에서는 불변성을 준수합니다. 따라서 변수를 정의 시 `const`가 우선시 됩니다. 객체가 복잡해지면 내부 값을 수정해야되는 경우도 있으니 주의합니다. 

우리가 만드는 애플리케이션에서는`const` 대신`var`을 사용합니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
import React, { Component } from 'react';
import './App.css';

class App extends Component {
  render() {
# leanpub-start-insert
    const helloWorld = '리액트에 오신 여러분을 환영합니다';
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

### 읽어보기

* [[MDN] ES6 const](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const)
* [[MDN] ES6 let](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let)

### 찾아보기
* 불변 데이터 구조에 대해 더 조사합니다.
  * 보편적인 프로그래밍에서 말하는 불변 데이터 구조란 무엇인지 알아봅니다.
  * 리액트와 그 생테계에서 불변 데이터 구조가 사용되는 이유에 대해 알아봅니다.

## ReactDOM

App 컴포넌트를 바꾸기 전에 이 컴포넌트가 어디에서 사용되고 있는지 확인해봅시다. 리액트의 첫 진입점이라 할 수 있는 *src/index.js* 파일에 있습니다.

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

대게 `ReactDOM.render()`는 HTML 내 DOM 노드를 JSX로 대체합니다. 때문에 타 애플리케이션에 리액트가 쉽게 통합됩니다. 애플리케이션 내 `ReactDOM.render()`를 여러 번 사용할 수 있습니다. 원하는 곳마다 JSX 구문, 단일 리액트 컴포넌트, 다중 리액트 컴포넌트, 또는 전체 응용 프로그램을 부트스트랩 할 수 있습니다. 그러나 일반 리액트 애플리케이션은 관용적으로 전체 컴포넌트 트리를 부트스트랩하기 위해 `ReactDOM.render()`를 한 번만 사용합니다.

`ReactDOM.render()`에는 두 개의 인자가 필요합니다. 첫 번째 인자는 렌더링된 JSX, 두 번째 인자는 리액트 애플리케이션이 HTML에 들어갈 위치인 id, 또는 클래스를 지정합니다. *public/index.html* 파일을 열어 이 id 속성을 확인합시다. App 컴포넌트는 `id='root'`에 들어가게 됩니다.

`ReactDOM.render()`가 실행되어 이미 App 컴포넌트를 사용하고 있습니다. 되도록 가능한 간단한 JSX로 만들어 컴포넌트로 전달하는 것이 좋습니다. 이 파일에서 컴포넌트를 정의하고 컴포넌트를 인스턴스화 시킬 필요가 없습니다.

{title="Code Playground",lang=javascript}
~~~~~~~~
ReactDOM.render(
  <h1>안녕 리액트</h1>,
  document.getElementById('root')
);
~~~~~~~~

### 실습하기

* *public/index.html*을 열어 HTML 안에 리액트 애플리케이션 들어가는 곳을 찾아봅니다.

### 읽어보기
* [[리액트 공식문서] - 리액트 내 렌더링되는 요소](https://facebook.github.io/react/docs/rendering-elements.html)

## Hot Module Replacement

더 나은 개발 환경을 위해 *src/index.js*에서 할 일이 더 있습니다. 이 내용은 필수가 아니라 옵션이기 때문에 리액트 입문자가 꼭 해야할 내용은 아닙니다.

*create-react-app*은 소스 코드 변경 시, 자동으로 브라우저를 새로고침합니다. *src/App.js* 파일 내 `helloWorld`를 다른 변수로 변경해보면 브라우저가 자동으로 새로고침되기 때문에 매번 수정 후 새로고침을 하지 않아도 됩니다. 그러나 더 좋은 방법이 있습니다.

Hot Module Replacement(HMR)란 브라우저 내 애플리케이션을 재실행하는 도구입니다. *create-reaction-app*에서 쉽게 사용할 수 있습니다. *src/index.js*에서 아래와 같이 약간의 설정을 추가합니다. 

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

위 코드가 전부입니다. *src/App.js* 파일에서 `helloWorld` 변수를 다른 것으로 바꿔봅시다. 브라우저는 페이지 새로고침되지 않지만, 애플리케이션이 재실행되어 올바른 결과가 출력됩니다. 이처럼 HMR은 장점이 많습니다.

`console.log()`로 디버깅을 할 겁니다. 코드를 수정해도 브라우저 페이지가 새로고침되지 않기 때문에 개발자 콘솔에 그대로 남아있습니다. 때문에 디버깅이 훨씬 편리해집니다.

애플리케이션이 점차 고도화되면서 페이지 새로고침은 개발 생산성을 지연시킵니다. 페이지가 로드될 때까지 기다려야 하며, 대규모 애플리케이션에서 페이지를 다시 로드하는데 몇 초 이상이 걸릴 수 있습니다. HMR은 이러한 단점을 해결합니다.

가장 큰 장점은 HMR로 애플리케이션 상태를 유지할 수 있다는 것입니다. 애플리케이션에 3단계의 대화창이 있다고 가정해봅시다. 쉽게 다이얼로그 창 기능이라고 생각하면 됩니다. HMR이 없다면 소스 코드를 변경할 때마다 브라우저 페이지가 새로고침 됩니다. 창을 다시 열어 1 단계에서 3 단계로 이동시켜야하는 번거로운 일을 해야합니다. HMR을 사용하면 다이얼로그 창은 3 단계 상태로 유지되며, 소스코드가 수정되어도 애플리케이션 내 상태가 유지됩니다. 애플리케이션 자체는 다시 실행되지만 페이지는 새로고침되지 않습니다.

### 실습하기

* *src/App.js* 소스코드를 수정해 HMR이 어떻게 실행되는지 확인합니다.

### 영상보기
* 댄 애브라몹(Dan Abramov)의 [리액트 라이브 : Hot Reloading으로 시간 여행 떠나기](https://www.youtube.com/watch?v=xsSnOQynTHs) 10분간 시청합니다.

## JSX 내 복잡한 자바스크립트 작성 Complex JavaScript in JSX

App 컴포넌트로 다시 돌아갑시다. 지금까지 JSX에서 초기 변수를 렌더링했습니다. 이제 배열의 각 항목을 차례로 렌더링 해봅시다. 지금은 샘플 데이터를 사용하지만 앞으로 외부 [API](https://www.robinwieruch.de/what-is-an-api-javascript/)를 통해 데이터를 가져올 것입니다. 앞으로 점점 재밌고 흥미있을 겁니다.

먼저 아래와 같이 list를 정의합시다.

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

list는 객체로 이루어진 배열입니다. 프로퍼티에는 제목(title), url, 작성사(author), 식별자(objectID), 기사의 인기도를 나타내는 점수(point), 댓글 수(num_comments)가 있습니다.

이제 JSX에서  자바스크립트 반복 메서드인 `map()`을 사용해 배열의 모든 데이터를 출력해봅시다. JSX에서는 자바스크립트 표현식을 캡슐화하기 위해 중괄호(`{}`)를 사용합니다.

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

JSX는 HTML과 자바스크립트를 함께 사용할 수 있습니다. `map()`를 사용해 각 요소를 출력합니다. 이번에는 `map`를 사용해 HTML 태그와 섞어 작성해봅시다.

지금까지 `title` 프로퍼티 값만 표시했습니다. 이제 모든 프로퍼티 값을 보여줍시다.

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

이제 `map()`메서드로 JSX에서 반복문을 작성했습니다. 각 프로퍼티를 HTML `<span>` 태그로 감싸고, url은 a 태그의 href 속성과 함께 사용했습니다.

그러나 아직 한 가지가 남았습니다 바로 각 요소마다 `key` 속성을 추가해야 합니다. 리액트는 `key`로 배열의 수정 및 제거된 항목을 식별합니다. 우리는 `objectID` 프로퍼티를 `key`로 지정하겠습니다.

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

`key` 값은 고유값으로 식별가능한 값이어야 합니다. 배열의 인덱스값은 고정값이 아니기 때문에 사용하지 말아야 합니다. 배열의 순서가 변경되면, 리액트가 각 항목을 식별할 수 없기 때문입니다.

{title="src/App.js",lang=javascript}
~~~~~~~~
// 이렇게 사용하면 안됩니다.
{list.map(function(item, key) {
  return (
    <div key={key}>
      ...
    </div>
  );
})}
~~~~~~~~

이제 두 배열의 모든 항목이 보입니다. 브라우저를 열어 변경 내용을 확인합시다.

### 읽어보기

* [[리액트 공식문서] 리액트 리스트(lists)와 키(keys)](https://facebook.github.io/react/docs/lists-and-keys.html)
* [[MND] 자바스크립트 map()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map)

### 실습하기
* JSX 내 자바스크립트 표현식(expression)을 작성합니다.

## ES6 화살표 함수 

ES6는 화살표 함수를 도입했습니다. 화살표 표현식으로 함수 표현식 문법이 간결해졌습니다.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// 함수 표현식 
function () { ... }

// 화살표 함수 표현식
() => { ... }
~~~~~~~~

그러나 화살표의 기능을 알고 있어야 합니다. 화살표 함수는 `this`를 다르게 정의합니다. 기존 함수 표현식은 자기 자신을 `this` 객체로 정의하지만, 화살표 함수 표현식은 자기 자신을 `this`를 생성하지 않고, 감싸고 있는 본문 컨텍스트로에서 의미를 갖습니다. 화살표 함수에서 `this` 사용을 혼동하지 말아야 합니다.

화살표 함수 내 괄호 사용도 유의해야 합니다. 단일 매개변수라면 괄호 생략이 가능하지만, 복수 매개변수이거나 매개변수가 없는 함수는 괄호를 생략할 수 없습니다.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// 가능
item => { ... }

// 가능
(item) => { ... }

// 불가능
item, key => { ... }

// 가능
(item, key) => { ... }
~~~~~~~~

앞서 작성한 `map()` 코드 부분을 ES6 화살표 표현식으로 작성해봅시다.

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

또한 ES6 화살표 함수에서 중괄호(`{}`) 부분인 *블록 본문*을 생략합니다. *간결한 본문* 에서는 반환(return)이 포함되어, `return` 문을 삭제할 수 있습니다. 화살표 함수를 자주 보게 될 것이기 때문에, 화살표 기능을 사용할 때 블록 본문과 간결한 본문의 차이점을 이해하고 있어야 합니다.

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

이제 JSX가 간결해지고 보기 쉬워졌습니다. 함수 선언, 중괄호(`{}`) , return 문을 생략할 수 있습니다.  이로써 세부적인 구현 내용에 집중할 수 있게 됩니다. 

### 읽어보기

* [[MDN] ES6 화살표 함수](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Arrow_functions)

## ES6 클래스 ES6 Class

ES6는 클래스를 도입했습니다. 클래스는 일반적으로 객체 지향 프로그래밍 언어에서 사용됩니다. 자바스크립트는 융통성과 유연성이 뛰어난 프로그래밍 언어로 함수형 프로그래밍과 객체지향 프로그래밍으로 작성할 수 있습니다.

리액트는 함수형 프로그래밍과 객체 프로그래밍의 각기 장점을 취하고 있습니다. 리액트는 불변하는 데이터 구조를 다룰 때는 함수형 프로그래밍을, 컴포넌트를 만들 때는 클래스로 선언합니다. 이들 모두 ES6 클래스 컴포넌트라고 합니다. 

다음 `Developer` 클래스를 리액트 컴포넌트가 아닌, ES6 클래스로 간주해봅시다.

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

클래스에는 인스턴스화 할 수 있는 생성자(constructor)가 있습니다. 생성자는 매개변수를 사용해 클래스 인스턴스에 할당합니다. 또한 클래스는 함수를 정의합니다. 함수는 클래스와 연관되어 있어 메서드(method)라고도 불립니다. 이 메서드는 클래스 메서드로 참조됩니다.

Developer 클래스는 클래스 선언문일 뿐입니다. 클래스를 호출해 클래스 내 여러 인스턴스를 작성할 수 있습니다. ES6 클래스 컴포넌트와 비슷하지만, 다른 인스턴스를 사용하여 인스턴스를 생성해야 합니다.

클래스를 인스턴스화하는 방법과 클래스를 사용하는 방법을 살펴봅시다.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const robin = new Developer('Robin', 'Wieruch');
console.log(robin.getName());
// output: Robin Wieruch
~~~~~~~~

리액트는 ES6 클래스 컴포넌트에 자바스크립트 ES6 클래스를 사용합니다. 이미 하나의 ES6 클래스 컴포넌트를 사용했습니다.

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

App 클래스는 `Component`에서 확장(extend)됩니다. 기본적으로 App 컴포넌트에서 확장을 선언했지만, 다른 컴포넌트에서도 선언할 수 있습니다. 확장(extend)이란 것은 무엇일까요? 객체 지향 프로그래밍에는 상속의 원칙이 있습니다. 한 클래스의 기능을 다른 클래스로 전달하는데 사용됩니다.

App 클래스는 `Component` 클래스의 기능을 확장하는데, `Component` 기능을 상속받았습니다. `Component` 클래스는 기본 ES6 클래스를 ES6 컴포넌트 클래스로 확장합니다. 따라서 컴포넌트 클래스 메서드를 쓸 수 있습니다. 그중 `render()`은 우리가 이미 사용했던 메서드입니다. 앞으로 여러 컴포넌트 클래스 메서드에 대해 배우게 될 것입니다.

`Component` 클래스는 리액트 컴포넌트의 모든 구현 세부 사항을 캡슐화합니다. 때문에 여러분은 리액트에서 클래스를 컴포넌트로 사용할 수 있습니다.

지금까지 ES6 클래스 기초와 이를 리액트 컴포넌트로 사용하는 방법을 배웠습니다. 컴포넌트 메서드는 리액트 생명주기(Life Cycle)에 대해 배울 때 보다 자세히 알아볼 것입니다.

### 읽어보기

* [[MND] ES6 클래스](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes)
{pagebreak}

이제 여러분은 리액트 애플리케이션을 부트스트래핑을 할 수 있게 되었습니다! 이번 장에서 배운 내용을 복습합시다.

* React
  * create-react-app 으로 리액트 애플리케이션을 부트스트래핑합니다.
  * JSX 문법으로 `render()` 메서드 내 HTML과 자바스크립트를 같이 사용해 컴포넌트의 결과값을 출력합니다.
  * 컴포넌트, 인스턴스, 요소는 서로 다릅니다.
  * `ReactDOM.render()`는 리액트 애플리케이션이 리액트를 DOM에 연결시키는 엔트리 포인트(entry point)입니다.
  * JSX에서 자바스크립트 내장 함수를 사용할 수 있습니다.
  * 배열의 요소를 렌더링할 때 `map()`와 HTML 구문과 함께 사용합니다.
  
* ES6
  * 특정 상황에 따라 `const`와 `let`로 변수를 선언합니다. 
  * 리액트 애플리케이션에서는 가능한한 `const`로 변수를 선언합니다.
  * 화살표 함수를 사용해 간결한 함수로 만듭니다.
  * 클래스는 확장하여 리액트 컴포넌트로 정의합니다.

잠시 휴식 시간을 가집시다. 학습 내용을 되새기고 스스로 적용해보며 지금까지 작성한 코드와 배운 내용을 내 것으로 만듭시다. 코드로 이것저것 실험해보길 바랍니다.

앞으로 만들 애플리케이션에 대해 궁금하다면 [해커뉴스 깃허브 저장소](https://github.com/rwieruch/hackernews-client/tree/4.1)를 참고합니다.
