## jest

> ### 기능 소개 
의도한 대로 코드가 실행되는지 확인하는 테스팅 라이브러리

<br />

> ### 선택 이유

- 테스팅 라이브러리 중 사용 횟수가 높은 편.
  - jest: 17,710,276 (2022.03.28 기준)
  - mocha: 6,538,519 
  - jasmin: 17,710,276
- 사용 횟수가 많은 만큼 레퍼런스도 많기 때문.

<br />

> ### 사용 방법

- 필요한 라이브러리를 설치.
```
npm i --save-dev jest    // 테스트할 때만 사용하므로 개발용으로만 사용됨.
```

<br />

- babel 설치하기
- jest는 es6 문법을 지원하지 않으므로 babel을 통해 es5 문법으로 컴파일링 해줘야 함.  
[babel 설치하기](https://github.com/ka0824/react/blob/main/library/babel.md)

<br />

- package.json 파일 수정하기
```
"script": {
  "test": "jest"
}
```

<br />

- test 할 함수를 담은 js 파일 생성하기

```
const add = (a, b) => {
  return a + b;
}

export default add;
```

<br />

- test 할 내용을 담은 js 파일 생성하기
```
import add from "./add"

test('add 함수에 3과 5를 매개 변수로 넣으면 8이 반환됩니다.', () => {
  expect(add(3,5).toBe(8);
)
```

<br />

- 터미널에서 테스트를 실행.
```
npm test
```
