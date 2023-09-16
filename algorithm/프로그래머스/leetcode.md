## 30 Days of JavaScript

[2637. Promise Time Limit](https://leetcode.com/problems/promise-time-limit/description/?envType=study-plan-v2&envId=30-days-of-javascript)

```js
/**
 * @param {Function} fn
 * @param {number} t
 * @return {Function}
 */
var timeLimit = function (fn, t) {
  return async function (...args) {
    return Promise.race([
      fn(...args),
      new Promise((resolve, reject) =>
        setTimeout(() => reject("Time Limit Exceeded"), t)
      ),
    ]);
  };
};

/**
 * const limited = timeLimit((t) => new Promise(res => setTimeout(res, t)), 100);
 * limited(150).catch(console.log) // "Time Limit Exceeded" at t=100ms
 */
```

[2622. Cache With Time Limit](https://leetcode.com/problems/cache-with-time-limit/description/?envType=study-plan-v2&envId=30-days-of-javascript)

```js
var TimeLimitedCache = function () {
  var cache = {}; // 키-값 쌍을 저장할 객체

  function isKeyValid(key) {
    return cache[key] && cache[key].expirationTime > Date.now();
  }

  this.set = function (key, value, duration) {
    // 이미 캐시에 동일한 키가 존재하고 유효한 경우
    if (isKeyValid(key)) {
      cache[key] = { value, expirationTime: Date.now() + duration };
      return true;
    }

    // 캐시에 동일한 키가 없거나 유효하지 않은 경우
    cache[key] = { value, expirationTime: Date.now() + duration };
    return false;
  };

  this.get = function (key) {
    if (isKeyValid(key)) {
      return cache[key].value; // 유효한 경우 해당 값 반환
    }
    return -1; // 유효하지 않은 경우 -1 반환
  };

  this.count = function () {
    var count = 0;
    var currentTime = Date.now();

    // 캐시에서 유효한 키의 개수를 센다.
    for (var key in cache) {
      if (isKeyValid(key)) {
        count++;
      }
    }

    return count;
  };
};

/**
 * @param {number} key
 * @param {number} value
 * @param {number} duration time until expiration in ms
 * @return {boolean} if un-expired key already existed
 */
TimeLimitedCache.prototype.set = function (key, value, duration) {};

/**
 * @param {number} key
 * @return {number} value associated with key
 */
TimeLimitedCache.prototype.get = function (key) {};

/**
 * @return {number} count of non-expired keys
 */
TimeLimitedCache.prototype.count = function () {};

/**
 * Your TimeLimitedCache object will be instantiated and called as such:
 * var obj = new TimeLimitedCache()
 * obj.set(1, 42, 1000); // false
 * obj.get(1) // 42
 * obj.count() // 1
 */
```

[2627. Debounce](https://leetcode.com/problems/debounce/description/?envType=study-plan-v2&envId=30-days-of-javascript)

```js
/**
 * @param {Function} fn
 * @param {number} t milliseconds
 * @return {Function}
 */
var debounce = function (fn, t) {
  let timer;

  return function (...args) {
    clearTimeout(timer);
    timer = setTimeout(() => {
      fn(...args);
    }, t);
  };
};

/**
 * const log = debounce(console.log, 100);
 * log('Hello'); // cancelled
 * log('Hello'); // cancelled
 * log('Hello'); // Logged at t=100ms
 */
```
