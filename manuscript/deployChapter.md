# 7. 애플리케이션 배포하기
마지막 장에서는 리액트 애플리케이션을 프로덕션 환경에 배포하는 방법을 알아보겠습니다. 무료 호스팅 서비스인 헤로쿠(Heroku)에 애플리케이션을 배포할 것입니다. 배포 과정에서 *create-react-app*에 대해 자세히 알아봅시다.

## 7.1 Eject

애플리케이션 배포를 위해 'Eject'에 대해 알 필요는 없습니다. *create-react-app*은 확장을 유지하면서 벤더 종속을 방지하는 기능입니다. (벤더 종속(vendor lock-in)이란 어떤 회사의 제품에 대해 소비자가 종속되는 상황에 이르게 되는 현상을 말합니다. 소비자가 다른 회사의 상품으로 옮겨가고 싶어도 갈 수 없도록 만들어 놓은 상황에 이르게 됩니다. “비호환성”이 대표적인 예 입니다.)  벤더 종속은 특정 기술을 구매하여 도입할 때 문제시 됩니다. 특정 기술을 사용하게 되면 탈피할 수 있는 방법은 없습니다. 다행이도 *create-react-app*은 "eject(꺼내기)"로 벤더 종속을 탈피할 수 있습니다.

*package.json* 파일에 *start*, *test*, *build* 스크립트 명령어가 보일 것입니다. 맨 마지막에 *eject*가 있습니다. *eject* 스크립트를 실행해볼 수는 있겠지만, 다시 전 상태로 되돌릴 수 있는 방법은 없습니다. **단방향 조작이기 때문에 절대로 전 상태로 갈 수 없다는 것을 명심하길 바랍니다!** 이제 리액트 입문자일수록, *create-react-app* 제공되는 기본 도구를 충실히 사용해야 합니다. 

`npm run eject` 명령어를 실행하면  *package.json* 에 정의된 모든 설정과 의존성을 복사해 새 폴더인 *config/* 생성하여 이 폴더로 모두 이동시킵니다. 바벨(Babel)과 웹팩(Webpack)을 사용해 만든 커스텀 설정으로 전체 프로젝트를 컨버팅할 수 있습니다. 바벨과 웹팩 등 모든 툴을 능숙하게 다룰 수 있을 때 해보길 바랍니다.

*create-react-app* 는 소규모 프로젝트에 적합합니다. 따라서 지금 단계에서 "eject"를 사용할 필요가 없습니다.

### 읽어보기

* [[리액트 공식 문서] eject](https://github.com/facebookincubator/create-react-app#converting-to-a-custom-setup)

## 7.2 Heroku 배포

우리가 만든 애플리케이션을 헤로쿠(Heroku)에 배포해봅시다. 헤로쿠는 *create-react-app*의 제로 구성 설치 철학을 따르고 있습니다. 몇 분만에 *create-react-app* 앱을 배포할 수 있습니다. 

배포 전 꼭 해야할 두 가지가 있습니다.

* [헤로쿠 CLI](https://devcenter.heroku.com/articles/heroku-command-line)를 설치합니다.
* [헤로쿠 무료 계정](https://www.heroku.com/)을 만듭니다.


홈브로우(Homebrew)로 헤로쿠 CLI를 설치할 수 있습니다.

{title="Command Line",lang="text"}
~~~~~~~~
brew update
brew install heroku-toolbelt
~~~~~~~~

이제 깃과 헤로쿠 CLI로 애플리케이션을 배포합니다.

{title="Command Line",lang="text"}
~~~~~~~~
git init
heroku create -b https://github.com/mars/create-react-app-buildpack.git
git add .
git commit -m "react-create-app on Heroku"
git push heroku master
heroku open
~~~~~~~~

이 것으로 배포를 마치겠습니다. 지금까지 개발한 애플리케이션이 잘 구현되었길 바랍니다. 앞으로 아래 참고자료를 보고 더 깊게 학습해보길 바랍니다.

### 읽어보기

* [[저자 블로그] 깃과 깃허브 사용 기초 사용법](https://www.robinwieruch.de/git-essential-commands/)
* [[저자 블로그] create-react-app를 위한 헤로쿠 빌드팩](https://github.com/mars/create-react-app-buildpack)
* [[헤로쿠 공식 블로그] 제로 구성 설치로 리액트 배포하기](https://blog.heroku.com/deploying-react-with-zero-configuration)
