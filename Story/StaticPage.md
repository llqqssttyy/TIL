# 2023-04-01 TIL : `Static Web Page`
> 정적 웹 페이지 120개 생성기(라 쓰고 시행착오 기록이라 읽는다.)

### 어쩌다가..
사람들이 특정 문장을 보고 그와 비슷한 문장을 창작하기 쉽도록 여러 참고 자료와 창작한 문장을 업로드 도구를 제공하는 사이트를 만드는 중이었다. <br> 아무래도 원문 자체가 어렵다 보니 사용자 입장에서 참고 할만한 문장(한 주제 당 100쌍 정도)를 한 페이지에 보기 좋게 띄워주는 것이 가장 중요했다. <br> 문제는 20일동안 운영할 사이트를 일주일만에 만들어야만 했다는 건데, 다음과 같은 우려가 있었다.
1. 아무래도 나와 백엔드 담당 팀원 모두 배포 경험은 처음이라 우리가 언제 어떤 이슈가 일으킬지 예측할 수 없었다. 즉 서툴렀기 때문에 의도치 않게 서버를 다운시킬 수 있었다.(!!)
2. 창작한 문장 업로드 자체는 word나 메모장 파일에 백업하며 작성해서 서버 복구 후 한꺼번에 하면 되지만, 원문을 파악하기 위한 참고자료가 없다면 문장 창작 난이도가 확 올라간다.

20일이라는 짧다면 짧은 기한동안 한 사람이 많게는 2100문장을 생성해내야 하기 때문에 하루라도 작업이 원활하게 돌아가지 않는다면 곤란했다.<br>
따라서 서버가 다운돼도 작업이 가능하도록 서버 없이 참고 자료를 웹 페이지 형식으로 보여주는 '정적 페이지'가 필요했다!
<br><br>

***

### 데이터를 뽑자!
가이드라인 제작 일정 때문에 해당 페이지를 의뢰 받고 만드는 데 주어진 시간은 24시간 정도였다. 백엔드 팀원이 여행을 간 상태였기 때문에 db에서 데이터를 받기 어려워 한 가지 묘수를 생각해냈다.<br>
백엔드에서 참고자료 데이터를 태스크 별로 받아오는 api는 있었기 때문에 이 api 요청을 처리하는 `.then()` 에서 request data를 json 파일로 다운받았다!<br>

빠른 작업을 위해 `FileSaver.js` 라이브러리를 다운받아 사용했다.
```jsx
import axios from 'axios';
import { saveAs } from 'file-saver';

axios.get('/api/.../list')
  .then(res => {
    const jsonData = JSON.stringify(res.data);
    const blob = new Blob([jsonData], { type: 'application/json' });
    saveAs(blob, `task${formattedNum(taskNum)}.json`); // task001.json
  })
  .catch(err => {
    //...
  });
```

***

### 데이터를 보여주자! - take #1
내 첫 계획은 이러했다.
> index.html에서 fetch로 json 파일에서 데이터를 task 별로 뽑아온 뒤 
> map으로 테이블을 생성해서 데이터를 보여주자!

그리고 어찌됐든 만들어냈다. 친절하게 task 넘버를 확인할 수 있는 UI도 넣어주고.. 여유롭게 css로 좀 꾸며주고.. 이 정도면 효율적으로 일을 처리했다는 생각에 뿌듯해하는 와중에 생성된 index.html 파일만 클릭해서 들어갔는데 아뿔싸.  ~~fetch도 웹서버가 필요하지..........~~<br><br>
참고자료를 볼 대상이 개발자들이라면 모를까 코딩이 익숙하지 않은 분들에게, 더더욱이 이해해야 할 일이 많은 초반에 나눠줄 가이드라인에 '웹 서버 설치법'을 넣었다가는 저항받을 거란 생각에 얼른 다른 솔루션을 찾아야 겠다는 생각을 했다.<br><br>

### 데이터를.. 보여주자! - take #2
그렇다면 내가 시도 가능한 방법은 뭘까. 아무리 생각해도 결국 task 120개에 대응하는 html 문서를 따로 만들어서 접근하기 쉽도록 구축자 별로 index 파일을 만들어 주는 게 사용자가 원하는 참고자료에 가장 쉽게 접근 가능한 방법이라고 생각했다. <br><br>
결국 답은 복붙인가? 하는 생각이 드는 찰나, json 데이터를 다운받는 데 사용했던 코드를 바닐라 js 버전으로 만들어서 지금 만들어진 index 파일의 input이 제출되면 html 파일을 다운받으면 되겠다! 하는 생각이 스쳐지나갔다.<br><br>
```js
function downloadHtml() {
  const htmlString = document.documentElement.outerHTML;

  const blob = new Blob([htmlString], { type: "text/html" });

  const a = document.createElement("a");
  a.href = URL.createObjectURL(blob);
  a.download = `task${formattedNum(taskNum)}.html`; //task001.html

  a.click();
  a.remove();
}

```
<br>

### html을 수정하자!
딱히 상관 없는 일이었지만, index.html 페이지를 그대로 다운받았기 때문에 `<script>` 태그라던가, `<input>` 태그 같은 사용자가 볼 필요 없거나 괜히 리소스만 낭비시키는 불필요한 태그들이 존재했다.<br>
사실 이쯤 되니까 머리가 아득해져서 120개 밖에 되지 않는다며 긍정파워(~~노가다~~)로 해치웠지만, 작업 후에 더 좋은 방법이 있을 것 같아 찾아본 바를 기록한다.<br><br>
```js
// HTML 문서 내의 script 태그 삭제 함수
function removeScriptTags(html) {
  const parser = new DOMParser(); // HTML을 파싱하여 Document로 반환
  const doc = parser.parseFromString(html, 'text/html');

  // <script> 안 모든 태그를 제거
  const scriptTags = doc.querySelectorAll('script');
  scriptTags.forEach(tag => tag.remove()); 

  // 파싱한 HTML 문서 전체 내용을 반환
  return doc.documentElement.outerHTML;
}

// HTML 파일 읽기
fetch('index.html')
  .then(response => response.text())
  .then(html => removeScriptTags(html))
  .then(cleanedHtml => downloadHtml(cleanedHtml));
```
***

## 느낀 점
blob를 얘기만 들었지 직접 사용해볼 수 있어서 좋았지만 한편으론 내가 모르는 api와 js 지식이 많구나 하는 반성도 들었다! 리액트로 프로젝트 투두를 해치우기만 급급했는데 리프레쉬가 되는 경험이었던 것 같다. <br>
기왕 자극 받은 거 모던 자바스크립트 딥다이브 공부를 다시 시작할까 하는데.. 느리더라도 포기하지는 말자고!
