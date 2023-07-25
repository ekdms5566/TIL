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
