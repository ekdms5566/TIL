# CRA 프로젝트에 code-splitting 적용하기

목차  
[Reduce unused JavaScript](#reduce-unused-javascript)  
[webpack Code Splitting](#webpack-code-splitting)  
[lazy-loading(지연 로딩)](#lazy-loading지연-로딩)

<hr />

## 📍 Reduce unused JavaScript

<img src="https://user-images.githubusercontent.com/78911818/243170585-52a3fa04-567f-46f2-8fb4-61465ed05ca8.png" width="400px">

Lighthouse 검사를 받으면 performance 부분에서 `Reduce unused JavaScript`라는 문구를 많은 페이지에서 볼 수 있다. 직역하면 `사용하지 않는 JavaScript 줄이기`라는 의미로, 사용하지 않는 Javascript는 페이지 로드 속도를 느리게 만들기 때문에 이에 대한 경고 문구를 출력하는 것이다.

[Chrome Developer](https://developer.chrome.com/docs/lighthouse/performance/unused-javascript/?utm_source=lighthouse&utm_medium=devtools)에서는 사용하지 않는 Javascript의 문제에 대해 이렇게 말하고 있다.

> 페이지가 로드될 때마다 브라우저는 페이지에서 무엇이든 렌더링하기 전에 JavaScript 파일을 다운로드, 구문 분석 및 실행해야 합니다(JavaScript 파일이 명시적으로 지연되거나 비동기적으로 로드되지 않는 한).
>
> Lighthouse는 20킬로바이트 이상의 미사용 코드가 포함된 모든 JavaScript 파일에 플래그를 지정합니다.

그렇기 때문에 사용하지 않는 Javascript 코드를 줄일 경우, 브라우저의 실행 시간과 소비되는 대역폭이 감소하여 페이지 로딩 속도가 빨라질 수 있게 된다.

사용하지 않는 자바스크립트의 종류는 두가지 범주로 나뉜다.

1. Non-critical JavaScript(중요하지 않은 코드)

2. Dead JavaScript(더 이상 사용되지 않는 코드)

이를 해결하기 위해서 Chrome Developer에서는 3가지 방법을 추천하고 있다.

**1. Code Splitting**  
**2. Unused Code Elimination**  
**3. Unused Imported Code**

## 📍 webpack Code Splitting

Code Splitting이란 지정된 페이지에 필요한 이외의 javascript는 다운로드하거나 실행하지 않도록 설정하는 것을 말한다. Code Splitting은 번들 코드를 필요에 따라 독립적으로 로드하고 실행할 수 있는 여러 개의 작은 번들로 나누는 프로세스를 의미한다.

코드 스플리팅을 하는 주체는 webpack이기 때문에 `webpack.config.js`에서 코드 스플리팅에 대한 설정을 해야 한다.

이 부분은 아직 공부하는 중이기 때문에 나중에 추가로 내용 정리할 예정

만약, cra로 만든 프로젝트라면 웹팩 작업을 cra가 해주기 때문에 해당 작업은 하지 않아도 된다.

## 📍 lazy-loading(지연 로딩)

[Route-based code splitting 공식 문서](https://ko.legacy.reactjs.org/docs/code-splitting.html#route-based-code-splitting)

> 앱에 코드 분할을 어느 곳에 도입할지 결정하는 것은 조금 까다롭습니다. 여러분은 사용자의 경험을 해치지 않으면서 번들을 균등하게 분배할 곳을 찾고자 합니다.
>
> 이를 시작하기 좋은 장소는 라우트입니다. 웹 페이지를 불러오는 시간은 페이지 전환에 어느 정도 발생하며 대부분 페이지를 한번에 렌더링하기 때문에 사용자가 페이지를 렌더링하는 동안 다른 요소와 상호작용하지 않습니다.

공식문서에서는 아래와 같이 lazy를 사용하는 방법을 소개하고 있다. lazy는 **동적으로 필요할 때 import**를 하여 lazy-loading(지연 로딩)시키고, 실제로 로드되는 것으로 정해진 주소로 접속하면 로딩하게 된다.

```js
import OtherComponent from "./OtherComponent";
```

```js
const OtherComponent = lazy(() => import("./OtherComponent"));
```

이런 식으로 lazy 함수를 사용하면 동적 import를 사용해서 컴포넌트를 렌더링 할 수 있게 된다.

```js
import { Suspense, lazy } from "react";
import { BrowserRouter as Router, Route, Switch } from "react-router-dom";

const Home = lazy(() => import("./routes/Home"));
const About = lazy(() => import("./routes/About"));

const App = () => (
  <Router>
    <Suspense fallback={<div>Loading...</div>}>
      <Switch>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
      </Switch>
    </Suspense>
  </Router>
);
```

위처럼 lazy 컴포넌트를 사용하기 위해서는 `Suspense` 컴포넌트 하위에 위치해야 하며 `Suspense`의 `fallback`은 lazy 컴포넌트가 로드되기 전까지 보여줄 컴포넌트를 보여주게 된다.

<img src="https://user-images.githubusercontent.com/78911818/243419403-4edb63d3-87ce-402c-8202-8148eef721c5.png" width="200px" >

이렇게 적용하고 나니 확실히 렌더링 속도가 개선되어서 lighthouse 퍼포먼스 점수가 엄청나게 오른 것을 확인할 수 있다...!!

<hr />

참고  
[webpack 공식 문서 code splitting](https://webpack.js.org/guides/code-splitting/)  
[React 공식 문서 code splitting](https://ko.legacy.reactjs.org/docs/code-splitting.html#route-based-code-splitting)
