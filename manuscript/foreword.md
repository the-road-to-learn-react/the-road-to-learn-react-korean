# 서문

'리액트 배움의 길(The Road to Learn React)'은 기초적인 리액트를 지도하고자 한다. 복잡한 도구없이 아주 평범한 리액트 방식으로 실제 응용 프로그램을 구축할 수 있다. 이 책은 프로젝트 시작부터 서버 배포까지 모든 내용을 다룬다. 각 장 마다 연습문제와 참고 자료가 수록되어 있다. 이 책을 마쳤다면 리액트에서 나만의 응용 프로그램을 만들어 볼 수 있을 것이다. 필자인 로빈 위어크(Robin Wieruch)와 커뮤니티가 이 책의 최신 콘텐츠를 유지 및 관리하고 있다.

초보자들이 광범위한 리액트 생태계로 뛰어들기 전, '리액트 배움의 길'를 통해 기본적인 내용을 충실히 학습을 하길 바란다. 본 책에서는 많은 도구를 사용하거나 외부 상태 관리 도구들 다루지 않지만, 실제 리액트 앱 개발 환경에서 사용하고 있는 패턴과 모범 사례를 설명했다.

이 책은 해커 뉴스 애플리케이션을 개발 과정을 다뤘다. 페이지네이션,  클라이언트 캐싱, 검색과 정렬 등 실제 기능을 다룬다. 자바스크립트 ES5 사용자들은 자바스크립트 ES6으로 버전으로 전환하게 될 것이다. 이 책을 읽는 독자들이 리액트와 자바스크립트 개발에 열정을 발견하고 입문할 수 있도록 도움이 되길 바란다.

{pagebreak}

# 추천사

