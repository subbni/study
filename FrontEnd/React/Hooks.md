# useState
- 함수형 컴포넌트에서 상태를 관리할 때 사용
- 배열을 반환하며 첫 번째 원소는 상태 값, 두 번째 원소는 상태를 설정하는 함수
- 하나의 useState 함수는 하나의 상태 값만 관리할 수 있으며, 관리해야 할 상태가 여러 개라면 useState를 여러 번 사용해야 한다.
---
# useEffect
- 컴포넌트가 렌더링 될 때마다 호출되는 함수
- 특정 값이 변경될 때만 호출하고 싶다면 두 번째 파라미터로 배열 안에 해당 값을 넣어주면 된다.
  ```javascript
  useEffect(() => {
    console.log(name);
  }, [name]); // name의 값이 변경될 때마다 호출

    useEffect(() => {
    console.log("mounted");
  }, []); // 빈 배열을 넣으면 컴포넌트가 맨 처음 랜더링될 때(마운트될 때)만 실행
  ```
## cleanup 함수
- 컴포넌트가 언마운트 되기 전 / 업데이트 되기 직전에 작업을 수행하고 싶을 때, useEffect 함수 내에서 뒷정리(cleanup) 함수를 반환해주면 된다.
---
# useReducer
- 여러 상황에 따라 상태를 업데이트해 주고 싶을 때 사용
- 첫 번째 파라미터에는 리듀서 함수를, 두 번째 파라미터에는 해당 리듀서의 기본값(initialState)를 넣어준다
> 리듀서 : `현재 상태`, 상황을 의미하는 `액션`을 전달 받아서 새로운 상태를 반환하는 함수
- 배열을 반환하며 첫 번째 원소는 state 값, 두 번째 원소는 dispatch 함수이다.
> `state` : 현재 가리키고 있는 상태
> `dispatch 함수` : 액션을 발생시키는 함수로, dispatch(action)과 같은 형태로 액션 값을 넣어주면 리듀서 함수가 호출된다.
- useReducer에서의 액션은 어떤 값도 사용 가능하다.
    ```javascript
  function reducer(state, action) {
    switch (action.type) {
      case 'INCREMENT' :
        return { value : state.value + 1 };
      case 'DECREMENT' :
        return { value : state.value - 1 );
      default :
        return state;
    }
  }
  ```
  ```javascript
  function reducer(state, action) { // 이벤트 객체의 e.target값 자체를 액션 값으로 사용한 경우
    return {
      ...state,
      [action.name] : action.value
    }
  }
  ```
---
# useMemo / useCallback
렌더링 성능 최적화를 위해 사용하는 함수들
## useMemo
- 특정 값의 계산 결과를 메모이제이션하여, 디펜던시가 변경되지 않는 한 동일한 값을 반환
```javascript
const memoizedValue = useMemo(() => {
    return computeExpensiveValue(a, b);
}, [a, b]); // a와 b의 값이 바뀌지 않는 한, memoziedValue 값을 위한 computeExpensiveValue 메서드를 호출하지 않는다.
```
## useCallback
- 특정 함수를 메모이제이션하여, 디펜던시가 변경되지 않는 한 동일한 함수를 반환
- useCallback을 사용하지 않는다면 컴포넌트가 업데이트 될 때마다 새로운 함수를 생성하게 되는데, 이를 방지하고 이미 생성된 함수를 재사용하고 싶을 때 사용
```javascript
const onChange = useCallback( e => {
  setNumber(e.target.name);
}, []); // 컴포넌트가 처음 렌더링될 때만 onChange 함수 생성

const onInsert = useCallback( () => {
  const nextList = list.concat(parseInt(number));
  setList(nextList);
  setNumber('');
}, [number, list]); // number 혹은 list의 값이 바뀌었을 때만 onInsert 함수 생성
// * 함수 내부에서 상태 값에 의존해야 할 경우 반드시 두 번째 파라미터 내에 포함시켜 주어야 함.
```
### 사용 시 유의
- memoization 자체에도 비용이 발생하기 때문에 남용해서는 안 되며, 메모이제이션 비용과 성능 최적화의 이점을 잘 고려하여 사용할 것
---
# useRef
- 함수형 컴포넌트에서 ref를 쉽게 사용할 수 있는 함수
- useRef를 사용하여 객체를 생성한 후, 원하는 엘리먼트에 `ref` 속성으로 할당
- useRef를 통해 만든 객체 안의 current로 엘리멘트에 접근 가능
```javascript
const inputEl = useRef(null);

... 
// 이후 input 엘리먼트에 할당
<input value={number} onChange={onChange} ref={inputEl} />

...

// 다음과 같이 사용 가능함
inputEl.current.focus();
```
## useRef와 로컬 변수
- 렌더링과 관련되지 않은 값을 관리할 때 useRef를 사용할 수 있다.
```javascript
const id = useRef(1);
const setId = (n) => {
  id.current = n;
}
const printId = () => {
  console.log(id.current);
}
```
- id의 값이 바뀌어도 컴포넌트는 렌더링되지 않는다.
## 단순 로컬 변수와의 차이점
- 단순 로컬 변수의 경우에도 변수의 값이 바뀌어도 선언된 컴포넌트가 렌더링을 수행하지 않는다.
- 그렇지만 **useRef로 선언된 변수의 경우 렌더링 간에도 값을 유지**하며, 단순 로컬 변수의 경우 렌더링 시마다 초기화된다는 차이점을 가진다.
- ---
