## dataset 

> ### 개념 소개

- 특정 엘리먼트 안에 특정 정보를 집어 넣음.
- e.target.dataset을 이용해 해당 엘리먼트에서 할당된 값을 불러 올 수 있음.

<br />

> ### 예시
- 클릭 했을 때에 엘리먼트에 부여된 dataset 값 출력하기

```
const APP = () => {
  return (
    <div>
      <div onClick={e => console.log(e.target.dataset.name} data-name="테스트입니다." />
    </div>
  )
}
```
