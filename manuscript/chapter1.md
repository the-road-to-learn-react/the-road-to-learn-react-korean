# 1. 리액트 기초 다지기

첫 장에서는 리액트의 가장 기본적인 내용을 배웁니다. 아직도 리액트를 배워야 하는 이유를 모르겠다면 이번 장을 마칠 때쯤, 그 답을 찾을 수 있을 것입니다. 리액트 애플리케이션 설치를 시작으로 리액트 세계에 첫 발을 내딛어 보겠습니다. JSX와 ReactDOM를 배우고 간단한 리액트 컴포넌트를 만들어 보며 리액트 기초를 다져봅시다.

## 1.1 리액트를 배워야 하는 이유

최근 몇 년 간 단일 페이지 애플리케이션([SPA: Single Page Application](https://en.wikipedia.org/wiki/Single-page_application))이 각광받고 있습니다. 앵귤러(Angular), 엠버(Ember) 및 백본(Backbone) 등 자바스크립트 프레임워크의 등장으로 바닐라 자바스크립트(Vanilla JavaScript, 타 라이브러리나 프레임워크 사용 없이 순수한 자바스크립트로 개발하는 것을 말함)와 제이쿼리(jQuery)를 사용하지 않고도 최신 웹 응용 프로그램을 구축할 수 있게 되었습니다. 이외에도 SPA 프레임워크 종류는 매우 다양합니다. 초기 SPA 프레임워크 1세대는 앵귤러 2010, 백본 2010, 엠버 2011 등 입니다.

리액트는 페이스북이 만든 라이브러리로 2013년 공개되었습니다. 리액트는 SPA 프레임워크가 아닌, 뷰 라이브러리(View Library)입니다. 여기서 뷰(View)란 [MVC 패턴](https://en.wikipedia.org/wiki/Model–view–controller)(Model–View–Controller Pattern)의 'V'를 말합니다. 뷰는 브라우저 내 특정 컴포넌트를 보여주는 역할을 하기 때문에, 리액트로 단일 페이지 애플리케이션을 개발할 수 있는 것입니다.

그렇다면 수많은 1세대 SPA 프레임워크 중 리액트를 선택해야 하는 이유는 무엇일까요? 1세대 프레임워크는 한 번에 많은 테스크를 해결하려 했지만, 리액트는 뷰 레이어를 만드는 역할만 합니다. 앞서 말했듯이 리액트는 프레임워크가 아닌 라이브러리입니다. 뷰는 컴포넌트로서 다른 컴포넌트와 함께 계층구조를 이룹니다.

리액트의 장점은 여러 복잡한 기능을 추가하기 전 오롯이 뷰 레이어에 집중하여 개발할 수 있다는 점입니다. SPA가 집이라면 리액트는 집을 짓기 위한 벽돌과 같습니다. 마치 집을 짓는 것처럼 복잡한 애플리케이션을 개발할 때 벽돌을 하나씩 만들고 쌓는 작업이 필요합니다. 이러한 개발 방법은 두 가지 장점이 있습니다.

첫째, 단계별로 하나씩 벽돌을 쌓는 법을 배울 수 있습니다. 처음부터 한꺼번에 집을 짓기 위한 모든 도구를 공부하지 않아도 됩니다. 시작부터 모든 도구와 구조를 갖춘 프레임워크와 다릅니다. 제일 먼저 리액트를 먼저 배우고 그 이후 다른 벽돌(기술)을 배우며 더 많은 벽돌들을 연결시켜가며 여러 기술들을 점차적으로 넓고 깊게 배울 수 있습니다.

둘째, 모든 벽돌을 상호 교환할 수 있습니다. '리액트 생태계가 혁신적이다'라고 평가받는 이유도 이 때문입니다. 리액트 생태계에서 여러 솔루션들이 서로 경쟁하며 발전하고 있습니다. 개발 상황에 적합한 솔루션을 도입할 수 있습니다.

현재 SPA 프레임워크 1세대는 상용화를 넘어 안정화 · 고도화 단계에 이르렀습니다. 그에 반해 리액트는 여전히 혁신적입니다. [페이스북은 물론 에어비앤비, 넷플릭스](https://github.com/facebook/react/wiki/Sites-Using-React) 등 선도적인 테크 회사들이 리액트를 도입해 플랫폼을 개발하고 있습니다. 그들 모두가 리액트와 그 생태계에 만족하며 리액트의 발전을 위해 적극적으로 투자하고 있습니다.

최신 기술을 도입해 웹 애플리케이션을 개발하고자 한다면 리액트는 가장 좋은 선택이 될 것입니다. 리액트의 가장 큰 장점은 [거의 모든 프레임워크와 서로 상호 교환 가능하다는 점입니다.](https://www.robinwieruch.de/essential-react-libraries-framework/) 리액트는 간결한 API, 놀라운 생태계, 훌륭한 커뮤니티를 갖추고 있습니다. 과거 저는 앵귤러로 개발했지만 여러 한계점을 느끼고 리액트로 넘어왔습니다. 블로그에 ['앵귤러에서 리액트로 옮긴 이유](https://www.robinwieruch.de/reasons-why-i-moved-from-angular-to-react/)'에 대해 개인적인 경험과 소견을 작성했습니다. 

리액트를 배우기 전에 다른 프레임워크나 라이브러리가 아닌, 리액트를 선택한 이유를 자신에게 물어보기 바랍니다. 본인 스스로 리액트로 개발하는 이유와 그 답을 알고 있어야, 더 많은 사람들이 앞으로 리액트의 발전과 행보에 관심을 가지게 될 것이니까요.

### 읽어보기

* [[저자 블로그] 왜 나는 앵귤러에서 리액트로 옮겼는가](https://www.robinwieruch.de/reasons-why-i-moved-from-angular-to-react/)
* [[저자 블로그] 유연한 리액트 생태계](https://www.robinwieruch.de/essential-react-libraries-framework/)
* [[저자 블로그] 프레임워크 학습 방법](https://www.robinwieruch.de/how-to-learn-framework/)


## 1.2 준비사항

이미 SPA 프레임워크 또는 라이브러리 사용 경험이 있다면 어느 정도 웹 개발 기초 지식이 있을 것입니다. 이제 막 웹 개발에 입문했다면 리액트를 배우기 전에 HTML, CSS, 자바스크립트 ES5를 잘 다룰 수 있어야 합니다. 이 책은 자바스크립트 ES6를 사용합니다. [공식 슬랙 그룹](https://slack-the-road-to-learn-react.wieruch.com/)에 가입해 동료들을 만나고 서로에게 도움을 주길 바랍니다.

### 1.2.1 코드에디터 · 터미널

웹 개발 환경을 갖춰 봅시다. 코드에디터나 IDE(Integrated Development Environment, 통합개발환경), 터미널(terminal, 또는 커맨드 라인(command line)이라고도 함)이 필요합니다. 자세한 방법은 [개발 환경 설정 가이드](https://www.robinwieruch.de/developer-setup/)를 참고하길 바랍니다. MacOS를 대상으로 작성했으나 타 운영체제에 적용하여도 무방합니다. 이미 개발 환경 설정 방법에 관한 수많은 가이드 문서가 있기 때문에 사용 중인 운영체제에 알맞은 가이드를 쉽게 찾을 수 있을 것입니다.

책에서 실습한 내용을 깃허브 리퍼지토리를 만들고 지속적으로 커밋하면서 프로젝트를 진행할 수 있습니다. 자세한 내용은 [git 가이드](https://www.robinwieruch.de/git-essential-commands/)를 읽어보고 실습해보길 바랍니다. git 사용은 의무 사항이 아닙니다. 웹 개발 입문자일 경우 초보자는 git과 깃허브를 쓰지 말고 본문 내용에 집중하기 바랍니다. 모든 것을 한꺼번에 배우려고 하면 매우 부담될 수 있으니까요. 

### 1.2.2 node · npm 

마지막으로 [node(노드)와 npm](https://nodejs.org/en/) 설치가 필요합니다. 이 책에서는 npm(node package manager, 노드 패키지 관리자 명령어)을 통해 외부 노드 패키지를 설치합니다. 노드 패키지는 라이브러리 또는 전체 프레임워크가 될 수 있습니다.

터미널에서 node와 npm의 버전을 확인합시다. 터미널에 출력되지 않으면 설치가 안된 것입니다.

{title="Command Line",lang="text"}
~~~~~~~~
node --version
*v8.9.4
npm --version
*v5.6.0
~~~~~~~~

## 1.3 노드 패키지 설치 · 관리

이 부분은 노드와 npm에 대한 아주 짧은 설명입니다. 완벽하지는 않지만, 필요한 부분은 다룰 것입니다. 패키지의 설치 및 관리에 익숙하다면 이 부분을 건너뛰어도 좋습니다.

**노드 패키지 매니저** (npm: node package manager)로 커맨드라인에서 외부 **노드 패키지**를 설치할 수 있습니다. 이들 패키지는 유틸리티 함수, 라이브러리 또는 프레임워크 전체가 될 수도 있는데, 애플리케이션은 설치된 패키지에 의존해서 동작하게 됩니다. 이때 패키지는 전역 노드 패키지 폴더 또는 프로젝트 내 지역 폴더에 설치될 수 있습니다.

전역 노드 패키지는 모든 터미널에서 액세스 할 수 있으며, 전역 폴더에 한 번만 설치하면 됩니다. 전역 패키지를 설치하는 명령은 다음과 같습니다.

{title="Command Line",lang="text"}
~~~~~~~~
npm install -g <패키지 이름>
~~~~~~~~

`-g` 옵션은 `npm`에게 패키지의 전역 설치를 명령합니다. 지역 패키지는 현재 폴더에서 개발 중인 애플리케이션에서 사용됩니다. 우리의 목적에 따라 다음 명령으로 리액트를 지역 폴더에 설치합시다.

{title="Command Line",lang="text"}
~~~~~~~~
npm install react
~~~~~~~~


패키지는 자동 생성된 *node_modules/* 폴더에 저장되고 이미 설치된 다른 패키지들과 함께 *package.json*에 기록됩니다.

프로젝트를 위해 *node_modules/* 폴더와 *package.json* 파일을 초기화 하려면 아래의 `npm` 명령을 실행합니다. 그후에 `npm`을 이용해 새 지역 패키지를 설치할 수 있게됩니다.
 
{title="Command Line",lang="text"}
~~~~~~~~
npm init -y
~~~~~~~~

`-y` 옵션은 *package.json*을 기본값들로 초기화하라는 지시자입니다. 프로젝트를 초기화하고나면 `npm install <package>`를 통해 새 패키지를 설치할 수 있습니다.

*package.json*을 이용하면 노드 패키지 파일 전부를 공유하지 않고도 다른 개발자와 프로젝트를 공유할 수 있습니다. 이 파일에는 프로젝트에 사용된 노드 패키지 목록이 전부 적혀 있는데, 이를 **dependencies**(애플리케이션이 의존하고 있는 패키지들을 부르는 명칭)라 부릅니다. 프로젝트를 복사할 때는 의존성 패키지들을 제외하고 복사해도 됩니다. *package.json*에 패키지 정보가 들어있으므로 프로젝트를 복사한 후 `npm install` 명령으로 모든 패키지를 쉽게 설치할 수 있게됩니다. `npm install` 스크립트는 *package.json* 파일에 나열된 모든 의존성 패키지 정보를 읽어서 *node_modules/* 폴더에 설치해줍니다.

마지막으로 한가지 명령만 더 알아봅시다.

{title="Command Line",lang="text"}
~~~~~~~~
npm install --save-dev <package>
~~~~~~~~

`--save-dev` 옵션은 설치할 패키지가 개발 환경에서만 사용된다는 의미입니다. 개발한 애플리케이션을 서버에 배포하는 경우, 혹은 프로덕션 환경에서는 필요하지 않은 패키지를 설치할 때 사용합니다. 테스트에만 사용하고 프로덕션 환경에서는 제외할 패키지를 설치할 때 유용합니다.

어떤 분들은 노드 패키지 관리에 다른 프로그램을 사용하고 싶을 수도 있습니다. **npm**과 비슷한 의존성 관리자로 **yarn**이 있습니다. yarn은 명령어는 다르지만 npm 레지스트리에 접근할 수 있습니다. yarn은 npm의 문제점을 개선하기 위해 만들어졌습니다. 그러나 두 도구 모두 빠르게 발전했기 때문에 지금은 어느 것을 써도 상관없습니다.

### 실습하기

* 테스트를 위한 npm 프로젝트를 터미널에서 생성합니다.
  * `mkdir <folder_name>`를 입력해 새 폴더를 생성합니다.
  * `cd <folder_name>` 생성한 폴더 내로 들어갑니다.
  * `npm init -y` 또는 `npm init` 을 실행합니다.
  * `npm install react`를 입력해 리액트를 지역 패키지로 설치합니다.
  * *package.json* 파일과 *node_modules/* 폴더가 생성되었는지 확인합니다.
  * *react* 노드 패키지를 제거하고 다시 설치하는 방법을 스스로 찾아봅니다.
* [npm 공식 문서](https://docs.npmjs.com/)를 읽어봅니다.
* [yarn 패키지 매니저](https://yarnpkg.com/en/docs/)에대해 읽어봅니다.

## 1.4 리액트 설치

리액트는 CDN 또는 `npm` 명령어로 설치합니다.

### 1.4.1 CDN
CDN는 [Content Delivery Network(콘텐츠 전송 네트워크)](https://en.wikipedia.org/wiki/Content_delivery_network)의 약자로 콘텐츠를 효율적으로 전달하기 위해 여러 노드를 가진 네트워크에 데이터를 저장하여 제공하는 시스템입니다. 많은 회사들이 CDN를 사용해 라이브러리를 공개하고 있습니다. CDN 파일은 리액트와 같은 라이브러리입니다. 리액트 역시 라이브러리인데, 번들링 된 파일은 *react.js* 자바스크립트 파일 하나뿐이기 때문입니다. CDN을 별도로 호스팅 하거나 응용 프로그램 내에 설치하여 사용 가능합니다. 

CDN을 사용하려면 HTML 파일 내 `<script>` 인라인 태그로 CDN url을 작성하면 됩니다. *react*와 *react-dom* 두 라이브러리 url을 추가합니다.

{title="Code Playground",lang="javascript"}
~~~~~~~~
<script crossorigin src="https://unpkg.com/react@16/umd/react.development.js"></script>
<script crossorigin src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>
~~~~~~~~

### 1.4.2 npm
가장 일반적인 방법인 `npm` 명령어로 리액트를 설치하겠습니다. 애플리케이션 내 *package.json* 파일이 있다면, npm 명령어로 *react*와 *react-dom*을 설치할 수 있습니다. 제일 먼저 *npm init -y* 명령어를 입력해 *package.json*을 초기화합니다. 그 다음 npm 명령어 한 줄로 여러 노드 패키지를 한 번에 설치합니다.

{title="Command Line",lang="text"}
~~~~~~~~
npm init -y
npm install react react-dom
~~~~~~~~

이와 같은 방법은 현재 개발 중인 애플리케이션에 리액트를 추가할 때 사용됩니다.

리액트 설치를 했다고 해서 모든 준비가 끝난 것이 아닙니다. JSX(리액트 문법)과 자바스크립트 ES6로 만든 애플리케이션은 [바벨(babel)](http://babeljs.io/)을 사용합니다. 모든 브라우저가 ES6 문법을 해석할 수 없기 때문에, 바벨을 통해 자바스크립트 ES6와 JSX를 ES5로 변환해야 합니다. 바벨 설치를 위해 많은 환경 설정과 도구가 필요하기 때문에, 입문 수준에서 바벨 사용은 적합하지 않습니다.

이러한 이유로 페이스북은 누구나 손쉽게 환경 설정 작업을 할 수 있는 제로 구성 설치(zero-configuration) 솔루션인 *create-react-app* 패키지를 만들었습니다. 우리는 이 패키지로 리액트 애플리케이션을 만들어 볼 것입니다.

### 읽어보기

* [[리액트 공식 문서] 리액트 설치](https://reactjs.org/docs/try-react.html)

## 1.5 create-react-app 

이 책에서는 [*create-react-app*](https://github.com/facebookincubator/create-react-app)으로 애플리케이션을 부트스트래핑합니다. 부트스트래핑(bootstrapping)이라는 뜻은 애플리케이션을 최초 생성하여 브라우저에서 실행하는 과정을 말합니다. *create-react-app*은 2016년 페이스북이 제안한 리액트 제로 구성 설치 스타터 킷(Zero-Configuration Setup Starter Kit)입니다. 트위터에서 진행한 한 [조사](https://twitter.com/dan_abramov/status/806985854099062785)에 따르면 96% 이상의 리액트 개발자들이 입문자의 경우 *create-react-app* 사용을 권장하고 있습니다.

*create-react-app*는 리액트 개발 도구와 환경 설정이 이미 세팅되어 있기 때문에, 우리는 오롯이 애플리케이션 구현에만 신경 쓰면 됩니다. 

이제 전역 노드 패키지에 *create-react-app* 패키지를 설치하겠습니다.

{title="Command Line",lang="text"}
~~~~~~~~
npm install -g create-react-app
~~~~~~~~

*create-react-app* 버전을 확인해, 성공적으로 패키지가 설치되었는지 확인합시다.

{title="Command Line",lang="text"}
~~~~~~~~
create-react-app --version
*v1.5.1
~~~~~~~~

여러분의 첫 번째 리액트 애플리케이션을 시작해보겠습니다. 앞으로 새로운 리액트 애플리케이션을 부트스트랩 할 때마다 `create-react-app <name>` 명령어를 입력하면 됩니다.

우리가 만들 애플리케이션은 해커 뉴스 플랫폼이므로 프로젝트 이름을 *hackernews*라 하겠습니다. 물론 다른 이름을 사용해도 괜찮습니다. 몇 초 후에 부트스트래핑이 완료될 것입니다. 설치가 완료되면 생성된 폴더 안으로 들어갑니다.

{title="Command Line",lang="text"}
~~~~~~~~
create-react-app hackernews
cd hackernews
~~~~~~~~

코드에디터에서 애플리케이션을 열어 봅시다. 디렉터리에 아래와 같은 *create-react-app* 구조가 보일 것입니다.

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
    manifest.json
  src/
    App.css
    App.js
    App.test.js
    index.css
    index.js
    logo.svg
    registerServiceWorker.js
~~~~~~~~

각 파일과 폴더 단위가 무엇을 지칭하는지 알아보겠습니다. 처음부터 모든 것을 이해하지 않아도 됩니다.

* **README.md:** *.md* 확장자는 마크다운(markdown) 파일입니다. 일반 텍스트와 함께 간단한 마크업 언어로 작성합니다. 대부분 프로젝트에는 *README.md* 파일에 프로젝트 설명 및 설치 방법 등 안내 사항을 작성합니다. 깃허브 리퍼지토리 페이지의 첫 화면에 *README.md* 파일을 보았을 겁니다. *create-react-app* 을 설치한 후 바로 깃허브에 프로젝트를 올린다면 *README.md* 파일 내용은 [create-react-app 공식 깃허브 리퍼지토리](https://github.com/facebookincubator/create-react-app)와 내용이 동일할 것입니다.

* **node_modules/** 이 폴더에는 npm으로 설치되었던 모든 노드 패키지를 포함합니다. 이미 *create-react-app* 으로 리액트 애플리케이션을 부트스트래핑 했기 때문에 여러 모듈이 설치되어 있습니다. 이 폴더는 절대로 건드리지 말아야 합니다. `npm` 명령어를 사용해 패키지를 설치 또는 제거합니다.

* **package.json** 노드 패키지 의존성 및 기타 프로젝트 구성 목록을 포함합니다.

* **.gitignore** git을 사용할 때, 원격 git 리퍼지토리에 제외시킬 모든 파일과 폴더 목록이 나열되어 있습니다. 예를 들어 *node_module*은 로컬에만 있어야 합니다. 의존성 폴더 전체를 올리지 않고도 공유된 *package.json* 파일만으로 의존성 패키지를 설치할 수 있습니다.

* **public/** 배포를 위해 프로젝트 빌드 시 필요한 모든 파일을 말합니다. 프로젝트를 빌드할 때, *src/* 폴더 내 모든 코드는 몇 개의 파일로 묶여 *public* 폴더에 배치됩니다.

* **build/** 프로덕션 빌드 시 생성되는 폴더입니다. 빌드 시 *src/*와 *public* 폴더에 있는 파일들이 번들되어 *build* 폴더에 저장됩니다.

* **manifest.json** 및 **registerServiceWorker.js** 지금 단계에서는 무시해도 됩니다. 프로젝트에서 사용하지 않은 파일입니다.

위에서 언급된 파일 및 폴더를 건드릴 필요가 없습니다. 우리가 필요한 모든 것은 *src/* 폴더 안에 있습니다. 제일 중요한 파일은 *src/App.js*로 이 파일에 리액트 컴포넌트가 있습니다. 현재 이 파일이 전체 애플리케이션을 이루고 있지만, 나중에 리액트 컴포넌트를 여러 파일로 분절하여 나눠 관리할 수 있습니다.

이외에 테스트를 위한 *src/App.test.js* 파일과 리액트의 진입점인 *src/index.js* 파일이 보일 겁니다. 다음 장에서 두 파일에 대해 알게 될 것입니다. 또한 애플리케이션과 컴포넌트 스타일을 지정하는 *src/index.css* 및 *src/App.css* 파일이 있습니다. 현재는 기본 스타일만 적용되어 있습니다.

* *create-react-app* 애플리케이션은 npm 프로젝트입니다. npm을 사용하여 프로젝트에 노드 패키지를 설치하고 제거할 수 있습니다.

{title="Command Line",lang="text"}
~~~~~~~~
// http://localhost:3000 에서 애플리케이션 실행
npm start

// 테스트 실행
npm test

// 배포를 위한 애플리케이션 빌드
npm run build
~~~~~~~~

*package.json*에 스크립트 명령어를 정의합니다. 이제 브라우저를 열어 리액트 애플리케이션이 구동되는지 확인해봅시다.

### 실습하기

* `npm start`을 실행해 브라우저에서 애플리케이션을 확인합니다.
* `npm test`를 실행합니다.
* *public/* 폴더 내 어떤 내용이 있는지 확인하고, `npm run build` 스크립트를 실행해 폴더에 어떤 파일이 추가되었는지 확인합니다. (추가된 파일은 프로젝트에 영향을 주지 않으므로 제거해도 됩니다.)
* 폴더 구조에 익숙해집니다.
* 파일 내용에 익숙해집니다.

### 읽어보기
* [create-react-app 깃허브 리퍼지토리](https://github.com/facebookincubator/create-react-app)

## 1.6 JSX 기초

리액트 JSX 문법에 대해 알아봅시다. *create-react-app*로 애플리케이션을 부트스트래핑 하면 리액트 애플리케이션이 이미 구현되어 있습니다. *src/App.js* 소스 코드를 열어 내용을 읽어봅시다.

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

자바스크립트 ES6 기능인 `import`과 `export`, 클래스(class) 문법을 벌써부터 공부하려고 조급해할 필요가 없습니다. 앞으로 차근차근 배워볼 것입니다.

`App.js` 파일에 **리액트 ES6 클래스 컴포넌트(React ES6 Class Component)** 를 볼 수 있습니다. 이 파일에서 컴포넌트를 선언했습니다. 

리액트 컴포넌트는, HTML 엘레먼트(element)와 같이 애플리케이션의 어느 곳이든지 재사용될 수 있습니다. 컴포넌트가 인스턴스화 되었기 때문입니다. 다른 말로 컴포넌트의 인스턴스를 사용한다고 말합니다.

`render()` 메서드에서 **엘레먼트(element)** 가 반환(return)됩니다. 여러 엘레먼트가 모여 컴포넌트 전체를 구성합니다. 컴포넌트(component), 인스턴스(instance), 엘레먼트(element) 간의 용어와 각 쓰임의 차이를 알고 있다면 이해가 쉬울 것입니다. 

이제 `App` 컴포넌트가 인스턴스화 된 것을 볼 수 있습니다. 인스턴스화 되지 않았다면, 브라우저에서 렌더링 된 결과를 볼 수 없습니다. App 컴포넌트가 선언되었지만 다른 곳에 재사용되지 않았습니다. JSX문법인 `<App/>`를 사용하면 어느 컴포넌트에서나 인스턴스 화하여 재사용할 수 있습니다. 

`render()` 블록 안 내용은 HTML과 비슷해 보이지만, JSX 문법으로 작성되었습니다. HTML과 자바스크립트를 결합해 JSX 문법을 작성합니다. 지금까지 HTML과 자바스크립트를 따로 사용해 개발해왔다면 새로운 문법이 꽤 혼란스러울 것입니다. 먼저 JSX에서 기본적인 HTML 엘레먼트를 작성하겠습니다. 먼저 `App` 컴포넌트의 모든 내용을 지우고 아래와 같이 작성합니다.

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

자바스크립트 없이 `render()` 메서드로 HTML를 반환했습니다. 새 변수를 만들어 "리액트에 오신 여러분을 환영합니다"라는 값을 주고, 이 변수를 JSX 문법인 중괄호(`{}`) 안에 넣습니다.

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

아마 `className`가 속성(attribute) 임을 눈치챘을 겁니다. JSX의 `className`은 HTML 표준 `class`로부터 기인했습니다. 기술적인 이유로 JSX는 몇 가지 내부 HTML 속성을 바꿔야 했습니다. 자세한 내용은 리액트 공식 문서 중 ['지원하는 HTML 속성'](https://reactjs.org/docs/dom-elements.html) 장에서 확인하길 바랍니다. HTML 속성은 카멜 케이스(camelCase) 표기법을 따릅니다. 앞으로 몇 가지 JSX 속성을 좀 더 다뤄볼 것입니다.

### 실습하기

* JSX 안에 새 변수를 만들고 렌더링 합니다.
  * `user` 객체를 만들고 JSX 문법으로 `firstname`과 `lastname`을 표시합니다.
  * `render()`메서드 안에 `user` 프로퍼티를 사용합니다. 
  
### 읽어보기  

* [[리액트 공식 문서] JSX](https://reactjs.org/docs/introducing-jsx.html)
* [[리액트 공식 문서] 리액트 컴포넌트, 엘레먼트, 인스턴스](https://reactjs.org/blog/2015/12/18/react-components-elements-and-instances.html)

## 1.7 ES6 const · let

이전 장에서 `var` 문을 사용해 변수 `helloWorld`를 선언했습니다. 자바스크립트 ES6에서 변수 선언은 `const` 또는 `let`을 사용합니다. ES6에서는 `var`를 사용하는 일은 거의 없습니다.

`const`로 선언된 변수는 다시 할당하거나 선언할 수 없습니다. 불변 데이터 구조(immutable data structures)이기 때문에 한 번 데이터 구조가 정의된 이후, 다시 변경할 수 없습니다.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// 불가능
const helloWorld = '리액트에 오신 여러분을 환영합니다';
helloWorld = '안녕 리액트';
~~~~~~~~

반면 `let`은 변경 가능합니다. 아래와 같이 변수를 다시 할당할 수 있습니다.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// 가능
let helloWorld = '리액트에 오신 여러분을 환영합니다';
helloWorld = '안녕 리액트';
~~~~~~~~

기본적으로 `const`로 선언된 변수는 변경할 수 없습니다. 그러나 변수가 배열이나 객체 타입일 경우 변경 가능합니다.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// 가능
const helloWorld = {
  text: '리액트에 오신 여러분을 환영합니다'
};
helloWorld.text = '안녕 리액트';
~~~~~~~~

그렇다면 언제 `let` 또는 `const`를 사용해야 할까요? 정해진 법칙은 없지만, 가능하면 기본적으로 `const`를 사용하는 것이 좋습니다. `const`를 사용함은 객체와 배열의 값이 변경되더라도 데이터 구조를 변경되지 않음을 뜻하기 때문입니다. 반대로 변경되는 변수는 `let`을 사용합니다.

리액트 생태계는 **불변성 원칙**을 준수하기 때문에, 변수를 정의할 때 `let`보다 `const` 사용을 우선시 합니다. 복잡한 객체에서 내부 값을 수정해야 되는 경우도 있으니 `const` 사용에 주의합시다. 

이 책은 `var` 대신 `const`을 사용합니다. `var`을 `const`로 바꿉니다.

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
* 불변 데이터 구조에 대해 더 알아봅시다.
  * 보편적인 프로그래밍에서 말하는 불변 데이터 구조란 무엇인지 알아봅시다.
  * 리액트와 그 생태계에서 불변 데이터 구조가 사용되는 이유에 대해 알아봅시다.

## 1.8 ReactDOM

App 컴포넌트를 바꾸기 전에, 이 컴포넌트가 어디에서 사용되고 있는지 확인해봅시다. 리액트의 첫 진입점이라 할 수 있는 *src/index.js* 파일에 App 컴포넌트가 있습니다.

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

`ReactDOM.render()` 메서드는 HTML의 DOM 노드를 JSX로 대체하는 일을 합니다. 때문에 타 애플리케이션에서도 리액트가 쉽게 통합될 수 있습니다. 애플리케이션 내 `ReactDOM.render()` 메서드를 여러 번 사용할 수 있습니다. JSX 구문, 단일 리액트 컴포넌트, 다중 리액트 컴포넌트, 또는 전체 응용 프로그램을 부트스트랩 할 수 있습니다. 그러나 관용적으로 리액트 애플리케이션은 전체 컴포넌트 트리를 부트스트랩 하기 위해 `ReactDOM.render()` 메서드를 한 번만 사용합니다. 

`ReactDOM.render()` 메서드는 두 개의 인자가 필요합니다. 첫 번째 인자는 렌더링 된 JSX, 두 번째 인자는 리액트 애플리케이션이 HTML에 들어갈 위치입니다. `id`가 `root`인 엘레먼트에 들어가게 됩니다. *public/index.html* 파일을 열어 id 속성을 확인해봅시다.

`ReactDOM.render()` 메서드가 실행되어 이미 App 컴포넌트를 사용하고 있습니다. 되도록 애플리케이션 전체를 컴포넌트를 만들어 전달하는 것이 좋습니다. 이 파일에서 컴포넌트를 인스턴스화하지 않아도 됩니다.

{title="Code Playground",lang=javascript}
~~~~~~~~
ReactDOM.render(
  <h1>안녕 리액트</h1>,
  document.getElementById('root')
);
~~~~~~~~

### 실습하기

* *public/index.html* 파일을 열어 HTML 안에 리액트 애플리케이션 들어가는 곳을 찾아봅니다.

### 읽어보기
* [[리액트 공식 문서] - 리액트 내 렌더링 되는 엘레먼트](https://reactjs.org/docs/dom-elements.html#all-supported-html-attributes)

## 1.9 Hot Module Replacement

더 나은 개발 환경을 위해 *src/index.js* 파일에서 할 일이 하나 더 있습니다. 이는 필수가 아닌 옵션 사항으로 리액트 입문자가 꼭 해야 할 내용은 아닙니다.

*create-react-app*은 소스 코드 변경 시, 자동으로 브라우저를 새로고침 합니다. *src/App.js* 파일 내 `helloWorld`를 다른 변수로 변경해보면 브라우저가 자동으로 새로고침 됩니다. 때문에 매번 수정할 때마다 새로고침을 하지 않아 편리합니다. 그러나 더 좋은 방법이 있습니다. 

Hot Module Replacement(핫 모듈 리플레이스먼트, 이하 HMR)란 브라우저 내 애플리케이션을 재실행하는 도구입니다. *create-react-app*에서도 쉽게 사용할 수 있습니다. 이를 위해 *src/index.js* 파일을 열고 아래와 같이 설정을 추가하겠습니다.

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

위 코드가 전부입니다. *src/App.js* 파일에서 `helloWorld` 변수를 다른 것으로 바꿔봅시다. 브라우저는 페이지 새로고침 되지 않지만, 애플리케이션이 재실행되어 올바른 결과가 출력됩니다. 이처럼 HMR은 장점이 많습니다.

`console.log()`로 디버깅을 해봅시다. 코드를 수정해도 브라우저 페이지가 새로고침 되지 않아 개발자 도구 콘솔에 과거 출력 히스토리가 그대로 남아있습니다. 디버깅이 훨씬 쉬워질 것입니다. 

애플리케이션이 점차 고도화되면서 페이지 새로고침은 개발 생산성을 지연시킵니다. 페이지가 로드될 때까지 기다려야 하며, 대규모 애플리케이션에서 페이지를 다시 로드하는데 몇 초 이상이 걸릴 수 있습니다. HMR은 이러한 단점을 해결합니다. 

가장 큰 장점은 HMR로 애플리케이션 상태를 유지할 수 있다는 것입니다. 애플리케이션에 3단계의 대화창이 있다고 가정해봅시다. 쉽게 다이얼로그 창 기능이라고 생각하면 됩니다. HMR이 없다면 소스 코드를 변경할 때마다 브라우저 페이지가 새로고침 됩니다. 매번 창을 다시 열어 1단계에서 3단계로 이동시켜야 하는 번거로운 일을 해야 합니다. HMR을 사용하면 다이얼로그 창은 3단계 상태로 유지되며, 소스 코드가 수정되어도 애플리케이션의 상태는 그대로 유지됩니다. 애플리케이션 자체는 다시 실행되지만 페이지는 새로고침 되지 않습니다.

### 실습하기

* *src/App.js* 코드를 수정해 HMR이 어떻게 실행되는지 확인합니다.

### 영상보기

* 댄 애브라몹(Dan Abramov)의 [리액트 라이브 : Hot Reloading으로 시간 여행 떠나기](https://www.youtube.com/watch?v=xsSnOQynTHs) 10분간 시청합니다.

## 1.10 JSX 내 자바스크립트 객체 처리

App 컴포넌트로 다시 돌아갑시다. 지금까지 JSX에서 초기 변수를 렌더링 했습니다. 이제 배열의 각 항목을 차례로 렌더링 해봅시다. 지금은 샘플 데이터를 사용하지만 앞으로 외부 [API](https://www.robinwieruch.de/what-is-an-api-javascript/)를 통해 데이터를 가져올 것입니다. 앞으로 점점 재밌고 흥미 있어질 거니 기대해도 좋습니다.

먼저 아래와 같이 `list`를 정의합시다.

{title="src/App.js",lang=javascript}
~~~~~~~~
import React, { Component } from 'react';
import './App.css';

# leanpub-start-insert
const list = [
  {
    title: 'React',
    url: 'https://reactjs.org/',
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

`list`는 객체로 이루어진 배열입니다. 각 프로퍼티에는 제목(title), url, 작성자(author), 식별자(objectID), 기사의 인기도를 나타내는 점수(point), 댓글 수(num_comments)가 있습니다.

이제 JSX에서 자바스크립트 반복 메서드인 `map()`을 사용해 배열의 모든 데이터를 출력해봅시다. JSX는 자바스크립트 표현식을 캡슐화하기 위해 중괄호(`{}`)를 사용합니다.

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

JSX는 HTML과 자바스크립트를 함께 사용합니다. `map()`를 사용해 HTML 태그와 섞어 엘레먼트를 출력해보겠습니다.

현재는 `title` 프로퍼티 값만 출력됩니다. 이제 모든 프로퍼티 값을 보여줍시다.

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

JSX와 `map()` 메서드로 반복문을 작성했습니다. 각 프로퍼티를 HTML `<span>` 태그로 감싸고, url은 a 태그의 href 속성과 함께 사용했습니다.

그러나 아직 한 가지가 남았습니다. 바로 각 엘레먼트마다 `key` 속성을 추가해야 합니다. 리액트는 `key`로 배열의 수정 및 제거된 항목을 식별합니다. 우리는 `objectID` 프로퍼티를 `key`로 지정하겠습니다.

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

`key` 값은 고윳값으로 식별 가능해야 합니다. 배열의 인덱스 값은 고정값이 아니기 때문에 사용하지 말아야 합니다. 배열의 순서가 변경되면, 리액트가 각 항목을 식별할 수 없기 때문입니다.

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

* [[리액트 공식 문서] 리액트 리스트(lists)와 키(keys)](https://reactjs.org/docs/lists-and-keys.html)
* [[MND] 자바스크립트 map()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map)

### 실습하기
* JSX 내 자바스크립트 표현식(expression)을 작성합니다.

## 1.11 ES6 화살표 함수

자바스크립트 ES6에서 함수 표현식을 위해 새로운 문법인 화살표 함수(Arrow function)가 도입되었습니다. 화살표 함수를 사용하면 기존 함수 표현식보다 코드가 간결해집니다.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// 함수 표현식 
function () { ... }

// 화살표 함수 표현식
() => { ... }
~~~~~~~~

화살표(`=>`)가 하는 일을 알고 있어야 합니다. 화살표 함수는 기존 함수 표현식과 `this`를 다르게 정의합니다. 기존 함수 표현식은 자기 자신을 `this` 객체로 정의하지만, 화살표 함수 표현식은 자기 자신을 `this`를 생성하지 않고, 감싸고 있는 본문 컨텍스트로에서 의미를 갖습니다. 화살표 함수에서 `this` 사용을 혼동하지 맙시다.

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

앞서 작성한 `map()` 코드 부분을 ES6 화살표 표현식으로 고쳐봅시다.

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

또한 ES6 화살표 함수에서 중괄호(`{}`) 부분인 *블록 본문*을 생략합니다. *간결한 본문*에서는 반환(return)이 포함되어, `return` 문을 삭제할 수 있습니다. 화살표 함수를 자주 보게 될 것이기 때문에, 화살표 기능을 사용할 때 블록 본문과 간결한 본문의 차이점을 이해하고 있어야 합니다.

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

이제 JSX가 간결해지고 보기 쉬워졌습니다. 함수 선언, 중괄호(`{}`) , return 문을 생략할 수 있습니다. 이로써 세부적인 구현 내용에 집중할 수 있게 되었습니다. 

### 읽어보기

* [[MDN] ES6 화살표 함수](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Arrow_functions)

## 1.12 ES6 클래스

자바스크립트 ES6에서 클래스(Class)가 도입되었습니다. 클래스는 일반적으로 객체 지향 프로그래밍 언어에서 사용합니다. 자바스크립트는 융통성과 유연성이 뛰어난 프로그래밍 언어로 함수형 프로그래밍과 객체지향 프로그래밍으로 모두 작성할 수 있습니다.

리액트는 함수형 프로그래밍과 객체 프로그래밍의 모든 장점을 취하고 있습니다. 리액트는 불변하는 데이터 구조를 다룰 때는 함수형 프로그래밍을, 컴포넌트를 만들 때는 클래스로 선언합니다.

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

클래스를 인스턴스 화하는 방법과 클래스를 사용하는 방법을 살펴봅시다.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const robin = new Developer('Robin', 'Wieruch');
console.log(robin.getName());
// 출력: Robin Wieruch
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

App 클래스는 `Component`로 확장(extend)됩니다. 기본적으로 App 컴포넌트에서 확장을 선언했지만, 다른 컴포넌트에서도 선언할 수 있습니다. 그렇다면 확장(extend)이란 무엇일까요? 객체 지향 프로그래밍에는 상속의 원칙이 있습니다. 한 클래스의 기능을 다른 클래스로 전달하는 데 사용됩니다. 

App 클래스는 `Component` 클래스의 기능을 확장함으로, `Component` 기능을 상속받습니다. `Component` 클래스는 ES6 클래스를 ES6 컴포넌트 클래스로 확장됩니다. 따라서 컴포넌트 클래스 메서드를 쓸 수 있는 것입니다. 그중 `render()`은 우리가 이미 사용했던 컴포넌트 클래스 메서드입니다. 앞으로 여러 컴포넌트 클래스 메서드에 대해 배울 것입니다.

`Component` 클래스는 리액트 컴포넌트의 모든 구현 세부 사항을 캡슐화하기 때문에 리액트에서 클래스를 컴포넌트로 사용할 수 있습니다. 

지금까지 ES6 클래스 기초와 이를 리액트 컴포넌트로 사용하는 방법을 배웠습니다. 컴포넌트 메서드는 리액트 생명주기(Life Cycle) 절에서 자세히 배워볼 것입니다.

### 읽어보기

* [[MND] ES6 클래스](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes)

{pagebreak}

## 1.13 정리하면

1장에서는 *create-react-app*으로 리액트 애플리케이션을 부트스트래핑을 했습니다. 지금까지 학습한 내용을 정리해봅시다.

* 리액트 
    * *create-react-app*으로 리액트 애플리케이션을 부트스트래핑 합니다. 
    * JSX 문법으로 `render()` 메서드 내 HTML과 자바스크립트를 같이 사용해 컴포넌트의 결괏값을 출력합니다. 
    * 컴포넌트, 인스턴스, 엘레먼트는 서로 다릅니다. 
    * `ReactDOM.render()`는 리액트 애플리케이션이 리액트를 DOM에 연결시키는 엔트리 포인트(entry point)입니다. 
    * JSX에서 자바스크립트 내장 함수를 사용할 수 있습니다. 
    * 배열의 엘레먼트를 렌더링 할 때 `map()`와 HTML 구문과 함께 사용합니다. 

* ES6 
    * 상황에 따라 `const`와 `let`로 변수를 선언합니다. 
    * 리액트에서는 가능한 `const`로 변수를 선언합니다. 
    * 화살표 함수를 사용해 간결한 함수로 만듭니다. 
    * 클래스를 확장하여 리액트 컴포넌트로 정의합니다. 

잠시 휴식 시간을 가집시다. 학습 내용을 되새기고 적용해보며 지금까지 작성한 코드와 배운 내용을 내 것으로 만듭시다. 이것저것 만들어보며 테스트해보길 바랍니다. 

앞으로 만들어볼 해커 뉴스 애플리케이션이 궁금하다면 [깃허브 리퍼지토리](https://github.com/the-road-to-learn-react/hackernews-client/tree/5.1)를 확인하세요.
