## 스크롤바 아래로 가게 하기

scrollTop 속성을 scrollHeight 값으로 지정.

```
scrollRef.current.scrollTop = scrollRef.current.scrollHeight;

=> scrollRef.current.scrollTop <= 할당된 값으로 스크롤을 이동

=> scrollRef.current.scrollHeight <= 스크롤의 가장 하단 값을 가짐.
```
<br>
  
> ### 활용 예시   
  
```
scrollRef.current.textContent += 'hi';
scrollRef.current.scrollHeight = scrollRef.current.scrollHeight;

=> 1. scrollRef에 할당된 태그에 'hi' 텍스트를 추가.
   2. 스크롤을 제일 하단으로 이동시킴.
   3. 업데이트된 내용 바로 확인가능.

```
