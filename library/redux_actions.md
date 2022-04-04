## redux-actions

> ### 사용 이유
- redux를 사용할 때에, action, reducer 코드 작성을 편리하게 함.
- createAction, handleActions를 이용.

<br />

> ### 사용 예시
- createAction을 이용해서 액션을 생성함.

```
action.js

import { createAction } from "redux-actions";

export const ADDONEYEAR = 'ADD_ONE_YEAR';
export const CHANGENAME = 'CHANGE_NAME';

export const addOneYear = createAction(ADDONEYEAR);
export const changeName = createAction(CHANGENAME, name => name);  2번째 인자는 생략 무방하지만, 안에 들어가는 인자를 명시적으로 나타내기 위해 추가해 줌.
```
<br />

- handleActions()를 사용하여 reducer 생성
```
infoReducer.js

import { handleActions } from 'redux-actions';
import { ADDONEYEAR, CHANGENAME } from '../actions/action';

const initialState = {
    age: 30,
    name: 'sam'
}

const reducer = handleActions(
    {
        [ADDONEYEAR]: state => {return { ...state, age: state.age + 1}},
        [CHANGENAME]: (state,action) => {return { ...state, name: action.payload}}
    },
    initialState
)

export default reducer;
```

<br />

- 이 후 과정은 기존 redux를 사용해 코드를 작성하였을 때와 동일.

```
index.js

import React from 'react';
import ReactDOM from 'react-dom';
import {composeWithDevTools} from 'redux-devtools-extension';
import { createStore } from 'redux';
import { Provider } from 'react-redux';
import reducer from './reducer/reducer';
import './index.css';
import App from './App';

const store = createStore(reducer, composeWithDevTools());

ReactDOM.render(
  <React.StrictMode>
    <Provider store={store}>
      <App />
    </Provider>
  </React.StrictMode>,
  document.getElementById('root')
);

```

<br />

```
import './App.css';
import { useSelector, useDispatch } from 'react-redux';
import { addOneYear, changeName } from './actions/action';

function App() {
  const dispatch = useDispatch();
  const age = useSelector(state => state.age);
  const name = useSelector(state => state.name);

  return (
    <div className="App">
      <div>{`현재 나이는 ${age}입니다.`}</div>
      <div>{`현재 이름은 ${name}입니다.`}</div>
      <button onClick={() => dispatch(addOneYear())}>나이 변경</button>
      <button onClick={() => dispatch(changeName('james'))}>이름 변경</button>
    </div>
  );
}

export default App;

```
