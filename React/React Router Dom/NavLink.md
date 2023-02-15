# 2023-02-05 TIL : NavLink activestyle

### `<NavLink>`

> `<Link>` 태그의 한 종류로 `active` 여부를 알 수 있다.
> 

💙 active란 현재 URL과 NavLink의 to 속성의 URL이 같을 때 true가 되는 값이다.

<br>

### 문제 상황

- NavLink의 activestyle이 적용되지 않는 상황이 발생했다.

<br>

### 해결

- 구글링을 오래 해도 비슷한 문제가 없었고, 다른 문제들의 원인이 주로 잘못된 사용법이어서 공식 문서를 확인해 보니 버전 문제였다. 
- 내가 현재 사용하고 있는 버전은 v6로, `activestyle` prop은 이전 버전에서 지원되던 기능이었다.

<br>

## V6에서 달라진 점

v5의 activeStyle, activeClassName이 사라지는 대신, active의 여부를 `isActive`로 제공해준다.
```jsx
// 이전 코드
<NavLink to="/board" activeStyle={{ color: "blue" }}>Board</NavLink>

// v6 코드
<NavLink to="/board" style={( isActive ) => ({ color: isActive ? "blue" : "black"})}>Board</NavLink>
```

<br>

### 느낀 점
구글링으로만 해결하려고 하니 시간 낭비를 많이 했다. 다음 부터는 공식 문서도 살펴 보자!