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

[야근 지수 - 힙(Heap)](https://school.programmers.co.kr/learn/courses/30/lessons/12927)

```js
class Heap {
  constructor() {
    this.items = [];
  }

  swap(index1, index2) {
    let temp = this.items[index1];
    this.items[index1] = this.items[index2];
    this.items[index2] = temp;
  }

  parentIndex(index) {
    return Math.floor((index - 1) / 2);
  }

  leftChildIndex(index) {
    return index * 2 + 1;
  }

  rightChildIndex(index) {
    return index * 2 + 2;
  }

  parent(index) {
    return this.items[this.parentIndex(index)];
  }

  leftChild(index) {
    return this.items[this.leftChildIndex(index)];
  }

  rightChild(index) {
    return this.items[this.rightChildIndex(index)];
  }

  peek() {
    return this.items[0];
  }

  size() {
    return this.items.length;
  }
}

class MinHeap extends Heap {
  bubbleUp() {
    let index = this.items.length - 1;
    while (
      this.parent(index) !== undefined &&
      this.parent(index) > this.items[index]
    ) {
      this.swap(index, this.parentIndex(index));
      index = this.parentIndex(index);
    }
  }

  bubbleDown() {
    let index = 0;
    while (
      this.leftChild(index) !== undefined &&
      (this.leftChild(index) < this.items[index] ||
        this.rightChild(index) < this.items[index])
    ) {
      let smallerIndex = this.leftChildIndex(index);
      if (
        this.rightChild(index) !== undefined &&
        this.rightChild(index) < this.items[smallerIndex]
      ) {
        smallerIndex = this.rightChildIndex(index);
      }
      this.swap(index, smallerIndex);
      index = smallerIndex;
    }
  }

  add(item) {
    this.items[this.items.length] = item;
    this.bubbleUp();
  }

  poll() {
    let item = this.items[0];
    this.items[0] = this.items[this.items.length - 1];
    this.items.pop();
    this.bubbleDown();

    return item;
  }
}

class MaxHeap extends MinHeap {
  bubbleUp() {
    let index = this.items.length - 1;
    while (
      this.parent(index) !== undefined &&
      this.parent(index) < this.items[index]
    ) {
      this.swap(index, this.parentIndex(index));
      index = this.parentIndex(index);
    }
  }

  bubbleDown() {
    let index = 0;

    while (
      this.leftChild(index) !== undefined &&
      (this.leftChild(index) > this.items[index] ||
        this.rightChild(index) > this.items[index])
    ) {
      let largerIndex = this.leftChildIndex(index);
      if (
        this.rightChild(index) !== undefined &&
        this.rightChild(index) > this.items[largerIndex]
      ) {
        largerIndex = this.rightChildIndex(index);
      }
      this.swap(largerIndex, index);
      index = largerIndex;
    }
  }
}

function solution(n, works) {
  if (works.reduce((a, c) => a + c) <= n) return 0;

  const heap = new MaxHeap();
  works.forEach((val) => heap.add(val));

  while (n > 0) {
    heap.add(heap.poll() - 1);
    n--;
  }

  return heap.items.reduce((a, c) => a + c ** 2, 0);
}
```

```js
// 최대힙
class MaxHeap {
  constructor() {
    this.heap = [null];
  }

  push(value) {
    this.heap.push(value); // 마지막 트리에 추가
    let currentIndex = this.heap.length - 1;
    let parentIndex = Math.floor(currentIndex / 2);

    while (parentIndex !== 0 && this.heap[parentIndex] < value) {
      const temp = this.heap[parentIndex];
      this.heap[parentIndex] = value;
      this.heap[currentIndex] = temp;

      currentIndex = parentIndex;
      parentIndex = Math.floor(currentIndex / 2);
    }
  }

  pop() {
    if (this.heap.length === 2) return this.heap.pop(); // 루트 정점만 남은 경우

    const returnValue = this.heap[1];
    this.heap[1] = this.heap.pop();

    let currentIndex = 1;
    let leftIndex = 2;
    let rightIndex = 3;
    while (
      this.heap[currentIndex] < this.heap[leftIndex] ||
      this.heap[currentIndex] < this.heap[rightIndex]
    ) {
      if (this.heap[leftIndex] < this.heap[rightIndex]) {
        const temp = this.heap[currentIndex];
        this.heap[currentIndex] = this.heap[rightIndex];
        this.heap[rightIndex] = temp;
        currentIndex = rightIndex;
      } else {
        const temp = this.heap[currentIndex];
        this.heap[currentIndex] = this.heap[leftIndex];
        this.heap[leftIndex] = temp;
        currentIndex = leftIndex;
      }
      leftIndex = currentIndex * 2;
      rightIndex = currentIndex * 2 + 1;
    }
    return returnValue;
  }
}

function solution(n, works) {
  const heap = new MaxHeap();

  for (let i = 0; i < works.length; i++) {
    heap.push(works[i]);
  }

  for (let i = 0; i < n; i++) {
    let peekValue = heap.pop() - 1;
    if (peekValue < 0) peekValue = 0;
    heap.push(peekValue);
  }

  return heap.heap
    .filter((item) => item !== null)
    .reduce((acc, item) => acc + item ** 2, 0);
}
```

[단어 변환 - DFS/BFS](https://school.programmers.co.kr/learn/courses/30/lessons/43163)

최단 경로 찾기 - DFS

```js
function compare(stra, strb) {
  let count = 0;
  for (let i = 0; i < stra.length; i++) {
    if (stra[i] !== strb[i]) {
      count++;
    }
  }
  return count === 1;
}

function solution(begin, target, words) {
  if (!words.includes(target)) return 0;

  let n = words.length;
  let index = new Array(n).fill().map(() => []);

  for (let i = 0; i < n; i++) {
    for (let j = 0; j < n; j++) {
      if (i !== j && compare(words[i], words[j])) {
        index[i].push(j);
      }
    }
  }

  let dist = new Array(n).fill(Infinity);
  let q = [];
  for (let i = 0; i < n; i++) {
    if (compare(begin, words[i])) {
      dist[i] = 1;
      q.push(i);
    }
  }
  while (q.length > 0) {
    let x = q.shift();
    if (words[x] === target) {
      return dist[x];
    }
    for (let idx of index[x]) {
      if (dist[idx] === Infinity) {
        dist[idx] = dist[x] + 1;
        q.push(idx);
      }
    }
  }

  return 0;
}
```

[여행 경로 - DFS/BFS](https://school.programmers.co.kr/learn/courses/30/lessons/43164)

```js
function solution(tickets) {
  var answer = [];
  DFS(tickets, "ICN", ["ICN"]);
  return answer.sort()[0];
  function DFS(t, start, str) {
    if (t.length == 0) answer.push(str);
    for (var i in t) {
      if (t[i][0] == start) {
        let tmp = t.slice();
        tmp.splice(i, 1);
        let tmp2 = str.slice();
        tmp2.push(t[i][1]);
        DFS(tmp, t[i][1], tmp2);
      }
    }
  }
}
```

```js
function solution(tickets) {
  function recursive(p, array) {
    if (array.length === 0) {
      return [[p]];
    }

    let result = [];
    for (let i = 0; i < array.length; ++i) {
      let ticket = array[i];
      if (ticket[0] === p) {
        for (let path of recursive(
          ticket[1],
          array.filter((val, index) => index !== i)
        )) {
          path.unshift(ticket[0]);
          result.push(path);
        }
      }
    }
    return result;
  }

  let result = recursive("ICN", tickets);
  result.sort((a, b) => {
    if (a.toString() > b.toString()) return 1;
    else return -1;
  });
  return result[0];
}
```

```js
function solution(tickets) {
  const n = tickets.length + 1; // 방문해야하는 총 공항 수
  const graph = {};
  for (let [start, end] of tickets) {
    if (!graph[start]) {
      graph[start] = [];
    }
    if (!graph[end]) {
      graph[end] = [];
    }
    // 2번 테스트 케이스(런타임에러)를 통과하기 위해서 end지점도 빈 배열을 생성해줘야합니다.
    graph[start].push(end);
    graph[start].sort();
  }

  const answer = [];
  const visited = new Set();
  const dfs = (current, route) => {
    if (route.length === n) {
      answer.push([...route]);
      return;
    }
    for (let i = 0; i < graph[current].length; i++) {
      const des = graph[current][i];
      const ticket = `${current}#${des}`;
      if (
        !visited.has(ticket) ||
        (i >= 1 && ticket === `${current}#${graph[current][i - 1]}`)
      ) {
        // 중복된 티켓팅도 경로를 탐색할 수 있도록 만든 추가 조건문입니다. 1번 테스트 케이스를 위한거 입니다.
        visited.add(ticket);
        route.push(des);
        dfs(des, route);
        route.pop();
        visited.delete(ticket);
      }
    }
  };
  dfs("ICN", ["ICN"]);

  return answer[0];
}
```

[등굣길 - 동적 프로그래밍(Dynamic programming)](https://school.programmers.co.kr/learn/courses/30/lessons/42898)

```js
function solution(m, n, puddles) {
  const dp = new Array(n).fill().map((_) => new Array(m).fill(0));
  dp[0][0] = 1;

  for (let y = 0; y < n; y++) {
    for (let x = 0; x < m; x++) {
      if (y === 0 && x === 0) {
        continue;
      }

      const has =
        puddles.findIndex(([_x, _y]) => {
          const puddleX = m - _x;
          const puddleY = n - _y;
          return puddleX === x && puddleY === y;
        }) >= 0;
      if (has) {
        continue;
      }

      if (y === 0) {
        dp[y][x] = dp[y][x - 1];
      } else if (x === 0) {
        dp[y][x] = dp[y - 1][x];
      } else {
        dp[y][x] = (dp[y - 1][x] + dp[y][x - 1]) % 1000000007;
      }
    }
  }
  return dp[n - 1][m - 1];
}
```

```js
function solution(m, n, puddles) {
  var shortest = Array.from({ length: n }, () =>
    Array.from({ length: m }, () => 0)
  );
  shortest[0][0] = 1;

  for (const [x, y] of puddles) {
    shortest[y - 1][x - 1] = -1;
  }

  for (let i = 0; i < n; i++) {
    for (let j = 0; j < m; j++) {
      if (i === 0 && j === 0) continue;
      if (shortest[i][j] === -1) continue;

      var top = 0;
      if (i - 1 >= 0 && shortest[i - 1][j] !== -1) {
        top = shortest[i - 1][j] % 1000000007;
      }
      var left = 0;
      if (j - 1 >= 0 && shortest[i][j - 1] !== -1) {
        left = shortest[i][j - 1] % 1000000007;
      }

      shortest[i][j] = top + left;
    }
  }

  return shortest[n - 1][m - 1] % 1000000007;
}
```

```python
def solution(m, n, puddles):
    puddles = [[q,p] for [p,q] in puddles]
    dp = [[0] * (m + 1) for i in range(n + 1)]
    dp[1][1] = 1

    for i in range(1, n + 1):
        for j in range(1, m + 1):
            if i == 1 and j == 1: continue
            if [i, j] in puddles:
                dp[i][j] = 0
            else:
                dp[i][j] = (dp[i - 1][j] + dp[i][j - 1]) % 1000000007
    return dp[n][m]
