# 상명대학교 공지사항 크롤러 만들기  

## Chapter 2.
<br/>

## 프로젝트 세팅하기
재미없지만 중요한 세팅! 첫 node.js 프로젝트인지라 모르는 게 많았기 때문에..  
비슷한 프로젝트를 원하는 다른 분들에게 도움이 되고자 자세히 적어보려 한다.  

사용할 도구는 다음과 같다.
- npm
- GitHub
- cheerio
- axios (Chapter 3)
- nodemailer (Chapter 4)
- GitHub Actions (Chapter 5)

<br/>

## npm과 GitHub 세팅하기  

1. GitHub 레포지토리를 생성한다.
2. 원하는 디렉토리에 해당 레포지토리를 클론한다.  
   
    ```
    git clone "url"
    ```
3. npm에 관련한 기본 세팅을 한다.  
   
   ```
   npm init -y              //-y 옵션을 입력하면 기본 세팅으로 초기화된다.
   ```
4. 사용 할 라이브러리를 설치한다.  
   ```
   npm install axios cheerio
   ```
   이 단계를 거치면 `node_modules` 디렉토리가 생긴다.
5. index.js 파일을 생성한다.(지금 단계에선 아무 것도 작성하지 않는다.)  
   index.js 파일은 node.js 프로그램이 실행될 때 entry point가 되는 곳이다.  
   package.json의 "main" 속성의 값으로 "index.js"가 잘 들어가 있는지 확인하자.
6. .gitignore 파일을 생성한다.  
   GitHub엔 이미 잘 만들어진 종류별 gitignore 파일을 공유하는 레포지터리가 있다.  
   [이곳](https://github.com/github/gitignore/blob/main/Node.gitignore)에서 Node.gitignore 파일 내용을 복사해 우리의 .gitignore 파일에 붙여넣는다.

<br/>
세팅이 모두 끝났다! 다음은 cheerio의 Getting Started를 따라가며 cheerio와 친해져보자.  

<br/>

---
<br/>

## Cheerio 소개
Cheerio는 Node.js 환경에서 사용되는 간편하고 유연한 HTML 파싱 및 조작 라이브러리다.  
`$`와 같이 jQuery와 유사한 API를 지원하지만, 몇가지 다른 점이 있다.  
- Cheerio은 Node.js 환경에서 작동하지만, jQuery는 브라우저에서 동작한다.
- Cheerio는 서버 측에서 사용하기 적합하며, jQuery는 클라이언트 측에서 사용하기 적합하다.
- Cheerio는 HTML 혹은 XML을 파싱하여 만든 **가상 DOM 구조를 조작**하고, jQuery는 **브라우저의 실제 DOM을 조작한다**

<br/>

## [Getting Started](https://cheerio.js.org/docs/intro)

Cheerio를 처음 써보기 때문에 감을 잡기 위해서 Getting Started를 따라가 보려 한다.  
궁금하지 않다면 넘겨도 좋다!  

### Importing Cheerio
JavaScript에 임포트는 아래 코드로 할 수 있다.
``` js
import * as cheerio from 'cheerio';
```
ECMAScript 모듈 대신 CommonJS 모듈을 사용할 경우 아래의 코드로 임포트 한다.
``` js
const cheerio = require('cheerio');
```  
나는 `import` 문으로 넣어줬다.  
그런데 이때 `node index.js`로 코드를 실행하면 다음과 같은 에러가 뜬다.
```
(node:64929) Warning: To load an ES module, set "type": "module" in the package.json or use the .mjs extension.
```
에러 메시지에 써있듯 ES module을 사용하기 위해선 package.json 파일에 "type"과 "module"을 키와 값으로 넣어줘야 한다.  
나와 같은 환경에서 세팅한다면 "repository" 속성에 "type" 속성이 있을텐데,  
이는 깃 관련 설정이니 무시하고 바깥에 `"type": "module"`을 따로 추가하면 된다!

<br/>

### Using Cheerio
다시 복기하자면 Cheerio는 HTML를 불러와 가상돔을 조작하는 라이브러리다.  
따라서 가장 처음엔 조작할 HTML 문서를 불러오는 과정이 필요하다.  
load() 함수는 HTML 문서를 인자로 받아 DOM 조작을 돕는 함수를 제공하는 Cheerio 객체를 반환한다.

```js
const $ = cheerio.load('<h2 class="title">Hello world</h2>');
```

<br/>

### Selecting Elements
cheerio는 jQuery와 거의 동일한 방법으로 element를 선택할 수 있다.  
jQuery를 모른다고 좌절할 필요는 없다. (사실 나도 모른다)  
잠깐 조사해본 결과 jQuery 방식은 js의 query selector처럼 css 선택자를 사용하여 원하는 element를 선택하고, 제공되는 API를 사용해 원하는 대로 문서를 조작하는 것인듯 하다.  

위에서 로드한 문서에서 h2 태그 안에 있는 'Hello world'를 추출하는 코드를 짜보자.  
``` js
$('h2.title').text();
```

<br/>


### Traversing the DOM
traversing이란 루트 노드에서 시작하여 자식 및 형제 노드를 탐색하는 과정을 말하며,  
웹 페이지의 HTML 요소 구조를 탐색하는 작업을 의미한다고 한다.  
cheerio는 $() 함수를 통해서 탐색의 시작이 되는 루트 노드를 cheerio 객체로 받아볼 수 있다.  
이렇게 받은 요소를 `find()`, `children()`, `contents()` 등 제공되는 API를 통해 탐색할 수 있다.  
``` js
$('h2.title').find('.subtitle').text();
```

<br/>


### Manipulating Elements
특정 노드를 선택했다면 내가 원하는 대로 조작할 수도 있다.
아래 예제는 h2 태그 안의 text를 바꾸고, 자식 요소를 추가하는 예제이다.  
```js
$('h2.title').text('Hello there!');

$('h2').after('<h3>How are you?</h3>');
```

<br/>

---

<br/>

Cheerio의 getting started를 따라가며 cheerio란 **가상 DOM의 요소를 선택, 탐색, 조작**할 수 있는 라이브러리라는 걸 알았다.  
다음 시간엔 axios를 사용해서 크롤링을 위해 우리가 사용할 html 문서를 불러오는 작업을 해보자!  
