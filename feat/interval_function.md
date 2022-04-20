## 일정 시간마다 함수 실행하기

> ### 사용 의도
- 로그인을 토큰으로 구현.
- 액세스 토큰을 일정 주기마다 확인 및 재갱신하는 함수를 일정 주기마다 실행하려 함.

<br />

> ### 구현 코드
- 시간을 관리할 상태를 생성.

```

const [minutes, setMinutes] = useState(10);
const [seconds, setSeconds] = useState('00');
const [isTimeOut, setIsTimeOut] = useState(false);

```

<br />

- 시간을 두번째 인자로 받는 useEffect 함수 생성

```

useEffect(() => {
        if (minutes === 0 && seconds === '00') {
            setIsTimeOut(true);
        }
        const countdown = setInterval(() => {
          if (parseInt(seconds) > 0) {
            setSeconds(parseInt(seconds) - 1);
            if (parseInt(seconds) <= 10) {
                setSeconds(`0${parseInt(seconds) - 1}`)
            }
          }
          if (parseInt(seconds) === 0) {
            if (parseInt(minutes) === 0) {
              clearInterval(countdown);
            } else {
              setMinutes(parseInt(minutes) - 1);
              setSeconds(59);
            }
          }
        }, 1000);
        return () => clearInterval(countdown);
        
}, [minutes, seconds]);

```