```

[숫자 게임](https://school.programmers.co.kr/learn/courses/30/lessons/12987)

```js
function solution(A, B) {
  let point = 0,
    temp = 0;
  A = A.sort((a, b) => a - b);
  B = B.sort((a, b) => a - b);

  for (let i = 0; i < A.length; i++) {
    for (let j = temp; j < B.length; j++) {
      if (A[i] < B[j]) {
        point++;
        temp = j + 1;
        break;
      }
    }
  }
  return point;
}
```

[단속카메라 - 탐욕법(Greedy)](https://school.programmers.co.kr/learn/courses/30/lessons/42884)

```js
function solution(routes) {
  let answer = 0;

  routes.sort((a, b) => a[0] - b[0]);
  let min = 0;
  let max = 0;

  for (let i = 0; i < routes.length; i++) {
    if (i == 0) {
      answer += 1;
      min = routes[i][0];
      max = routes[i][1];
      continue;
    } else {
      if (min <= routes[i][0] && routes[i][1] <= max) {
        min = routes[i][0];
        max = routes[i][1];
        continue;
      }

      if (min <= routes[i][0] && routes[i][0] <= max) {
        continue;
      } else {
        answer += 1;
        min = routes[i][0];
        max = routes[i][1];
      }
    }
  }
  return answer;
}
```

```js
function solution(routes) {
  var answer = [];
  routes.sort((a, b) => a[1] - b[1]);
  for (let item of routes) {
    if (!checkCam(item, answer)) {
      answer.push(item[1]);
    }
  }

  return answer.length;
}

function checkCam(route, cameras) {
  for (let cam of cameras) {
    if (route[0] <= cam && cam <= route[1]) {
      return true;
    }
  }
  return false;
}
```

```js
function solution(routes) {
  const N = routes.length;
  routes.sort((a, b) => {
    return a[1] - b[1];
  });
  let e = -Infinity,
    cnt = 0;
  for (let i = 0; i < N; i++) {
    if (routes[i][0] > e) {
      cnt++;
      e = routes[i][1];
    }
  }
  return cnt;
}
```
