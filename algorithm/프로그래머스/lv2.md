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

[올바른 괄호](https://school.programmers.co.kr/learn/courses/30/lessons/12909)

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
