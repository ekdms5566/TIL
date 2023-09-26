# setState의 동작방식

## setState란?

- setState가 호출 될 때 컴포넌트의 state에 대한 업데이트가 실행된다.
- `Object.is` 비교를 통해 새로운 값이 현재 상태와 동일한 경우 렌더링을 하지 않는다.
- 리액트는 상태 업데이트를 일괄로 처리한다. 즉 모든 이벤트 핸들러가 실행되고 해당 기능을 호출한 후 화면으 업데이트 한다.
- setState는 비동기로 작동한다. 동기로 작동할 경우 변경될 때마다 바로바로 렌더링이 일어나면 비효율적이기 때문.

**참고 코드**

```js
const [state, setState] = useState(0);

const onClickCount = () => {
  setState(state + 1);
  setState(state + 1);
  setState(state + 1);
  setState(state + 1);
  setState(state + 1);

  console.log(state); // 0
  // 렌더링이 되지 않았기 때문에 state도 변하지 않음.
};
```

## prevState

prev를 사용하면 임시 저장공간에 있는 값을 가져올 수 있다. 임시 저장공간에 값이 없으면 default 값을 가져온다.

boolean 값을 변경시킬 때 편리하게 활용할 수 있다.

```js
const [isOpen, setIsOpen] = useState(false);

const onToggleModal = () => {
  setIsOpen((prev) => !prev);
};
```

만약 카운트를 구현하기 위해 아래와 같이 default 상태를 정의했을 때,  
`const [state, setState] = useState(0);`

```js
const onClickCount = () => {
  setState(state + 1);
  setState(state + 1);
  setState(state + 1);
};
```

위와 같이 구현하면 state는 이전 값을 기억하지 않고, 결국 큐에 쌓이는 것은 `state + 1`이기 때문에 렌더링이 된 후에 state 값은 1이 된다.

```js
const onClickCount = () => {
  setState((prev) => prev + 1);
  setState((prev) => prev + 1);
  setState((prev) => prev + 1);
};
```

위와 같이 구현하면 임시 저장공간에 있는 값이 나오기 때문에 렌더링 이후 나오는 값은 3이 된다.

```js
const onClickCount = () => {
  setState((prev) => prev + 1);
  setState((prev) => prev + 1);
  setState(state + 1);
  setState((prev) => prev + 1);
  setState((prev) => prev + 1);
};
```

위와 같이 구현하면 `state + 1` 이전에 실행된 setState는 사라지며 `state + 1` 부터 기억되기 때문에 렌더링 이후 나오는 값은 3이 된다.

## setState(0)과 setState([])

**Q1. setCount(0) 으로 상태를 업데이트했다. 이 때 초기 지정한 값과 동일하게 상태를 업데이트 했다면 렌더링이 될까?**

```js
const [count, setCount] = useState(0);

useEffect(() => {
  setCount(0);
}, []);
```

**Q2. setCount([]) 으로 상태를 업데이트했다. 이 때 초기 지정한 값과 동일하게 상태를 업데이트 했다면 렌더링이 될까?**

```js
const [list, setList] = useState([]);

useEffect(() => {
  setList([]);
}, []);
```

1번의 답은 X이고, 2번의 답은 O다. 리액트의 공식 문서에서는 상태 비교 시, `Object.is`를 사용한다고 말하고 있다. Object.is()는 JavaScript에서 두 값이 같은지를 비교하는 메서드로, 값의 동등성을 비교할 때 사용하며 이 메소드는 `===` 과 비슷하다. 하지만 다른 점이 있다면 NaN과 NaN을 비교할 때 true를 return 하며 +0과 -0을 비교하면 false를 return 한다는 점이다.

아래와 같이 할 경우 +0, -0을 다른 값으로 처리하여서 렌더링을 하게 된다.

```js
const [count, setCount] = useState(+0);

useEffect(() => {
  setCount(-0);
}, []);
```

즉, 1번 같은 경우 `Object.is(0, 0)`은 true이기 때문에 같은 값으로 판단하여 렌더링을 하지 않는 것이며, 2번에서는 `Object.is([], [])`는 false이기 때문에 다른 값으로 판단하여 렌더링을 하는 것이다.
