# 깃허브 페이지(GitHub Pages)에 SPA 배포/호스팅 하기

* 원문 : [Rafael Pedicini](https://github.com/rafrex) -  [Single Page Apps for GitHub Pages](https://github.com/rafrex/spa-github-pages) 

* 번역 : 이수진([@sujinleeme](https://github.com/sujinleeme))

-------

시작하기 전 [데모 사이트][liveExample]를 확인하세요.  
 
 단일 페이지 응용 프로그램(SPA: Single Page Application)을 [깃허브 페이지(GitHub Pages)][ghPagesOverview]에 배포할 수 있는 방법을 소개합니다. [리액트 라우터(React Router)][reactRouter]의 `<BrowserRouter />`를 사용하면 [데모 사이트][liveExample]와 같은 [리액트(react)][react], 그 외 프론트엔드 라이브러리 및 프레임워크를 사용한 애플리케이션을 쉽게 배포할 수 있습니다.

#### 왜 필요한가요?
엄격히 말해 깃허브 페이지는 SPA를 지원하지 않습니다. 예를 들어 URL이 `example.tld/foo`이고 `/foo`가 프론트엔드 경로인 경우, 깃허브 페이지 서버는 `/foo`를 모르기 때문에 404 에러를 반환합니다. 이 문제를 해결하는 방법을 아래 제시합니다. 

#### 어떻게 작동되나요?
깃허브 페이지 서버로 프론트엔드 경로인 `example.tld/foo` URL 요청을 받으면, `404.html` 페이지를 리턴합니다. 이 문제를 해결하기 위해 [`404.html` 파일에 스크립트][404html]를 만듭니다. 이 스크립트는 현재 URL를 받아, URL의 경로(path)와 쿼리(query)에 해당하는 문자열을 모두 쿼리 문자열로 변환한 다음, 쿼리(query)와 해시(hash)로 구성된 새로운 URL을 만들어 리디렉션 합니다. 예를 들어 URL이 `example.tld/one/two?a=b&c=d#qwe`인 경우, `example.tld/?p=/one/two&q=a=b~and~c=d#qwe`로 변경됩니다.

깃허브 페이지 서버는 `example.tld?p=/...`이라는 URL을 요청받으면 `example.tld?p=/...`,에 포함된 쿼리와 해시 문자열을 무시하고 `index.html`을 반환합니다. `index.html` 파일에도 스크립트가 있습니다. 이 [스크립트][indexHtmlScript]는 SPA가 로드되기 전에 쿼리 문자열의 리디렉션을 확인합니다. 리다이렉트가 존재하면 올바른 URL로 다시 변환되어 `window.history.replaceState(...)`로 브라우저 히스토리에 추가되지만, 브라우저는 새로운 URL를 로드하지 않습니다. `index.html` 파일에서 [SPA가 로드되면][indexHtmlSPA], URL이 브라우저 히스토리에 대기하여 그에 따라 SPA 경로를 지정합니다. (리디렉션은 새로운 페이지가 로드될 때만 필요합니다. 이후 SPA 내부를 탐색하는 경우 필요하지 않습니다.)

#### SEO  
물론 404를 반환하는 것은 절대로 좋은 방법은 아닙니다. [서치 엔진 랜드(Search Engine Land) 가 실시한 테스트][seoLand]에 따르면, 구글 크롤러는 `404.html` 파일의 자바스크립트 `window.location`을 처리하는데, 이는 검색 색인 생성을 위한 301 리디렉션과 동일하다고 말했습니다. 301 리디렉션(Permanent Redirect: 영구 전송)은 URL(A)가 URL(B)로 설정될 경우 콘텐츠도 URL도 새로운 B를 대상으로 표시합니다. 추가로 개인 테스트를 통해 구글이 문제없이 모든 페이지를 색인하여 검색에 반영하는 것을 확인했습니다. 다만 주의할 점은 리다이렉트 쿼리는 구글의 URL 색인 방식을 따라야 한다는 것입니다. 예를 들어, `example.tld/about`은`example.tld/?p=/about`로 색인화 됩니다. 사용자가 검색 결과를 클릭하면, url은 `example.tld/about`로 다시 바뀝니다.


## 사용 설명
* 깃허브 페이지 기본 사용법은 [공식 GitHub Pages Basics][ghPagesBasics]에서 확인하세요. 깃허브 페이지는 [개인, 조직, 프로젝트 웹사이트][ghPagesTypes] 등으로 사용할 수 있습니다.

### **기본 사용 설명** 
  이미 만들어진 리퍼지토리에 내용을 추가해 깃허브 페이지를 호스팅 하는 방법입니다.
  
  1. 작업 중인 리퍼지토리에 `404.html`파일을 만들고 [`404.html`][404 html]에 있는 내용을 복사 붙여 넣기 합니다. 
     - [커스텀 도메인][customDomain]을 사용하지 않고 프로젝트 페이지로 사용할 경우 (예를 들어 `username.github.io/repo-name` 형식의 주소를 사용할 경우) [`404.html` 파일에 있는 `segmentCount` 변수를 `1`로 설정하세요.][segmentCount] 리디렉션 된 후 루트 내 `/repo-name`를 유지하게 됩니다. 
  2. `index.html`파일 내 [리디렉션 스크립트][indexHtmlScript] 부분을 복사해 `index.html` 파일에 추가합니다. 
     - 리디렉션 스크립트 위치는 `index.html` 파일의 SPA 스크립트 전에 있어야 합니다.

### **상세 사용 설명**
  다운로드한 이 리퍼지토리를 보일러플레이트(boilerplate)로 사용해 깃허브 페이지에 호스팅하는 방법입니다.
  
  1. 리퍼지토리를 클론합니다. (`$ git clone https://github.com/sujinleeme/spa-github-pages.git`)
  
  2. 숨김 폴더인 `.git` 폴더를 지우세요 (`cd` 명령어로 `spa-github-pages` 폴더 들어가 `$ rm -rf .git` 명령어를 실행하세요.)
  
  3. 리퍼지토리를 인스턴스화 합니다.
      - 보일러플레이트를 새 리퍼지토리를 생성하길 원하는 경우
        - `spa-github-pages` 폴더에서 `$ git init` 명령어를 입력합니다. 그다음 `$ git add .`, `$ git commit -m "Add SPA for GitHub Pages boilerplate"` 명령어로 새 저장소를 초기화합니다.
        - 프로젝트 페이지 사이트일 경우, 브랜치 이름을 `master`에서 `gh-pages`로 변경합니다. (`$ git branch -m gh-pages`). 개인 사용자 또는 기관 페이지 사이트일 경우 브랜치 이름을 `master`로 유지합니다.
        - 깃허브 홈페이지에서 빈 리퍼지토리를 생성합니다. (`readme.md`, `.gitignore`, `license` 파일을 추가하지 마세요) 로컬 리퍼지토리에 원격 리퍼지토리를 추가합니다. (`$ git remote add origin <your-new-github-repo-url>`)
        - 현재 로컬 폴더 이름인 `spa-github-pages`를 원하는 이름으로 바꿉니다. (예 `프로젝트-이름`)
        
      - 보일러플레이트를 기존 리퍼지토리의 `gh-pages` 브랜치로 추가하는 경우
        - 기존 리퍼지토리에서 부모 커밋이 없는 새로운 브랜치인 `gh-pages`를 만듭니다 (`$ git checkout --orphan gh-pages`). 첫 커밋 전까지 `gh-pages`에 브랜치 리스트가 없어야 합니다.
        - 기존 리퍼지토리 폴더에서  `.git` 폴더를 제외한 모든 파일과 폴더를 삭제합니다. (`$ git rm -rf .`)
        - 새로운 `프로젝트-폴더`를 만들고 `spa-github-pages`폴더에 있는 모든 파일(숨김 파일 포함)과 폴더를 복사해 새 폴더로 복사합니다. (`$ mv path/to/spa-github-pages/{.[!.],}* path/to/프로젝트-폴더`)
        - `$ git add .` 명령어를 입력하고 `gh-pages`브랜치를 인스턴스화 하기 위해 `$ git commit -m "Add SPA for GitHub Pages boilerplate"` 명령어를 입력합니다.
  
  4. (옵션) 커스텀 도메인 설정 - [[GitHub Pages] 커스텀 도메인 설정][customDomain] 가이드를 참고하세요.
      - Update the  [`CNAME` 파일][cnameFile]에 `http://`을 제외한 커스텀 도메인을 업데이트하세요. `서브도메인`과  `www`는 추가가능합니다.
      -`CNAME` 파일 또는 `A` 레코드에 DNS 공급자를 업데이트 하세요.
      - `$ dig your-subdomain.your-domain.tld` 명령어를 실행해 DNS가 올바르게 설정되어 있는지 확인하세요. (명령어에 `http://`를 제외합니다.)
        
  5. (옵션) 커스텀 도메인 없이 설정 
      -  [`CNAME` 파일][cnameFile]을 삭제합니다.
      - 사용자 또는 조직 페이지 사이트를 만드는 경우, 이 것으로 마치면 됩니다.
      - 프로젝트 페이지 사이트를 생성을 하고 싶다면 아래와 같이 하세요. (사이트 주소는 `username.github.io/repo-name` 입니다.) 
        - [`404.html` 파일에 있는 `segmentCount` 변수를 `1`로 설정하세요.][segmentCount] 리디렉션된 후 루트 내 `/repo-name`를 유지하기 위함입니다.
        - `index.html` 파일 내 [리디렉션 스크립트][indexHtmlScript] 부분을 복사해 `index.html` 파일에 추가합니다.
            - [bundle.js 파일의 src 경로 부분을][indexHtmlSPA] to `"/repo-name/build/bundle.js"`으로 수정합니다.
        - 리액트 라우터(React Router)를 사용하는 경우, `basename` prop에 `repo-name`을 추가해야 합니다. 예를 들어 `<BrowserRouter basename="/repo-name" />` 이라고 URL의 `basename`을 설정합니다.

  6. `$ npm install`로 리액트와 의존성 패키지를 설치합니다. 이후 `$ npm run build`로 빌드 내용을 업데이트합니다.
  
  7. `$ git add .` 명령어 입력 후, `$ git commit -m "Update boilerplate for use with my domain"` 커밋 메시지를 작성하고, 깃허브에 푸시합니다. (프로젝트 페이지일 경우 `$ git push origin gh-pages` 를 입력, 사용자 또는 기관 페이지일 경우 `$ git push origin master`를 입력) 도메인에 사이트가 게시되어야 합니다.
  
  8. 내 사이트를 만들어 보세요.
      - 리액트 컴포넌트를 작성하고, 라우터를 추가하고, 스타일링을 적용해보세요.
        - 데모 사이트는 인라인 스타일로 작성되었으며 링크와 인터렉티브 컴포넌트는 [React Interactive][reactInteractive]를 사용해 만들었습니다. (`index.html` 파일 내 기본 스타일링을 초기화하는 CSS 스크립트를 제외하고, 별도 CSS 파일은 없습니다.)
      - 사이트 제목을 수정해보세요. [`index.html` 파일 중 `title`][indexHtmlTitle]과 [`404.html` 파일 내 `title`][404htmlTitle]를 수정하세요.
     - `index.html` 파일에서 `header`에 있는 [favicon links][favicon]를 삭제하세요.
     - `readme.md`, `.gitignore`, `license` 파일 내용을 수정하세요.
     - 개발 환경에서 변경 사항을 테스트하세요.
     - 수정 내용을 깃허브 페이지에 반영하기 위해 `$ npm run build` (이 명령어는 [프로덕션][webpackProduction]을 위해 `webpack -p`를 실행합니다.) 으로 빌드 내용을 업데이트 하고, `$ git commit` 커밋 후 `$ git push` 푸시합니다.

#### 개발 환경
개발 환경에서 `$ npm start` 명령어로 로컬 서버를 실행합니다. `webpack-dev-server`이 적용되어 있습니다. `webpack-dev-server`는 소스 파일을 주시하고 있다 변경이 감지되면 변경된 모듈만 새로 번들링 되어 디스크에는 번들 파일이 저장되지 않습니다. 
  - `package.json`파일 내 [`script`][startScript]에서 `$ npm start`를 확인할 수 있습니다. 명령어 입력 시 `$ webpack-dev-server --devtool eval-source-map --history-api-fallback --open`를 실행합니다.
  - `-devtool eval-source-map`은 개발 중 [소스 맵 생성(generating source maps)][webpackDevtool]을 합니다.
  - `--history-api-fallback`은 프론트엔드 라우팅을 허용하고 요청된 파일을 찾을 수 없을 때 `index.html`이 서빙됩니다.
  - `--open` 은 브라우저에 사이트가 자동으로 열립니다.
- `webpack-dev-server`는 `http://localhost:8080`에서  `index.html`를 제공합니다. (기본 포트 번호는 `8080`입니다.) 브라우저가 아닌, 서버에서 먼저 `index.html`를 읽어야 합니다. 반대로 브라우저에서 바로 열 경우 스크립트가 로드되지 않습니다.

#### 그 외
- 리퍼지토리 내 `.nojekyll` 파일은 [깃허브 페이지의 지킬(Jekyll)][nojekyll] 사용을 제거합니다.
- 정적 웹사이트에 폼 제출(form submission) 기능이 필요하다면 [Formspree][폼즈프리(formspree)]을 사용해보세요.
- 깃허브 페이지 CDN의 가장 좋은 점은 모든 파일이 gzip으로 자동 압축된다는 것입니다. 프로덕션 환경을 위해 별도로 HTML, 자바스크립트, CSS 파일을 압축하지 않아도 됩니다.


<!-- links to within repo -->
[404html]: https://github.com/rafrex/spa-github-pages/blob/gh-pages/404.html
[segmentCount]: https://github.com/rafrex/spa-github-pages/blob/gh-pages/404.html#L26
[indexHtmlScript]: https://github.com/rafrex/spa-github-pages/blob/gh-pages/index.html#L58
[indexHtmlSPA]: https://github.com/rafrex/spa-github-pages/blob/gh-pages/index.html#L94
[cnameFile]: https://github.com/rafrex/spa-github-pages/blob/gh-pages/CNAME
[indexHtmlTitle]: https://github.com/rafrex/spa-github-pages/blob/gh-pages/index.html#L6
[404htmlTitle]: https://github.com/rafrex/spa-github-pages/blob/gh-pages/404.html#L5
[favicon]: https://github.com/rafrex/spa-github-pages/blob/gh-pages/index.html#L34
[startScript]: https://github.com/rafrex/spa-github-pages/blob/gh-pages/package.json#L6

<!-- links to github docs -->
[ghPagesOverview]: https://pages.github.com/
[ghPagesBasics]: https://help.github.com/categories/github-pages-basics/
[ghPagesTypes]: https://help.github.com/articles/user-organization-and-project-pages/
[customDomain]: https://help.github.com/articles/quick-start-setting-up-a-custom-domain/
[nojekyll]: https://help.github.com/articles/files-that-start-with-an-underscore-are-missing/

<!-- other links -->
[liveExample]: http://spa-github-pages.rafrex.com
[react]: https://github.com/facebook/react
[reactRouter]: https://github.com/ReactTraining/react-router
[seoLand]: http://searchengineland.com/tested-googlebot-crawls-javascript-heres-learned-220157
[webpackProduction]: https://webpack.js.org/guides/production-build/#the-automatic-way
[webpackDevtool]: https://webpack.js.org/configuration/devtool/
[reactInteractive]: https://github.com/rafrex/react-interactive
[formspree]: http://formspree.io/
