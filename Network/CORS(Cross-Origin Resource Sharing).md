# SOP(Same-Origin Policy)와 CORS(Cross-Origin Resource Sharing)

목차  
[SOP(Same-Origin Policy)란?](#📍sopsame-origin-policy란)  
[CORS(Cross-Origin Resource Sharing)란?](#📍-corscross-origin-resource-sharing란)  
[CORS의 동작 원리](#📍-cors의-동작-원리)  
[CORS 이슈 해결하기](#📍-cors-이슈-해결하기)

<hr />

## 📍SOP(Same-Origin Policy)란?

- 같은 출처에서만 리소스를 공유할 수 있는 규칙으로 브라우저에서 다른 서버에 요청할 경우에 해당되고, 브라우저를 거치지 않고 서버 간 통신을 할 때는 이 정책이 적용되지 않는다.

```
https://www.naver.com/search

위와 같은 주소가 있을 때 'https://'는 protocol(scheme), 'www.naver.com'은 host(domain), ':443'은 port를 의미한다.
이 때 protocol, host, port가 모두 같을 때를 동일 출처라고 한다.
```

## 📍 CORS(Cross-Origin Resource Sharing)란?

- CORS(Cross-Origin Resource Sharing) 정책은 추가 HTTP 헤더를 사용하여, 한 출처에서 실행 중인 웹 애플리케이션이 다른 출처의 선택한 자원에 접근할 수 있는 권한을 부여하도록 브라우저에 알려주는 체제이다.

- CORS 정책은 브라우저와 서버 간의 상호작용을 위한 HTTP 헤더를 사용하여 정의된다. 서버에서 CORS 정책을 구성하려면 아래와 같은 헤더를 사용하여 응답을 반환하고, 클라이언트에서 요청을 보낼 때에도 해당하는 헤더를 설정해야 한다.

```
CORS 관련 헤더

Access-Control-Allow-Origin: 서버가 허용하는 출처
Access-Control-Allow-Methods: 서버가 허용하는 HTTP 메서드
Access-Control-Allow-Headers: 서버가 허용하는 헤더
Access-Control-Allow-Credentials: 클라이언트가 인증 정보를 서버로 보낼 수 있는지 여부
Access-Control-Max-Age: 프리플라이트 요청의 유효 기간
```

- 이렇게 CORS 정책을 적절히 구성하면 웹 애플리케이션에서 다른 출처의 리소스에 안전하게 접근할 수 있으며, 보안과 데이터 무결성을 보호할 수 있다.

## 📍 CORS의 동작 원리

1. Simple Request (단순 요청)

- 단순 요청은 아래의 조건을 모두 충족하는 요청을 의미한다. 아래 조건을 만족하는 단순 요청만 안전한 요청으로 취급되어 프리플라이트 요청이 필요 없어 한 번만 요청을 전송한다.

> **조건**
>
> `GET`, `HEAD`, `POST` 중 하나의 메서드여야 한다. 에러가 포함된 요청이 서버까지 가게 된다면 DB에 영향을 미치는 로직이 발생할 수 있기 때문에 영향을 미치지 않는 3개의 메서드만 허용한다.
> 헤더는 Accept, Accept-Language, Content-Language, Content-Type 만 허용한다.
> Content-Type 헤더는 'application/x-www-form-urlencoded', 'multipart/form-data', 'text/plain' 값만 허용한다.

2. Preflight request (프리플라이트 요청)

<img src="https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS/preflight_correct.png" width="400px">

- 프리플라이트 요청은 요청을 예비 요청으로 실제 요청이 안전한지 확인한 후, 본 요청을 보내는 것을 의미한다. cross-origin 요청은 데이터에 영향을 줄 수 있기 때문에 프리플라이트 요청을 하는 것이다.

- 프리플라이트 요청은 `OPTIONS` 메서드를 활용하여, 다른 도메인의 리소스에 요청이 가능한지 (실제 요청이 전송하기에 안전한지) 확인 작업을 하고 요청이 가능할 때 실제 요청을 보내는 것이다.

- Preflight Request

```
Access-Control-Request-Method: POST
Access-Control-Request-Headers: X-PINGOTHER, Content-Type
```

`OPTIONS` 요청과 함께 위 두 개의 헤더가 함께 전송된다. 첫 줄은 실제 요청을 전송할 때 어떠한 메소드로 전송되는지 뜻하는 것이고, 두 번째 줄은 실제 요청을 전송할 때 X-PINGOTHER과 Content-Type이 실제 요청의 추가 헤더로 함께 전송되는 것을 의미하는 것이다.

- Preflight Response

```
Access-Control-Allow-Origin: http://foo.example
Access-Control-Allow-Methods: POST, GET, OPTIONS
Access-Control-Allow-Headers: X-PINGOTHER, Content-Type
Access-Control-Max-Age: 86400
```

서버가 메소드와 헤더를 받을 수 있는지에 대해 알려준다.
첫번째 줄은 서버 측 허가 origin이며, 두번째 줄은 허가하는 메소드, 세번째 줄은 허가 헤더, 마지막 줄은 Preflight 응답을 캐시할 수 있는 시간을 의미한다. 프리플라이트 요청을 보내면 요청이 두 번 오고가기 때문에 브라우저가 캐싱을 해두고 똑같은 요청을 보낼 때는 사전 요청을 보내지 않고 본 요청을 보내게 된다.

3. Credentialed Request (인증정보 포함 요청)

<img src="https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS/cred-req-updated.png" width="400px" >

인증 관련 헤더를 포함할 때 사용하는 요청이다. 기본적으로 브라우저는 브라우저의 쿠키 정보나 인증 정보를 보내지 않기 때문에 쿠키를 보내기 위해서는 설정을 해야하는데 그 때 사용하는 것이 바로 credentials 옵션이다.

```js
// fetch
fetch(url, {
  credentials: "include",
});

// axios
axios.post(url, data, { withCredentials: true });
```

위와 같이 credentials 설정을 include/true 로 설정하면 CORS 정책에 의해 Access-Control-Allow-Origin을 모든 출처를 허용하는 "\*"로 지정할 수 없으며, "\*"로 입력한 것을 특정 출처를 명시하여 변경해야 한다.

## 📍 CORS 문제 해결하기

CORS 문제를 해결하기 위해 가장 많이 사용하는 방법은 서버에서 Access-Control-allow-origin 헤더를 추가하는 것이다.

```java
// 모든 요청 허가
Access-Control-Allow-Origin: *
// 모든 호스트에 대한 요청을 허용하는 것이기 때문에 보안에 취약할 수 있다.

// 특정 주소 요청 허가
Access-Control-Allow-Origin: '특정 origin'
```

만약 서버를 처리할 수 없다면 proxy를 사용하는 방법도 있다. proxy 서버는 헤더를 추가하거나 요청을 허용하거나 거부하는 중간 역할을 할 수 있기 때문에 서버에 CORS 설정을 할 수 없을 경우 사용할 수 있다.

CRA로 생성한 프로젝트라면 `package.json`에 'proxy'를 설정하면 간단하게 프록시 서버를 활성화할 수 있다.

```js
"proxy" : "특정 origin";
```

또는 http-proxy-middleware 라이브러리를 이용하는 방법도 있다.

```bash
npm install http-proxy-middleware --save
```

로컬 서버에서 프록시를 사용할 경우 src 폴더 하위에 `setupProxy.js` 파일을 만들어 아래와 같이 만들면 쉽게 프록시를 설정할 수 있다.

```js
const { createProxyMiddleware } = require("http-proxy-middleware");

module.exports = function (app) {
  app.use(
    "/api", // proxy가 필요한 api 파라미터
    createProxyMiddleware({
      target: "http://localhost:5000", // 서버 주소
      changeOrigin: true, // HOST 헤더가 변경되도록 설정하는 부분
    })
  );
};
```

<hr />

참고

[mdn CORS](https://developer.mozilla.org/ko/docs/Web/HTTP/CORS)
