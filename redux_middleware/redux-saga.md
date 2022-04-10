### redux-saga

> ### 사용 이유
- redux-saga를 사용하면, 특정 action이 실행되었을 때, 이후 과정을 분기로 나눌 수 있음.
- 즉, 상태가 변화하는 과정을 좀더 세부적으로 다룰 수 있게 됨.
- redux-thunk가 사용 비율도 높고, 사용법도 간편하지만, redux-saga는 thunk보다 좀더 세부적 기능을 구현하는데에 도움이 됨.

<br />

> ### 예시 코드

- redux-saga 설치

```
npm i redux-saga
```
<br />

- generator 함수를 이용해 saga를 만들어 줌.
```
fetchAction.js

import { createAction } from 'redux-actions';
import { call, put, takeEvery } from 'redux-saga/effects';     // redux-saga에서 제공하는 헬퍼함수를 불러옴.
import { getData } from '../../util/fetchFuncs';
import axios from 'axios';

export const FETCHDATA = 'FETCH_DATA';
export const FETCHDATASUCCESS = 'FETCH_DATA_SUCCESS';
export const FETCHDATAERROR = 'FETCH_DATA_ERROR';

export const fetchData = createAction(FETCHDATA);
export const fetchDataSuccess = createAction(FETCHDATASUCCESS, data => data);
export const fetchDataError = createAction(FETCHDATAERROR, error => error);

const axiosData = async() => {
    const result = await axios.get('http://localhost:3001')
    return result.data;
}

function* fetchDataSaga() {                     // fetchData 액션이 실행된 이후, 다음 과정에 따라 진행됨.
    try {
        const result = yield call(axiosData);   // 서버에서 데이터를 받아 옴.
        yield put(fetchDataSuccess(result));    // 데이터를 받아오는 데에 성공했으면, fetchDataSuccess 액션 실행
    } catch (err) {
        yield put(fetchDataError(err));         // 데이터를 받아오는 데에 실패했으면, fetchDataError 액션 실행
    }
}

export function* dataSaga() {                   // 만든 사가를 하나의 사가로 묶어 줌.
    yield takeEvery(FETCHDATA, fetchDataSaga);  // 첫번째 인자는 기준이 되는 action의 type, 두 번째 인자는 해당 type에 따라 실행될 이후 saga;
}                                               // 최종 saga는 다른 saga들과 합쳐야 하기 때문에, export 해줌.
```

<br />

- 만든 aciton.js에서 만든 사가들을 하나의 사가로 합침
```
rootReducer.js

import { combineReducers } from 'redux';
import dataReducer from './dataReducer';
import infoReducer from './infoReducer';
import { dataSaga } from '../actions/dataActions';
import { all } from 'redux-saga/effects';         // redux-saga에서 제공하는 헬퍼함수를 불러 옴.

const rootReducer = combineReducers({
    info: infoReducer,
    fetchData: dataReducer
});

export function* rootSaga() {               // 각각의 파일에서 불러온 saga들을 하나로 합쳐줌. 
    yield all([dataSaga()]);                // 배열에 각각의 saga를 하나씩 담아줌.
}

export default rootReducer;

```

<br />
  
- sagaMiddleware를 등록하고, 실행.
```  
store.js

import rootReducer from "./reducers/rootReducer";
import { composeWithDevTools } from 'redux-devtools-extension';
import { createStore, applyMiddleware } from 'redux';
import { rootSaga } from './reducers/rootReducer';
import createSagaMiddleware from 'redux-saga';

const sagaMiddleware = createSagaMiddleware();        //sagaMiddleware 생성.

const store = createStore(rootReducer, composeWithDevTools(
    applyMiddleware(sagaMiddleware)               // sagaMiddleware 등록.
));

sagaMiddleware.run(rootSaga);             //rootSaga 실행

export default store;
```

<br />

- 평소대로 dispatch를 사용.
```
app.js

import './App.css';
import { useDispatch, useSelector } from 'react-redux';
import React, { useEffect, useState } from 'react';
import { changeName, changeAge } from './store/actions/infoActions';
import { fetchData } from './store/actions/dataActions';
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
      {<button onClick={() => dispatch(fetchData())}>데이터를 받아오자</button>}       // fetchData 액션이 실행되면, 이후 saga에 담긴 코드대로 순차적으로 진행됨.   
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

> ### 에시 코드 보기

### [redux-basic](https://github.com/ka0824/redux-basic)
