# 리덕스 관련 코드 작성
## 1. 액션 타입 정의
상태 처리를 하고 싶은 이벤트에 이름을 붙여준다.
```js
// modules/counter.js
const INCREASE = 'counter/INCREASE';
const DECREASE = 'counter/DECREASE';
```
- 액션 타입은 대문자로 정의
- `모듈 이름/액션 이름` 과 같은 형태로 작성하여 서로 다른 모듈 간 액션의 이름이 충돌되지 않게 한다.

## 2. 액션 생성 함수 만들기
```js
// modules/counter.js
const INCREASE = 'counter/INCREASE';
const DECREASE = 'counter/DECREASE';

export const increase = () => {{type: INCREASE}};
export const decrease = () => {{type: DECREASE}};
```
type을 키로 액션 값을 갖는 객체를 반환하는 액션 생성 함수를 작성한다.
export 처리하여 다른 파일에서 불러와 사용할 수 있도록 한다.

## 3. 초기 상태 및 리듀서 함수 만들기

리듀서 함수에 현재 상태를 참조하여, 각 액션에 따라  새로운 객체를 생성하여 반환하는 코드를 작성한다.

```jsx
// modules/counter.js
const INCREASE = 'counter/INCREASE';
const DECREASE = 'counter/DECREASE';

export const increase = () => {{type: INCREASE}};
export const decrease = () => {{type: DECREASE}};

const initialState = {
  number: 0
}

// 리듀서 함수
function counter(state = initialState, action) {
  switch(action.type) {
    case INCREASE:
      return {
        number: state.number + 1
      };
    case DECREASE:
      return {
        number: state.number - 1
      };
    default:
      return state;
  }
}

export default conter;
```

## 4. 루트 리듀서 만들기
`createStore`  함수를 사용하여 스토어를 만들고 리듀서를 등록하는데, 이 때 리듀서를 하나만 사용해야 한다. 
그렇기 때문에 만든 리듀서가 하나 이상이라면 이를 하나로 합쳐주어야 한다.
이 작업은 리덕스에서 제공하는 combineReducers 라는 유틸 함수를 사용하면 쉽게 처리할 수 있다.

다음과 같은 리듀서를 하나 더 만들었다고 하자.
```jsx
// modules/todos.js
const CHANGE_INPUT = 'todos/CHANGE_INPUT';
const INSERT = 'todos/INSERT';
const TOGGLE = 'todos/TOGGLE';
const REMOVE = 'todos/REMOVE';

export const changeInput = (input) => ({
  type: CHANGE_INPUT,
  input,
});

let id = 3;
export const insert = (text) => ({
  type: INSERT,
  todo: {
    id: id++,
    text,
    done: false,
  },
});

export const toggle = (id) => ({
  type: TOGGLE,
  id,
});

export const remove = (id) => ({
  type: REMOVE,
  id,
});

const initialState = {
  input: '',
  todos: [
    {
      id: 1,
      text: '리덕스 배우기',
      done: true,
    },
    {
      id: 2,
      text: '리액트와 리덕스 사용하기',
      done: false,
    },
  ],
};

// 리듀서 함수
function todos(state = initialState, action) {
  switch (action.type) {
    case CHANGE_INPUT:
      return {
        ...state,
        input: action.input,
      };
    case INSERT:
      return {
        ...state,
        todos: state.todos.concat(action.todo),
      };
    case TOGGLE:
      return {
        state,
        todos: state.todos.map((todo) =>
          todo.id === action.id ? { ...todo, done: !todo.done } : todo,
        ),
      };
    case REMOVE:
      return {
        state,
        todos: state.todos.filter((todo) => todo.id !== action.id),
      };
    default:
      return state;
  }
}

export default todos;

```

이제 같은 디렉터리에 index.js 파일을 만들고 루트 리듀서를 생성한다.

```jsx
// modules/index.js
import { combineReducers } from 'redux';
import counter from './counter';
import todos from './todos';

const rootReducer = combineReducers({
  counter,
  todos,
});

export default rootReducer;
```

# 리액트 앱에 리덕스 적용
## 1. 스토어 만들기
index.js에 스토어를 생성한다
```jsx
// src/index.js
... 생략
import { createStore } from 'redux';
import rootReducer from './modules';

const store = createStore(rootReducer);

...생략
```

## 2. Provider 컴포넌트로 리덕스 적용하기
리액트 컴포넌트에서 스토어를 사용할 수 있도록 App 컴포넌트를 react-redux에서 제공하는 Provider 컴포넌트로 감싸준다.
> 해당 store를 사용할 범위만큼 감싸주면 된다.

- store를 props로 전달해주어야 한다.
```jsx
... 생략
import { createStore } from 'redux';
import rootReducer from './modules';

const store = createStore(rootReducer);

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>
    <Provider store={store}>
      <App />
    </Provider>
  </React.StrictMode>,
);

```

# 컨테이너 컴포넌트 만들기
리덕스 스토어와 연동된 컴포넌트를 `컨테이너 컴포넌트`라고 부른다.
컨테이너 컴포넌트에서는 스토어에 접근하여 원하는 상태를 받아오고, 액션을 디스패치한다.

```jsx
// containers/CounterContainer.js
import Counter from '../component/Counter';

const CounterContainer = () => {
  return <Counter />;
}

export default CounterContainer;
```

컴포넌트를 리덕스와 연동하기 위해서는 react-redux에서 제공하는 `connect` 함수를 사용한다.

```
connect(mapStateToProps,matDispatchToProps(연동할 컴포넌트)
```
> connect 함수를 호출하고 나면 또 다른 함수를 반환하는데, 여기에 컴포넌트를 파라미터로 넣어 호출하면 리덕스와 연동된 컴포넌트가 만들어진다. 

1.  mapStateToProps : 리덕스 **스토어 안의 상태를 컴포넌트의 props로 넘겨주기 위해** 설정하는 함수
- 스토어에 저장된 state를 가져온다. 
- state에서 컴포넌트의 props에 넣을 정보들을 뽑아 설정한다.
- 만일, 여러 개의 리듀서가 있는 경우 `state.리듀서이름.상태프로퍼티` 형태로 접근한다.
    
2.  mapDispatchToProps : **액션 생성 함수를 컴포넌트의 props로 넘겨주기 위해** 사용하는 함수
- 액션 생성 함수를 컴포넌트의 props로 넘겨준다. (개발자는 특정 이벤트 발생 시 액션 생성 함수를 호출하여 액션을 발생 시키도록 한다.)
- 액션 생성 함수와 `dispatch` 함수를 자동으로 연결하여 액션 생성 함수 호출 시 dispatch 함수 역시 호출하여 액션 객체를 스토어로 전달하고, 해당 리듀서에 전달될 수 있도록 한다.

```jsx
import Counter from '../component/Counter';
import { connect } from 'react-redux';

const CounterContainer = ({number, increase, decrease}) => {
  return <Counter number={number} onIncrease={onIncrease} onDecrease={onDecrease}/>;
}

const mapStateToProps = (state) => ({
  number: state.counter.number,
});

const mapDispatchToProps = {
  increase,
  decrease,
};

export default connect(mapStateToProps, mapDispatchToProps)(CounterContainer);    
```

