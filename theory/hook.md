## Hook

> ### 도입 이유
- 함수형 컴포넌트는 렌더링될 때 마다 모든 변수 함수가 초기화.
- 따라서 동적으로 변화하는 값을 저장할 수 없음.(무상태성)
- hook을 통해 상태를 저장 가능.

<br />

> ### Hook의 종류

- useState()
  - 상태를 저장하고, 변경할 때 사용됨.

```
const [상태저장명, 상태 바꾸는 함수] = useState(초기 값);

예시)
const [isOpen, setIsOpen] = useState(false); 

상태 조회하기)
console.log(isOpen);

상태 변경하기)
setIsOpen(true);
```
<br />

- useEffect()
  - 렌더링이 된 이후에 실행되어야 할 코드를 실행시켜 줌.
  - 두번째 인자 배열에 담긴 상태가 변화할 때마다 콜백함수가 실행됨. (생략 가능)
  - 첫 렌더링시에는 무조건 실행됨.

```
useEffect(() => {
  console.log('Hello World!');
});                                 => 두 번째 인자가 없으므로 매 렌더링 때마다 'Hello World!'가 출력 됨.

useEffect(() => {
  console.log('Hello World!');      => 두 번째 인자가 빈 배열이므로 첫 렌더링 시에만 'Hello World!'가 출력 됨.
}, []);

useEffect(() => {
  console.log('Hello World!');      => isOpen이라는 상태가 바뀌어서 렌더링 될 때마다 'Hello World!'가 출력 됨.
}, [isOpen]);
```

<br />

- useContext()
  - state를 전역으로 관리 가능.
  - 하위 컴포넌트에서 state를 받아오기 위해서는 props로 계속 내려 받아야 하는데, useContext()를 이용하면 이러한 과정을 줄일 수 있음.

```
App.js

import { createContext, useState } from 'react';

export const AppContext = createContext();             // context 생성

const App = () => {
  const [age, setAge] = useState();
  
  return (
    <div>
      <AppContext.Provider value = {{age, setAge}}>    // 생성한 context를 하위 컴포넌트들이 사용할 수 있게 Provider 태그로 감싸 줌
        <NewComponent />
      <AppContext.Provider />
    </div>
  );
}


NewComponent.js

import { AppContext } from './App';

const NewComponent = () => {
  const { age, setAge } = useContext(AppContext);     // 상위 컴포넌트에서 생성한 state와 state 변경함수를 context를 통해 받아옴
  
  return (
    <div />
  )
}
```

<br />

- useReducer()
  - 상태 변경 함수를 component와 분리된 파일에서 작성할 수 있게 함.
```
reducer.js

const reducer = (state, action) => {
  switch (action.type) {
    case (CHANGE_AGE): 
      return {
        ...state,
        age: action.age
      };
    case (CHANGE_NAME):
      return {
        ...state,
        name: action.name
      }
  }
}

export default reducer;


App.js

import React, { useReducer } from 'react';
import reducer from './reducer'; 

const App = () => {

  const [info, dispatch] = useReducer(reducer, {name: 'kane', age: 30}) // info = 상태, dispatch = 상태와 reducer를 연결 해주는 함수
  return (                                                              // reducer = 액션을 받아 새로운 상태 반환
    <div />                                                             // {name: 'kane, age: 30} = 초기 상태
  )
}
```

<br />

- useMemo()
  - 설정한 값(2번째 인자의 배열에 담긴 값)이 변하지 않는다면 함수를 재실행하지 않고 이전 값 그대로 반환함.
```
App.js

import React, { useState, useMemo } from 'react';

const countChild = (people) => {
  return people.child;
}

const App = () => {
  const [people, setPeople] = useState({child: 0, adult: 0});
  const count = useMemo(() => countChild(people), [people.child])      // people.child 값이 바뀌지 않는한 count는 실행되지 않고 이전 값을 재사용.
return (
  <div>어린이의 수 : {count} </div>  
)
}
```

<br />

- useCallback()
  - 설정한 값(2번째 인자 배열에 담긴 값)이 변하지 않는 한 이전에 사용한 함수를 새로 선언하지 않고 기존 함수를 사용가능하게 함.

```
App.js

import React, { useCallback, useEffect, useState } from 'react';

const App = () => {

  const [age, setAge] = useState(30);

  const addOneYear = () => {       // 렌더링 될 때마다 주소 값이 변함.(참조형 타입)
    setAge(age + 1);
  }
  
  useEffect(() => {
    addOneYear();
  },[addOneYear]);                 // 함수 addOneYear가 실행되면서 상태 age 값이 바뀜 => 재렌더링 =>
                                   // 재렌더링되는 과정에서 addOneYear 주소값 바뀜 => useEffect 다시 실행 => 함수 addOneYear 다시 실행 => 반복
  return (
    <div />
  )

}



수정 후
App.js

import React, { useCallback, useEffect, useState } from 'react';

const App = () => {

  const [age, setAge] = useState(30);

  const addOneYear = () => {       // 렌더링 될 때마다 주소 값이 변함.(참조형 타입)
    setAge(age + 1);
  }
  
  const addOneYear = useCallback(() => {      // useCallback의 2번째 인자 배열에 age가 없으므로 함수가 렌더링 될 때마다 초기화되지 않음.
    setAge(age + 1);
  }, []);
  
  useEffect(() => {
    addOneYear();
  },[addOneYear]);                 
                       
  return (
    <div />
  )

}

```

<br />

- useRef
  - 재렌더링될 때도 초기화 되면 안되는 변수를 관리해야 할 때 사용
  - state 와의 차이점은 값이 바뀌어도 재렌더링이 되지 않는다는 점.
  - 혹은 dom을 직접 제어하고 싶을 때 사용.

```
App.js

import React, {useRef} from 'react';

const App = () => {

const age = useRef(30);

const addOneYear = () => {
  age + 1;
}
  
useEffect(() => {         // useEffect의 2번째 인자가 생략되어, 만약 age가 state 였다면 무한히 useEffect가 실행되게 됨.
  addOneYear();           // 하지만 useRef로 저장한 값은 변하더라도 재렌더링 되지 않으므로 문제 X.
})

return (
  <div />
)
}
```
