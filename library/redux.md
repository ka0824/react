## Redux

> ### 사용 이유

- 장점
  - 상태를 한 곳에서 관리.
  - 리덕스 툴을 통해 상태 변화를 확인하기 쉬움.
  - 로컬 스토리지 저장하기 간편.
  - 초기 설정 (initial State) 설정 간편.
  
- 단점
  - 관리해야 할 상태가 많지 않을 시 오히려 구조와 코드만 복잡해 질 수 있음.
  
<br />
  
> ### 설치 하기

```
  npm i redux
  npm i react-redux
  npm i --save-dev redux-devtools-extensions   // 리덕스 상태를 확인하기 위한 확장프로그램
```

<br />

> ### 리덕스 기본 개념

- action : 현 상태를 어떻게 변화시키려는 것인지 명시.
- reducer : 현 상태를 받아 action을 적용하여 새로운 상태 반환.
- store : reducer와 상태를 저장하는 공간.
- dispatch : action과 reducer를 연결.

<br />

> ### 구현 과정

- action.js 파일 생성
```
export const singing = () => {        // 동적인 값으로 상태가 변할 시, 인자에 해당 매개변수를 넣어 줘야 함.
    return {
        type: 'SINGING'
    };
}

export const studying = () => {
    return {
        type: 'STUDYING'
    };
}

export const sleeping = () => {
    return {
        type: 'SLEEPING'
    };
}
```

<br />

- 초기 상태 / initialState 생성
- initialState.js 생성

```
const initialState = {              // 브라우저를 처음 실행했을 때의 상태가 됨.
    behavior: "nothing"
}

export default initialState;
```
<br />

- reducer 생성
- reducer.js 생성

```
import initialState from './initialState';              // 만들어 둔 초기 설정을 불러 옴.

const behaviorReducer = (state = initialState, action) => {   // state가 주어지지 않을 시, 초기 설정을 적용 시킴.
    switch (action.type) {                                    
        case 'SLEEPING': 
            return {
                ...state,
                behavior: 'sleep'
            }
        case 'STUDYING':
            return {
                ...state,
                behavior: 'study'
            }     
        case 'SINGING':
            return {
                ...state,
                behavior: 'sing'
            }
        default: 
        return state
    }
}

export default behaviorReducer;
```

<br />

- store 생성
```
import { createStore } from 'redux';
import behaviorReducer from '../reducers/behaviorReducer';       // 만들어 준 reducer를 모두 불러 옵니다.
import { composeWithDevTools } from 'redux-devtools-extension';  // 상태 확인을 브라우저에서 확인할 수 있게 하는 확장 프로그램을 연결.

const store = createStore(behaviorReducer, composeWithDevTools()); // reducer가 여러 개 있을 시, combineReducer로 reducer를 모두 결합시킴.

export default store;
```

<br />

- react에 store 연결하기.
```
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import reportWebVitals from './reportWebVitals';
import { Provider } from 'react-redux';     // 해당 태그로 모든 태그를 감싸 줌.
import store from './store/store';          // store 불러옴.

ReactDOM.render(
  <React.StrictMode>
    <Provider store={store}>                // Provider 태그에 store 속성의 인자로 불러온 store을 넣어 줌.
      <App />
    </Provider>
  </React.StrictMode>,
  document.getElementById('root')
);
```

<br />

- 상태 값을 불러오고, 상태를 변화시키기
```
import './App.css';
import { useSelector, useDispatch } from 'react-redux';              // useSelector, useDispatch를 불러 옴.
import { singing, studying, sleeping } from './actions/behavior';    // 생성한 액션들을 불러 옴.

function App() {
  const behavior = useSelector(state => state.behavior);             // useSelector를 통해 상태 값을 조회할 수 있음.
  const dispatch = useDispatch();                                    // 상태를 변화시키기 위해 dispatch를 만듬.

  return (
    <div className="App">
      <button onClick={() => dispatch(sleeping())}>자고 있습니다.</button>     // dispatch의 인자로 action이 반환하는 객체를 넣어 줌.
      <button onClick={() => dispatch(singing())} >노래 부릅니다.</button>
      <button onClick={() => dispatch(studying())}>공부 합니다.</button>
      <div>{`지금 상태는 ${behavior}입니다.`}</div>
    </div>
  );
}

export default App;
```
