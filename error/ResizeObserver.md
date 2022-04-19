## ResizeObserver 에러 해결하기

> ### 에러 상황

- ag-grid-react를 이용해 테이블을 그리려 했지만 다음과 같은 에러로 인해 렌더링이 되지 않음.

```
Uncaught TypeError: Failed to execute 'observe' on 'ResizeObserver': parameter 1 is not of type 'Element'.
```

<br />

> ### 시도 방법

- 해당 오류문구를 구글로 검색해봤지만, 무한 스크롤 등 해당 기능을 실제로 코드로 사용하는 경우가 많았음.
- 이외의 경우도 뚜렷한 해결책을 찾을 수 없었음.
- ag-grid-react의 예시 코드를 붙여 넣어가며 확인한 결과, 제일 상단이라고 할 수 있는 'index.js'에서만 그래프가 렌더링됨.
- 여러차례 시도해 본 결과, <React.StrictMode> 엘리먼트로 감싸져 있을 시 에러가 발생함.
- 따라서, 해당 엘리먼트를 제거.
- StrictMode를 제거하여 생기는 단점을 따로 공부해 볼 필요가 있음.

<br />

> ### 결론

- index.js의 <React.StrictMode>을 제거.
