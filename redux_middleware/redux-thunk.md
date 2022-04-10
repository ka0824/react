## redux-thunk

> ## 사용 이유
- 기존의 action은 값 자체를 정적으로 변화시키거나 action.payload를 이용해 값을 변화 시키는 역할을 함.
- action을 적용할 때에 다른 side-effect를 실행시킬 수 있도록 해줌.
- 즉, 기존의 action 생성함수는 객체를 반환한다면, redux-thunk를 이용한 action 생성 함수는 다시 함수를 반환.
- 가장 많이 쓰이는 방법은 api 비동기 요청을 할 때에 사용.


<br />

> ## 적용 방법

- redux-thunk 설치
```
npm i redux-thunk
```

<br />

- 비동기 작업을 하는 action 작성
```
dataAction.js

import { createAction } from 'redux-actions';
import { call, put, takeEvery } from 'redux-saga/effects';
import { getData } from '../../util/fetchFuncs';
import axios from 'axios';

export const FETCHDATA = 'FETCH_DATA';
export const FETCHDATASUCCESS = 'FETCH_DATA_SUCCESS';
export const FETCHDATAERROR = 'FETCH_DATA_ERROR';

export const fetchData = createAction(FETCHDATA);
export const fetchDataSuccess = createAction(FETCHDATASUCCESS, data => data);
export const fetchDataError = createAction(FETCHDATAERROR, error => error);


export const asyncFetch = () => async (dispatch) => {               // 비동기 작업을 하는 함수형 action 입니다.
     dispatch(fetchData());
     try {
         const result = await axios.get('http://localhost:3001');
         dispatch(fetchDataSuccess(result.data));
     } catch (error) {
         dispatch(fetchDataError(error));
     }
}

const axiosData = async() => {
    const result = await axios.get('http://localhost:3001')
    return result.data;
}


```

</ br>

- action을 다루는 reducer 작성
```
import { FETCHDATA, FETCHDATASUCCESS, FETCHDATAERROR } from '../actions/dataActions';
import { handleActions } from 'redux-actions';

const initialState = {
    loading: false,
    data: [],
    error: null
}

const dataReducer = handleActions({
    [FETCHDATA]: (state, action) => ({ ...state, loading: true }),                   // 데이터를 받아오기 시작하면 로딩만 true 값으로 변화시킴.
    [FETCHDATASUCCESS]: (state, action) => ({ ...state, data: action.payload, loading: false }),  // 데이터 받기를 성공했을 때의 상태 변화
    [FETCHDATAERROR]: (state, action) => ({ ...state, error: action.payload, loading: false })   // 데이터 받기를 실패했을 때의 상태 변화
}, initialState);

export default dataReducer;
```

<br />

- rootReducer에 해당 reducer를 등록
```
import { combineReducers } from 'redux';
import dataReducer from './dataReducer';
import { dataSaga } from '../actions/dataActions';

const rootReducer = combineReducers({
    fetchData: dataReducer        // 저장하고자 하는 상태의 key를 설정.
});

export default rootReducer;
```

<br />

- redux-thunk middleware를 store에 등록
```
store.js

import rootReducer from './reducers/rootReducer';
import { composeWithDevTools } from 'redux-devtools-extension';
import { createStore, applyMiddleware } from 'redux';     //redux에서 applyMiddleware를 불러옴.
import ReduxThunk from 'redux-thunk';

const store = createStore(rootReducer, composeWithDevTools( applyMiddleware(Redux-thunk) ));

export default store;
```

<br />

- Provider 컴포넌트로 전체 컴포넌트를 감싸줌. (redux 사용할 때의 공통과정)
```
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import reportWebVitals from './reportWebVitals';
import store from './store/store';
import { Provider } from 'react-redux';

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

- 비동기 작업을 수행하는 action을 디스패치의 인자로 넣어줌
```
App.js

import './App.css';
import { useDispatch, useSelector } from 'react-redux';
import React, { useEffect, useState } from 'react';
import { changeName, changeAge } from './store/actions/infoActions';
import { asyncFetch } from './store/actions/dataActions';        // 데이터를 받아오는 액션생성함수 전체를 불러오는 것이 아니라, 비동기 함수를 반환하는 action 생성 함수만 가져옴.
import { asyncChange } from './store/actions/infoActions'
import axios from 'axios';

function App() {
  const age = useSelector(state => state.info.age);
  const name = useSelector(state => state.info.name);
  const data = useSelector(state => state.fetchData.data);
  const isLoading = useSelector(state => state.fetchData.loading)
  const dispatch = useDispatch();

  const [nameInput, setNameInput] = useState('');
  const [ageInput, setAgeInput] = useState(0);

  return (
    <div className="App">
      <div>{`이름은 ${name}이고, 나이는 ${age}입니다.`}</div>
      <div>
        <input onChange={(e)=> setNameInput(e.target.value)}></input>
        <button onClick={() => dispatch(changeName(nameInput))}>이름 수정</button>
      </div>
      <div>
        <input onChange={(e) => setAgeInput(e.target.value)}></input>
        <button onClick={() => dispatch(changeAge(ageInput))} >나이 수정</button>  
      </div>
      <div>
        <button onClick={() => console.log(nameInput)}>nameInput 확인</button>
        <button onClick={() => console.log(ageInput)} >ageInput 확인</button>
      </div>
        <button onClick={() => dispatch(asyncFetch())}>데이터를 받아오자</button>          // 데이터를 받아올 때 실행되는 비동기 작업을 실행.
      <div>
        <button onClick={() => console.log(data)}>데이터를 받아왔나 확인하기</button>  
      </div>
      <div>
        <button onClick={() => dispatch(asyncChange())}>비동기 처리 확인</button>
      </div>
    </div>
  );
}

export default App;

```

<br />

> ## 예시 코드 레파지토리 바로가기
### [redux-basic 코드](https://github.com/ka0824/redux-basic)
