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
