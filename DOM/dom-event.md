# 2023-04-01 TIL : `DOM Event`
> DOM 이벤트 종류와 이에 대응되는 react dom event 처리
<br/>

## DOM Event
> DOM에서 발생하는 이벤트. `Event` 인터페이스를 상속하여 만들어진다.
- `Evnet` 인터페이스를 상속하므로 event 자체는 모든 이벤트와 공통된 속성과 메서드를 가진다. (`preventDefault()`와 같은 메서드)
- 웹페이지에서 발생하는 이벤트는 자바스크립트 코드와 연계해 동적으로 핸들링할 수 있다.

<br>

***

## 대표적인 이벤트 목록
### focus event
| DOM event | React handler | 설명 |
|----|----|---|
|focus|onFocus|엘리먼트가 포커스를 받았을 때 실행되는 이벤트|
|blur|onBlur|엘리먼트가 포커스를 잃었을 때 실행되는 이벤트|

<br>

### mouse event
> UIEvent와 Event를 상속하고 있다.

| DOM event | React handler | 설명 |
|----|----|---|
|click|onClick|마우스 클릭 시 실행|
|mouseover|onMouseOver|마우스 오버 시 실행|
|select|


### React DOM Event의 특징
- 리액트 엘리먼트에서 이벤트를 처리하는 방식은 DOM 엘리먼트 이벤트 처리 방식과 유사하다.
- 리액트의 이벤트는 소문자 대신 **캐멀 케이스**를 사용한다.
- JSX를 사용하여 **문자열이 아닌 함수**로 이벤트 핸들러를 전달한다. 단 몇몇 리액트 이벤트 핸들러는 native event(브라우저 이벤트)와 이름이 같지 않을 수 있다.
- 리액트에서는 `false`를 반환해도 기본 동작을 방지할 수 없다. 반드시 `preventDefault`를 명시적으로 호출해야 한다.

***
### 참고 자료
[React DOM Event api](https://react.dev/reference/react-dom/components/common#inputevent-handler)