**[무하마드 카쉬프(Muhammad Kashif)](https://twitter.com/appsdevpk/status/848625244956901376):** "리액트 배움의 길은 입문자부터 전문가에 이르기까지 관심있는 학생, 전문가 모두가 읽을 수 있는 책이다. 다른 곳에서 얻을 수 없는 통찰과 기술을 만날 수 있다. 문제 해결을 위한 참고자료와 예제가 구성 또한 완벽하다. 지난 17년 간 웹과 데스크톱 응용 프로그램 개발 분야에 있었던 나 조차 리액트에 처음 입문할 때 어려움을 겪었다. 이 책은 마법과도 같다."

**[안드레아 배가스(Andre Vargas)](https://twitter.com/andrevar66/status/853789166987038720):** "로빈 위어크의 리액트 배움의 길은 놀라움 그 자체이다! 리액트와 ES6 대부분을 이 책을 통해 배웠다!"

**[니콜라스 헌터-워커(Nicholas Hunt-Walker), 시애틀 코딩 스쿨 강사](https://twitter.com/nhuntwalker/status/845730837823840256):** "이제껏 봤던 코딩 책 중 가장 훌륭한 책이다. 리액트와 & ES의 정수를 느낄 수 있다."

**[어스틴 그린(Austin Green)](https://twitter.com/AustinGreen/status/845321540627521536):** "더할 나위없이 좋다. 리액트, ES6, 고급 프로그래밍 개념까지 모든 것을 갖춘 책."

**[니콜라스 퍼그순(Nicole Ferguson)](https://twitter.com/nicoleffe/status/833488391148822528):** "주말 내내 이 책을 봤다. 너무 재밌어서 양심의 가책을 느낄 정도다."

**[카란(Karan)](https://twitter.com/kvss1992/status/889197346344493056):** "방금 이 책을 끝냈다. 리액트와 자바스크립트 입문자들을 위한 최고의 책이다. 우아하다! :)"

**[Eric Priou](https://twitter.com/erixtekila/status/840875459730657283):** "The Road to learn React by Robin Wieruch is a must read. Clean and concise for React and JavaScript."

**신입 개발자:** "신입 개발자로서 이 책을 읽을 수 있어 매우 좋았다. 따라하기 시웠고 앞으로 새로운 앱을 개발할 수 있다는 자신감을 갖게 됐다. 리액트 공식 튜토리얼보다 훨씬 유익하다. 각 장의 연습문제 실습은 매우 유익했다."

**어느 학생:** "리액트를 배우기 위한 가장 좋은 책이다. 실습 프로젝트를 통해 학습 개념과 주제를 이해할 수 있다. '코드와 배움' 이 두 가지를 정확히 마스터할 수 있다."

**[토마스 라크니(Thomas Lockney)](https://www.goodreads.com/review/show/1880673388):** "이 책은 포괄적인 리액트를 다루지 않다는 방침을 지녔다. 나는 리액트가 어떤 것인지 맛보고 싶었고, 이 책은 나를 만족시켰다. 현재 사용해보지 않은 ES6 기능을 배우기 위해 본문 내 각주를 모두 살펴보며 따라가지 않았다. (일부러 빠트린 것은 아니다) 부지런한 독자라면, 이 책의 내용 그 이상의 것을 얻을 수 있을 것이다."

{pagebreak}

# 어린이를 위한 교육

이 책은 모든 사람들이 활용할 수 있는 오픈 소스이다. 모든 사람들이 리액트를 배울 수 있어야한다. 그러나 지구상 모든 사람들이 영어교육을 받지 않았기 때문에 영어로 된 오픈 소스 자료를 열람할 수 있는 특권을 가지고 있지 않다. 이 책의 정가는 없다. 독자들은 각자 원하는 가격으로 구매할 수 있다. 도서 수익금은 개발도상국 어린이를 위한 영어 교육 프로젝트 후원을 위해 사용될 예정이다.

* 11. April to 18. April, 2017, [리액트를 공부하며 기부하기](https://www.robinwieruch.de/giving-back-by-learning-react/)

{pagebreak}

# 자주 찾는 질문

**How do I get updates?** You can [subscribe](https://www.getrevue.co/profile/rwieruch) to the Newsletter or follow me on [Twitter](https://twitter.com/rwieruch) for updates. Once you have a copy of the book, it will stay updated when a new edition gets released. But you have to grab the copy again when an update is announced.

**Does it use the recent React version?** The book always receives an update when the React version got updated. Usually books are outdated pretty soon after their release. Since this book is self-published, I can update it whenever I want.

**Does it cover Redux?** It doesn't. Therefore I have written a second book. The Road to learn React should give you a solid foundation before you dive into advanced topics. The implementation of the sample application in the book will show that you don't need Redux to build an application in React. After you have read the book, you should be able to implement a solid application without Redux. Then you can read my second book to learn [Redux](https://roadtoreact.com/course-details?courseId=TAMING_THE_STATE).

**Does it use JavaScript ES6?** Yes. But don't worry. You will be fine if you are familiar with JavaScript ES5. All JavaScript ES6 features, that I describe on the journey to learn React, will transition from ES5 to ES6 in the book. Every feature along the way will be explained. The book does not only teach React, but also all useful JavaScript ES6 features for React.

**Will you add more chapters in the future?** You can have a look at the Change Log chapter for major updates that already happened. There will be unannounced improvements in between too. In general, it depends on the community whether I continue to work on the book. If there is an acceptance for the book, I will deliver more chapters and improve the old material. I will keep the content up to date with recent best practices, concepts and patterns.

**What are the reading formats?** In addition to the .pdf, .epub, and .mobi formats, you can read it in pure markdown on [GitHub](https://github.com/rwieruch/the-road-to-learn-react). In general, I recommend reading it on a suitable format, otherwise the code snippets will have ugly line breaks.

**How can I get help while reading the book?** The book has a [Slack Group](https://slack-the-road-to-learn-react.wieruch.com/) for people who are reading the book. You can join the channel to get help or to help others. After all, helping others can improve your learnings too.

**Is there any troubleshoot area?** If you run into problems, please join the Slack Group. In addition, you could have a look into the [open issues on GitHub](https://github.com/rwieruch/the-road-to-learn-react/issues) for the book. Perhaps your problem was already mentioned and you can find the solution for it. If your problem wasn't mentioned, don't hesitate to open a new issue where you can explain your problem, maybe provide a screenshot, and some more details (e.g. book page, node version). After all, I try to ship all fixes in next editions of the book.

**Can I help to improve it?** Yes. You can have a direct impact with your thoughts and [contributions on GitHub](https://github.com/rwieruch/the-road-to-learn-react). I don't claim to be an expert nor to write in native English. I would appreciate your help very much.

**Why is the book pay what you want?** I have put a lot of effort into this and will do so in the future. My desire is to reach as many people as possible. Everyone should be enabled to learn React. Still you could pay, if you can afford it. In addition, the [book attempts to support projects that educate children in the developing world](https://www.robinwieruch.de/giving-back-by-learning-react/). You can have an impact too.

**프로젝트 후원 방법은?** Yes. Feel free to reach out. I invest a lot of my time into open source tutorials and learning resources. You can have a look at my [about me](https://www.robinwieruch.de/about/) page. I would love to have you as my [Patron on Patreon](https://www.patreon.com/rwieruch).

**Is there a call to action?** Yes. I want you to take a moment to think about a person who would be a good match to learn React. The person could have shown the interest already, could be in the middle of learning React or might not yet be aware about wanting to learn React. Reach out to that person and share the book. It would mean a lot to me. The book is intended to be given to others.

{pagebreak}

# Change Log

**10. January 2017:**

* [v2 Pull Request](https://github.com/rwieruch/the-road-to-learn-react/pull/18)
* even more beginner friendly
* 37% more content
* 30% improved content
* 13 improved and new chapters
* 140 pages of learning material
* [+ interactive course of the book on educative.io](https://www.educative.io/collection/5740745361195008/5676830073815040)

**08. March 2017:**

* [v3 Pull Request](https://github.com/rwieruch/the-road-to-learn-react/pull/34)
* 20% more content
* 25% improved content
* 9 new chapters
* 170 pages of learning material

**15. April 2017:**

* upgrade to React 15.5

**5. July 2017:**

* upgrade to node 8.1.3
* upgrade to npm 5.0.4
* upgrade to create-react-app 1.3.3

**17. October 2017:**

* upgrade to node 8.3.0
* upgrade to npm 5.5.1
* upgrade to create-react-app 1.4.1
* upgrade to React 16
* [v4 Pull Request](https://github.com/rwieruch/the-road-to-learn-react/pull/72)
* 15% more content
* 15% improved content
* 3 new chapters (Bindings, Event Handlers, Error Handling)
* 190+ pages of learning material
* [+9 Source Code Projects](https://roadtoreact.com/course-details?courseId=THE_ROAD_TO_LEARN_REACT)

{pagebreak}

# How to read it?

The book is my attempt to teach React while you will write an application. It is a practical guide to learn React and not a reference work about React. You will write a Hacker News application that interacts with a real world API. Among several interesting topics, it covers state management in React, caching and interactions (sorting and searching). On the way you will learn best practices and patterns in React.

In addition, the book gives you a transition from JavaScript ES5 to JavaScript ES6. React embraces a lot of JavaScript ES6 features and I want to show you how you can use them.

In general each chapter of the book will build up on the previous chapter. Each chapter will teach you something new. Don't rush through the book. You should internalize each step. You could apply your own implementations and read more about the topic. After each chapter I give you some reading material and exercises. If you really want to learn React, I highly recommend to read the extra material and do some hands on exercises. After you have read a chapter, make yourself comfortable with the learnings before you continue.

In the end you will have a complete React application in production. I am very keen to see your results, so please text me when you have finished the book. The final chapter of the book will give you a handful of options to continue your React journey. In general you will find a lot of React related topics on [my personal website](https://www.robinwieruch.de/).

Since you are reading the book, I guess you are new to React. That's perfect. In the end I hope to get your feedback to improve the material to enable everyone to learn React. You can have a direct impact on [GitHub](https://github.com/rwieruch/the-road-to-learn-react) or text me on [Twitter](https://twitter.com/rwieruch).

# What you can expect (so far...)

* [Hacker News App in React](https://intense-refuge-78753.herokuapp.com/)
* no complicated configurations
* create-react-app to bootstrap your application
* efficient lightweight code
* only React setState as state management (so far...)
* transition from JavaScript ES5 to ES6 along the way
* the React API with setState and lifecycle methods
* interaction with a real world API (Hacker News)
* advanced user interactions
  * client-sided sorting
  * client-sided filtering
  * server-sided searching
* implementation of client-side caching
* higher order functions and higher order components
* snapshot test components with Jest
* unit test components with Enzyme
* neat libraries along the way
* exercises and more readings along the way
* internalize and reinforce your learnings
* deploy your application to production

{pagebreak}
