# 2023-02-12 TIL : `useRef`

## useRef란?
> 렌더링에 영향을 주지 않는 값을 참조할 수 있도록 하는 리액트 훅

### `const ref = useRef(initialValue)`
- `useRef`는 initial value를 인자로 받는다.
- `useRef`는 단 하나의 프로퍼티를 갖는 객체를 반환한다. 그것이 `current`이다.