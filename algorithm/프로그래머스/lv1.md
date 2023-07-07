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

[3진법 뒤집기](https://school.programmers.co.kr/learn/courses/30/lessons/68935)

```js
function solution(n) {
  return parseInt([...n.toString(3)].reverse().join(""), 3);
}
```

[K번째 수](https://school.programmers.co.kr/learn/courses/30/lessons/42748)

```js
function solution(array, commands) {
  return commands.map(
    (i) => array.slice(i[0] - 1, i[1]).sort((a, b) => a - b)[i[2] - 1]
  );
}
```

```js
function solution(array, commands) {
  return commands.map((command) => {
    const [sPosition, ePosition, position] = command;
    const newArray = array
      .filter(
        (value, fIndex) => fIndex >= sPosition - 1 && fIndex <= ePosition - 1
      )
      .sort((a, b) => a - b);

    return newArray[position - 1];
  });
}
```

[비밀지도](https://school.programmers.co.kr/learn/courses/30/lessons/17681)

```js
function solution(n, arr1, arr2) {
  answer = [];
  const zip = (a, b) => a.map((v, i) => [v, b[i]]);
  for ([i, j] of zip(arr1, arr2)) {
    answer.push(
      (i | j).toString(2).padStart(n, "0").replace(/1/g, "#").replace(/0/g, " ")
    );
  }
  return answer;
}
```

```js
function solution(n, arr1, arr2) {
  answer = [];
  for (let i = 0; i < n; i++) {
    answer.push(
      (arr1[i] | arr2[i])
        .toString(2)
        .padStart(n, "0")
        .replace(/1/g, "#")
        .replace(/0/g, " ")
    );
  }
  return answer;
}
```

```js
var solution = (n, a, b) =>
  a.map((a, i) =>
    (a | b[i]).toString(2).padStart(n, 0).replace(/0/g, " ").replace(/1/g, "#")
  );
```

```js
function solution(n, arr1, arr2) {
  return arr1.map((v, i) =>
    addZero(n, (v | arr2[i]).toString(2)).replace(/1|0/g, (a) =>
      +a ? "#" : " "
    )
  );
}

const addZero = (n, s) => {
  return "0".repeat(n - s.length) + s;
};
```

[x만큼 간격이 있는 n개의 숫자](https://school.programmers.co.kr/learn/courses/30/lessons/12954)

```js
function solution(x, n) {
  return Array(n)
    .fill(x)
    .map((_, i) => x * i + x);
}
```

```js
function solution(x, n) {
  return [...Array(n).keys()].map((v) => (v + 1) * x);
}
```

```js
function solution(x, n) {
  return Array(n)
    .fill(x)
    .map((v, i) => (i + 1) * v);
}
```

[가운데 글자 가져오기](https://school.programmers.co.kr/learn/courses/30/lessons/12903)\

```js
function solution(s) {
  return s.length % 2 === 0
    ? s.slice(Math.floor(s.length / 2 - 1), Math.floor(s.length / 2 + 1))
    : s.slice(Math.floor(s.length / 2), Math.floor(s.length / 2 + 1));
}
```

```js
function solution(s) {
  return s.substr(Math.ceil(s.length / 2) - 1, s.length % 2 === 0 ? 2 : 1);
}
```

```js
function solution(s) {
  const mid = Math.floor(s.length / 2);
  return s.length % 2 === 1 ? s[mid] : s[mid - 1] + s[mid];
}
```

[제일 작은 수 제거하기](https://school.programmers.co.kr/learn/courses/30/lessons/12935)

```js
function solution(arr) {
  return arr.length === 1 ? [-1] : arr.filter((v) => v > Math.min(...arr));
}
```

```js
function solution(arr) {
  arr.splice(arr.indexOf(Math.min(...arr)), 1);
  if (arr.length < 1) return [-1];
  return arr;
}
```

[같은 숫자는 싫어](https://school.programmers.co.kr/learn/courses/30/lessons/12906)

```js
function solution(arr) {
  return arr.filter((v, i) => arr[i] !== arr[i + 1]);
}
```

```js
function solution(arr) {
  return arr.filter((val, index) => val != arr[index + 1]);
}
```

```js
let solution = (_) => _.filter((i, $) => i != _[--$]);
```

[나누어 떨어지는 숫자 배열](https://school.programmers.co.kr/learn/courses/30/lessons/12910)

```js
function solution(arr, divisor) {
  let list = arr.filter((i) => i % divisor === 0).sort((a, b) => a - b);
  return list.length !== 0 ? list : [-1];
}
```

```js
function solution(arr, divisor) {
  var answer = arr.filter((v) => v % divisor == 0);
  return answer.length == 0 ? [-1] : answer.sort((a, b) => a - b);
}
```

[나머지가 1이 되는 수 찾기](https://school.programmers.co.kr/learn/courses/30/lessons/87389)

```js
function solution(n) {
  for (let i = 0; i <= n; i++) {
    if (n % i === 1) {
      return i;
    }
  }
}
```

```js
function solution(n, x = 1) {
  while (x++) {
    if (n % x === 1) {
      return x;
    }
  }
}
```

[내적](https://school.programmers.co.kr/learn/courses/30/lessons/70128)

```js
function solution(a, b) {
  for (let i = 0; i < a.length; i++) {
    a[i] *= b[i];
  }
  return a.reduce((a, c) => a + c);
}
```

```js
function solution(a, b) {
  return a.reduce((acc, _, i) => (acc += a[i] * b[i]), 0);
}
```

[두 개 뽑아서 더하기](https://school.programmers.co.kr/learn/courses/30/lessons/68644)

```js
function solution(numbers) {
  const list = [];
  for (let i = 0; i < numbers.length; i++) {
    for (let j = i + 1; j < numbers.length; j++) {
      list.push(numbers[i] + numbers[j]);
    }
  }
  return [...new Set(list)].sort((a, b) => a - b);
}
```

```js
function solution(numbers) {
  var answer = [];
  numbers = numbers.sort();
  console.log(numbers);
  for (var i = 0; i < numbers.length; i++) {
    for (var k = i + 1; k < numbers.length; k++) {
      if (!answer.includes(numbers[i] + numbers[k])) {
        answer.push(numbers[i] + numbers[k]);
      }
    }
  }
  answer = answer.sort(function (a, b) {
    return a - b;
  });
  return answer;
}
```

[두 정수 사이의 합](https://school.programmers.co.kr/learn/courses/30/lessons/12912)

```js
function solution(a, b) {
  let num = 0;
  for (let i = Math.min(a, b); i <= Math.max(a, b); i++) {
    num += i;
  }
  return num;
}
```

```js
// 양 끝의 합 * 양 끝의 합의 개수
// 등차수열의 합 공식과 동일
function adder(a, b) {
  return ((a + b) * (Math.abs(b - a) + 1)) / 2;
}
```

[문자열 내 p와 y의 개수](https://school.programmers.co.kr/learn/courses/30/lessons/12916)

```js
function solution(s) {
  let p = 0,
    y = 0;
  s.toLowerCase()
    .split("")
    .map((i) => (i === "p" ? p++ : i === "y" ? y++ : true));
  return p === y ? true : false;
}
```

```js
function numPY(s) {
  return (
    s.toUpperCase().split("P").length === s.toUpperCase().split("Y").length
  );
}
```

```js
function solution(s) {
  return [...s.toLowerCase()].reduce((acc, cur) => {
    if (cur === "p") return acc + 1;
    else if (cur === "y") return acc - 1;
    return acc;
  }, 0)
    ? false
    : true;
}
```

[삼총사](https://school.programmers.co.kr/learn/courses/30/lessons/131705)

```js
function solution(number, count = 0) {
  for (let i = 0; i < number.length; i++) {
    for (let j = i + 1; j < number.length; j++) {
      for (let k = j + 1; k < number.length; k++) {
        if (number[i] + number[j] + number[k] === 0) count++;
      }
    }
  }
  return count;
}
```

```js
function solution(number) {
  let result = 0;

  const combination = (current, start) => {
    if (current.length === 3) {
      result += current.reduce((acc, cur) => acc + cur, 0) === 0 ? 1 : 0;
      return;
    }

    for (let i = start; i < number.length; i++) {
      combination([...current, number[i]], i + 1);
    }
  };
  combination([], 0);
  return result;
}
```

```js
function solution(number) {
  var answer = 0;
  for (let i = 0; i < number.length - 2; i++) {
    for (let j = i + 1; j < number.length - 1; j++) {
      for (let k = j + 1; k < number.length; k++) {
        const sum = number[i] + number[j] + number[k];
        if (sum === 0) answer++;
      }
    }
  }
  return answer;
}
```

[콜라츠추측](https://school.programmers.co.kr/learn/courses/30/lessons/12943)

```js
function solution(num, count = 0) {
  for (let i = 0; i < 500; i++) {
    if (num === 1) {
      return count;
    } else if (num % 2 === 0) {
      num /= 2;
    } else {
      num = num * 3 + 1;
    }
    count++;
  }
  return -1;
}
```

```js
function collatz(num) {
  var answer = 0;
  while (num != 1 && answer != 500) {
    num % 2 == 0 ? (num = num / 2) : (num = num * 3 + 1);
    answer++;
  }
  return num == 1 ? answer : -1;
}
```

```js
// 재귀함수 활용
function collatz(num, count = 0) {
  return num == 1
    ? count >= 500
      ? -1
      : count
    : collatz(num % 2 == 0 ? num / 2 : num * 3 + 1, ++count);
}
```

```js
const solution = (num) => collatzGuessCount(num, 0);

const collatzGuessCount = (num, acc) =>
  num === 1
    ? acc > 500
      ? -1
      : acc
    : collatzGuessCount(processCollatz(num), acc + 1);

const processCollatz = (num) => (num % 2 === 0 ? num / 2 : num * 3 + 1);
```

[시저암호](https://school.programmers.co.kr/learn/courses/30/lessons/12926#qna)

```js
function solution(s, n) {
  return s
    .split("")
    .map((i) => {
      let code = i.charCodeAt();
      console.log(code);
      if ((code + n > 90 && code <= 90) || code + n > 122) {
        return String.fromCharCode(code + n - 26);
      } else {
        return String.fromCharCode(code + n);
      }
    })
    .join("")
    .replaceAll(/[^a-zA-Z]/g, " ");
}
```

```js
function solution(s, n) {
  var upper = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
  var lower = "abcdefghijklmnopqrstuvwxyz";
  var answer = "";

  for (var i = 0; i < s.length; i++) {
    var text = s[i];
    if (text == " ") {
      answer += " ";
      continue;
    }
    var textArr = upper.includes(text) ? upper : lower;
    var index = textArr.indexOf(text) + n;
    if (index >= textArr.length) index -= textArr.length;
    answer += textArr[index];
  }
  return answer;
}
```

```js
function caesar(s, n) {
  return s.replace(/([a-z])|([A-Z])/g, (c, lowerCase) => {
    var startCode = lowerCase ? "a".charCodeAt(0) : "A".charCodeAt(0);
    return String.fromCharCode(
      ((c.charCodeAt(0) - startCode + n) % 26) + startCode
    );
  });
}
```
