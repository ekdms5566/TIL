[정수 삼각형 - 동적 계획법(Dynamic Programming)](https://school.programmers.co.kr/learn/courses/30/lessons/43105)

```js
function solution(triangle) {
  let size = triangle.length - 1;

  for (let i = size; i > 0; i--) {
    for (let j = 0; j < triangle[i].length - 1; j++) {
      triangle[i - 1][j] +=
        triangle[i][j] > triangle[i][j + 1]
          ? triangle[i][j]
          : triangle[i][j + 1];
    }
  }

  return triangle[0][0];
}
```

```js
function solution(triangle) {
  return Math.max(
    ...triangle.reduce((cost, line) => {
      return line.map((v, index) => {
        return (
          v +
          Math.max(
            index < cost.length ? cost[index] : 0,
            index > 0 ? cost[index - 1] : 0
          )
        );
      });
    }, [])
  );
}
```

[최고의 집합](https://school.programmers.co.kr/learn/courses/30/lessons/12938)

```js
function solution(n, s) {
  let answer = Array(n).fill(Math.floor(s / n));
  if (n > s) return [-1];

  for (let i = 0; i < s % n; i++) answer[answer.length - 1 - i] += 1;

  return answer;
}
```

```js
const solution = (n, s) =>
  n > s
    ? [-1]
    : Array.apply(null, Array(n)).map((_, i) =>
        i < n - (s % n) ? parseInt(s / n) : parseInt(s / n) + 1
      );
```

[네트워크](https://school.programmers.co.kr/learn/courses/30/lessons/43162)

```js
function solution(n, computers) {
  let visited = [];
  let intranet = [];
  let network = [];

  function connect(node) {
    visited.push(node);
    intranet.push(node);
    for (let nextnode = 0; nextnode < n; nextnode++) {
      if (visited.indexOf(nextnode) === -1 && computers[node][nextnode] === 1)
        connect(nextnode);
    }
    return intranet;
  }

  for (let node = 0; node < n; node++) {
    if (visited.indexOf(node) === -1) {
      network.push(connect(node));
      intranet = [];
    }
  }

  return network.length;
}
```

```js
let arr;
let visitArr;

function solution(n, computers) {
  let ctr = 0;
  arr = computers;
  visitArr = new Array(n).fill(false);

  for (let i in arr) {
    ctr += dfs(i);
  }

  return ctr;
}

function dfs(i) {
  if (visitArr[i] == true) return 0;
  else visitArr[i] = true;

  for (let j in arr[i]) {
    if (arr[i][j] == 1) dfs(j);
  }

  return 1;
}
```

```js
function solution(n, computers) {
  var answer = 0;
  var isVisited = [];

  for (var i = 0; i < n; i++) {
    isVisited.push(false);
  }

  var DFS = function (computers, i) {
    console.log("DFS excuted");
    if (isVisited[i]) {
      return;
    }
    isVisited[i] = true;
    console.log(isVisited);

    for (var j = 0; j < computers.length; j++) {
      if (computers[i][j] === 1) {
        console.log(i + ", " + j);
        console.log("connected");
        DFS(computers, j);
      }
    }
  };

  for (var i = 0; i < n; i++) {
    if (!isVisited[i]) {
      answer++;
      console.log(isVisited, "도입부");
      console.log(answer);
      DFS(computers, i);
    }
  }

  return answer;
}
```

[이중우선순위큐 - 힙(Heap)](https://school.programmers.co.kr/learn/courses/30/lessons/42628)

```js
function solution(operations) {
  let q = [];

  operations.map((val) => {
    if (val.includes("I")) {
      q.push(parseInt(val.split(" ")[1]));
      q.sort((a, b) => b - a);
    } else if (val === "D 1") q.shift();
    else if (val === "D -1") q.pop();
  });

  if (q.length) return [Math.max(...q), Math.min(...q)];
  else return [0, 0];
}
```

```js
function solution(arg) {
  var list = [];
  arg.forEach((t) => {
    if (t[0] === "I") {
      list.push(+t.split(" ")[1]);
    } else {
      if (!list.length) return;

      var val = (t[2] === "-" ? Math.min : Math.max)(...list);
      var delIndex = list.findIndex((t) => t === val);

      list.splice(delIndex, 1);
    }
  });

  return list.length ? [Math.max(...list), Math.min(...list)] : [0, 0];
}
```

```js
function solution(operations) {
  const heap = [];

  function swap(num) {
    if (heap.length === 0) heap.push(num);
    else {
      let flag = false,
        i = 0;
      while (!flag) {
        if (num < heap[i]) i++;
        else {
          heap.splice(i, 0, num);
          flag = true;
        }
      }
    }
  }

  for (let i = 0; i < operations.length; i++) {
    let arr = operations[i].split(" ");
    arr[1] = arr[1] | 0;
    if (arr[0] === "I") swap(arr[1]);
    else {
      if (arr[1] === 1) heap.shift();
      else heap.pop();
    }
  }

  if (heap.length === 0) return [0, 0];

  let result = [heap[0], heap[heap.length - 1]];

  return result;
}
```
