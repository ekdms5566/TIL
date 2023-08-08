# 힙(Heap)과 우선순위 큐(Priority Queue)

[힙(Heap)](#힙heap)  
[힙(Heap) 구현 설명](#힙heap-구현-설명)

[우선순위 큐(Priority Queue)](#우선순위-큐priority-queue)  
[우선순위 큐(Priority Queue) 구현 설명](#우선순위-큐priority-queue-구현-설명)

## 힙(Heap)

- 트리의 일종으로 트리에 대해 적용되는 것은 일반적으로 힙에도 적용된다.
- 여러 개의 값들 중에서 최댓값이나 최솟값을 빠르게 찾아내도록 만들어진 자료구조
- 힙을 이용해서 우선순위 큐를 이용할 수 있다.
- 힙은 항상 큰 값이 상위 레벨에 있고 작은 값이 하위 레벨에 있어야 한다.
- **이진 탐색 트리와 비슷하지만 힙에는 왼쪽 오른쪽 순서가 없다.**
- 종류 : 피보나치 힙, 레오나르도 힙, Soft Heap, Leftship Heap 등
- 힙의 경우 보통의 완전 이진 트리와 다르게 반정렬(느슨한 정렬 상태) 상태를 유지한다.

### 힙(Heap)의 특징

- 힙에서 부모는 최대 두개의 자식 노드를 갖는다.
- 이진 힙은 언제나 가능한 가장 적은 공간을 차지한다. 다음 레벨로 내려가기 전에 모든 왼쪽/오른쪽 노드가 다 채워진다.
- 한 줄에서는 왼쪽 자식이 언제나 먼저 채워진다.
- 형제 노드 사이에는 어느 관계도 존재하지 않는다.

### 힙(Heap) 사용 사례

- 우선순위 큐를 구현하는데 사용한다.
- 게임엔진에서 각 액터의 우선순위를 정한다.
- 서버에서 많은 트래픽을 처리할 때 우선 처리해야 할 트래픽을 정한다.
- 시뮬레이션 시스템
- 네트워크 트래픽 제어
- 운영 체제에서의 작업 스케쥴링 (우선 순위가 높은 일을 바로 조회할 수 있다.)
- 수치 해석적인 계산

## 힙(Heap) 구현 설명

### 최소힙(Heap) 구현

배열의 첫 번째 요소가 가지는 index는 0이기 때문에 '1번째'라는 말과 인지부조화가 생기기에 계산의 편의성을 위해 0을 비우는 것으로 설정했다.

```js
class Heap {
  constructor() {
    this.heap = [null]; // 첫 원소는 사용 X
  }
}
```

배열의 첫 원소는 사용하지 않으므로 부모와 자식 간 다음의 관계가 성립한다. 완전 이진 트리의 일종이기 때문에 Binaray Search tree에서의 부모-자식 간 관계와 유사하다.

```
자신 : N
부모 :  (N-1) / 2
왼쪽 자식 : (N*2) + 1
오른쪽 자식 : (N*2) + 2
```

- **삽입**

재귀적 또는 반복문을 활용하여 부모노드를 확인하면서 들어온 값이 부모노드보다 작은지 큰지를 구분하여 위치를 교환하며 정렬한다.

```js
heappush(value) {
  this.heap.push(value);
  let curIdx = this.heap.length - 1;
  let parIdx = (curIdx / 2) >> 0;

  // 최소힙이므로 부모노드가 제일 작아야 한다. 즉 부모노드가 현재노드보다 큰 지 검사하며 반복한다.
  while(curIdx > 1 && this.heap[parIdx] > this.heap[curIdx]) {
    [ this.heap[parIdx], this.heap[curIdx] ] = [ this.heap[curIdx], this.heap[parIdx] ];
    // 구조분해 할당을 이용해 부모와 자식을 swap 한다. 따로 함수로 빼주어 작성해도 좋다.
    curIdx = parIdx;
    parIdx = (curIdx / 2) >> 0;
  }
}
```

- **삭제**

힙에서는 최소힙이나 최대힙 모두 루트 노드가 항상 먼저 배출되어야 한다. 배출되고 나서 생기는 빈자리는 가장 마지막 노드, 즉 배열에서 제일 뒤에 있는 값을 가져온다. 그리고 다시 루트노드부터 재정렬을 실행한다.

```js
heappop() {
  const min = this.heap[1];	// 배열 첫 원소를 비워두므로 root는 heap[1]에 항상 위치한다.
  if(this.heap.length <= 2) this.heap = [ null ];
  else this.heap[1] = this.heap.pop();
  // 배열 마지막 원소를 root 위치에 먼저 배치하는 과정이다.
  // if-else로 분기되는 이유는 추후 heap이 비었는지 아닌지 확인하기 위해 size 체크 함수를 만들때 -1을 통해 0을 만들어주기 때문.

  let curIdx = 1;
  let leftIdx = curIdx * 2;
  let rightIdx = curIdx * 2 + 1;

  if(!this.heap[leftIdx]) return min;
  // 왼쪽 자식이 없다는 것은 오른쪽 자식도 없는, 즉 루트만 있는 상태이므로 바로 반환!
  if(!this.heap[rightIdx]) {
    if(this.heap[leftIdx] < this.heap[curIdx]) {
      [ this.heap[leftIdx], this.heap[curIdx] ] = [ this.heap[curIdx], this.heap[leftIdx] ];
      // 오른쪽 자식이 없다면 왼쪽 자식하나만 있다는 것을 의미한다.
    }
    return min;
  }

  // 위에 조건에 걸리지 않는 경우 왼쪽과 오른쪽 자식이 모두 있는 경우이다.
  // 따라서 현재 노드가 왼쪽 또는 오른쪽 보다 큰 지 작은지를 검사하며 반복한다.
  while(this.heap[leftIdx] < this.heap[curIdx] || this.heap[rightIdx] < this.heap[curIdx]) {
    // 왼쪽과 오른쪽 자식 중에 더 작은 값과 현재 노드를 교체하면 된다.
    const minIdx = this.heap[leftIdx] > this.heap[rightIdx] ? rightIdx : leftIdx;
    [ this.heap[minIdx], this.heap[curIdx] ] = [ this.heap[curIdx], this.heap[minIdx] ];
    curIdx = minIdx;
    leftIdx = curIdx * 2;
    rightIdx = curIdx * 2 + 1;
  }

  return min;
}
```

## 기본 힙, 최소 힙, 최대 힙 구현

- bubbleUp : 힙에 값을 삽입할 때 부모와 비교해서 값이 크거나 작으면(최소 힙의 경우 부모가 자신보다 크면, 최대 힙의 경우 부모가 자신보다 작으면) 부모와 값을 교환해서 올바르게 정렬이 될 때 까지 올라가는 것

- bubbleDown : 힙에서 값을 꺼내올 때 아래 자식들과 비교해서 값이 크거나 작으면(최소 힙의 경우 자식이 자신보다 값이 작으면, 최대 힙의 경우 자식이 자신보다 값이 크면) 자식과 값을 교환해서 올바르게 정렬이 될 때 까지 내려가는 것

### 기본 힙(Heap)

```js
class Heap {
  constructor() {
    this.items = [];
  }

  //값을 서로 바꾸는 메소드
  swap(index1, index2) {
    let temp = this.items[index1]; // items의 index1의 값을 temp(임시공간)에 담음

    this.items[index1] = this.items[index2]; // index1에 index2의 값을 저장

    this.items[index2] = temp; // index2에 아까 index1의 값을 temp에 넣어놓은 값을 저장
  }

  //부모 인덱스 구하는 메소드
  parentIndex(index) {
    return Math.floor((index - 1) / 2);
  }

  //왼쪽 자식 인덱스 구하는 메소드
  leftChildIndex(index) {
    return index * 2 + 1;
  }

  //오른쪽 자식 인덱스 구하는 메소드
  rightChildIndex(index) {
    return index * 2 + 2;
  }

  //부모 노드 구하는 메소드
  parent(index) {
    return this.items[this.parentIndex(index)];
  }

  //왼쪽 자식 노드 구하는 메소드
  leftChild(index) {
    return this.items[this.leftChildIndex(index)];
  }

  //오른쪽 자식 노드 구하는 메소드
  rightChild(index) {
    return this.items[this.rightChildIndex(index)];
  }

  //최대 힙의 경우 최댓값을 반환하고 최소 힙의 경우 최솟값을 반환하는 메소드
  peek() {
    return this.items[0];
  }

  //힙의 크기(항목 개수)를 반환하는 메소드
  size() {
    return this.items.length;
  }
}
```

### 최소 힙(Heap)

```js
class MinHeap extends Heap {
  // MinHeap 클래스는 Heap 클래스를 상속받았으므로 Heap 클래스의 메소드를 모두 사용할 수 있다.
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

  // 힙에 원소를 추가하는 함수
  add(item) {
    this.items[this.items.length] = item;
    this.bubbleUp();
  }

  // 힙에서 원소를 빼내는 함수
  // 최소 힙이라면 최솟값이 빠져나올 것이고 최대힙이라면 최댓값이 빠져나온다.
  poll() {
    let item = this.items[0]; // 첫번째 원소 keep
    this.items[0] = this.items[this.items.length - 1]; // 맨 마지막 원소를 첫번째 원소로 복사
    this.items.pop(); // 맨 마지막 원소 삭제
    this.bubbleDown();

    return item; // keep해둔 값 반환
  }
}
```

### 최대 힙(Heap)

```js
class MaxHeap extends MinHeap {
  //MaxHeap의 경우 MinHeap을 상속받았으므로 MinHeap의 모든 함수를 사용할 수 있지만 bubbleUp과 bubbleDown은 Overriding(재정의)하였다.
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
```

## 우선순위 큐(Priority Queue)

- 우선순위 큐는 일반적인 큐와 다르게 선입선출 방식이 아닌 우선순위를 기준으로 삭제한다
- 우선순위가 같다면 큐에 삽입된 시점을 기준으로 삭제한다
- 배열, 연결리스트, 힙 기반으로 우선순위 큐를 구현할 수 있으며 각각 시간복잡도가 다르다.
- 배열과 연결리스트의 경우, 삽입을 위한 적절한 위치를 찾기 위해 모든 인덱스를 탐색해야 하므로 최악의 경우 성능이 좋지 않을 수 있다. 하지만 구현이 간단하다.

| -    | 배열 기반 | 연결리스트 기반 | 힙 기반 |
| ---- | --------- | --------------- | ------- |
| 삽입 | O(n)      | O(n)            | O(logN) |
| 삭제 | O(1)      | O(1)            | O(logN) |

## 우선순위 큐(Priority Queue) 구현 설명

- 클래스 정의

```js
// 우선순위 큐에 삽입되는 데이터 클래스
class QElement {
  constructor(element, priority) {
    this.element = element;
    this.priority = priority;
  }
}

// 우선순위 큐
class PriorityQueue {
  constructor() {
    this.queue = [];
  }
}
```

- 삽입 메소드 구현

```js
// 데이터의 우선순위에 따라 큐의 적절한 위치에 삽입
enqueue(element, priority) {
  // QElement 객체 생성
  const qElement = new QElement(element, priority);
  let isContain = false;

  // 전체 데이터를 순회하면서 자신의 우선순위가 더 높은 위치를  탐색
  // splice 함수는 배열의 기존 요소를 삭제 또는 교체하거나 새 요소를 추가하여 배열의 내용을 변경
  // array.splice(startIndex, deleteCount, item1, item2, ...)
  for(let i=0, j=this.queue.length; i<j; i++) {
    if(this.queue[i].priority < element.priority) {
      this.queue.splice(i, 0, qElement);
      isContain = true;
      break;
    }
  }

  // 큐에 자신보다 더 낮은 우선순위를 가진 요소가 없을 때, 큐의 맨 마지막에 삽입
  if(!isContain) {
    this.queue.push(qElement);
  }
}
```

- 삭제 메소드 구현

```js
dequeue() {
  // 큐가 비어있을 때는 오류를 발생시킨다.
  // 비어있지 않다면 첫번째 요소를 삭제하고 반환한다.
  if(this.queue.length === 0) {
    return new Error("우선순위 큐에 데이터가 없습니다.");
  }
  return this.queue.shift();
}
```

- 유틸리티 메소드 구현

```js
front() {
  // 큐가 비어있을 때는 오류를 발생시킨다.
  // 비어있지 않다면 첫번째 요소를 반환한다.
  if(this.queue.length === 0) {
    return new Error("우선순위 큐에 데이터가 없습니다.");
  }
  return this.queue[0];
}

rear() {
  // 큐가 비어있을 때는 오류를 발생시킨다.
  // 비어있지 않다면 마지막 요소를 반환한다.
  if(this.queue.length === 0) {
    return new Error("우선순위 큐에 데이터가 없습니다.");
  }
  return this.queue[this.queue.length-1];
}

isEmpty() {
  // 큐의 길이를 0과 비교하여 큐가 비어있는지 반환한다.
  return this.queue.length === 0;
}

print() {
  // ES6 Array.forEach 메소드로 큐를 순회하면서 문자열을 만들어낸다.
  let str = "";
  this.queue.forEach(item => str += item.element + " ");
  return str;
}
```

<hr>

minHeap으로 구성되며, 생성할 때 파라미터로 comp 함수를 넣어주면 maxHeap 구성도 가능

```js
class PriorityQueue {
  constructor(comp) {
    this.heap = [];
    this.comp = comp;
    if (comp == undefined) {
      this.comp = (a, b) => {
        return a - b;
      };
    }
  }
  removeMin() {
    if (this.isEmpty()) {
      return null;
    }
    let min = this.heap[0];
    let last = this.heap.pop();
    if (this.size() != 0) {
      this.heap[0] = last;
      this.downHeap(0);
    }
    return min;
  }
  downHeap(pos) {
    while (this.isInternal(pos)) {
      let s = null;

      //왼쪽과 오른쪽 자식중에 작은 것을 s에 넣는다.
      if (!this.hasRight(pos)) {
        s = this.left(pos);
      } else if (
        this.comp(this.heap[this.left(pos)], this.heap[this.right(pos)]) <= 0
      ) {
        s = this.left(pos);
      } else {
        s = this.right(pos);
      }
      if (this.comp(this.heap[s], this.heap[pos]) < 0) {
        this.swap(pos, s);
        pos = s;
      } else {
        break;
      }
    }
  }
  upHeap(pos) {
    while (!this.isRoot(pos)) {
      let p = this.parent(pos);
      if (this.comp(this.heap[p], this.heap[pos]) <= 0) {
        break;
      }
      this.swap(p, pos);
      pos = p;
    }
  }
  swap(x, y) {
    let tmp = this.heap[x];
    this.heap[x] = this.heap[y];
    this.heap[y] = tmp;
  }
  parent(pos) {
    return parseInt((pos - 1) / 2);
  }
  left(pos) {
    return 2 * pos + 1;
  }
  right(pos) {
    return 2 * pos + 2;
  }
  size() {
    return this.heap.length;
  }
  isInternal(pos) {
    return this.hasLeft(pos);
  }
  isRoot(pos) {
    if (pos == 0) return true;
    return false;
  }
  hasLeft(pos) {
    if (this.left(pos) < this.size()) {
      return true;
    }
    return false;
  }
  hasRight(pos) {
    if (this.right(pos) < this.size()) {
      return true;
    }
    return false;
  }
  isEmpty() {
    return this.heap.length == 0;
  }
  insert(value) {
    this.heap.push(value);
    this.upHeap(this.size() - 1);
  }
  min() {
    return this.heap[0];
  }
}
```
