## ag-grid 그래프 x축 값 표시 삭제하기

> ### 문제 상황
- 다뤄야 할 데이터가 많은 경우 x축의 값 (주로 날짜)이 겹침
- 해당 값을 표시안할 수는 없을까?

<br />

> ### 결론
- options을 통해 라벨 값을 빈 문자열로 변환 시킴.

<br />

> ### 문제 코드
```
let options = {
    title: {
      text: 'test'
    },
    data: fakeData,
    series: [
      {
        xKey: 'date',
        yKey: 'score',
        nice: true
      }
    ],
 }
```

<br/>

> ### 개선 코드

- axes 속성을 이용하려면, 사용하는 라벨들을 모두 등록 해줘야 함.
- 예를 들어 x축에서만 조작을 하고 싶더라도, y축 부분도 같이 작성해줘야 함.
- formatter 속성을 통해 원하는 값으로 변환시킬 수 있음.
```
let options = title: {
      text: 'test'
    },
    data: fakeData,
    series: [
      {
        xKey: 'date',
        yKey: 'score',
        nice: true
      }
    ],
    axes: [
      {
        type: 'category',
        position: 'bottom',
        label: {
          formatter: function(params) {
            return "";
          }
        }
      },
      {
        type: 'number',
        position: 'left',
      }
    ]
  }
 ```
