## 태그 안의 text 동적으로 수정하기

1. useRef로 해당 태그를 선택한다.
2. 선택한 태그의 textContent 속성을 이용한다.

```
import { useRef } from 'react';

const test = () => {

  const tagSelect = useRef();
  
  const editText = () => {
    tagSelect.current.textContent='클릭했습니다!'      // button을 클릭했을 시 div 태그에 '클릭했습니다!' 내용 추가    
  }
  
  return (
    <div ref={tagSelect}>
      <button onClick={editText}>
    </div>
  )


}

export deafult test;

```
