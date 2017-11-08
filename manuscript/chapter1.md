# 리액트 시작하기

이번 장은 리액트 기초적인 내용을 다룬다. 아직도 리액트를 왜 배워야하는지 모르겠다면 이번 장을 통해 그 답을 찾을 수 있을 것이다. 리액트 애플리케이션 설치를 시작으로 리액트의 세계에 첫 발을 담궈 보자. JSX와 ReactDOM를 배우고 첫 번째 리액트 컴포넌트를 만들어 보자.

## 반가워, 내 이름은 리액트야

**왜 리액트를 배워야 할까?** 최근 몇 년 간 단일 페이지 애플리케이션 ([SPA] (https://en.wikipedia.org/wiki/Single-page_application))이 각광받고 있다. Angular, Ember 및 Backbone 등 자바스크립트 프레임워크의 등장은 바닐라 자바스크립트('Vanilla JavaScript' 타 라이브러리나 프레임워크 사용 없이 순수한 자바스크립트로 개발하는 것을 말함)와 jQuery를 사용하지 않고도 최신 웹 응용 프로그램을 구축할 수 있게 만들었다. 이외에도 SPA 프레임워크는 매우 다양하다. Angular 2010, Backbone 2010, Ember 2011 등 배포된 대부분 SPA 프레임워크는 1세대이다.

리액트는 2013년 페이스북이 처음 공표했다. 리액트는 SAP 프레임워크가 아닌, 뷰 라이브러리(View Library)이다. 여기서 뷰(View)란 [MVC](https://de.wikipedia.org/wiki/Model_View_Controller) (Model–View–Controller, MVC는 소프트웨어 공학에서 사용되는 소프트웨어 디자인 패턴을 말함) 패턴의 'V'를 말한다. 뷰는 브라우저 내 특정 컴포넌트를 보여준다. 그러나 리액트를 이루는 전체는 하나의 단일 페이지 애플리케이션을 제작할 수 있게 해준다.

그렇다면 수많은 1세대 SPA 프레임워크 중 리액트를 선택해야하는 이유는 무엇일까? 1세대 프레임워크는 한 번에 많은 것을 해결하려고 노력했지만, 리액트는 뷰 레이어를 만드는 역할만 한다. 리액트는 프레임워크가 아닌 라이브러리이다. 뷰는 컴포넌트로서 다른 컴포넌트와 함께 계층구조를 이룬다.

리액트 장점은 여러 복잡한 기능을 추가하기전 오롯이 뷰 레이어에 집중하여 개발할 수 있다는 것이다. SPA를 집이라고 한다면 리액트는 집을 짓는 벽돌과 같다. 복잡한 애플리케이션을 개발할 때 한 벽돌씩 만드는 작업이 필요하다 두 가지 장점이 있다. 

단계별로 하나씩 벽돌을 쌓는 법을 배울 수 있다. 한꺼번에 모두 이해하는 것을 걱정할 필요가 없다. 처음부터 모든 구조를 갖춘 프레임워크와 다르다. 리액트는 그 중 첫 번째이며 더 많은 벽돌들과 연결된다.

모든 벽돌은 상호 교환이 가능하다. 때문에 리액트 생태계는 혁신적이다. 여러 솔루션들이 서로 경쟁하고 있으며 각자 상황에 맞게 가장 적합한 솔루션을 선택하면 된다.

SPA 프레임 워크의 1세대는 이미 상용화 단계에 이르렀고 더욱 견고했지만, 여전히 리액트는 혁신적이다. [페이스북은 물론이거니와, 에어비앤비, 넷플릭스](https://github.com/facebook/react/wiki/Sites-Using-React) 등 선도적인 테크 회사들이 리액트를 채택해 플랫폼 개발을 하고 있다. 그들 모두가 리액트와 생태계에 만족하고 있으며 리액트의 미래에 투자하고 있다.

리액트는 최근 웹 애플리케이션을 개발하는데 가장 좋은 선택이 될 것이다. 뷰 레이어만 제공하지만 (그러나 리액트 생태계는 모든 프레임워크와 서로 상호 교환 가능하다) (https://www.robinwieruch.de/essential-react-libraries-framework/). 리액트는 간결한 API, 놀라운 생태계, 훌륭한 커뮤니티를 갖추고 있다. ["왜 나는 앵귤러(Angular)에서 리액트로 옮겼는가](https://www.robinwieruch.de/reasons-why-i-moved-from-angular-to-react/)" 블로그에서 리액트를 선택한 개인적인 경험을 작성했다. 굳이 타 프레임워크나 라이브러리가 아닌, 리액트를 선택해 개발하는 이유를 스스로에게 묻는 것이 매우 중요하다. 결국 모든 사람들이 다음 해 리액트 행보에 대해 궁금해 할 것이다.  


### 더 읽어보기

* [[영문] 왜 나는 앵귤러에서 리액트로 옮겼는가](https://www.robinwieruch.de/reasons-why-i-moved-from-angular-to-react/)
* [[영문] 유연한 리액트 생태계](https://www.robinwieruch.de/essential-react-libraries-framework/)

## 요구사항

이전에 SPA 프레임워크나 라이브러리 사용 경험이 있다면 이미 웹 개발 기초 지식이 있을 것이다. 이제 막 웹 개발에 입문했다면 리액트를 배우기 전에 HTML, CSS, 자바스크립트 ES5를 잘 다룰 수 있어야 한다. 이 책에서 자바스크립트 ES6로 점차적으로 변환할 것이다.[공식 슬랙 그룹](https://slack-the-road-to-learn-react.wieruch.com/)에 가입해 동료들을 만나고 서로에게 도움을 주길 바란다.

### 코드에디터, 터미널

개발 환경은 어떻게 갖춰야할까? 코드에디터나 IDE, 터미널(ter커맨드라인 command line)이 필요하다. [개발 환경 설정 가이드](https://www.robinwieruch.de/developer-setup/)를 참고하라. MacOS을 대상으로 작성했으나 타 운영체제에 적용하여도 무방하다. 이미 개발 환경 설정 방법에 관한 수 많은 가이드 문서가 있기 때문에 사용 중인 운영체제에 알맞는 가이드를 쉽게 찾을 수 있을 것이다.

git와 깃헙(GitHub)을 사용하여 책에서 실습한 내용을 깃헙 저장소에 커밋하여 프로젝트를 계속해서 진행할 수 있다. 자세한 내용은 [GIT 가이드](https://www.robinwieruch.de/git-essential-commands/)를 읽기 바란다. git 사용은 의무 사항이 아니다. 웹 개발 초보자인 경우 모든 것을 한꺼번에 배우려고 하면 매우 부담스러울 수 있다. 때문에 초보자는 git과 깃헙을 쓰지말고 본문 내용에 집중하기 바란다.

### Node, NPM

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

The installed package will automatically appear in a folder called *node_modules/* and will be listed in the *package.json* file next to your other dependencies.

But how to initialize the *node_modules/* folder and the *package.json* file for your project in the first place? There is a npm command to initialize a npm project and thus a *package.json* file. Only when you have that file, you can install new local packages via npm.

설치된 패키지는 생성된 *node_modules/* 폴더에 저장되며 종속된 *package.json* 파일에 패키지 리스트가 나열된다.

 *package.json* 파일이 있어야만 npm 명령어로 *node_modules/* 폴더를 초기화할 수 있다. 아래 명령어로 새 지역 패키지를 설치한다.
 
{title="Command Line",lang="text"}
~~~~~~~~
npm init -y
~~~~~~~~

The `-y` flag is a shortcut to initialize all the defaults in your *package.json*. If you don't use the flag, you have to decide how to configure the file. After initializing your npm project you are good to install new packages via `npm install <package>`.

One more word about the *package.json*. The file enables you to share your project with other developers without sharing all the node packages. The file has all the references of node packages used in your project. These packages are called dependencies. Everyone can copy your project without the dependencies. The dependencies are references in the *package.json*. Someone who copies your project can simply install all packages by using `npm install` on the command line. The `npm install` script takes all the dependencies listed in the *package.json* file and installs them in the *node_modules/* folder.

I want to cover one more npm command:

{title="Command Line",lang="text"}
~~~~~~~~
npm install --save-dev <package>
~~~~~~~~

The `--save-dev` flag indicates that the node package is only used in the development environment. It will not be used in production when you deploy your application on a server. What kind of node package could that be? Imagine you want to test your application with the help of a node package. You need to install that package via npm, but want to exclude it from your production environment. Testing should only happen during the development process but not when your application is already running in production. There you don't want to test your application anymore. It should be tested already and work out of the box for your users. That's only one use case where you would want to use the `--save-dev` flag.

You will encounter more npm commands on your way. But these will be sufficient for now.

### Exercises:

* setup a throw away npm project
  * create a new folder with `mkdir <folder_name>`
  * navigate into the folder with `cd <folder_name>`
  * execute `npm init -y` or `npm init`
  * install a local package like React with `npm install react`
  * have a look into the *package.json* file and the *node_modules/* folder
  * find out on your own how to uninstall the *react* node package again
* read more about [npm](https://docs.npmjs.com/)

## Installation

There are multiple approaches to get started with a React application.

The first one is to use a CDN. That may sound more complicated than it is. A CDN is a [content delivery network](https://en.wikipedia.org/wiki/Content_delivery_network). Several companies have CDNs that host files publicly for people to consume them. These files can be libraries like React, because after all the bundled React library is only a *react.js* JavaScript file. It can be hosted somewhere and you can require it in your application.

How to use a CDN to get started in React? You can inline the `<script>` tag in your HTML that points to a CDN url. To get started in React you need two files (libraries): *react* and *react-dom*.

{title="Code Playground",lang="javascript"}
~~~~~~~~
<script crossorigin src="https://unpkg.com/react@16/umd/react.development.js"></script>
<script crossorigin src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>
~~~~~~~~

But why should you use a CDN when you have npm to install node packages such as React?

When your application has a *package.json* file, you can install *react* and *react-dom* from the command line. The requirement is that the folder is initialized as npm project by using `npm init -y` with a *package.json* file. You can install multiple node packages in one line with npm.

{title="Command Line",lang="text"}
~~~~~~~~
npm install react react-dom
~~~~~~~~

That approach is often used to add React to an existing application that is managed with npm.

Unfortunately that's not everything. You would have to deal with [Babel](http://babeljs.io/) to make your application aware of JSX (the React syntax) and JavaScript ES6. Babel transpiles your code so that browsers can interpret JavaScript ES6 and JSX. Not all browsers are capable of interpreting the syntax. The setup includes a lot of configuration and tooling. It can be overwhelming for React newcomers to bother with all the configuration.

Because of this reason, Facebook introduced *create-react-app* as a zero-configuration React solution. The next chapter will show you how to setup your application by using this bootstrapping tool.

### Exercises:

* read more about [React installations](https://facebook.github.io/react/docs/installation.html)

## Zero-Configuration Setup

In the Road to learn React, you will use [create-react-app](https://github.com/facebookincubator/create-react-app) to bootstrap your application. It's an opinionated yet zero-configuration starter kit for React introduced by Facebook in 2016. People would [recommend it to beginners by 96%](https://twitter.com/dan_abramov/status/806985854099062785). In *create-react-app* the tooling and configuration evolve in the background while the focus is on the application implementation.

To get started, you will have to install the package to your global node packages. After that, you always have it available on the command line to bootstrap new React applications.

{title="Command Line",lang="text"}
~~~~~~~~
npm install -g create-react-app
~~~~~~~~

You can check the version of *create-react-app* to verify a successful installation on your command line:

{title="Command Line",lang="text"}
~~~~~~~~
create-react-app --version
*v1.4.1
~~~~~~~~

Now you can bootstrap your first React application. We call it *hackernews*, but you can choose a different name. The bootstrapping takes a couple of seconds. Afterward, simply navigate into the folder:

{title="Command Line",lang="text"}
~~~~~~~~
create-react-app hackernews
cd hackernews
~~~~~~~~

Now you can open the application in your editor. The following folder structure, or a variation of it depending on the *create-react-app* version, should be presented to you:

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

A short break down of the folder and files. It is fine if you don't understand all of them in the beginning.

* **README.md:** The .md extension indicates that the file is a markdown file. Markdown is used as a lightweight markup language with plain text formatting syntax. Many source code projects come with a *README.md* file to give you initial instructions about the project. When pushing your project to a platform such as GitHub eventually, the *README.md* file will show its content prominently when you access the repository. Because you have used *create-react-app*, your *README.md* should be the same as shown in the official [create-react-app GitHub repository](https://github.com/facebookincubator/create-react-app).

* **node_modules/:** The folder has all the node packages that were and are installed via npm. Since you have used *create-react-app*, there should be already a couple of node modules installed for you. Usually you will never touch this folder, but only install and uninstall node packages with npm from the command line.

* **package.json:** The file shows you a list of node package dependencies and other project configuration.

* **.gitignore:** The file indicates all files and folders that shouldn't be added to your remote git repository when using git. They should only live in your local project. The *node_modules/* folder is such a use case. It is sufficient to share the *package.json* file with your peers to enable them to install all dependencies on their own without sharing the whole dependency folder.

* **public/:** The folder holds all your files when building your project for production. Eventually all your written code in the *src/* folder will be bundled into a couple of files when building your project and placed in the public folder.

After all, you don't need to touch the mentioned files and folders. In the beginning everything you need is located in the *src/* folder. The main focus lies on the *src/App.js* file to implement React components. It will be used to implement your application, but later you might want to split up your components into multiple files whereas each file maintains one or a few components on its own.

Additionally, you will find a *src/App.test.js* file for your tests and a *src/index.js* as entry point to the React world. You will get to know both files in a later chapter. In addition, there is a *src/index.css* and a *src/App.css* file to style your general application and your components. They all come with default style when you open them.

The *create-react-app* application is a npm project. You can use npm to install and uninstall node packages to your project. Additionally it comes with the following npm scripts for your command line:

{title="Command Line",lang="text"}
~~~~~~~~
// Runs the application in http://localhost:3000
npm start

// Runs the tests
npm test

// Builds the application for production
npm run build
~~~~~~~~

The scripts are defined in your *package.json*. Your boilerplate React application is bootstrapped now. The exciting part comes in the exercises to finally run your bootstrapped application in the browser.

### Exercises:

* `npm start` your application and visit the application in your browser
* run the interactive `npm test` script
* check the content of your *public/* folder, run the `npm run build` script and verify that files were added to the folder (you can remove these files again, but they don't do any harm)
* make yourself familiar with the folder structure
* make yourself familiar with the content of the files
* read more about [the npm scripts and create-react-app](https://github.com/facebookincubator/create-react-app)

## Introduction to JSX

Now you will get to know JSX. It is the syntax in React. As mentioned before, *create-react-app* has already bootstrapped a boilerplate application for you. All files come with default implementations. Let's dive into the source code. The only file you will touch in the beginning will be the *src/App.js* file.

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

Don't let yourself get confused by the import/export statements and class declaration. These features are already JavaScript ES6. We will revisit those in a later chapter.

In the file you have an **React ES6 class component** with the name App. It is a component declaration. Basically after you have declared a component, you can use it as element everywhere in your application. It will produce an **instance** of your **component** or in other words: the component gets instantiated.

The **element** it returns is specified in the `render()` method. Elements are what components are made of. It is useful to understand the differences between component, instance and element.

Pretty soon, you will see where the App component is instantiated. Otherwise you wouldn't see the rendered output in the browser, would you? The App component is only the declaration, but not the usage. You would instantiate the component somewhere in your JSX with `<App />`.

The content in the render block looks pretty similar to HTML, but it's JSX. JSX allows you to mix HTML and JavaScript. It's powerful yet confusing when you are used to separate your HTML and JavaScript. That's why a good starting point is to use basic HTML in your JSX. In the beginning, remove all the distracting content in the file.

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

Now, you only return HTML in your `render()` method without JavaScript. Let's define the "Welcome to the Road to learn React" as a variable. A variable can be used in your JSX by using curly braces.

{title="src/App.js",lang=javascript}
~~~~~~~~
import React, { Component } from 'react';
import './App.css';

class App extends Component {
  render() {
# leanpub-start-insert
    var helloWorld = 'Welcome to the Road to learn React';
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

It should work when you start your application on the command line with `npm start` again.

Additionally you might have noticed the `className` attribute. It reflects the standard `class` attribute in HTML. Because of technical reasons, JSX had to replace a handful of internal HTML attributes. You can find all of the [supported HTML attributes in the React documentation](https://facebook.github.io/react/docs/dom-elements.html). They all follow the camelCase convention. On your way to learn React, you will come across some more JSX specific attributes.

### Exercises:

* define more variables and render them in your JSX
  * use a complex object to represent an user with a first name and last name
  * render the user properties in your JSX
* read more about [JSX](https://facebook.github.io/react/docs/introducing-jsx.html)
* read more about [React components, elements and instances](https://facebook.github.io/react/blog/2015/12/18/react-components-elements-and-instances.html)

## ES6 const and let

I guess you noticed that we declared the variable `helloWorld` with a `var` statement. JavaScript ES6 comes with two more options to declare your variables: `const` and `let`. In JavaScript ES6, you will rarely find `var` anymore.

A variable declared with `const` cannot be re-assigned or re-declared. It cannot get mutated (changed, modified). You embrace immutable data structures by using it. Once the data structure is defined, you cannot change it.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// not allowed
const helloWorld = 'Welcome to the Road to learn React';
helloWorld = 'Bye Bye React';
~~~~~~~~

A variable declared with `let` can get mutated.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// allowed
let helloWorld = 'Welcome to the Road to learn React';
helloWorld = 'Bye Bye React';
~~~~~~~~

You would use it when you would need to re-assign a variable.

However, you have to be careful with `const`. A variable declared with `const` cannot get modified. But when the variable is an array or object, the value it holds can get updated. The value it holds is not immutable.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// allowed
const helloWorld = {
  text: 'Welcome to the Road to learn React'
};
helloWorld.text = 'Bye Bye React';
~~~~~~~~

But when to use each declaration? There are different opinions about the usage. I suggest using `const` whenever you can. It indicates that you want to keep your data structure immutable even though values in objects and arrays can get modified. If you want to modify your variable, you can use `let`.

Immutability is embraced in React and its ecosystem. That's why `const` should be your default choice when you define a variable. Still, in complex objects the values within can get modified. Be careful about this behavior.

In your application, you should use `const` over `var`.

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

### Exercises:

* read more about [ES6 const](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const)
* read more about [ES6 let](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let)
* research more about immutable data structures
  * why do they make sense in programming in general
  * why are they used in React and its ecosystem

## ReactDOM

Before you continue with the App component, you might want to see where it is used. It is located in your entry point to the React world: the *src/index.js* file.

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

Basically `ReactDOM.render()` uses a DOM node in your HTML to replace it with your JSX. That's how you can easily integrate React in every foreign application. It is not forbidden to use `ReactDOM.render()` multiple times across your application. You can use it at multiple places to bootstrap simple JSX syntax, a React component, multiple React components or a whole application. But in plain React application you will only use it once to bootstrap your whole component tree.

`ReactDOM.render()` expects two arguments. The first argument is JSX that gets rendered. The second argument specifies the place where the React application hooks into your HTML. It expects an element with an `id='root'`. You can open your *public/index.html* file to find the id attribute.

In the implementation `ReactDOM.render()` already takes your App component. However, it would be fine to pass simpler JSX as long as it is JSX. It doesn't have to be an instantiation of a component.

{title="Code Playground",lang=javascript}
~~~~~~~~
ReactDOM.render(
  <h1>Hello React World</h1>,
  document.getElementById('root')
);
~~~~~~~~

### Exercises:

* open the *public/index.html* to see where the React applications hooks into your HTML
* read more about [rendering elements in React](https://facebook.github.io/react/docs/rendering-elements.html)

## Hot Module Replacement

There is one thing that you can do in the *src/index.js* file to improve your development experience as a developer. But it is optional and shouldn't overwhelm you in the beginning when learning React.

In *create-react-app* it is already an advantage that the browser automatically refreshes the page when you change your source code. Try it by changing the `helloWorld` variable in your *src/App.js* file. The browser should refresh the page. But there is a better way of doing it.

Hot Module Replacement (HMR) is a tool to reload your application in the browser. The browser doesn't perform a page refresh. You can easily activate it in *create-react-app*. In your *src/index.js*, your entry point to React, you have to add one little configuration.

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

That's it. Try again to change the `helloWorld` variable in your *src/App.js* file. The browser shouldn't perform a page refresh, but the application reloads and shows the correct output. HMR comes with multiple advantages:

Imagine you are debugging your code with `console.log()` statements. These statements will stay in your developer console, even though you change your code, because the browser doesn't refresh the page anymore. That can be convenient for debugging purposes.

In a growing application a page refresh delays your productivity. You have to wait until the page loads. A page reload can take several seconds in a large application. HMR takes away this disadvantage.

The biggest benefit is that you can keep the application state with HMR. Imagine you have a dialog in your application with multiple steps and you are at step 3. Basically it is a wizard. Without HMR you would change the source code and your browser refreshes the page. You would have to open the dialog again and would have to navigate from step 1 to step 3. With HMR your dialog stays open at step 3. It keeps the application state even though the source code changes. The application itself reloads, but not the page.

### 실습:

* change your *src/App.js* source code a few times to see HMR in action
* watch the first 10 minutes of [Live React: Hot Reloading with Time Travel](https://www.youtube.com/watch?v=xsSnOQynTHs) by Dan Abramov

## Complex JavaScript in JSX

Let's get back to your App component. So far you rendered some primitive variables in your JSX. Now you will start to render a list of items. The list will be sample data in the beginning, but later you will fetch the data from an external [API](https://www.robinwieruch.de/what-is-an-api-javascript/). That will be far more exciting.

First you have to define the list of items.

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

The sample data will reflect the data we will fetch later on from the API. An item in the list has a title, an url and an author. Additionally it comes with an identifier, points (which indicate how popular an article is) and a count of comments.

Now you can use the built-in JavaScript `map` functionality in your JSX. It enables you to iterate over your list of items to display them. Again you will use curly braces to encapsulate the JavaScript expression in your JSX.

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

Using JavaScript in HTML is pretty powerful in JSX. Usually you might have used `map` to convert one list of items to another list of items. This time you use `map` to convert a list of items to HTML elements.

So far, only the `title` will be displayed for each item. Let's display some more of the item properties.

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

You can see how the map function is simply inlined in your JSX. Each item property is displayed in a `<span>` tag. Moreover the url property of the item is used in the `href` attribute of the anchor tag.

React will do all the work for you and display each item. But you should add one helper for React to embrace its full potential and improve its performance. You have to assign a key attribute to each list element. That way React is able to identify added, changed and removed items when the list changes. The sample list items come with an identifier already.

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

You should make sure that the key attribute is a stable identifier. Don't make the mistake of using index of the item in the array. The array index isn't stable at all. For instance, when the list changes its order, React will have a hard time identifying the items properly.

{title="src/App.js",lang=javascript}
~~~~~~~~
// don't do this
{list.map(function(item, key) {
  return (
    <div key={key}>
      ...
    </div>
  );
})}
~~~~~~~~

You are displaying both list items now. You can start your app, open your browser and see both items of the list displayed.

### Exercises:

* read more about [React lists and keys](https://facebook.github.io/react/docs/lists-and-keys.html)
* recap the [standard built-in array functionalities in JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map)
* use more JavaScript expressions on your own in JSX

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

### Exercises:

* read more about [ES6 arrow functions](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Arrow_functions)

## ES6 Classes

JavaScript ES6 introduced classes. A class is commonly used in object-oriented programming languages. JavaScript was and is very flexible in its programming paradigms. You can do functional programming and object-oriented programming side by side for their particular use cases.

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

Now you know the basics around JavaScript ES6 classes and how they are used in React to extend them to components. You will learn more about the Component methods when the book describes React lifecycle methods.

### Exercises:

* read more about [ES6 classes](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes)

{pagebreak}

You have learned to bootstrap your own React application! Let's recap the last chapters:

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

It makes sense to take a break at this point. Internalize the learnings and apply them on your own. You can experiment with the source code you have written so far.

You can find the source code in the [official repository](https://github.com/rwieruch/hackernews-client/tree/4.1).
