## hybrid-crypto-js

> ### 사용 이유
- 비대칭키 암호화 방식을 이용해 클라이언트에서 서버로 개인 정보를 보낼 때 암호화 하기 위해 사용
- crypto, crypto-js 등은 클라이언트 혹은 서버 중 한 곳에서만 사용 가능하여, 암호화, 복호화 방식을 통일하기 어려움.
- hybrid-crypto-js의 경우, 서버(express), 클라이언트(react) 모두에서 사용이 가능하여 직관적임.

<br />

> ### 사용 방식

- 설치

```
npm i hybrid-crypto-js
```

<br />

- 서버에서 비대칭키 한쌍을 생성하고, 공개키를 클라이언트로 보내고, 비밀키는 세션에 저장.

```
import { Crypt } from 'hybrid-crypto-js';
const RSA = require('hybrid-crypto-js').RSA;


export const makeKeys = async() => {

    try {

    const rsa = new RSA();

        return { publicKey: keyPair.publicKey, privateKey: keyPair.privateKey };     // 공개키, 비밀키 생성
    });

       return result;

    } catch (err) {
        return null;
    }
    
}
```

<br />

- 클라이언트에서 전달받은 공개키를 이용해 비밀번호 암호화
```
import axios from 'axios';
import { Crypt, RSA } from 'hybrid-crypto-js';

export const getLogin = async(id, password) => {

    let crypt = new Crypt();

    let rsa = new RSA();

    const publicKey = window.localStorage.getItem('publicKey')
    
    let encrypted = crypt.encrypt(publicKey, password);            // 공개키를 통해 암호화를 함.

    const result = await axios.post('https://localhost:3001/user/login', { id: id, password: encrypted }, { withCredentials: true });  // 암호화된 패스워드를 전송.

    return result;
}

```

<br />


- 서버에서는 암호화된 비밀번호를 받아, 세션에 저장된 비밀키로 복호화 진행

```
import { Crypt, RSA } from 'hybrid-crypto-js';

export const decryptData = async (encryptedPassword, privateKey) => {
    const rsa = new RSA();
    const crypt = new Crypt();


    const decrypted = crypt.decrypt(privateKey, encryptedPassword);
    
    console.log(decrypted);


    return decrypted.message;
}

```

