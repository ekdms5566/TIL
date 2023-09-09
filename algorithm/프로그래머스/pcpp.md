[외톨이 알파벳](https://school.programmers.co.kr/learn/courses/15008/lessons/121683)

```js
function solution(inputString) {
  let answer = "";
  const count = {};
  const answerList = [];

  for (let idx = 0; idx < inputString.length; idx++) {
    const alpha = inputString[idx];
    if (!(alpha in count)) {
      count[alpha] = [idx];
    } else {
      count[alpha].push(idx);
    }
  }

  for (const [key, value] of Object.entries(count)) {
    if (value.length >= 2) {
      for (let i = 0; i < value.length - 1; i++) {
        if (Math.abs(value[i] - value[i + 1]) > 1) {
          answerList.push(key);
          break;
        }
      }
    }
  }

  if (answerList.length === 0) {
    answer = "N";
  } else {
    answer = answerList.sort().join("");
  }

  return answer;
}
```

[체육 대회](https://school.programmers.co.kr/learn/courses/15008/lessons/121684)

```js
function solution(ability) {
  let mymax = 0;
  let sum = 0;
  const v = new Array(ability.length).fill(0);

  function dfs(index) {
    if (index === ability[0].length) {
      if (mymax < sum) {
        mymax = sum;
      }
      return;
    }

    for (let i = 0; i < ability.length; i++) {
      if (v[i] === 1) {
        continue;
      }
      v[i] = 1;
      sum += ability[i][index];
      dfs(index + 1);
      sum -= ability[i][index];
      v[i] = 0;
    }
  }

  dfs(0);
  return mymax;
}
```

[유전 법칙](https://school.programmers.co.kr/learn/courses/15008/lessons/121685)

```js
function getGene(pose) {
  let [n, p] = pose;
  let stack = [];

  p--;
  while (n > 1) {
    stack.push(p % 4);
    n--;
    p = parseInt(p / 4);
  }

  while (stack.length > 0) {
    num = stack.pop();
    if (num == 0) return "RR";
    if (num == 3) return "rr";
  }
  return "Rr";
}

function solution(queries) {
  return queries.map(getGene);
}
```

```js
function bean(generation, order) {
  if (generation === 1) return "Rr";

  const parent = bean(generation - 1, Math.floor((order - 1) / 4) + 1);
  if (parent === "RR" || parent === "rr") return parent;

  if (order % 4 === 0) {
    return "rr";
  } else if (order % 4 === 1) {
    return "RR";
  } else {
    return "Rr";
  }
}

function solution(queries) {
  const answer = [];
  for (const query of queries) {
    answer.push(bean(query[0], query[1]));
  }
  return answer;
}
```

[실습용 로봇](https://school.programmers.co.kr/learn/courses/15009/lessons/121687?language=javascript)

```js
function solution(command) {
  let path = [
    [0, 1],
    [1, 0],
    [0, -1],
    [-1, 0],
  ];
  let x = 0,
    y = 0,
    d = 0;
  let arr = command.split("");

  for (const word of arr) {
    if (word === "R") d = (d + 1) % 4;
    else if (word === "L") d = (d + 3) % 4;
    else if (word === "G") {
      x += path[d][0];
      y += path[d][1];
    } else if (word === "B") {
      x -= path[d][0];
      y -= path[d][1];
    }
  }
  return [x, y];
}
```
