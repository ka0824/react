### logger

> ### 사용 이유
- logger를 사용하면, 액션이 적용되기 전의 상태와, 적용 된 후의 상태를 콘솔 창에서 확인 가능.
- 개발 단계에서, 본인의 구상대로 코드가 작동하는 지 확인하기 용이함.

<br />

> ### 사용법

- logger를 설치
```
npm install redux-logger;
```

<br />

- logger를 middleware로 등록.
  - logger는 항상 middleware의 가장 마지막으로 등록.
```
import rootReducer from './reducers/rootReducer';
import { composeWithDevTools } from 'redux-devtools-extension';
import { createStore, applyMiddleware } from 'redux';
import logger from 'redux-logger';

const store = createStore(rootReducer, composeWithDevTools( logger) ));    // 만약 사용하는 middleware가 많다면 logger를 가장 나중에 넣어 줌.

export default store;
```
