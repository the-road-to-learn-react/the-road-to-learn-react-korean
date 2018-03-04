# [튜토리얼] 리액트 도움닫기 
[리액트 도움닫기(원제: The Road to learn React)](https://www.robinwieruch.de/the-road-to-learn-react/) 한국어 공식 깃허브 리퍼지토리입니다. 

* [해커 뉴스(HackerNews) 프로젝트 실습 코드](https://github.com/the-road-to-learn-react/hackernews-client)

* [리액트 온라인 강좌 "더 로두 투 리액트(The Rorad To React)"](https://roadtoreact.com/)

## 목차
0. [시작하는 글](manuscript/foreword.md)
1. [리액트 기초 다지기](manuscript/chapter1.md#1-%EB%A6%AC%EC%95%A1%ED%8A%B8-%EA%B8%B0%EC%B4%88-%EB%8B%A4%EC%A7%80%EA%B8%B0) 
	1. [리액트를 배워야 하는 이유](manuscript/chapter1.md#11-%EB%A6%AC%EC%95%A1%ED%8A%B8%EB%A5%BC-%EB%B0%B0%EC%9B%8C%EC%95%BC-%ED%95%98%EB%8A%94-%EC%9D%B4%EC%9C%A0)
	2. [준비 사항](manuscript/chapter1.md#12-%EC%A4%80%EB%B9%84-%EC%82%AC%ED%95%AD)
		1. [코드에디터 · 터미널](manuscript/chapter1.md#121-%EC%BD%94%EB%93%9C%EC%97%90%EB%94%94%ED%84%B0--%ED%84%B0%EB%AF%B8%EB%84%90)
		2. [node · npm](manuscript/chapter1.md#122-node--npm)
	3. [node · npm](manuscript/chapter1.md#13-node--npm)
	4. [리액트 설치](manuscript/chapter1.md#14-%EB%A6%AC%EC%95%A1%ED%8A%B8-%EC%84%A4%EC%B9%98)
		1. [CDN](manuscript/chapter1.md#141-cdn)
		2. [npm](manuscript/chapter1.md#142-npm)
	5. [create-react-app](manuscript/chapter1.md#15-create-react-app)
	6. [JSX 기초](manuscript/chapter1.md#16-jsx-%EA%B8%B0%EC%B4%88)
	7. [ES6 const · let](manuscript/chapter1.md#17-es6-const--let)
	8. [ReactDOM](manuscript/chapter1.md#18-reactdom)
	9. [Hot Module Replacement](manuscript/chapter1.md#19-hot-module-replacement)
	10. [JSX 내 자바스크립트 객체 처리](manuscript/chapter1.mdchapter1.md#110-jsx-%EB%82%B4-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EA%B0%9D%EC%B2%B4-%EC%B2%98%EB%A6%AC)
	11. [ES6 화살표 함수](manuscript/chapter1.md#111-es6-%ED%99%94%EC%82%B4%ED%91%9C-%ED%95%A8%EC%88%98)
	12. [ES6 클래스](manuscript/chapter1.md#chapter1.md#112-es6-%ED%81%B4%EB%9E%98%EC%8A%A4)
	
2. [리액트 기초 향상하기](manuscript/chapter2.md)
	1. [컴포넌트 내부 상태 관리](manuscript/chapter2.md#21-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8-%EB%82%B4%EB%B6%80-%EC%83%81%ED%83%9C-%EA%B4%80%EB%A6%AC)
	2. [ES6 객체 초기자](manuscript/chapter2.md#22-es6-%EA%B0%9D%EC%B2%B4-%EC%B4%88%EA%B8%B0%EC%9E%90) 
	3. [단방향 데이터 흐름](manuscript/chapter2.md#23-%EB%8B%A8%EB%B0%A9%ED%96%A5-%EB%8D%B0%EC%9D%B4%ED%84%B0-%ED%9D%90%EB%A6%84)
	4. [클래스 메서드 바인딩](manuscript/chapter2.md#24-%ED%81%B4%EB%9E%98%EC%8A%A4-%EB%A9%94%EC%84%9C%EB%93%9C-%EB%B0%94%EC%9D%B8%EB%94%A9)
	5. [이벤트 핸들러](manuscript/chapter2.md#25-%EC%9D%B4%EB%B2%A4%ED%8A%B8-%ED%95%B8%EB%93%A4%EB%9F%AC)
	6. [폼과 이벤트](manuscript/chapter2.md#26-%ED%8F%BC%EA%B3%BC-%EC%9D%B4%EB%B2%A4%ED%8A%B8)
	7. [ES6 구조해체](manuscript/chapter2.md#27-es6-%EA%B5%AC%EC%A1%B0%ED%95%B4%EC%B2%B4)
	8. [제어되는 컴포넌트](manuscript/chapter2.md#28-%EC%A0%9C%EC%96%B4%EB%90%98%EB%8A%94-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8)
	9. [컴포넌트 분리](manuscript/chapter2.md#29-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8-%EB%B6%84%EB%A6%AC)
	10. [구성가능한 컴포넌트](manuscript/chapter2.md#210-%EA%B5%AC%EC%84%B1%EA%B0%80%EB%8A%A5%ED%95%9C-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8)
	11. [재사용 가능한 컴포넌트](manuscript/chapter2.md#211-%EC%9E%AC%EC%82%AC%EC%9A%A9-%EA%B0%80%EB%8A%A5%ED%95%9C-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8)
	12. [컴포넌트 선언](manuscript/chapter2.md#212-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8-%EC%84%A0%EC%96%B8)
	13. [컴포넌트 스타일링](manuscript/chapter2.md#213-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8-%EC%8A%A4%ED%83%80%EC%9D%BC%EB%A7%81)
	
3. [API 사용하기](manuscript/chapter3.md)
	1. [생명주기 메서드](manuscript/chapter3.md#31-%EC%83%9D%EB%AA%85%EC%A3%BC%EA%B8%B0-%EB%A9%94%EC%84%9C%EB%93%9C)
	2. [검색 결과 데이터 가져오기](manuscript/chapter3.md#32-%EA%B2%80%EC%83%89-%EA%B2%B0%EA%B3%BC-%EB%8D%B0%EC%9D%B4%ED%84%B0-%EA%B0%80%EC%A0%B8%EC%98%A4%EA%B8%B0)
	3. [ES6 전개 연산자](manuscript/chapter3.md#33-es6-%EC%A0%84%EA%B0%9C-%EC%97%B0%EC%82%B0%EC%9E%90)
	4. [조건부 렌더링](manuscript/chapter3.md#34-%EC%A1%B0%EA%B1%B4%EB%B6%80-%EB%A0%8C%EB%8D%94%EB%A7%81)
	5. [Search 컴포넌트 클라이언트 · 서버 처리](manuscript/chapter3.md#35-search-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8-%ED%81%B4%EB%9D%BC%EC%9D%B4%EC%96%B8%ED%8A%B8--%EC%84%9C%EB%B2%84-%EC%B2%98%EB%A6%AC)
	6. [페이지 매김 데이터 가져오기](manuscript/chapter3.md#36-%ED%8E%98%EC%9D%B4%EC%A7%80-%EB%A7%A4%EA%B9%80-%EB%8D%B0%EC%9D%B4%ED%84%B0-%EA%B0%80%EC%A0%B8%EC%98%A4%EA%B8%B0)
	7. [클라이언트 캐시](manuscript/chapter3.md#37-%ED%81%B4%EB%9D%BC%EC%9D%B4%EC%96%B8%ED%8A%B8-%EC%BA%90%EC%8B%9C)
	8. [오류 처리](manuscript/chapter3.md#38-%EC%98%A4%EB%A5%98-%EC%B2%98%EB%A6%AC)
	9. [Axios 라이브러리 사용](manuscript/chapter3.md#39-axios-%EB%9D%BC%EC%9D%B4%EB%B8%8C%EB%9F%AC%EB%A6%AC-%EC%82%AC%EC%9A%A9)
	
4. [컴포넌트 모듈 구성 · 테스트](manuscript/chapter4.md)
	1. [ES6 Import · Export](manuscript/chapter4.md#41-es6-import--export)
	2. [ES6 모듈 구성](manuscript/chapter4.md#42-es6-%EB%AA%A8%EB%93%88-%EA%B5%AC%EC%84%B1)
	3. [Jest 스냅샷 테스트](manuscript/chapter4.md#43-jest-%EC%8A%A4%EB%83%85%EC%83%B7-%ED%85%8C%EC%8A%A4%ED%8A%B8)
	4. [Enzyme 단위 테스트](manuscript/chapter4.md#44-enzyme-%EB%8B%A8%EC%9C%84-%ED%85%8C%EC%8A%A4%ED%8A%B8)
	5. [PropTypes 컴포넌트 인터페이스](manuscript/chapter4.md#45-proptypes-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8-%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4)

5. [심화: 리액트 컴포넌트](manuscript/chapter5.md)
	1. [Ref · DOM](manuscript/chapter5.md#51-ref--dom)
	2. [Loading 컴포넌트](manuscript/chapter5.md#52-loading-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8)
	3. [고차 컴포넌트](manuscript/chapter5.md#53-%EA%B3%A0%EC%B0%A8-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8)
	4. [심화: 정렬](manuscript/chapter5.md#54-%EC%8B%AC%ED%99%94-%EC%A0%95%EB%A0%AC)

6. [심화: 리액트 상태 관리](manuscript/chapter6.md)
	1. [상태 끌어올리기](manuscript/chapter6.md#61-%EC%83%81%ED%83%9C-%EB%81%8C%EC%96%B4%EC%98%AC%EB%A6%AC%EA%B8%B0)
	2. [심화: `setState()`](manuscript/chapter6.md#62-%EC%8B%AC%ED%99%94-setstate)
	3. [상태 제어](manuscript/chapter6.md#63-%EC%83%81%ED%83%9C-%EC%A0%9C%EC%96%B4)

7. [애플리케이션 배포하기](manuscript/deployChapter.md)
	1. [Eject](manuscript/deployChapter.md#71-eject)
	2. [헤로쿠 배포](manuscript/deployChapter.md#72-%ED%97%A4%EB%A1%9C%EC%BF%A0-%EB%B0%B0%ED%8F%AC)

8. [마치는 글](manuscript/foreword.md)
---

## 지은이 · 옮긴이
### 지은이
로빈 위워크(Robin Wieruch) @rwieruch
* [website](https://www.robinwieruch.de/) · [github](github.com/rwieruch) · [twitter](https://twitter.com/rwieruch)

## 옮긴이
이수진(Sujin Lee) @sujinleeme
* [website](https://www.sujinlee.me/) · [github](github.com/sujinleeme) · [facebook](https://www.facebook.com/sujinlee.me) · [twitter](https://twitter.com/sujinleeme)  · [instagram](https://www.instagram.com/sujinlee.me/)
* email: sujinlee.me@gmail.com
---

## 서평 작성하기
* [아마존](https://www.amazon.com/dp/B077HJFCQX?tag=21moves-20)과 [굿리드](https://www.goodreads.com/book/show/37503118-the-road-to-learn-react)에서 여러분의 서평과 리뷰를 기다리고 있습니다.

## 소식받기
* 도서 수정 관련 소식은 [이메일](https://www.getrevue.co/profile/rwieruch)과 [트위터](https://twitter.com/rwieruch)를 통해 전하고 있습니다.

## 도움받기
* 이 책으로 리액트를 학습하는 도중 도움이 필요하거나, 다른 사람을 도와주고 싶다면 공식 [슬랙(Slack) 채널](https://slack-the-road-to-learn-react.wieruch.com/)에 들어오세요.

## 지지하기
* 이 책의 프로젝트 후원자가 되실 수 있습니다. [후원 방법](https://www.robinwieruch.de/about/)에서 자세한 내용을 확인하세요.

## 기여하기
깃허브 풀 리퀘스트(Pull Request :PR) 또는 이슈(Issues)를 통해 누구나 참여할 수 있습니다.

본문 내 오탈자를 수정하거나, 추가 설명이 필요한 경우 풀 리퀘스트를 보내주세요. 

실습 도중 문제가 생기면 깃허브 이슈로 알려주세요. 이슈를 게시할 때, 문제가 발생한 부분의 오류 로그 메시지, 스크린 샷, 책 페이지, 노드 버전를 함께 명시해주세요. 여러분들의 리뷰와 이슈는 콘텐츠를 개선하는데 큰 도움이 될 것입니다

---
도와주신 여러분들에게 감사드립니다.

로빈(Robin)

