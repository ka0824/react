## 이벤트 위임

> ### 결론

- 구현이 가능은 하나, 실질적으로 효율성이 높아지지는 않음.
- react 자체적으로 이벤트 위임을 구현하고 있기 때문.<br />           << react의 이벤트 함수에 대한 공부가 좀더 필요!

<br />

> ### 시행 의도
- 상위 엘리먼트에서 이벤트 함수를 작성하여 하위 엘리먼트에서 반복되는 이벤트 함수 작성을 줄여 효율성 증대
- 예를들어, input 엘리먼트가 여러개 있을 시 해당 값을 반영하기 위한 이벤트 함수를 하나로 합칠 때에 사용

<br />

> ### 구현

- 엘리먼트의 dataset을 사용하면 각 엘리먼트를 구별할 수 있음.
- dataset을 통해 분기 나누기.
- 그러나 정작 하나의 이벤트 함수에 추가되는 코드가 길어지는 단점이 있음.

```
import React, { useState, useRef } from "react";
import './App.css';

function App() {
  const [input, setInput] = useState({
    input1: "",
    input2: "",
    input3: ""
  });

  return (
    <div className="App">
      <div onBlur={(e) => {
        const dataName = e.target.dataset.name;
        if (dataName === "input1") {
          setInput({ ...input, input1: e.target.value });
        } else if (dataName === "input2") {
          setInput({ ...input, input2: e.target.value });
        } else if (dataName === "input3") {
          setInput({ ...input, input3: e.target.value});
        }
      }}>
        <input data-name="input1" />
        <input data-name="input2" />
        <input data-name="input3" />
      </div>
    </div>
  );
}

export default App;
```

<br />

> ### 참고 문서
### [Event delegation in React](https://github.com/facebook/react/issues/13635)
