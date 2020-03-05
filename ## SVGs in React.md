## SVGs in React

SVGs in React


To create a modern React application, we'll likely need to use SVGs. Instead of giving every button element text, for instance, we might want to make it lightweight with an icon. In this section, we'll use a scalable vector graphic (SVG) as an icon in one of our React components.



최신 React 애플리케이션을 만들기 위해, SVG를 사용하는 것이 좋습니다. 모든 버튼 개체 텍스트를 만드는 대신, 예를 들면, 여러분들은 아이콘으로 좀 더 가볍게 만들 수 있습니다. 이번에는 React components 중 하나인 scalable vector graphic (SVG)를 사용해 보겠습니다. 




This section builds on the "CSS in React" we covered earlier, to give the SVG icon a good look and feel right away. It's acceptable to use a different styling approach, or no styling at all, though the SVG might look off without it.



이번 장은 SVG 아이콘을 더 멋지고 정확하게 사용할 수 있도록 하는, 이전에 다뤘던 "CSS in React"를 기반으로 합니다. SVG는 다른 스타일링 혹은 스타일링이 전혀 없어도 괜찮습니다. 




This icon as SVG is taken from [Flaticon's Freepick](https://www.flaticon.com/authors/freepik). Many of the SVGs on this website are free to use, though they require you to mention the author. You can download the icon from [here](https://www.flaticon.com/free-icon/check_109748) as SVG and put it in your project as *src/check.svg*. Downloading the file is the recommended way, however, for the sake of completion, this is the verbose SVG definition:



이 SVG 아이콘은 [Flaticon's Freepick]에 있습니다. 이 사이트의 많은 SVG들은 원작자를 남기면 무료로 사용할 수 있습니다. (https://www.flaticon.com/free-icon/check_109748) 여기서 SVG로 아이콘을 다운로드 할 수 있으며 프로젝트 *src/check.svg*파일에 적용하면 됩니다. 이 파일을 다운로드 받는 것이 좋으나, 더 자세히 알기 위해 SVG 정의를 살펴보면 아래와 같습니다.


{title="Code Playground",lang="html"}
~~~~~~~


http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">
http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px"
   viewBox="0 0 297 297" style="enable-background:new 0 0 297 297;" xml:space="preserve">
  
          c1.9-1.9,4.479-2.97,7.167-2.97c2.689,0,5.268,1.068,7.169,2.969l41.661,41.676L225.023,27.332c1.9-1.901,4.48-2.97,7.168-2.97l0,0
      c2.688,0,5.268,1.068,7.167,2.97l54.675,54.701c3.956,3.957,3.956,10.372,0,14.328L120.803,269.668
      C118.903,271.57,116.325,272.638,113.636,272.638z M24.463,158.958l89.173,89.209l158.9-158.97l-40.346-40.364L120.803,160.264
      c-1.9,1.902-4.478,2.971-7.167,2.971c-2.688,0-5.267-1.068-7.168-2.97l-41.66-41.674L24.463,158.958z"/>
  

~~~~~~~

Because we're using create-react-app again , we can import SVGs (similar to CSS) as React components right away. In *src/App.js*, use the following syntax for importing the SVG:



create-react-app을 다시 쓰기 때문에, React components(CSS와 유사한)로 SVG를 바로 import할 수 있습니다. *src/App.js*에서 SVG를 import하기 위해 아래 syntax를 사용합니다.




{title="src/App.js",lang="javascript"}
~~~~~~~
import React from 'react';
import axios from 'axios';

import './App.css';
# leanpub-start-insert
import { ReactComponent as Check } from './check.svg';
# leanpub-end-insert
~~~~~~~

We are importing an SVG, and this works for many different uses for SVGs (e.g. logo, background). Instead of a button text, pass the SVG component as a `height` and `width` attribute:



SVG를 import하게 되면, (e.g. logo, background)다양한 SVG를 활용할 수 있습니다. 버튼 텍스트 대신, SVG component로 'height'와 'width' 속성을 사용할 수 있습니다. 


{title="src/App.js",lang="javascript"}
~~~~~~~
const Item = ({ item, onRemoveItem }) => (
  


    
      item.url}>{item.title}
    
    {item.author}
    {item.num_comments}
    {item.points}
    
      <button
        type="button"
        onClick={() => onRemoveItem(item)}
        className="button button_small"
      >
# leanpub-start-insert
        
# leanpub-end-insert
      
    
  

);
~~~~~~~

Regardless of the styling approach you are using, you can give your SVG icon in the button a hover effect too. In the basic CSS approach, it would look like the following in the *src/App.css* file:



여러분이 사용하는 스타일링 기법에 상관없이, SVG 아이콘 버튼에 hover 효과도 줄 수 있습니다. 기본 CSS 기법에서 *src/App.css* 파일에서 아래와 같이 표현할 수 있습니다.


{title="src/App.css",lang="css"}
~~~~~~~
.button:hover > svg > g {
  fill: #ffffff;
  stroke: #ffffff;
}
~~~~~~~

The create-react-app project makes using SVGs straightforward, with no extra configuration needed. This is different if you create a React project from scratch with build tools like Webpack, because you have to take care of it yourself. Anyway, SVGs make your application more approachable, so use them whenever it suits you.



create-react-app 프로젝트에서 별도의 설치없이 SVG를 쉽게 사용할 수 있습니다. 하지만 여러분이 Webpack같은 빌드 툴과 함께 scracth로 React 프로젝트를 만든다면, 여러분이 직접 관리해야 하기 때문에 다릅니다. SVG는 여러분의 애플리케이션을 좀 더 사용하기 쉽게 만들어 주기 때문에 활용할 수 있을 때마다 사용하길 권장합니다.


### Exercises:

* Confirm your [source code for the last section](https://codesandbox.io/s/github/the-road-to-learn-react/hacker-stories/tree/hs/CSS-in-React-SVG).
  * Confirm the [changes from the last section](https://github.com/the-road-to-learn-react/hacker-stories/compare/hs/CSS-in-React...hs/CSS-in-React-SVG?expand=1).
* Read more about [SVGs in create-react-app](https://create-react-app.dev/docs/adding-images-fonts-and-files).
* Read more about [SVG background patterns in React](https://www.robinwieruch.de/react-svg-patterns).
* Add another SVG icon in your application.



* 마지막 섹션의 소스 코드를 확인해보세요.

* 마지막 섹션의 변경사항들을 확인해보세요.

* [SVGs in create-react-app]에 대해 더 알아보세요.

* [SVG background patterns in React]에 대해 더 알아보세요.

* 다른 SVG 아이콘을 애플리케이션에 적용해보세요.