[최댓값과 최솟값](https://school.programmers.co.kr/learn/courses/30/lessons/12939)

```js
function solution(s) {
  const arr = s.split(" ");

  return `${Math.min(...arr)} ${Math.max(...arr)}`;
}
```

[JadenCase 문자열 만들기](https://school.programmers.co.kr/learn/courses/30/lessons/12951)

```js
function solution(s) {
  return s
    .split(" ")
    .map((i) => i.charAt(0).toUpperCase() + i.slice(1).toLowerCase())
    .join(" ");
}
```

```js
function solution(s) {
  return s
    .split(" ")
    .map((v) => v.charAt(0).toUpperCase() + v.substring(1).toLowerCase())
    .join(" ");
}
```

[올바른 괄호(스택/큐)](https://school.programmers.co.kr/learn/courses/30/lessons/12909)

```js
function solution(s) {
  let count = 0;

  for (let val of s) {
    if (count < 0) return false;
    else if (val === "(") count++;
    else count--;
  }

  return count === 0;
}
```

[최솟값 만들기](https://school.programmers.co.kr/learn/courses/30/lessons/12941)

```js
function solution(A, B) {
  A.sort((a, b) => b - a);
  B.sort((a, b) => b - a).reverse();
  let num = 0;

  for (let i = 0; i < A.length; i++) {
    num += A[i] * B[i];
  }

  return num;
}
```

[이진 변환 반복하기](https://school.programmers.co.kr/learn/courses/30/lessons/70129)

```js
function solution(s) {
  let count = 0,
    num = 0;
  let str = s;

  while (str !== "1") {
    num += str.length - str.replaceAll("0", "").length;
    str = str.replaceAll("0", "").length.toString(2);
    count++;
  }
  return [count, num];
}
```

[숫자의 표현](https://school.programmers.co.kr/learn/courses/30/lessons/12924)

```
n 이하인 숫자 a부터 k개의 연속된 숫자의 합이 n이라고 가정한다면

a + (a+1) + (a+2) + ... + (a+k-1) = k(2a+k-1)/2 = n
a = n/k + (1-k)/2
a <= n
k < n
a, k : 자연수

a가 자연수라는 조건이 성립하기 위해서는

n/k 가 자연수이려면 : k는 n의 약수여야 한다.
(1-k)/2 가 정수가 되려면 : 1-k 가 짝수여야 하므로 k는 홀수여야 한다.
k < n
```

```js
function solution(n) {
  return [...Array(n + 1).keys()].filter((i) => i % 2 !== 0 && n % i === 0)
    .length;
}
```

```js
// n 의 약수 중 홀수 약수의 개수
function solution(n) {
  var answer = 0;
  for (let i = 1; (i * (i - 1)) / 2 < n; i++)
    if ((n - (i * (i - 1)) / 2) % i == 0) answer++;
  return answer;
}
```

```
홀수의 경우, 약수는 홀수 밖에 안나옴. 15의 약수는 1,3,5,15.
약수를 이용해서 연속된 수의 합이 15가 나오도록 할 수도 있음.
15=1+2+3+4+5 (중간값 3) 3x5
15=4+5+6 (중간값 5) 5x3
15=7+8 (연속된 수) 7+8 - 홀수인 경우 무조건 가능.
15=15

중간값이 3인 경우, 중간값이 5인 경우, 연속된 수(7, 8), 15(n) 해서 4개인데, 이게 공교롭게 홀수의 약수 수와 같음. 그리고 짝수의 경우는 홀수의 연장선이라고 보면 됨. n=30인 경우, 30의 약수는 1, 2, 3, 5, 6, 10, 15, 30임.

30=4 + 5 + 6 + 7 + 8 (중간값 3의 연장) 2x3x5
30=9 + 10 + 11 (중간값 5의 연장) 2x5x3
30=6+7+8+9 (연속된 두 수의 연장) 2x(7+8)
30=30

결과적으로 n의 홀수 약수 개수만 구해도 답이랑 같음.
```

[다음 큰 숫자](https://school.programmers.co.kr/learn/courses/30/lessons/12911)

```js
function solution(n) {
  let num = n + 1;

  while (
    n.toString(2).replaceAll("0", "").length !==
    num.toString(2).replaceAll("0", "").length
  ) {
    num++;
  }
  return num;
}
```

```js
function solution(n, a = n + 1) {
  return n.toString(2).match(/1/g).length == a.toString(2).match(/1/g).length
    ? a
    : solution(n, a + 1);
}
```

[피보나치 수](https://school.programmers.co.kr/learn/courses/30/lessons/12945)

```js
function solution(n) {
  let answer = [];
  for (let i = 0; i <= n; i++) {
    if (i == 0) answer.push(0);
    if (i == 1) answer.push(1);
    if (i >= 2) {
      answer.push((answer[i - 1] + answer[i - 2]) % 1234567);
    }
  }
  return answer[n];
}
```

```js
function solution(n) {
  let result = [0, 1];
  while (result.length !== n + 1) {
    let fibonacci =
      (result[result.length - 2] + result[result.length - 1]) % 1234567;
    result.push(fibonacci);
  }
  return result[n];
}
```

[짝지어 제거하기](https://school.programmers.co.kr/learn/courses/30/lessons/12973)

```js
function solution(s) {
  let result = [];
  for (let i = 0; i < s.length; i++) {
    if (result.length === 0 || s[i] !== result[result.length - 1])
      result.push(s[i]);
    else result.pop();
  }
  return result.length ? 0 : 1;
}
```

[영어 끝말잇기](https://school.programmers.co.kr/learn/courses/30/lessons/12981)

```js
// 앞 단어로 시작하지 않을 경우, 중복 단어가 있을 경우 검사
function solution(n, words) {
  let arr = [],
    num = 0;

  for (let i = 0; i < words.length; i++) {
    if (
      arr.includes(words[i]) ||
      (i > 0 && words[i][0] !== words[i - 1][words[i - 1].length - 1])
    ) {
      return [(num % n) + 1, Math.floor(num / n) + 1];
    }

    arr.push(words[i]);
    num++;
  }
  return [0, 0];
}
```

```js
function solution(n, words) {
  let answer = 0;
  words.reduce((prev, now, idx) => {
    answer =
      answer ||
      (words.slice(0, idx).indexOf(now) !== -1 || prev !== now[0]
        ? idx
        : answer);
    return now[now.length - 1];
  }, "");

  return answer ? [(answer % n) + 1, Math.floor(answer / n) + 1] : [0, 0];
}
```

```js
function solution(n, words) {
  const ans = (i, n) => [(i + 1) % n || n, Math.ceil((i + 1) / n)],
    said = [words[0]];

  for (let i = 1; i < words.length; i++) {
    const cur = words[i],
      pre = words[i - 1];

    if (cur[0] !== pre[pre.length - 1] || said.includes(cur)) return ans(i, n);
    else said.push(cur);
  }
  return [0, 0];
}
```

[카펫(완전탐색)](https://school.programmers.co.kr/learn/courses/30/lessons/42842)

```js
function solution(brown, yellow) {
  const size = brown + yellow;

  for (let i = size + 1; i > 1; i--) {
    if (size % i === 0) {
      const div = size / i;
      if ((i - 2) * (div - 2) === yellow) return [i, div];
    }
  }
}
```

[예상 대진표](https://school.programmers.co.kr/learn/courses/30/lessons/12985)

```js
function solution(n, a, b) {
  let answer = 0;

  while (a !== b) {
    a = Math.round(a / 2);
    b = Math.round(b / 2);
    answer++;
  }
  return answer;
}
```

[구명보트(Greedy)](https://school.programmers.co.kr/learn/courses/30/lessons/42885)

```js
function solution(people, limit) {
  let count = 0;
  people.sort((a, b) => b - a);

  for (let i = 0, j = people.length - 1; i <= j; i++) {
    if (people[i] + people[j] <= limit) j--;
    count++;
  }

  return count;
}
```

```js
function solution(people, limit) {
  let count = 0,
    l = 0,
    r = people.length - 1;

  people.sort((a, b) => b - a);

  while (l < r) {
    let sum = people[l] + people[r];

    if (sum > limit) l++;
    else {
      l++;
      r--;
    }
    count++;
  }

  if (l == r) count++;

  return count;
}
```

```js
function solution(people, limit) {
  people.sort(function (a, b) {
    return a - b;
  });
  for (var i = 0, j = people.length - 1; i < j; j--) {
    if (people[i] + people[j] <= limit) i++;
  }
  return people.length - i;
}
```

[점프와 순간 이동](https://school.programmers.co.kr/learn/courses/30/lessons/12980)

0에 어떤 수를 더하고 곱했을때 최대한 덜 더해서 n까지 가는 거리
반대로 하면 n에 어떤 수를 뺴고 나눴을 때 최대한 덜 빼서 0까지 가는 거리를 의미한다.

주어진 값을 2로 나눠가면서 나머지가 없을 경우 2로 나누기 반복, 나머지가 있다면 1회 점프(-1)해주고 반복

```js
function solution(n) {
  let num = 0;

  while (n > 0) {
    if (n % 2 !== 0) {
      n--;
      num++;
    }
    n /= 2;
  }
  return num;
}
```

```js
// 이진수 사용
function solution(n) {
  if (n === 1) return 1;
  const nArr = Array.from(n.toString(2));
  return nArr.reduce((a, b) => +a + +b);
}

// 어떤 수 a에 2를 곱하는 방법을 a * 2로 생각할 수 있지만 시프트 연산(a << 1) 도 동일한 결과입니다. 왼쪽 시프트 연산의 경우 이진수 관점에서 가장 오른쪽에 0을 넣는 것과 마찬가지죠 ( "0b101" << 1 = "0b1010"). 주어진 수를 이진수로 바꾸어 보면 1이 나오는 위치에서 1칸 이동(에너지 1) 0이 나오는 위치에서 2배 이동(에너지 0)하면 되니, 답은 n을 이진수로 바꿨을 때 1의 갯수입니다.
```

```js
function solution(n) {
  return n.toString(2).replace(/0/g, "").length;
}
```

[N개의 최소공배수](https://school.programmers.co.kr/learn/courses/30/lessons/12953)

```js
function solution(arr) {
  let gcd = getGcd(arr[0], arr[1]);
  let gcf = (arr[0] * arr[1]) / gcd;

  for (let i = 1; i < arr.length; i++) {
    gcd = getGcd(gcf, arr[i]);
    gcf = (gcf * arr[i]) / gcd;
  }

  return gcf;
}

function getGcd(a, b) {
  return b ? getGcd(b, a % b) : a;
}
```

[멀리 뛰기](https://school.programmers.co.kr/learn/courses/30/lessons/12914)

피보나치 수열을 재귀함수로 구현할 때 콜 스택(Call Stack)에 굉장히 무리가 가게 되는데, 이는 동일한 계산을 계속 반복하기 때문이다. 때문에 이를 해결하기 위해 메모이제이션(Memoization)이라는 방식을 사용하는데 쉽게 말하면 DP(Dynamic Programming)과 유사하다.

보통 피보나치를 DP로 구할 때 편의를 위해 1번째 값은 1, 2번째 값은 2로 미리 초기화하여 구현하기도 한다.

```js
// DP 활용한 피보나치 수열

// n = 1일 때, 1
// n = 2일 때, 2
// n = 3일 때, 3
function solution(n) {
  const dp = new Array(n + 1).fill(0);
  dp[0] = 1;
  dp[1] = 1;

  for (let i = 2; i <= n; i++) dp[i] = (dp[i - 1] + dp[i - 2]) % 1234567;

  return dp[n];
}
```

```js
function solution(n) {
  if (n === 1) return 1;
  if (n === 2) return 2;
  if (n === 3) return 3;

  let arr = [1, 2, 3];

  for (let i = 3; i < n; i++) {
    arr[i] = arr[i - 1] + (arr[i - 2] % 1234567);
    arr.push(arr[i]);
  }

  return arr[n] % 1234567;
}
```

[귤 고르기](https://school.programmers.co.kr/learn/courses/30/lessons/138476)

각 값의 인덱스

```js
function solution(k, tangerine) {
  let count = 0;
  const countObj = {};

  tangerine.forEach((value) => (countObj[value] = (countObj[value] || 0) + 1));

  Object.values(countObj)
    .sort((a, b) => b - a)
    .map((val) => {
      if (k > 0) {
        k -= val;
        count++;
      }
    });

  return count;
}
```

```js
function solution(k, tangerine) {
  let answer = 0;
  const tDict = {};
  tangerine.forEach((t) => (tDict[t] = (tDict[t] || 0) + 1));
  const tArr = Object.values(tDict).sort((a, b) => b - a);
  for (const t of tArr) {
    answer++;
    if (k > t) k -= t;
    else break;
  }
  return answer;
}
```

[괄호 회전하기](https://school.programmers.co.kr/learn/courses/30/lessons/76502)

```js
function solution(s) {
  if (s.length % 2 === 1) return 0;
  const sLen = s.length;
  let answer = 0;

  for (let i = 0; i < sLen; i++) {
    let str = s.slice(i) + s.slice(0, i);
    const stack = [];
    let flag = 1;
    for (let n of str) {
      if (n === "(" || n === "{" || n === "[") stack.push(n);
      else {
        const bracket = stack.pop();
        if (n === ")" && bracket === "(") continue;
        if (n === "}" && bracket === "{") continue;
        if (n === "]" && bracket === "[") continue;
        flag = 0;
        break;
      }
    }
    if (flag) answer++;
  }
  return answer;
}
```

```js
function solution(s) {
  let answer = 0;
  let sArr = s.split("");

  for (let i = 0; i < sArr.length; i++) {
    sArr.push(sArr.shift());

    if (check(sArr)) {
      answer++;
    }
  }

  return answer;
}

function check(arr) {
  let checkArr = [];
  const obj = {
    "[": "]",
    "(": ")",
    "{": "}",
  };

  for (let i = 0; i < arr.length; i++) {
    if (obj[checkArr[checkArr.length - 1]] === arr[i]) {
      checkArr.pop();
    } else {
      checkArr.push(arr[i]);
    }
  }

  return checkArr.length === 0;
}
```

[H-Index(정렬)](https://school.programmers.co.kr/learn/courses/30/lessons/42747)

존재하는 인용수의 인용수 이상의 개수를 기준으로 잡는것이 아니라 max부터 1씩 내려오면서 검사

```js
function solution(citations) {
  let result = 0;
  const arr = citations.sort((a, b) => b - a);
  for (let i = 1; i <= arr.length; i++) {
    if (arr.filter((val) => val >= i).length >= i) {
      result = i;
    }
  }
  return result;
}
```

```js
const solution = (citations) =>
  citations.sort((a, b) => b - a).filter((el, idx) => el >= idx + 1).length;
```

```js
function solution(citations) {
  citations = citations.sort(sorting);
  let i = 0;
  while (i + 1 <= citations[i]) {
    i++;
  }
  return i;

  function sorting(a, b) {
    return b - a;
  }
}
```

[연속 부분 수열 합의 개수](https://school.programmers.co.kr/learn/courses/30/lessons/131701)

```js
function solution(elements) {
  let answer = [];
  const extendElements = [...elements, ...elements];

  elements.forEach((element, idx) => {
    if (idx < elements.length) {
      for (let i = 0; i < elements.length; i++) {
        const slice = extendElements.slice(i, i + 1 + idx);
        answer.push(slice.reduce((acc, val) => acc + val, 0));
      }
    }
  });

  const set = new Set(answer);
  return [...set].length;
}
```

```js
function solution(elements) {
  const circular = elements.concat(elements);
  const set = new Set();
  for (let i = 0; i < elements.length; i++) {
    let sum = 0;
    for (let j = 0; j < elements.length; j++) {
      sum += circular[i + j];
      set.add(sum);
    }
  }
  return set.size;
}
```

```js
const solution = (elements) => {
  const sumSet = new Set();

  const getSum = (arr) => {
    let sum = 0;
    for (let i = 0; i < arr.length; i++) sum += arr[i];
    return sum;
  };

  const len = elements.length;
  for (let i = 1; i <= len; i++) {
    for (let j = 0; j < len; j++) {
      if (j + i > len) {
        sumSet.add(
          getSum(elements.slice(j, len)) +
            getSum(elements.slice(0, j + i - len))
        );
      } else {
        sumSet.add(getSum(elements.slice(j, j + i)));
      }
    }
  }

  return sumSet.size;
};
```

[n^2 배열 자르기](https://school.programmers.co.kr/learn/courses/30/lessons/87390)

```js
function solution(n, left, right) {
  let answer = [];
  for (var i = left; i <= right; i++)
    answer.push(Math.max(Number.parseInt(i / n), i % n) + 1);
  return answer;
}
```

```js
function solution(n, left, right) {
  if (n === 1)
    if (left >= 1) return [];
    else return [1];

  const answer = [];
  let rowCount = Math.floor(left / n) + 1;

  for (let i = left; i < right + 1; i++) {
    if ((i + 1) % n === 0) {
      answer.push(n);
      rowCount += 1;
      continue;
    }
    if (i % n < rowCount) answer.push(rowCount);
    else answer.push((i + 1) % n);
  }
  return answer;
}
```

[행렬의 곱셈](https://school.programmers.co.kr/learn/courses/30/lessons/12949)

**[a1,b1]** [a1*c1 + b1*c2, a1*d1+b1*d2]

**[a2,b2] x [c1,d1]** = [a2*c1 + b2*c2, a2*d1+b2*d2]

**[a3,b3] [c2,d2]** [a3*c1 + b3*c2, a3*d1+b3*d2]

행렬 곱셈 시, 앞에 행렬의 열의 수와 뒤에 행렬의 행의 수가 같아야하고 최종적으로 나오는 행렬은 앞에 행렬의 행의 수 x 뒤에 행렬의 열의 수

```js
function solution(arr1, arr2) {
  let b = arr1.length;
  let a = arr2[0].length;

  let answer = new Array(b);
  for (let i = 0; i < b; i++) {
    answer[i] = new Array(a).fill(0);
  }

  for (let i = 0; i < b; i++) {
    for (let k = 0; k < arr1[i].length; k++) {
      for (let j = 0; j < a; j++) {
        answer[i][j] += arr1[i][k] * arr2[k][j];
      }
    }
  }

  return answer;
}
```

```js
function solution(arr1, arr2) {
  return arr1.map((row) =>
    arr2[0].map((x, y) => row.reduce((a, b, c) => a + b * arr2[c][y], 0))
  );
}
```

[[1차] 캐시](https://school.programmers.co.kr/learn/courses/30/lessons/17680)

LRU(Least Recently Used) 알고리즘 사용

```
0일때 예외처리
순회하면서 cache 값 계산
cache가 여유있다면 push
캐시가 부족하다면 최신 캐시와 연관된 것이 있다면 그것을 삭제
캐시가 부족하다면 최신 캐시와 연관된 것이 없다면 오래된 것을 삭제
그후 현재 city push
```

```js
function solution(cacheSize, cities) {
  let time = 0;
  let cache = [];
  for (let i = 0; i < cities.length; i++) {
    let city = cities[i].toLowerCase();
    let index = cache.indexOf(city);
    if (index !== -1) {
      //hit
      cache.splice(index, 1);
      cache.push(city);
      time += 1;
    } else {
      //miss
      cache.push(city);
      time += 5;
      if (cacheSize < cache.length) {
        cache.shift();
      }
    }
  }
  return time;
}
```

```js
// 큐 활용
function solution(cacheSize, cities) {
  const MISS = 5,
    HIT = 1;

  if (cacheSize === 0) return MISS * cities.length;

  let answer = 0,
    cache = [];

  cities.forEach((city) => {
    city = city.toUpperCase();

    let idx = cache.indexOf(city);

    if (idx > -1) {
      cache.splice(idx, 1);
      answer += HIT;
    } else {
      if (cache.length >= cacheSize) cache.shift();
      answer += MISS;
    }

    cache.push(city);
  });

  return answer;
}
```

```js
// iterator
function solution(cacheSize, cities) {
  const map = new Map();
  const cacheHit = (city, map) => {
    map.delete(city);
    map.set(city, city);
    return 1;
  };
  const cacheMiss = (city, map, size) => {
    if (size === 0) return 5;
    map.size === size && map.delete(map.keys().next().value);
    map.set(city, city);
    return 5;
  };
  const getTimeCache = (city, map, size) =>
    (map.has(city.toLocaleLowerCase()) ? cacheHit : cacheMiss)(
      city.toLocaleLowerCase(),
      map,
      size
    );
  return cities
    .map((city) => getTimeCache(city.toLocaleLowerCase(), map, cacheSize))
    .reduce((a, c) => a + c, 0);
}
```

[의상 - 해시](https://school.programmers.co.kr/learn/courses/30/lessons/42578)

객체에 {부위명: 부위명에 속한 옷 갯수} 형태로 갯수를 누적시킨다음, 그 갯수들을 + 1 한 값으로 모두 곱한 후 최종 -1 하여 리턴
(해당 부위의 옷 개수 n(착용하는 경우의 수) + 1(착용하지 않는 경우)) \* 옷 타입 개수 만큼 누적 계산(경우의 수 곱의 법칙)... -1(모두 착용하지 않는 경우)

```js
function solution(clothes) {
  let category = {};
  let answer = 1;
  clothes.forEach((v) => (category[v[1]] = (category[v[1]] || 0) + 1));
  for (let v in category) answer *= category[v] + 1;
  return answer - 1;
}
```

```js
function solution(clothes) {
  return (
    Object.values(
      clothes.reduce((obj, t) => {
        obj[t[1]] = obj[t[1]] ? obj[t[1]] + 1 : 1;
        return obj;
      }, {})
    ).reduce((a, b) => a * (b + 1), 1) - 1
  );
}
```

```js
function solution(clothes) {
  let answer = 1;
  const obj = {};
  for (let arr of clothes) {
    obj[arr[1]] = (obj[arr[1]] || 0) + 1;
  }

  for (let key in obj) {
    answer *= obj[key] + 1;
  }

  return answer - 1;
}
```

[튜플](https://school.programmers.co.kr/learn/courses/30/lessons/64065)

```js
function solution(s) {
  let answer = [];
  let a = s.split(",{");
  a.sort((a, b) => a.length - b.length);
  for (let j of a) {
    let numbers = j.match(/\d+/g);
    for (let k of numbers) {
      if (!answer.includes(Number(k))) {
        answer.push(Number(k));
      }
    }
  }
  return answer;
}
```

```js
const tupleFrom = (str) =>
  str
    .slice(2, -2)
    .split("},{")
    .map((it) => toNumbers(it))
    .sort(accendingByLength)
    .reduce(
      (acc, cur) => [...acc, ...cur.filter((it) => !acc.includes(it))],
      []
    );

const toNumbers = (str) => str.split(",").map((it) => Number(it));

const accendingByLength = (arr1, arr2) => arr1.length - arr2.length;

const solution = (s) => tupleFrom(s);
```

```js
const solution = (s) =>
  s
    .match(/(\d+,)*\d+/g)
    .map((s) => s.split(",").map((n) => +n))
    .sort((a, b) => a.length - b.length)
    .reduce((a, s) => [...a, ...s.filter((n) => a.indexOf(n) === -1)], []);
```

```js
// Set 활용
const solution = (s) => tupple(changeMatrix(getSets(s)));

const getSets = (s) => {
  const sets = s.match(/{[\d,]+}/g);
  return sets
    .map((set) => set.match(/[\d]+,?/g).map((v) => parseInt(v)))
    .sort((a, b) => a.length - b.length);
};

const changeMatrix = (sets) => sets.reduce((_, set) => _.concat(set), []);

const tupple = (arr) => [
  ...arr.reduce((set, value) => set.add(value), new Set()),
];
```

[할인 행사](https://school.programmers.co.kr/learn/courses/30/lessons/131127)

```js
function solution(want, number, discount) {
  const dict = {};
  let answer = 0;

  for (let i = 0; i < want.length; i++) dict[want[i]] = number[i];

  for (let i = 0; i <= discount.length - 10; i++) {
    if (!want.includes(discount[i])) continue;
    const tmp = discount.slice(i, i + 10);
    if (tmp.filter((el) => !want.includes(el)).length) continue;
    let check = true;
    for (let i = 0; i < want.length; i++) {
      if (dict[want[i]] !== tmp.filter((el) => el === want[i]).length) {
        check = false;
        break;
      }
    }
    if (check) answer++;
  }
  return answer;
}
```

```js
function solution(want, number, discount) {
  let count = 0;
  for (let i = 0; i < discount.length - 9; i++) {
    const slice = discount.slice(i, i + 10);

    let flag = true;
    for (let j = 0; j < want.length; j++) {
      if (slice.filter((item) => item === want[j]).length !== number[j]) {
        flag = false;
        break;
      }
    }
    if (flag) count += 1;
  }
  return count;
}
```

```js
function solution(want, number, discount) {
  var answer = 0;
  const info = {};
  for (let i = 0; i < number.length; i++) {
    info[`${want[i]}`] = info[`${want[i]}`] || number[i];
  }
  for (let i = 0; i <= discount.length - 10; i++) {
    let flag = true;
    for (let w of want) {
      if (
        discount.slice(i, i + 10).filter((x) => x === w).length < info[`${w}`]
      ) {
        flag = false;
        break;
      }
    }
    if (flag) answer++;
  }
  return answer;
}
```

[기능 개발 - 스택/큐](https://school.programmers.co.kr/learn/courses/30/lessons/42586)

```js
function solution(progresses, speeds) {
  const newArr = new Array(100).fill(0);
  let answer = -1;
  for (let i = 0; i < progresses.length; i++) {
    while (progresses[i] + answer * speeds[i] < 100) {
      answer++;
    }
    newArr[answer]++;
  }
  return newArr.filter((i) => i !== 0);
}
```

```js
function solution(progresses, speeds) {
  const q = [];
  const answerList = [];

  for (let i = 0; i < speeds.length; i++) {
    const remain = (100 - progresses[i]) / speeds[i];
    const date = Math.ceil(remain);

    if (q.length > 0 && q[0] < date) {
      answerList.push(q.length);
      q.length = 0;
    }

    q.push(date);
  }

  answerList.push(q.length);

  return answerList;
}
```

```js
function solution(progresses, speeds) {
  let answer = [0];
  let days = progresses.map((progress, index) =>
    Math.ceil((100 - progress) / speeds[index])
  );
  let maxDay = days[0];

  for (let i = 0, j = 0; i < days.length; i++) {
    if (days[i] <= maxDay) {
      answer[j] += 1;
    } else {
      maxDay = days[i];
      answer[++j] = 1;
    }
  }

  return answer;
}
```

[프로세스 - 스택/큐](https://school.programmers.co.kr/learn/courses/30/lessons/42587)

```js
function solution(priorities, location) {
  const queue = [];
  priorities = priorities.map((priority, index) => [priority, index]);

  while (priorities.length) {
    const [priority, index] = priorities.shift();
    const higherImportance = priorities.findIndex(
      ([value, _]) => value > priority
    );

    if (higherImportance === -1) {
      if (index === location) return queue.length + 1;
      queue.push([priority, index]);
      continue;
    }

    priorities.push([priority, index]);
  }
}
```

```js
function solution(priorities, location) {
  var list = priorities.map((t, i) => ({
    my: i === location,
    val: t,
  }));
  var count = 0;
  while (true) {
    var cur = list.splice(0, 1)[0];
    if (list.some((t) => t.val > cur.val)) {
      list.push(cur);
    } else {
      count++;
      if (cur.my) return count;
    }
  }
}
```

```js
function solution(priorities, location) {
  var arr = priorities.map((priority, index) => {
    return {
      index: index,
      priority: priority,
    };
  });

  var queue = [];

  while (arr.length > 0) {
    var firstEle = arr.shift();
    var hasHighPriority = arr.some((ele) => ele.priority > firstEle.priority);
    if (hasHighPriority) {
      arr.push(firstEle);
    } else {
      queue.push(firstEle);
    }
  }

  return queue.findIndex((queueEle) => queueEle.index === location) + 1;
}
```

[뉴스 클러스터링 - 카카오 블라인드 인크루트](https://school.programmers.co.kr/learn/courses/30/lessons/17677)

자카드 유사도

```js
function solution(str1, str2) {
  let str1Arr = [],
    str2Arr = [];

  for (let i = 0; i < str1.length - 1; i++) {
    const word = str1.slice(i, i + 2).toLowerCase();
    if (/^[A-Za-z]+$/.test(word)) str1Arr.push(word);
  }

  for (let i = 0; i < str2.length - 1; i++) {
    const word = str2.slice(i, i + 2).toLowerCase();
    if (/^[A-Za-z]+$/.test(word)) str2Arr.push(word);
  }

  if (!str1Arr.length && !str2Arr.length) return 65536;

  let intersection = [];
  let union = str1Arr.concat(str2Arr);

  str1Arr.forEach((a) => {
    let i = str2Arr.findIndex((e) => e === a);
    if (i >= 0) {
      intersection.push(a);
      str2Arr.splice(i, 1);
    }
  });

  intersection.forEach((a) => {
    let i = union.findIndex((e) => e === a);
    union.splice(i, 1);
  });

  if (intersection.length === 0 && union.length > 0) return 0;

  return Math.trunc((intersection.length / union.length) * 65536);
}
```

```js
function solution(str1, str2) {
  function explode(text) {
    const result = [];
    for (let i = 0; i < text.length - 1; i++) {
      const node = text.substr(i, 2);
      if (node.match(/[A-Za-z]{2}/)) {
        result.push(node.toLowerCase());
      }
    }
    return result;
  }

  const arr1 = explode(str1);
  const arr2 = explode(str2);
  const set = new Set([...arr1, ...arr2]);
  let union = 0;
  let intersection = 0;

  set.forEach((item) => {
    const has1 = arr1.filter((x) => x === item).length;
    const has2 = arr2.filter((x) => x === item).length;
    union += Math.max(has1, has2);
    intersection += Math.min(has1, has2);
  });
  return union === 0 ? 65536 : Math.floor((intersection / union) * 65536);
}
```

[피로도 - 완전 탐색](https://school.programmers.co.kr/learn/courses/30/lessons/87946)

```js
let solution = (k, d) =>
  Math.max(
    ...d.map(([m, v], i) =>
      k < m ? 0 : solution(k - v, [...d.slice(0, i), ...d.slice(i + 1)]) + 1
    ),
    0
  );
```

- 순열 활용

: 순열을 만들어서 최대한 공량 가능한 모든 조합

```js
function getPermutations(arr) {
  if (arr.length === 0) return [[]];
  const first = arr[0];
  const rest = arr.slice(1);
  const subPermutations = getPermutations(rest);
  const allPermutations = [];
  for (const subPermutation of subPermutations) {
    for (let i = 0; i <= subPermutation.length; i++) {
      const copy = subPermutation.slice();
      copy.splice(i, 0, first);
      allPermutations.push(copy);
    }
  }
  return allPermutations;
}

function solution(k, dungeons) {
  const permutations = getPermutations(dungeons);
  let answer = 0;

  for (const permute of permutations) {
    let hp = k; // 최대피로도
    let count = 0; // 돌 수 있는 던전의 수

    for (const pm of permute) {
      // 던전을 1개씩 돌면서 피로도가 가능한지 확인

      if (hp >= pm[0]) {
        // hp가 최소피로도 pm[0]보다 크다면 진행시키고 던전을 돈 것이므로 count++
        hp -= pm[1];
        count += 1;
      }

      if (count > answer) {
        // answer보다 크다면 가장 큰 값을 answer에 저장
        answer = count;
      }
    }
  }

  return answer;
}
```

```js
function solution(k, dungeons) {
  const answer = [];
  function permute(nums) {
    const result = [];

    function backtrack(arr, current) {
      // 순열 완성
      if (current.length === arr.length) {
        result.push(current.slice()); // 현재 순열을 결과 배열에 추가
        return;
      }

      for (let i = 0; i < arr.length; i++) {
        if (current.includes(arr[i])) {
          continue; // 이미 선택된 숫자인 경우 건너뜀
        }

        current.push(arr[i]); // 숫자 선택
        backtrack(nums, current); // 재귀 호출
        current.pop(); // 선택한 숫자 제거 (백트래킹)
      }
    }

    backtrack(nums, []);
    return result;
  }

  // 모든 경우의수를 탐색하기 위해 순열을 생성하는 함수
  const permutations = permute(dungeons);

  // 모든 경우의 수 마다 던전을 몇개 돌 수 있나 계산해서 answer에 push
  permutations.forEach((item) => {
    // 순열 하나하나 처리할때마다 피로도 초기화
    let p = k;

    let pushItem = item.reduce((prev, curr) => {
      if (curr[0] > p)
        return prev; // 피로도가 최소필요피로도보다 작다면 던전을 돌 수 없음
      else {
        // 던전을 돌 수 있음
        p -= curr[1]; // 피로도 빼주고
        return (prev += 1); // 던전 한개 돌았다 표시
      }
    }, 0);

    answer.push(pushItem); // 최종적으로 던전 돈 수 push
  });

  // 모든 경우의 수 중 가장 큰 수 (최대 많이 돌 수 있는 경우의 수)
  return Math.max(...answer);
}
```

- DFS(깊이 우선 탐색) 활용

: 한 번에 탐색할 수 있는 DFS의 끝단까지 탐색을 완료한 후 다시 이전 단계로 돌아가는 작업을 해줘야 한다. 이 과정에서 이전 노드로 돌아갈 때, 방문 여부(check 배열)와 방문 횟수(cnt)를 이전 노드까지 탐색했을 때의 값으로 복구해줘야 한다.

```js
let answer = 0;

function DFS(k, cnt, dungeons, check) {
  answer = Math.max(answer, cnt);

  for (let i = 0; i < dungeons.length; i++) {
    if (check[i] === 0 && k >= dungeons[i][0]) {
      check[i] = 1;
      DFS(k - dungeons[i][1], cnt + 1, dungeons, check);
      check[i] = 0;
    }
  }
}

function solution(k, dungeons) {
  let check = new Array(dungeons.length).fill(0);
  DFS(k, 0, dungeons, check);
  return answer;
}
```

```js
function solution(k, d) {
  const N = d.length;
  const visited = new Array(N).fill(0);
  let ans = 0;

  function dfs(k, cnt) {
    ans = Math.max(cnt, ans);

    for (let j = 0; j < N; j++) {
      if (k >= d[j][0] && !visited[j]) {
        visited[j] = 1;
        dfs(k - d[j][1], cnt + 1);
        visited[j] = 0;
      }
    }
  }

  dfs(k, 0);
  return ans;
}
```

[타겟 넘버 - 깊이/너비 우선 탐색(DFS/BFS)](https://school.programmers.co.kr/learn/courses/30/lessons/43165)

```js
function solution(numbers, target) {
  let answer = 0;
  getAnswer(0, 0);
  function getAnswer(x, value) {
    if (x < numbers.length) {
      getAnswer(x + 1, value + numbers[x]);
      getAnswer(x + 1, value - numbers[x]);
    } else {
      if (value === target) {
        answer++;
      }
    }
  }
  return answer;
}
```

```js
function solution(numbers, target) {
  var answer = 0;
  var answer = 0;

  let root = new BinarySearchTree();
  root.insert(0);
  numbers.forEach(function (val) {
    root.insert(val);
  });

  answer = root.DFSPreOrder(target);
  return answer;
}

class Node {
  constructor(value) {
    this.value = value;
    this.left = null;
    this.right = null;
  }
}
class BinarySearchTree {
  constructor() {
    this.root = null;
  }
  insert(value) {
    let newNode = new Node(value);
    if (this.root === null) {
      this.root = newNode;
      return this;
    } else {
      let current = this.root;
      function traverse(node) {
        if (node.left) traverse(node.left);
        if (node.right) traverse(node.right);
        if (node.left === null) {
          let leftNode = new Node(-value);
          let rightNode = new Node(value);
          node.left = leftNode;
          node.right = rightNode;
        }
      }
      traverse(current);
      return this;
    }
  }
  DFSPreOrder(target) {
    let count = 0;
    let data = 0;
    let current = this.root;
    function traverse(node) {
      data = data + node.value;
      if (node.left) traverse(node.left);
      if (node.right) traverse(node.right);
      if (node.left === null) {
        if (data === target) {
          count++;
        }
      }
      data = data - node.value;
    }
    traverse(current);
    return count;
  }
}
```

```js
function solution(numbers, target) {
  const n = 2 ** numbers.length;
  let cnt = 0;
  for (let i = 0; i < n; i++) {
    let sum = 0;
    for (let j = 1; j <= numbers.length; j++) {
      const sign = i % (n / 2 ** (j - 1)) < n / 2 ** j ? +1 : -1;
      sum += sign * numbers[j - 1];
    }
    if (target === sum) cnt++;
  }

  return cnt;
}
```

```js
function solution(numbers, target) {
  // 값을 더하고 빼는 과정에서 음수가 나올수 있으므로
  // 숫자의 최대 제한사항인  50 * 20 을 기준으로 dp테이블을 잡는다.
  // dp[i][j] == numbers[i]까지 계산한 후 j를 만드는 방법의 수
  const dp = Array.from({ length: numbers.length }, () => Array(2001).fill(0));

  // 초기테이블 세팅
  // ex) 만약 첫 요소가 2일 경우  -2을 만드는 가지수와 +2를 만드는 가지수를 dp테이블에 추가
  // ex) dp[0][998] =1 , dp[0][1002] = 1

  dp[0][1000 - numbers[0]] = 1;
  dp[0][1000 + numbers[0]] = 1;
  for (let i = 1; i < numbers.length; i++) {
    for (let j = 0; j < 2001; j++) {
      // 요소를 계산하기전 , 해당 수를 만드는 가지수가 있는 경우
      // 가지수를 해당 수와 현재 요소를 계산한 경우에 더해준다.
      if (dp[i - 1][j] > 0) {
        dp[i][j - numbers[i]] += dp[i - 1][j];
        dp[i][j + numbers[i]] += dp[i - 1][j];
      }
    }
  }
  // 모든 요소를 계산하고  타겟이 만들어진 갯수를 리턴한다.
  return dp[numbers.length - 1][1000 + target];
}
```

[전화번호 목록 - 해시](https://school.programmers.co.kr/learn/courses/30/lessons/42577)

```js
function solution(phoneBook) {
  const table = {};

  for (const number of phoneBook) {
    table[number] = true;
  }

  for (const number of phoneBook) {
    for (let i = 1; i < number.length; i += 1) {
      const prefix = number.slice(0, i);
      if (table[prefix]) return false;
    }
  }

  return true;
}
```

```js
// 효율성 더 좋음
function solution(phoneBook) {
  return !phoneBook.sort().some((t, i) => {
    if (i === phoneBook.length - 1) return false;

    return phoneBook[i + 1].startsWith(phoneBook[i]);
  });
}
```

[k진수에서 소수 개수 구하기](https://school.programmers.co.kr/learn/courses/30/lessons/92335)

```js
function solution(n, k) {
  let answer = 0;
  const strArr = n
    .toString(k)
    .split("0")
    .filter((el) => el > 1);

  strArr.forEach((num) => {
    let isDivide = false;
    for (let i = 2; i <= Math.sqrt(num); i++) {
      if (num % i === 0) {
        isDivide = true;
        break;
      }
    }

    answer += isDivide ? 0 : 1;
  });

  return answer;
}
```

```js
function isPrime(num) {
  if (!num || num === 1) return false;
  for (let i = 2; i <= +Math.sqrt(num); i++) {
    if (num % i === 0) return false;
  }
  return true;
}

function solution(n, k) {
  const candidates = n.toString(k).split("0");
  return candidates.filter((v) => isPrime(+v)).length;
}

console.log(solution(930909, 10));
```

```js
function solution(n, k) {
  let numbers = n
    .toString(k)
    .split("0")
    .filter((a) => a > 1);

  let answer = 0;
  for (const number of numbers) {
    answer++;
    if (number > 3) {
      for (let i = 2; i * i <= number; i++) {
        if (number % i == 0) {
          answer--;
          break;
        }
      }
    }
  }
  return answer;
}
```

[압축](https://school.programmers.co.kr/learn/courses/30/lessons/17684)

```js
function solution(msg) {
  const answer = [];

  const dictionary = {};
  let wordIdx = 1;
  for (let i = 65; i < 65 + 26; i++) {
    dictionary[String.fromCharCode(i)] = wordIdx++;
  }

  const msgs = msg.split("");
  let msgsIdx = 0;
  let msgString = "";

  while (msgs.length !== msgsIdx) {
    msgString = msgString.concat(msgs[msgsIdx]);

    if (dictionary[msgString]) {
      msgsIdx++;
    } else {
      answer.push(dictionary[msgString.slice(0, msgString.length - 1)]);
      dictionary[msgString] = wordIdx++;
      msgString = "";
    }
    if (msgs.length === msgsIdx) {
      answer.push(dictionary[msgString]);
    }
  }
  return answer;
}
```

```js
function solution(msg) {
  const dict = "ABCDEFGHIJKLMNOPQRSTUVWXYZ".split("").reduce((dict, c, i) => {
    dict[c] = i + 1;
    return dict;
  }, {});
  dict.nextId = 27;
  const ans = [];
  for (let i = 0, j = 0; i < msg.length; i = j) {
    j = msg.length;
    let longest = "";
    while (dict[(longest = msg.substring(i, j))] === undefined) --j;
    ans.push(dict[longest]);
    dict[longest + msg[j]] = dict.nextId++;
  }

  return ans;
}
```

```js
function solution(msg) {
  let answer = [];
  let wordMap = new Map();
  let idx = 27;

  for (let i = 1; i <= 26; i++) {
    const code = i + 64;

    wordMap[String.fromCharCode(code)] = i;
  }

  while (msg !== "") {
    for (let i = msg.length; i >= 0; i--) {
      const w = msg.slice(0, i);
      const wc = msg.slice(0, i + 1);

      if (wordMap[w]) {
        answer.push(wordMap[w]);

        msg = msg.slice(i, msg.length);

        wordMap[wc] = idx;

        idx++;
        break;
      }
    }
  }
  return answer;
}
```

[게임 맵 최단거리 - DFS/BFS](https://school.programmers.co.kr/learn/courses/30/lessons/1844)

```js
function solution(maps) {
  let q = [[0, 0]];
  let dx = [-1, 1, 0, 0];
  let dy = [0, 0, -1, 1];

  while (q.length) {
    let [x, y] = q.shift();
    for (let i = 0; i < 4; i += 1) {
      let nx = x + dx[i];
      let ny = y + dy[i];

      if (nx < 0 || ny < 0 || nx >= maps.length || ny >= maps[0].length)
        continue;
      if (maps[nx][ny] === 0 || (nx === 0 && ny === 0)) continue;
      if (maps[nx][ny] === 1) {
        maps[nx][ny] = maps[x][y] + 1;
        q.push([nx, ny]);
      }
    }
  }
  return maps[maps.length - 1][maps[0].length - 1] === 1
    ? -1
    : maps[maps.length - 1][maps[0].length - 1];
}
```

[N진수 게임](https://school.programmers.co.kr/learn/courses/30/lessons/17687)

```js
function solution(n, t, m, p) {
  let str = "";
  let number = 0;

  while (t * m > str.length) {
    str = str.concat(number.toString(n));
    number++;
  }

  return str
    .slice(0, t * m)
    .split("")
    .filter((num, idx) => (idx - p + 1) % m === 0)
    .join("")
    .toUpperCase();
}
```

```js
function solution(n, t, m, p) {
  var tubeT = Array.apply(null, Array(t)).map((a, i) => i * m + p - 1);
  var line = "";
  var max = m * t + p;
  for (var i = 0; line.length <= max; i++) {
    line += i.toString(n);
  }
  return tubeT
    .map((a) => line[a])
    .join("")
    .toUpperCase();
}
```

```js
function solution(n, t, m, p) {
  let res = "";
  let num = 0;
  let seq = "";
  while (res.length < t) {
    if (seq.length >= m) {
      res += seq[p - 1];
      seq = seq.slice(m);
    } else {
      seq += num.toString(n).toUpperCase();
      num++;
    }
  }
  return res;
}
```

[더 맵게 - 힙(Heap)](https://school.programmers.co.kr/learn/courses/30/lessons/42626)

```js
class MinHeap {
  constructor() {
    this.heap = [];
  }

  size() {
    return this.heap.length;
  }

  swap(idx1, idx2) {
    [this.heap[idx1], this.heap[idx2]] = [this.heap[idx2], this.heap[idx1]];
    return this.heap;
  }

  getParentIdx(childIdx) {
    return Math.floor((childIdx - 1) / 2);
  }

  getLeftChildIdx(parentIdx) {
    return parentIdx * 2 + 1;
  }

  getRightChildIdx(parentIdx) {
    return parentIdx * 2 + 2;
  }

  heappop() {
    const heapSize = this.size();
    if (!heapSize) return null;
    if (heapSize === 1) return this.heap.pop();

    const value = this.heap[0];
    this.heap[0] = this.heap.pop();
    this.bubbledown();
    return value;
  }

  heappush(value) {
    this.heap.push(value);
    this.bubbleup();

    return this.heap;
  }

  bubbleup() {
    let child = this.size() - 1;
    let parent = this.getParentIdx(child);

    while (this.heap[child] < this.heap[parent]) {
      this.swap(child, parent);
      child = parent;
      parent = this.getParentIdx(parent);
    }
  }

  bubbledown() {
    let parent = 0;
    let leftChild = this.getLeftChildIdx(parent);
    let rightChild = this.getRightChildIdx(parent);

    while (
      (leftChild <= this.size() - 1 &&
        this.heap[leftChild] < this.heap[parent]) ||
      (rightChild <= this.size() - 1 &&
        this.heap[rightChild] < this.heap[parent])
    ) {
      if (
        rightChild <= this.size() - 1 &&
        this.heap[leftChild] > this.heap[rightChild]
      ) {
        this.swap(parent, rightChild);
        parent = rightChild;
      } else {
        this.swap(parent, leftChild);
        parent = leftChild;
      }
      leftChild = this.getLeftChildIdx(parent);
      rightChild = this.getRightChildIdx(parent);
    }
  }
}

function solution(scoville, K) {
  let count = 0;
  const heap = new MinHeap();
  scoville.forEach((el) => heap.heappush(el));

  while (heap.heap[0] < K && heap.size() > 1) {
    count++;
    heap.heappush(heap.heappop() + heap.heappop() * 2);
  }
  return heap.heap[0] >= K ? count : -1;
}
```

```js
class Heap {
  constructor() {
    this.items = [];
  }

  swap(index1, index2) {
    [this.items[index1], this.items[index2]] = [
      this.items[index2],
      this.items[index1],
    ];
  }

  insert(val) {
    this.items.push(val);
    let index = this.items.length - 1;
    while (true) {
      let parentIndex = Math.floor((index - 1) / 2);
      //부모보다 자식이 작으면 자리 바꾸기
      if (this.items[index] < this.items[parentIndex]) {
        this.swap(index, parentIndex);
      } else break;
      index = parentIndex;
      if (index < 1) break;
    }
  }

  removeMin() {
    this.items[0] = this.items[this.items.length - 1];
    this.items.pop();
    if (this.items.length <= 1) return;

    let index = 0;
    while (true) {
      //두 자식중 작은값의 자식 인덱스 찾기
      let lChildIndex = index * 2 + 1;
      let rChildIndex = index * 2 + 2;
      let minIndex = index;
      if (
        lChildIndex < this.items.length &&
        this.items[minIndex] > this.items[lChildIndex]
      ) {
        minIndex = lChildIndex;
      }
      if (
        rChildIndex < this.items.length &&
        this.items[minIndex] > this.items[rChildIndex]
      ) {
        minIndex = rChildIndex;
      }
      //위치 바꾸기
      if (minIndex !== index) {
        this.swap(index, minIndex);
        index = minIndex;
      } else break;
    }
  }
}

function solution(scoville, K) {
  let answer = 0;

  //힙생성과 scoville 힙에 저장
  let scovilleHeap = new Heap();
  scoville.forEach((el) => {
    scovilleHeap.insert(el);
  });

  //스코빌 지수 설정
  while (true) {
    if (scovilleHeap.items[0] >= K) break;
    if (scovilleHeap.items.length <= 1) {
      answer = -1;
      break;
    }

    const low1 = scovilleHeap.items[0];
    scovilleHeap.removeMin();
    const low2 = scovilleHeap.items[0];
    scovilleHeap.removeMin();
    scovilleHeap.insert(low1 + low2 * 2);

    answer++;
  }

  return answer;
}
```
