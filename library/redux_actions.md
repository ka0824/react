## redux-actions

> ### 사용 이유
- redux를 사용할 때에, action, reducer 코드 작성을 편리하게 함.
- createAction, handleActions를 이용.

<br />

> ### 사용 예시

```
info.js

import { createAction, handleActions } from 'redux-actions';

export const ADDONEYEAR = 'ADDONEYEAR';       // type 명으로 사용하므로 대문자 문자열을 할당.
export const CHANGENAME = 'CHANGENAME';       

export const addOneYear = createAction(ADDONEYEAR);       // createAction 함수에 사용하고자 하는 타입명만 인자로 넣어 줌.
export const changeName = createAction(CHANGENAME, name => name);  // 만약 매개 변수를 받는다면 두 번째 인자에 해당 형태를 추가.(생략해도 되자만, 가시성 때문)



infoReducer.js

import { handleActions } from 'redux-actions';
import { ADDONEYEAR, CHANGENAME } from 'info.js'

const initialState = {
  age: 30,
  name: 'sam'
}

const infoReducer = handleActions(
  {
    [ADDONEYEAR]: (state, action) => { ...state, age: age + 1 },
    [CHANGENAME]: (state, action) => { ...state, name: action.payload }      // 인자로 name 하나만 받으므로 payload를 그대로 사용.
  },
  initialState
)

export default infoReducer;


index.js

import { createStore } from 'redux';
import { Provider } from 'react-redux';
import infoReducer from './infoReducer.js';
import App from './App.js';

const Store = createStore(infoReducer);

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
);
```
