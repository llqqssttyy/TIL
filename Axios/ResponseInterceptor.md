# 2023-02-05 TIL : Axios Interceptor - response

## Interceptor
> 요청이 try catch문에 의해 처리되기 이전에 요청과 응답을 가로채 처리한다.

<br>

## Why?
jwt를 활용해 로그인을 구현하는데, 자연스럽게 인증 정보를 담고 있는 요청을 반복적으로 해야 할 일이 생겼다. <br>
여러 페이지에서 코드가 중복되는 것을 방지하기 위해 axios instance를 만들어 사용했고,<br>
해당 인스턴스 config에 토큰을 넣어주는 로직을 추가했지만 제대로 되지 않았다. <br>
그러던 중 이전에 공식 문서에서 interceptor에 대한 글을 본 것을 기억해내 적용했다.

<br>

***
## 활용 방법
- 요청 인터셉터를 이용해 인증이 필요한 요청에 access token을 넣는다.
- 응답 인터셉터를 이용해 토큰이 만료됐을 경우 access token을 갱신한다.

access token은 `local storage`에 저장되며, refresh token은 `http only cookie`로 전달된다.

<br>

### `axios.interceptors.request.use`
앱의 로직 상 로그인 버튼을 눌렀을 때 토큰을 받아 저장하고, 메인 페이지로 리다이렉트되어 유저의 정보를 받아 와야 했는데 <br>
이 과정이 비동기적으로 처리되어 메인 페이지에서의 요청을 보내는 시점에 토큰이 제대로 들어가지 않았다. <br>
이를 해결하기 위해 request interceptor를 활용하여 요청 헤더에 토큰이 확실하게 들어가도록 처리했다.
<br>
```
// 요청시 access token을 header에 넣어서 보내주기
authInstance.interceptors.request.use(function (config) {
    const token = localStorage.getItem("token");
    config.headers["Authorization"] = token;
    
    // 변경된 config를 반환한다.    
    return config;
});
```

<br>

### `axios.interceptors.response.use`
로그인은 정상적으로 처리되었지만 만료된 토큰을 갱신하는 로직이 없어 유저 정보를 다시 받아오는 데 실패했다. <br>
따라서 response interceptor를 이용하게 됐다. <br>
백엔드와 소통을 통해 토큰의 갱신 여부를 헤더에 담아 알려주기로 결정했다. <br>
로직은 다음과 같다.
- response의 헤더를 검사해 갱신되었으면 local storage를 업데이트 하고 request instance를 반환한다. <br>
- 토큰이 유효하면 response를 그대로 반환한다.

```
// 요청 후 access token 만료 여부에 따라 토큰 업데이트
authInstance.interceptors.response.use(async (res) => {
    const grantType = res.headers.get("Grant-Type");
    if(grantType === "reissued-grant"){
        localStorage.setItem("token", res.headers.get("Authorization"));
        
        return authInstance.request(res.config);
    }
    return res;
})
```