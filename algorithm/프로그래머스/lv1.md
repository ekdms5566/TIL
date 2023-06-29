[히샤드 수](https://school.programmers.co.kr/learn/courses/30/lessons/12947)

```js
function solution(x) {
  return x %
    x
      .toString()
      .split("")
      .reduce((v, i) => ~~v + ~~i) ===
    0
    ? true
    : false;
}
```

```js
// 숫자로 접근하는 것이 실행 속도는 더 빠르다.
// 속도 우선 방법
function solution(x) {
  let num = x;
  let sum = 0;
  do {
    sum += x % 10;
    x = Math.floor(x / 10);
  } while (x > 0);

  return !(num % sum);
}
```

[콜라 문제](https://school.programmers.co.kr/learn/courses/30/lessons/132267)

```js
function solution(a, b, n) {
  let num = 0,
    now = 0,
    inp = n;

  while (inp >= a) {
    now = parseInt(inp / a) * b;
    num += now;
    inp = now + (inp % a);
  }
  return num;
}
```

```js
// ?
solution = (a, b, n) => Math.floor(Math.max(n - b, 0) / (a - b)) * b;
```

```js
function solution(a, b, n) {
  let answer = 0;
  while (n >= a) {
    answer += parseInt(n / a) * b;
    n = parseInt(n / a) * b + (n % a);
  }
  return answer;
}
```

[가장 가까운 같은 글자](https://school.programmers.co.kr/learn/courses/30/lessons/142086)

```js
const solution = (s) =>
  [...s].map((char, i) => {
    const count = s.slice(0, i).lastIndexOf(char);
    return count < 0 ? count : i - count;
  });
```

```js
function solution(s) {
  const hash = {};

  return [...s].map((v, i) => {
    let result = hash[v] !== undefined ? i - hash[v] : -1;
    hash[v] = i;
    return result;
  });
}
```

[다트 게임](https://school.programmers.co.kr/learn/courses/30/lessons/17682)

```js
function solution(dartResult) {
  let answer = [];
  let result = 0;
  let temp = 0;

  for (let i = 0; i < dartResult.length; i++) {
    if (dartResult[i] >= 0 && dartResult[i] <= 9) {
      if (dartResult[i] == 1 && dartResult[i + 1] == 0) {
        temp = 10;
        // continue
        i++;
      } else {
        temp = parseInt(dartResult[i]);
      }
    } else if (dartResult[i] == "S") {
      answer.push(temp);
    } else if (dartResult[i] == "D") {
      answer.push(temp ** 2);
    } else if (dartResult[i] == "T") {
      answer.push(temp ** 3);
    } else if (dartResult[i] == "*") {
      answer[answer.length - 1] *= 2;
      answer[answer.length - 2] *= 2;
    } else if (dartResult[i] == "#") {
      answer[answer.length - 1] *= -1;
    }
  }
  for (const value of answer) {
    result += value;
  }
  return result;
}
```

```js
function solution(dartResult) {
  const bonus = { S: 1, D: 2, T: 3 },
    options = { "*": 2, "#": -1, undefined: 1 };

  let darts = dartResult.match(/\d.?\D/g);

  for (let i = 0; i < darts.length; i++) {
    let split = darts[i].match(/(^\d{1,})(S|D|T)(\*|#)?/),
      score = Math.pow(split[1], bonus[split[2]]) * options[split[3]];

    if (split[3] === "*" && darts[i - 1]) darts[i - 1] *= options["*"];

    darts[i] = score;
  }

  return darts.reduce((a, b) => a + b);
}
```

[소수 찾기](https://school.programmers.co.kr/learn/courses/30/lessons/12921)

```js
function solution(n, count = 0) {
  Array(n)
    .fill(0)
    .map((_, i) => i + 1)
    .forEach((i) => {
      let num = 0;
      for (let j = 1; j <= i; j++) {
        if (i % j === 0) num++;
        if (num === 2 && i === j) count++;
      }
    });
  return count;
}
```

```js
function solution(n, count = 0) {
  let list = Array(n - 1)
    .fill(2)
    .map((v, i) => v + i);
  console.log(list);
  for (let i = 2; i <= n; i++) {
    if (list[i] === 0) continue;
    for (let j = i + i; j <= n; j += i) {
      list[j] = 0;
    }
  }
  for (let i = 2; i <= n; i++) {
    if (list[i] !== 0) count++;
  }
  return count;
}
```
