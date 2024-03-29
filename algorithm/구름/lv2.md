[거울 단어](https://level.goorm.io/exam/49066/%EA%B1%B0%EC%9A%B8-%EB%8B%A8%EC%96%B4/quiz/1)

```js
function solution(line) {
  const obj = {
    b: "d",
    d: "b",
    i: "i",
    l: "l",
    m: "m",
    n: "n",
    o: "o",
    p: "q",
    q: "p",
    s: "z",
    z: "s",
    u: "u",
    v: "v",
    w: "w",
    x: "x",
  };

  line.map((str) => {
    if (
      str.split("").reverse().join("") ===
      str
        .split("")
        .map((val) => obj[val])
        .join("")
    )
      console.log("Mirror");
    else console.log("Normal");
  });
}

const readline = require("readline");
(async () => {
  let rl = readline.createInterface({ input: process.stdin });
  let data = [];

  for await (const line of rl) {
    if (!line) rl.close();
    else data.push(line);
  }
  data.shift();

  solution(data);
  process.exit();
})();
```

[근묵자흑](https://level.goorm.io/exam/47881/%EA%B7%BC%EB%AC%B5%EC%9E%90%ED%9D%91/quiz/1)

```js
function solution(n, m) {
  let cnt = 0;

  while (n > m) {
    cnt += Math.floor(n / m);
    n = Math.floor(n / m) + (n % m);
  }

  console.log(cnt + 1);
}

const readline = require("readline");
const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout,
});

let n = null,
  m = null;

rl.on("line", function (line) {
  if (!n && !m) {
    n = parseInt(line.split(" ")[0]);
    m = parseInt(line.split(" ")[1]);
    rl.close();
  }
}).on("close", function () {
  solution(n, m);
  process.exit();
});
```

[M배 배열](https://level.goorm.io/exam/174909/m%EB%B0%B0-%EB%B0%B0%EC%97%B4/quiz/1)

```js
function solution(n, m, data) {
  const result = data.map((val) => (val % m ? val * m : val));
  console.log(result.join(" "));
}

const readline = require("readline");

(async () => {
  let rl = readline.createInterface({ input: process.stdin });
  let n = null,
    m = null;
  let data = [];

  for await (const line of rl) {
    if (!line) rl.close();
    else if (!n && !m) {
      n = parseInt(line.split(" ")[0]);
      m = parseInt(line.split(" ")[1]);
    } else line.split(" ").forEach((el) => data.push(+el));
  }

  solution(n, m, data);
  process.exit();
})();
```

[연속 점수](https://level.goorm.io/exam/174924/%EC%97%B0%EC%86%8D-%EC%A0%90%EC%88%98/quiz/1)

```js
const fs = require("fs");
const input = fs.readFileSync("/dev/stdin", "utf8").trim().split("\n");
const count = parseInt(input[0]);
const scores = input[1].split(" ").map(Number);
let max = scores[0];
let currentStreak = scores[0];
for (let i = 1; i < count; i++) {
  if (scores[i] === scores[i - 1] + 1) {
    currentStreak += scores[i];
  } else {
    currentStreak = scores[i];
  }
  max = Math.max(max, currentStreak);
}
console.log(max);
```

[폭탄 구현하기](https://level.goorm.io/exam/159666/%ED%8F%AD%ED%83%84-%EA%B5%AC%ED%98%84%ED%95%98%EA%B8%B0/quiz/1)

```js
function solution(n, data) {
  const arr = Array.from({ length: n }, () => Array(n).fill(0));

  for (let [y, x] of data) {
    arr[y][x]++;

    const directions = [
      [-1, 0],
      [1, 0],
      [0, -1],
      [0, 1],
    ];

    for (let [dy, dx] of directions) {
      const ny = y + dy;
      const nx = x + dx;

      if (ny >= 0 && ny < n && nx >= 0 && nx < n) arr[ny][nx]++;
    }
  }

  console.log(arr.flat().reduce((a, c) => a + c, 0));
}

const readline = require("readline");

(async () => {
  let rl = readline.createInterface({ input: process.stdin });
  let n = null;
  let data = [];

  for await (const line of rl) {
    if (!line) rl.close();
    else if (!n) n = ~~line.split(" ")[0];
    else data.push(line.split(" ").map((el) => +el - 1));
  }

  solution(n, data);

  process.exit();
})();
```

[단어장 만들기](https://level.goorm.io/exam/148704/%EA%B8%B0%EB%B3%B8-%EB%8B%A8%EC%96%B4%EC%9E%A5-%EB%A7%8C%EB%93%A4%EA%B8%B0/quiz/1)

길이순 정렬 - sort((a, b) => a.length - b.length)

```js
function solution(n, data) {
  data.sort();
  data.sort((a, b) => a.length - b.length);

  console.log(data[n - 1]);
}

const readline = require("readline");

(async () => {
  let rl = readline.createInterface({ input: process.stdin });
  let n = null;
  let data = [];

  for await (const line of rl) {
    if (!line) rl.close();
    else if (!n) {
      n = ~~line.split(" ")[1];
    } else data.push(line);
  }

  solution(n, data);

  process.exit();
})();
```

[파손된 램](https://level.goorm.io/exam/49074/%ED%8C%8C%EC%86%90%EB%90%9C-%EB%9E%A8/quiz/1)

```js
const readline = require("readline");

(async () => {
  let rl = readline.createInterface({ input: process.stdin });
  let n = null;
  let data = [];

  let cnt = 0;
  let result = [];

  for await (const line of rl) {
    if (!line) rl.close();
    else if (!n) n = ~~line;
    else line.split(" ").forEach((val) => data.push(parseInt(val)));
  }

  data.forEach((val, idx) => {
    if (val > 0 && (val & (val - 1)) !== 0) {
      result.push(idx + 1);
      cnt++;
    }
  });

  if (cnt !== 0) {
    console.log(cnt);
    console.log(result.join(" "));
  } else console.log(cnt);

  process.exit();
})();
```

[빙글빙글 1](https://level.goorm.io/exam/60331/%EB%B9%99%EA%B8%80%EB%B9%99%EA%B8%80-1/quiz/1)

```js
function solution(n) {
  let arr = Array.from({ length: n }, () => Array(n).fill(" "));
  let x = 0,
    y = 0,
    i = 0;

  while (i <= n / 2) {
    for (; x >= i; x--) arr[x][y] = "#";
    x++;

    for (; y < n - i; y++) arr[x][y] = "#";
    y--;

    for (; x < n - i; x++) arr[x][y] = "#";
    x--;

    for (; y >= i; y--) arr[x][y] = "#";
    y++;

    i += 2;
  }

  for (let line of arr) console.log(line.join(" "));
}

const readline = require("readline");
const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout,
});

let n;

rl.on("line", function (line) {
  n = +line;
  rl.close();
}).on("close", function () {
  solution(n);
  process.exit();
});
```

[방 탈출하기 - 이진 탐색](https://level.goorm.io/exam/49105/%EB%B0%A9-%ED%83%88%EC%B6%9C%ED%95%98%EA%B8%B0/quiz/1)

````python
import bisect

n = input()
arr1 = list(map(int, input().split()))
arr1.sort()
m = input()
arr2 = list(map(int, input().split()))

for val in arr2:
	index = bisect.bisect_left(arr1, int(val))
	if index != len(arr1) and arr1[index] == int(val):
		print(1)
	else:
		print(0)
    ```
````

[소희와 버스, 버스 선택](https://level.goorm.io/exam/49107/%EC%86%8C%ED%9D%AC%EC%99%80-%EB%B2%84%EC%8A%A4/quiz/1)

각 버스의 도착시간이 T와 같거나 크게 되는 최초의 시각 찾기

```js
const readline = require("readline");

(async () => {
  let rl = readline.createInterface({ input: process.stdin });
  let time = null;
  let data = [];

  for await (const line of rl) {
    if (!line) rl.close();
    else if (!time) time = ~~line.split(" ")[1];
    else data.push(line.split(" ").map((el) => ~~el));
  }

  let result = [];

  for (let i = 0; i < data.length; i++) {
    while (data[i][0] < time) data[i][0] += data[i][1];
    result.push([data[i][0], i + 1]);
  }
  result.sort((a, b) => a[0] - b[0]);

  console.log(result[0][1]);

  process.exit();
})();
```

[진법 변환](https://level.goorm.io/exam/49062/%EC%A7%84%EB%B2%95-%EB%B3%80%ED%99%98/quiz/1)

```js
const readline = require("readline");

const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout,
});

const solution = (n, q) => {
  let ah_jji = "";

  if (q > 10) {
    while (n > 0) {
      const [qoutient, mod] = [Math.floor(n / q), n % q];
      const alphabet = { 10: "A", 11: "B", 12: "C", 13: "D", 14: "E", 15: "F" };

      if (mod >= 10) {
        ah_jji += alphabet[mod];
      } else {
        ah_jji += mod;
      }

      n = qoutient;
    }
  } else {
    while (n > 0) {
      const [qoutient, mod] = [Math.floor(n / q), n % q];
      ah_jji += mod;
      n = qoutient;
    }
  }

  return ah_jji.split("").reverse().join("");
};

rl.on("line", function (line) {
  const [tenNum, targetNum] = line.trim().split(" ");

  for (let notation = 2; notation <= 16; notation++) {
    if (solution(parseInt(tenNum), notation) === targetNum) {
      console.log(notation);
      break;
    }
  }

  rl.close();
}).on("close", function () {
  process.exit(0);
});
```

[1등과 2등](https://level.goorm.io/exam/49072/1%EB%93%B1%EA%B3%BC-2%EB%93%B1/quiz/1)

```js
function solution(data) {
  if (
    (data.indexOf("12") !== -1 &&
      data.substr(data.indexOf("12") + 2).indexOf("21") !== -1) ||
    (data.indexOf("21") !== -1 &&
      data.substr(data.indexOf("21") + 2).indexOf("12") !== -1)
  )
    console.log("Yes");
  else console.log("No");
}

const readline = require("readline");

(async () => {
  let rl = readline.createInterface({ input: process.stdin });
  let data = [];

  for await (const line of rl) {
    solution(line);
    rl.close();
  }

  process.exit();
})();
```
