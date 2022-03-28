## babel

> ### 사용 이유
- 사용하는 라이브러리 중 es5 문법을 지원하지 않은 경우가 있음.
- 이러할 때 현재 사용 중인 es6 문법을 es5 문법으로 컴파일링 해주는 기능을 함.

<br />

> ### 사용 방법

- 설치하기
- @babel/node : nodejs에서 사용하는 명령어를 babel을 적용하여 사용 가능하게 해 줌.
- @babel/preset-env : babel 설정 하는 역할.
- @babel/core : 바벨의 핵심 기능이 담겨져 있음.
```
npm i @babel/node @babel/preset-env @babel/core
```

- 바벨 설정파일 생성하기
- root 디렉토리에 .babelrc 파일 생성

```
{
  "presets": ["@babel/preset-env"]
}
```

