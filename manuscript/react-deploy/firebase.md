## 파이어베이스(Firebase) 배포

리액트 애플리케이션 완성 후 남은 마지막 과정은 배포입니다. 배포 단계는 여러분의 아이디어를 세상에 보여주는 가장 중요한 단계입니다. 이번 장에서는 파이어베이스에 리액트 애플리케이션을 배포해보겠습니다.

파이어베이스는 create-react-app 뿐만 아니라, 앵귤러(Angular), 뷰(Vue) 같은 프레임워크에도 사용할 수 있습니다. 맨 처음, 파이어베이스 CLI를 전역으로 설치합니다.

{title="Command Line",lang="javascript"}
~~~~~~~
npm install -g firebase-tools
~~~~~~~

파이어베이스 CLI를 전역으로 설치해 프로젝트의 의존성을 신경 쓰지 않고 애플리케이션을 배포할 수 있습니다. 전역 노드 패키지는 새로운 버전의 커맨드로 가능한 자주 업데이트 해야 합니다.

{title="Command Line",lang="javascript"}
~~~~~~~
npm install -g firebase-tools
~~~~~~~

만약 아직 파이어베이스 프로젝트가 없다면, [파이어베이스 계정](https://console.firebase.google.com/?pli=1)을 만들고 새로운 프로젝트를 생성합니다. 이후 파이어베이스 CLI와 파이베이스 계정(구글 계정)과 연결합니다.

{title="Command Line",lang="javascript"}
~~~~~~~
firebase login
~~~~~~~

커맨드 라인에 보이는 브라우저 URL을 엽니다. 이후 구글 계정을 선택해 파이어베이스 해당하는 프로젝트를 고르고 구글의 권한 요청을 승인합니다. 다시 커맨드 라인으로 돌아와 로그인이 성공적으로 이루어졌는지 확인합니다.

이제 프로젝트의 폴더로 이동해 아래 커맨드를 실행합니다. 이 커맨드는 파이어베이스 호스팅 특징과 관련된 파이어베이스 프로젝트를 초기화합니다.

{title="Command Line",lang="javascript"}
~~~~~~~
firebase init
~~~~~~~

다음 과정은 호스팅 옵션 선택입니다. 만약 파이어베이스 호스팅 외의 도구에도 관심이 있다면 다른 항목도 추가합니다.

{title="Command Line",lang="javascript"}
~~~~~~~
? Which Firebase CLI features do you want to set up for this folder? Press Space to select features, then Enter to confirm your choices.
 ? Database: Deploy Firebase Realtime Database Rules
 ? Firestore: Deploy rules and create indexes for Firestore
 ? Functions: Configure and deploy Cloud Functions
-> Hosting: Configure and deploy Firebase Hosting sites
 ? Storage: Deploy Cloud Storage security rules
~~~~~~~

로그인 하면 구글이 계정과 관련된 모든 파이어베이스 프로젝트를 불러옵니다. 파이어베이스 플랫폼에서 사용할 프로젝트를 선택합니다.

{title="Command Line",lang="javascript"}
~~~~~~~
First, let's associate this project directory with a Firebase project.
You can create multiple project aliases by running firebase use --add,
but for now we'll just set up a default project.

? Select a default Firebase project for this directory:
-> my-react-project-abc123 (my-react-project)
i  Using project my-react-project-abc123 (my-react-project)
~~~~~~~

아직 몇 개의 환경설정이 남았습니다. 기본값으로 설정된 *public/* 폴더 대신 create-react-app의 *build/*폴더를 사용할 수 있습니다. 웹팩(Webpack) 같은 번들링 툴로 설정 한 경우, 빌드 폴더를 원하는 이름으로 지정할 수 있습니다.

{title="Command Line",lang="javascript"}
~~~~~~~
? What do you want to use as your public directory? build
? Configure as a single-page app (rewrite all urls to /index.html)? Yes
? File public/index.html already exists. Overwrite? No
~~~~~~~

create-react-app 애플리케이션은 `npm run build`를 처음 실행하고 나면 *build/* 폴더가 생성됩니다. *build/* 폴더에는 *public/* 폴더와 *src/* 폴더에서 합쳐진 모든 소스가 있습니다. 이 애플리케이션은 한 페이지로 이루어진 단일 애플리케이션이기 때문에 사용자를 *index.html* 파일로 리디렉트해 리액트 라우터가 클라이언트-사이드를 처리하도록 합니다.

이제 파이어베이스 초기화 과정이 끝났습니다. 지금까지 프로젝트 폴더에 파이어베이스 호스팅을 관리하는 설정 파일을 생성했습니다. [파이어베이스 공식 문서](https://firebase.google.com/docs/hosting/full-config)에서 리디렉트 동작구성, 404 페이지, 헤더 등과 관련해 더 읽을 수 있습니다. 마지막으로, 커맨드 라인으로 파이어베이스를 사용해 리액트 애플리케이션을 배포합니다.

{title="Command Line",lang="javascript"}
~~~~~~~
firebase deploy
~~~~~~~

성공적으로 배포를 하고 나면, 작성한 프로젝트와 결과가 같은지 확인해야 합니다.

{title="Command Line",lang="javascript"}
~~~~~~~
Project Console: https://console.firebase.google.com/project/my-react-project-abc123/overview
Hosting URL: https://my-react-project-abc123.firebaseapp.com
~~~~~~~

프로젝트 콘솔의 주소와 호스팅 URL의 결과를 모두 확인합니다. 첫 번째 링크는 새로운 파이어베이스 호스팅 패널을 볼 수 있는 파이어베이스 프로젝트 대시보드로 이동합니다. 두 번째 링크는 배포한 리액트 애플리케이션으로 이동합니다. 

만약 두 번째 링크에서 빈 페이지가 보인다면, *firebase.json*의 `public` key/value pair가 `build` 폴더 혹은 이름이 변경된 폴더에 올바르게 설정되었는지 확인합니다. 다음은 리액트 애플리케이션을  `npm run build`로 빌드 스크립트를 실행했는지 체크합니다. 마지막으로, [파이어베이스 공식 문서 내 create-react-app 배포 시 문제 해결 부분](https://create-react-app.dev/docs/deployment)을 확인해 봅니다. 그래도 해결이 되지 않는다면 `firebase deploy`로 배포를 다시 시도합니다.

### 읽어보기

- [파이어베이스 호스팅](https://firebase.google.com/docs/hosting/) 더 읽어보기
- [파이어베이스로 배포한 애플리케이션에 커스텀 도메인 연결하기](https://firebase.google.com/docs/hosting/custom-domain)
- 선택 사항: 파이어베이스 대신 클라우드 관리 서버를 선택하고자 한다면 [DigitalOcean](https://www.digitalocean.com/?refcode=fb27c90322f3&utm_campaign=Referral_Invite&utm_medium=Referral_Program&utm_source=CopyPaste)을 추천합니다. [저의 경우 클라우드 서비스를 통해 웹 사이트, 웹 애플리케이션, 백엔드 API를 호스팅하고 있습니다.](https://www.robinwieruch.de/deploy-applications-digital-ocean/)
