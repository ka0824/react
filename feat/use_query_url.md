## query 이용해 URL에 정보 담기

> ### 사용이유
- 링크를 통해 새로운 페이지로 이동할 때에, 전달하고자 하고자하는 데이터가 있을 경우 사용 가능

> ### 사용 방법

- Link 컴포넌트에 이동하고자 하는 주소 뒤에 변수명과 할당할 값을 덧붙임.
- ex) `/?id=99` => id라는 변수에 99라는 값을 담음.

```
import { Link } from 'react-router-dom';

.
.
.

<Link to={`/이동하고자하는 주소/?$id={담고자하는 정보}`}>
```

<br />

- query-string 라이브러리를 통해 url에 담긴 데이터를 객체 형태로 변환한다.
- useLocation을 통해 url의 정보를 받아옴.

```
import queryString from 'query-string';
import { useLocation } from 'react-router-dom';

const Component = () => {
  const location = useLocation();
  const id = queryString.parse(location.search).id; 

}
```
