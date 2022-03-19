## useRef 이용해서 태그 선택하기 (함수형 컴포넌트)

1. useRef를 import 해옴.
2. useRef()를 할당하는 변수 만들기.
3. 선택하고자 하는 태그 ref 속성에 해당 변수 할당.

```
import { useRef } from 'react';

const Header = () => {
  
  const divSelect = useRef(null)     // useRef의 인자는 초기값이 됨.
  
  return (
    <div ref={divSelect} />
  );
};

export default Header;

```
