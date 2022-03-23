## jsx란?

> ## 소개

- 자바스크립트와 HTML 마크업을 혼합한 형태.
- React의 element를 생성함.

```
const hiTag = <div className="greeting">hi</div>     // div 태그를 생성함. babel을 통해 아래 문법으로 컴파일됨.



const hiTag = React.createElement(                << 상단의 jsx 문법과 똑같은 의미를 가짐.
  'div',
  {className: 'greeting'},
  'hi'
);

```

> ## jsx 안에 변수 참조하기

- 이용하고자 하는 변수를 {} 안에 넣어줌.

```

const name = 'james';

const nameTag = <div>{name}</div>

```

> ## 규칙
- 변수 이름은 camelCase를 이용.
