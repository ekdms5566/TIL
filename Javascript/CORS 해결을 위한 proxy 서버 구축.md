# CORS 해결을 위한 proxy 서버 구축

목차  
[문제 상황](#📍-문제-상황)  
[해결 방법](#📍-해결-방법)  
[1) 이미 만들어진 proxy 서비스 사용하기](#1-이미-만들어진-proxy-서비스-사용하기)  
[2) proxy 서버 구축하기](#2-proxy-서버-구축하기)

<hr />

## 📍 문제 상황

현 프로젝트에서는 회원가입을 할 때 재학생 여부와 학과를 확인함에 따라 가입 여부를 결정해야 했다. 그러기 위해서는 교내 웹 서비스에서 학번 조회하기 기능을 사용해야 했지만, 다른 도메인에서 요청을 보낼 경우에 요청을 받는 서비스에서 지정한 Origin이 아니기 때문에 보안 상의 이유로 CORS 정책이 적용되어 오류가 발생한다.

이를 해결하기 위해서 프록시 서버를 사용하려고 한다. 프록시 서버는 클라이언트와 서버 사이에서 요청을 중개하는 역할을 수행한다. 클라이언트는 프록시 서버에 요청을 보내고 프록시 서버는 해당 요청을 받아 실제로 API를 호출하고 응답을 받은 후 클라이언트에게 전달한다. 이 때, 프록시 서버는 프로젝트 도메인과 API를 제공하는 도메인 사이에 위치하여 CORS 정책을 우회하는 역할을 한다.

따라서, 프록시 서버를 사용하여 다른 서비스의 엔드포인트를 내 웹 사이트에서 사용하면 해당 서비스에서 지정한 origin과 다르기 때문에 발생하는 CORS 에러를 해결할 수 있다.

## 📍 해결 방법

### 1. 이미 만들어진 proxy 서비스 사용하기

[cors.sh](https://cors.sh/)

해당 사이트는 이미 만들어진 프록시 서버를 쉽게 사용할 수 있도록 해주는 서비스다.

<img src="https://user-images.githubusercontent.com/78911818/246614836-60d1d9e8-ef42-4b9b-805a-d3cded0efeba.png" width="400px" >

서비스에 들어가 위 이메일을 입력하면 메일로 API 키를 받을 수 있다. 이 API 키는 요청을 보낼 때 함께 보내는 token 같은 것이기 때문에 저장해놓고 이제 사용을 할 수 있다.

<img src="https://user-images.githubusercontent.com/78911818/246615246-2ec4eb90-fff3-4f65-947c-cfe3e38d109c.png" width="400px" >

이런 식으로 cors.sh 서비스가 중개해주는 것이고, 요청을 보낼 때는 `https://proxy.cors.sh/사용할-API-주소`로 위에서 받은 API key를 header에 함께 보내면 된다.

```js
// cors.sh에서 제시하는 사용 방법

// fetch
fetch("https://proxy.cors.sh/https://acme.com", {
  headers: {
    "x-cors-api-key": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  },
});

// axios
import Axios from "axios";

Axios.get("https://proxy.cors.sh/https://acme.com", {
  headers: {
    "x-cors-api-key": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  },
});
```

아래와 같이 적용했다. env 파일에 API key와 `https://proxy.cors.sh/api-주소`를 저장하여 사용하였다.

```js
const corsKey = process.env.REACT_APP_CORS_KEY;
const classNum = process.env.REACT_APP_CLASSNUM_URL;

export const getClassNum = async (data) => {
  try {
    const response = await axios.post(classNum, data, {
      headers: {
        "x-cors-api-key": corsKey,
      },
    });

    return response.data[0];
  } catch (err) {
    console.error(err);
  }
};
```

이런 식으로 사용하면 쉽게 CORS 이슈를 해결할 수 있지만, 값을 받아오는데 약 2-3초가 걸리는 부분이 거슬렸다...

그리고 이 서비스를 배포 후에도 사용할 수 있을지와 서비스가 언제 종료될지 모른다는 점 때문에 나중까지 사용이 가능하지 않을 수 있다는 문제가 있었기 때문에 직접 proxy 서버를 구축하는 것으로 결정했다.

### 2. proxy 서버 구축하기

> **1) express로 proxy 서버 만들기**

쉽게 express.js를 사용해서 proxy 서버를 구축하고자 한다.

클라이언트의 POST 요청을 프록시 서버를 통해 서버로 전달하고, 서버에서의 응답을 클라이언트에 반환하는 역할을 수행하도록 구성하였다.

```js
const express = require("express");
const axios = require("axios");
const bodyParser = require("body-parser");
// bodyParser는 요청 본문을 파싱하기 위한 미들웨어

const app = express();
const port = process.env.PORT || 8001; // 8001번 port로 설정

app.use(bodyParser.json()); // JSON 형식의 요청 본문을 파싱할 수 있도록 설정
app.use(bodyParser.urlencoded({ extended: true })); // URL 인코딩된 요청 본문을 파싱할 수 있도록 설정

app.use(function (req, res, next) {
  // 모든 도메인에서의 요청에 대해 Access-Control-Allow-Origin 헤더를 "*"로 설정
  res.header("Access-Control-Allow-Origin", "*");

  // 추가적인 헤더를 허용하기 위해 Access-Control-Allow-Headers 헤더를 설정
  res.header(
    "Access-Control-Allow-Headers",
    "Origin, X-Requested-With, Content-Type, Accept"
  );
  next();
});

const url = "cors 문제를 해결하고자 하는 API 주소";

const getClassNum = async (req) => {
  try {
    const res = await axios.post(url, req);
    return res.data;
  } catch (e) {
    console.log("error");
  }
};

// 클라이언트에서 '/api' 엔드포인트에 대한 POST 요청을 처리하는 함수
app.post("/api", (req, res) => {
  // body를 CORS를 해결하기 위한 API 주소에 보내고, 그 응답값을 다시 클라이언트에게 보내준다.
  getClassNum(req.body).then((response) => {
    res.send(response);
  });
});

app.listen(port, () => console.log(`run server ${port}`));
```

여기까지 하고 터미널에 `node index.js` 명령어를 입력하면 서버가 시작된다. 만약 env 파일에 지정한 port 번호가 있다면 지정한 port 번호로 시작될 것이고, 없다면 8001번 포트로 시작할 것이다. console에 나오는 포트 번호에 따라 클라이언트 측에서 요청을 `http://localhost:8001/api`로 요청을 보내면 될 것이다.

여기까지 하면 1번에서 사용했던 방법보다 훨씬 빠르게 응답값을 불러오는 것을 확인할 수 있다!!! 하지만 서버를 항상 로컬에서 실행할 수는 없기 때문에 배포를 통해 로컬에서 서버를 실행하지 않아도 사용 가능하도록 하고자 한다.

> **2) cloudType에 서버 배포하기**

간단하게 무료로 서버를 배포할 수 있는 곳을 찾던 중에 `CLOUDTYPE`이라는 서비스를 소개하는 [영상](https://www.youtube.com/watch?v=SGGebq48h3Y)을 보게 되었다. 간편하게 배포가 가능한 거 같아서 이 서비스를 통해 서버를 배포해보려고 한다.

<div style="display: flex; gap: 20px">
  <img src="https://user-images.githubusercontent.com/78911818/246617101-966bdc0e-6474-4561-8e6c-979a660c4993.png" width="300px" >
  <img src="https://user-images.githubusercontent.com/78911818/246617193-e2dd0c64-086a-4540-aa06-4dcebdb99c1a.png" width="300px" >
</div>

서비스에 들어가면 새 프로젝트 생성이 가능하다. `Node.js`를 선택하면 이미 있는 Github 저장소 프로젝트를 연결하는 것이 가능하다. 위에서 만든 proxy 서버를 올린 레포지토리를 연결하고 저장소를 만들면 된다.

<img src="https://user-images.githubusercontent.com/78911818/246617364-918b291f-c316-46ea-84aa-03945250517a.png" width="400px" >

1. 사용한 Node 버전과 동일한 버전을 선택한다.
2. Start command 입력란에 서버 시작 명령어인 `node index.js`를 입력한다.
3. 배포하기 버튼 누르고 기다리기 🐶

<img src="https://user-images.githubusercontent.com/78911818/246617548-39909eed-1e7e-4b9f-9ea0-9c5ecc7a83a1.png" width="300px" >

배포가 완료되면 도메인 탭에서 배포 도메인을 확인할 수 있다. 이제 배포가 됐으니 도메인에 지정한 엔드포인트로 클라이언트에서 요청을 보내면 된다.

<img src="https://user-images.githubusercontent.com/78911818/246617656-23184646-bf18-47e5-ac77-95fa378d203d.png" width="300px" >

응답값이 아주 잘 오는 것을 확인할 수 있다. 아주 만족스러운 결과... ✨🥭🌱
