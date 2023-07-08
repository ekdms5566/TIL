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
