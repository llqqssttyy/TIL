# 2023-02-11 TIL : axios 와 async & await

## async & await
> 프로미스를 좀 더 편하게 사용하기 위한 키워드
### `async`
- 함수에 `async` 키워드를 붙이면 해당 함수의 반환값은 자동으로 프로미스 객체로 감싸진다.
### `await`
- `await`는 `async` 함수 안에서만 사용 가능한 키워드로, 프로미스가 처리될 때까지 함수 실행을 기다리게 한다.(동기적으로 처리되는 것 처럼 보이게 한다.)
- `await`는 thenable 객체(`then` 메서드가 있는 호출 가능한 객체)를 처리할 수 있다.

<br>  

***

## axios에서 async/await
> axios는 Promise 기반의 HTTP 라이브러리다. 따라서 `async`와 `await`를 사용할 수 있다.

<br>

### 왜 사용해야 하는가?
- `.then`과 `.catch`를 사용하는 것 보다 코드를 간결하게 짤 수 있다. 
- return이 될 때까지 기다리기 때문에 반환된 결과물을 사용하기에도 좋다. 
- 실행 순서를 예측하기 쉽게 만들어 준다.

<br>

### 예시
아래는 버튼을 클릭했을 때 api 요청을 보내고, 받아 온 데이터를 state에 넣어주는 코드다.<br>
안쪽 `await`는 api 요청의 응답 객체가 반환될 때 까지 기다리도록 하고, <br> 바깥 쪽 `await`는 응답 객체에서 원하는 데이터가 반환될 때 까지 기다리게 한다. <br>
아래와 같이 함으로써 setArticles의 실행이 지연되므로 유효한 getArticles의 값이 들어가도록 보장할 수 있다. <br>

```jsx
const [ articles, setArticles ] = useState({});

const handleClickBtn = async () => {
    const getArticles = await ( await authInstance.get(`${api}/?page=${page + 1}`) ).data.data.content;
    
    setArticles(getArticles);
};
```
<br>

*** 
