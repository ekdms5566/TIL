# Javascript의 날짜 라이브러리

목차

[Javascript의 Date 객체](#📍-javascript의-date-객체)  
[Moment.js란?](#📍-momentjs)  
[Day.js란?](#📍-dayjs)

<hr>

## 📍 Javascript의 Date 객체

Javascript의 Date 객체는 날짜와 시간을 위한 메소드를 제공하는 빌트인 객체이자 생성자 함수이다. Date 생성자 함수로 생성한 Date 객체는 UTC(1970년 1월 1일 00:00)을 기점으로 현재 시간까지의 밀리초를 계산한다. KST(한국 표준 시간)는 UTC에 9시간을 더한 시간으로 UTC 00:00 AM은 KST 09:00 AM을 의미한다.

<img src="https://user-images.githubusercontent.com/78911818/248457652-33e997a0-8d6f-4463-ac8a-2ef79d1adcf6.png">

이렇게 new Date 함수를 통해 Date 객체를 생성할 수 있으며, get/set 기본 함수로 년월일을 얻어오거 수정하는 것이 가능하다.

하지만 JavaScript의 Date 객체는 날짜와 시간을 증감하거나 형식을 변경하는 작업이 번거롭고 복잡하며 제한적이다. 또한, Date 객체는 브라우저마다 구현이 다를 수 있기 때문에 호환성 문제가 발생할 수 있다. 또한 복잡한 날짜 조작이 필요할 경우 코드가 길고 복잡해지기 때문에 날짜 라이브러리를 사영하여 빌트인 Date 객체의 한계를 극복하고 유연하게 간편한 날짜 처리를 하는 것이 가능하다.

## 📍 라이브러리를 사용하는 이유

대표적으로 Date 라이브러리에는 Moment.js와 Day.js가 있다. 해당 라이브러리는 빌트인 Date 객체보다 쉽고 다양한 기능을 제공하기 때문에 많이 사용된다.

날짜와 시간을 쉽게 조작할 수 있는 다양한 메소드를 제공하며, 날짜 형식 변환, 비교, 증강 등 다양한 작업을 간단하게 수행할 수 있다. 또한, locale과 다국어 처리에 특화되어 있으며, 날짜 형식을 locale에 맞게 자동으로 변환할 수 있으며, 다양한 형식의 문자열로 날짜를 파싱하는 것이 가능하다.

## 📍 Moment.js

Moment.js는 날짜 라이브러리 중 가장 많이 사용되었던 라이브러리로, 현재는 더 이상 업데이트 되고 있지 않다.

```bash
# moment.js 설치
npm install moment

# moment.js 사용하기
import moment from "moment";
# or
const moment = require("moment");
```

```js
moment().format("MMMM Do YYYY, h:mm:ss a"); // June 24th 2023, 8:11:54 pm
moment().endOf("day").fromNow(); // in 4 hours
moment().subtract(10, "days").calendar(); // 06/14/2023
moment.locale(); // en
moment().format("LLLL"); // Saturday, June 24, 2023 8:12 PM
```

위와 같이 format을 지정하거나 날짜를 비교하는 것도 가능하다. 이외에도 지원하는 메소드가 많기 때문에 유용하게 사용할 수 있다.

하지만 Moment.js는 문제점도 많이 있다. 현재 더 이상 업데이트를 하지 않는다는 점과 너무 큰 용량을 가지고 있다는 점, Tree shaking을 지원하지 않고 mutable 한 구조로 이루어져 있다는 단점이 있어 요즘은 많이 사용하고 있지 않다. Moment.js의 공식 문서에서는 해당 라이브러리를 대체하기 [좋은 라이브러리](https://momentjs.com/docs/#/-project-status/recommendations/)를 소개하고 있다. 라이브러리 자체가 사용을 추천하지 않는 것을 보면 정말 다른 것을 사용하는 게 좋을 것이다... Moment.js에서 소개하는 타 라이브러리를 비교하는 레포지토리도 있다. [You don't (may not) need Moment.js](https://github.com/you-dont-need/You-Dont-Need-Momentjs)

| Luxon                                | date-fns                                      |
| ------------------------------------ | --------------------------------------------- |
| moment.js의 단점을 없애준 라이브러리 | 날짜 라이브러리 중 유일하게 Tree shaking 지원 |
| Locales: Intl 제공                   | Functional Pattern으로 동작                   |
| Moment 진화판같은 느낌               | immutable 구조                                |
| Time Zones: Intl 제공                | coverage가 넓음                               |

Day.js를 자세히 다뤄볼 것이기 때문에 date-fns, Luxon에 대해서는 위와 같이 간단하게 정리할 수 있다.

## 📍 Day.js

### Day.js의 특징

- **가벼운 용량**

Day.js는 자바스크립트 날짜 관련 라이브러리 중 가장 가벼운 라이브러리로 moment.js 보다 약 33배 정도 가볍기 때문에 moment.js 대체로 많이 사용된다.

Day.js는 날짜를 받아오거나(year, month) 날짜 증감(add, sub), 출력 형식(format)과 같이 기본적으로 사용하는 메소드만 포함하고 있으며 나머지는 필요에 따라 plugin를 import하여 확장하여 사용하는 방식이기 때문에 가벼운 크기를 가지는 것이다.

- **단순한 사용방법**

Moment.js와 호환되는 API를 사용하여 최신 브라우저의 날짜와 시간을 구문 분석, 유효성 검사, 조작 및 표시하는 라이브러이기 때문에 Moment.js의 사용할 줄 안다면 Day.js도 쉽게 사용할 수 있을 것이다.

- **Immutable**

날짜를 변경하는 모든 작업은 새 인스턴스를 반환하기 때문에 불변성을 유지할 수 있다.

### Day.js 사용하기

```bash
# 설치 명령어
npm install dayjs
```

```js
const dayjs = require("dayjs");
// or
import dayjs from "dayjs";
```

위에서 말했듯이 format, get/set 등의 기본적인 메소드는 dayjs만 import하면 사용할 수 있다.

- **Display(format 메서드)**

```js
dayjs().format("YY년 MM월 DD일"); // 23년 06월 24일
dayjs().format("HH:mm:ss"); // 10:21:15
```

<img src="https://user-images.githubusercontent.com/78911818/248486029-1c1cadd5-6d51-4d7a-9c19-8a517c63e37e.png" width="400px">

날짜부터 시간과 UTC 형식 ordinal 형식도 지정하고 있기 때문에 필요에 따라 [공식 문서](https://day.js.org/docs/en/parse/string-format#list-of-all-available-parsing-tokens)를 참고하여 사용하면 된다.

- **GET**

```js
dayjs().minute(); // 분
dayjs().hour(); // 시
dayjs().date(); // 일
dayjs("2023-06-24").day(); // 요일
```

이런 형태로 날짜 또는 시간대를 받아오는 것이 가능하다.

- **Query**

day.js의 날짜 비교 관련 메소드

```js
dayjs().isBefore("2023-01-01", "month");
dayjs().isSame("2023-01-01", "year");
dayjs().isAfter("2023-01-01", "month");
dayjs().isBetween("2010-10-19", "2010-10-25", "month");
// milliseconds가 기준이기 때문에 비교하고 싶은 대상이 있다면 두 번째 매개변수로 전달한다.
```

- **Manipulate**

```js
dayjs().add(1, "days"); // 오늘 날짜 + 1
dayjs().sub(1, "month"); // 오늘 월 - 1
```

<img src="https://user-images.githubusercontent.com/78911818/248487365-98452824-24c3-421c-948b-0aae7d1051d2.png" width="400px">

날짜를 위와 같은 단위에 따라 쉽게 더하거나 뺄 수 있다.

```js
dayjs().startOf("year"); // January 1st, 00:00 this year
dayjs().endOf("month"); // the last day of this month, 00:00
```

또한, 특정 날짜 기준으로 단위에 따라 첫째 날, 마지막 날을 받아오는 것이 가능하다.

- **Plugins**

필요에 따라 플러그인을 import 하여 사용하는 것도 가능하다.

**년도 기준 주차 구하기**

```js
import dayjs from "dayjs";
import weekOfYear from "dayjs/plugin/weekOfYear";

// extend를 하지 않으면 사용할 수 없기 때문에 꼭 해야 한다.
dayjs.extend(weekOfYear);

dayjs().week(); // 오늘이 이번 년도의 몇 번째 주차인지 출력해주는 메소드
```

**요일 커스터마이징**

```js
import dayjs from "dayjs";
import updateLocale from "dayjs/plugin/updateLocale";

dayjs.extend(updateLocale);

dayjs.updateLocale("en", {
  weekdays: ["일", "월", "화", "수", "목", "금", "토"],
});

// 오늘 날짜의 요일을 위에서 지정한 요일로 출력된다.
dayjs().format("dd");
```

여기까지 정리한 내역은 주로 내가 프로젝트를 진행하며 가장 많이 사용한 메소드를 중점으로 정리한 것이지만 이외에도 날짜를 object 형태로 복제하거나 월 또는 주차를 customize 하는 방법도 있기 때문에 필요에 따라 사용하면 된다.

<hr>

참고

[Javascript Date 객체](https://ko.javascript.info/date)  
[Moment.js 공식 문서](https://momentjs.com/)  
[Momentjs와 dayjs](https://blog.hoseung.me/2022-03-13-dayjs-instead-of-momentjs/)  
[Day.js 공식 문서](https://day.js.org/)
