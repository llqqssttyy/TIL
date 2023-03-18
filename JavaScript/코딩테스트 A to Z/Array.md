# 2023-02-18 TIL : 자바스크립트 배열과 객체

## What?
> js에서 사용 가능한 배열 api들을 정리한다.

<br>

***

### JS에서의 배열
> 리스트 형태의 객체로서 순회와 변형 작업을 수행한다. JS에서 배열은 밀집성을 보장하지 않는다. 

- 객체로 취급되기 때문에 typeof를 통해 type을 뽑으면 object가 나온다.
- 

<br>


### 배열 생성하기
``` javascript
const arr1 = new Array();
const arr2 = [];
const arr3 = [1, 2, 3, 4, 5];
const arr4 = new Array(5); // 5칸짜리 빈 배열 생성

// fill: 배열 전체를 같은 값으로 초기화시키기
const arr5 = new Array(5).fill(5);

// from: 배열, 로직을 받는다. 로직은 배열의 값과 배열의 인덱스를 받는다.
const arr6 = Array.from(Array(5), function(v, k) {
    return k + 1;
})
```

<br>

***

### 유용한 프로퍼티 및 함수
<br>

***1. length*** -> 배열의 길이를 직접 조작할 수는 있지만 권장되지 않음.
``` javascript
const arr = [1, 2, 3, 4, 5, 6];
arr.length = 3;                 // length를 줄이면 값이 삭제됨
arr.length = 10;                // length를 늘리면 빈 값으로 초기화됨
```

***2. join*** : `arr.join([separator])`
> 배열의 모든 요소를 연결해 하나의 문자열로 만든다.
```javascript
const arr = [1, 2, 3, 4];

console.log(arr.join(", ")); // output: 1, 2, 3, 4
```

***3. reverse*** 
> 배열의 순서를 반전하는 함수. 호출 시 원본 배열을 변형하고 그 참조를 반환한다.
``` javascript
const arr = [1, 2, 3, 4];

arr.reverse();
```

***4. concat***
> 두 배열을 합치는 함수.
``` javascript
const arr1 = [1, 2, 3, 4];
const arr2 = [5, 6, 7];

arr1.concat(arr2);
```

***5. push, pop***
> 배열의 끝에 요소를 넣거나 빼는 함수.
```javascript
const arr = [1, 2, 3, 4];

arr.push(7);
arr.push(8, 9);         //여러 개를 push 할 수도 있다.

arr.pop();              // 배열의 맨 끝에 있는 요소를 빼낸다.
console.log(arr.pop()); // output: 8, pop의 반환값은 제거된 요소이다.
```

***6. shift, unshift***
> 맨 앞에 있는 요소를 추가하거나 제거하는 함수.
``` javascript
const arr = [1, 2, 3, 4];

arr.shift();            // [2, 3, 4];
arr.unshift(1);         // [1, 2, 3, 4];
```

***7. slice*** `arr.slice([begin[, end]])`
> 배열의 begin부터 end까지 얕은 복사를 하여 새로운 배열 객체로 반환. 원본 배열 수정은 없음. <br>
- `begin` : 추출 시작점에 대한 인덱스. 음수일 경우 배열의 끝에서부터의 길이를 나타낸다.
- `end` : 추출을 종료 할 기준 인덱스. end 인덱스를 제외하고 추출된다.
``` javascript
const arr = [1, 2, 3, 4];

arr.slice(1, 3); // output: 2, 3
```

***8. splice*** `array.splice(start[, deleteCount[, item1[, item2[, ...]]]])`
> 배열의 기존 요소를 삭제 또는 교체하거나 추가하는 함수.
``` javascript
const arr = [1, 2, 3, 4, 5, 6];

arr.splice(2, 2); // [1, 2, 5, 6]
```
<br>

***

### 배열 순회하기
***for of***
``` javascript
for (const item of arr){
    console.log(item);
}
```
