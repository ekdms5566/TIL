# redux-persist 적용하여 새로고침 시에도 store 유지하기

목차  
[redux-persist란?](#📍-redux-persist란)  
[redux-persist 적용하기](#📍-redux-persist-적용하기)

<hr />

## 📍 redux-persist란?

redux-toolkit을 사용하여 store에 저장한 상태를 관리하다 보면 새로고침을 했을 때 상태가 모두 초기화된다. 이러한 경우 redux-persist를 사용하면 해당 문제를 해결할 수 있다.

redux-persist를 사용하면 redux의 상태를 유지하고 지속성을 갖도록 도와주는 라이브러리다. 해당 라이브러리를 사용하면 로컬 스토리지, 세션 스토리지 등의 옵션을 선택하여 상태를 저장하고 새로고침 또는 애플리케이션을 종료해도 상태가 사라지는 것을 막을 수 있다.

```bash
# 설치 방법

npm i redux-persist
yarn add redux-persist
```

## 📍 redux-persist 적용하기

```js
// sessionStorage에 store 저장
import storageSession from "redux-persist/lib/storage/session";

// localStorage에 store 저장
import storage from "redux-persist/lib/storage";
```

localStorage 또는 sessionStorage 중 상태를 저장할 저장소에 따라 import 한다.

로컬 스토리지에 저장된 데이터는 브라우저를 종료하고 시작해도 데이터가 유지되지만, 세션 스토리지는 브라우저를 종료하면 데이터가 삭제된다. 본인의 필요에 따라 선택해서 사용하면 된다. 나같은 경우는 새로고침 시에만 데이터를 유지하고 브라우저 종료 시에는 상태가 사라져도 되는 값이기 때문에 세션 스토리지를 사용했다.

```js
const persistConfig = {
  key: "root",
  storageSession,
  whitelist: ["modifyEquip", "labControl"],
  // blacklist: ["datepicker"],
};
```

persistConfig는 persist의 동작을 구성하고 제어할 수 있는 역할이다. 사용할 수 있는 속성은 아래와 같은 것들이 있다.

`key`: persist가 사용하는 고유한 식별자  
`storage`: 데이터를 영구 저장하기 위해 사용되는 저장소. 주로 localStorage나 sessionStorage를 사용힌디.  
`whitelist` : 영속성을 유지할 리듀서 목록을 지정. whitelist에 지정된 리듀서만 영속성을 유지한다.  
`blacklist` : blacklist는 지정된 리듀서를 제외하고 나머지만 영속성을 유지한다.  
`timeout`: 데이터 저장 시간 제한을 설정한다. 지정된 시간 내에 저장이 완료되지 않으면 타임아웃된다.  
`version`: 저장된 데이터의 버전을 관리하기 위한 설정이다. 저장된 데이터가 업데이트된 경우, 이 값을 사용하여 변환 논리를 구현할 수 있다.  
`stateReconciler`: 이전 상태와 새로운 상태를 병합하는 방법을 제어하는 함수. 기본적으로 하나의 객체로 병합하지만 사용자 지정 병합 로직을 구현할 수도 있다.  
`getStoredState`: 저장된 상태를 가져오는 사용자 정의 함수. 기본적으로는 저장소에서 데이터를 로드하지만, 사용자 정의 로직을 구현하여 저장소가 아닌 다른 위치에서 데이터를 가져올 수도 있다.

```js
const rootReducer = combineReducers({
  cartList: cartListReducer,
  authReceive: authReceiveReducer,
  modifyEquip: modifyEquipReducer,
  datePicker: datePickerReducer,
  labControl: labControlReducer,
});
```

`combineReducers`는 여러 개의 reducer를 하나의 rootReducer로 합쳐주는 역할이다. 기존에는 `configureStore` 내부에서 `combineReducers` 기능을 처리하기 때문에 사용할 필요가 없었지만 persist를 사용하기 위해서는 persistStore에 하나의 reducer를 전달하고 반환받은 enhanced reducer를 configureStore에 전달해야 하기 때문에 persist를 사용할 경우에는 `combineReducers`를 사용해야 한다.

```js
const persistedReducer = persistReducer(persistConfig, rootReducer);
```

redux-persist의 `persistReducer` 함수를 통해 지속성이 있는 리듀서(persisted reducer)를 만들어준다. `persistReducer` 함수는 persistConfig 객체를 첫 번째 매개변수로 받고, rootReducer를 두 번째 매개변수로 받아 지속성이 있는 리듀서를 생성하여 상태를 저장 및 복원이 가능하게 한다. 결과적으로 `persistReducer`는 persistConfig와 rootReducer를 기반으로 한 Redux 스토어의 새로운 루트 리듀서가 되는 것이다.

```js
const store = configureStore({
  reducer: persistedReducer,
});
```

이제 `configureStore` 함수를 통해 스토어를 구성한다. 기존에 사용했던 것처럼 reducer 속성에 위에서 생성된 루트 리듀서를 저장해준다.

```js
export const persistor = persistStore(store);
```

그리고 store를 `persistStore` 함수에 인자로 store를 넣으면 persistor 객체를 반환하여 이를 export 해준다.

```js
// index.js
<Provider store={store}>
  <PersistGate loading={null} persistor={persistor}>
    <App />
  </PersistGate>
</Provider>
```

그리고 기존에 index.js 파일의 `<Provider>` 태그 하위에 `<PersistGate>` 태그로 store를 저장해주면 된다. 이 때, `<PersistGate>`에 스토리지에 저장할 persistor를 전달하면 된다. 이 때, loading은 로딩 과정에서 보여줄 컴포넌트를 의미한다.

여기까지 하면 store를 스토리지에 저장되어 새로고침 시에도 사용할 수 있게 되는데, 아마 아래와 같은 에러가 발생할 것이다..

<img src="https://user-images.githubusercontent.com/78911818/244385355-7a69c741-bcdc-4788-bf30-a79162354f44.png" />

이 에러에 대해 이유가 무엇인지 아래 링크를 눌러 확인해본다...

확인해보면 해당 에러는 Redux 액션에서 직렬화할 수 없는(non-serializable) 값이 감지되었을 때 발생하는 경고로 Redux는 상태 및 액션을 직렬화하여 관리하는데, 일부 값은 직렬화할 수 없거나 Redux 상태에 저장하기에 적합하지 않을 수도 있다. 결론은 'register' 경로에 직렬화할 수 없는 값이 감지되었다는 의미였다... 💦

이를 해결하기 위해서 사용하는 것이 바로 redux의 미들웨어다. 미들웨어를 사용하여 액션을 가로채고 처리할 수 있으며 여기에서 직렬화 및 비작렬화 작업을 수행할 수 있다.

```js
import {
  persistStore,
  persistReducer,
  FLUSH,
  REHYDRATE,
  PAUSE,
  PERSIST,
  PURGE,
  REGISTER,
} from "redux-persist";
```

아마 redux-persist에 대한 정보를 찾다보면 위와 같은 `FLUSH`, `REHYDRATE`, `PERSIST`를 적용하는 것을 본 적이 있을 것이다. 그렇다면 각각의 액션은 어떤 역할을 하는 것인지 짧게 정리해보자...

```
FLUSH: Redux Persist의 저장소에 저장된 모든 데이터를 비우는 액션입니다. 주로 데이터 초기화나 로그아웃 시 사용됩니다.

REHYDRATE: Redux Persist의 저장소에서 데이터를 가져와 리덕스 스토어를 복원하는 액션입니다. 저장소에 저장된 데이터를 다시 읽어와 상태를 초기화하는 역할을 합니다.

PAUSE: Redux Persist의 자동 저장을 일시적으로 중지하는 액션입니다. 주로 특정 작업 중에 저장을 일시적으로 중단해야 할 때 사용됩니다.

PERSIST: Redux Persist의 저장을 활성화하는 액션입니다. 주로 일시적으로 중지된 저장을 다시 활성화할 때 사용됩니다.

PURGE: Redux Persist의 저장소에서 특정 리듀서의 데이터를 삭제하는 액션입니다. 특정 데이터를 완전히 제거하고 싶을 때 사용됩니다.

REGISTER: Redux Persist에 리듀서를 등록하는 액션입니다. Redux Persist가 관리해야 할 리듀서를 등록하는 역할을 합니다.
```

이러한 액션에서 필요한 요소들을 선택해서 아래왁 같이 사용하면 된다.

```js
const store = configureStore({
  reducer: persistedReducer,
  middleware: (getDefaultMiddleware) =>
    getDefaultMiddleware({
      serializableCheck: {
        ignoredActions: [FLUSH, REHYDRATE, PAUSE, PERSIST, PURGE, REGISTER],
      },
    }),
});
```

`configureStore` 함수에서 middleware 옵션을 설정하여 사용할 미들웨어를 설정한다. serializableCheck 옵션을 전달하여 직렬화 가능성 검사(serializable check)를 수행하는 방식을 설정하는데 이 때, `ignoredActions` 속성에 직렬화 가능성 검사에서 무시할 액션들을 배열로 전달하면 끝!

이렇게 하면 오류도 안 뜨고 잘 적용된 것도 확인할 수 있다!!!

## <img src="https://user-images.githubusercontent.com/78911818/244419040-75a11327-bf6d-4719-bd1a-ae8eea2fda54.png">

결과적으로 store에 저장한 값이 새로고침하면서 사라지면 왼쪽 이미지처럼 깔끔한(원치 않게) 페이지가 되었지만 persist 적용하니까 오른쪽 이미지처럼 아주 잘 출력된다 🐠🌱

<div style="display: flex; gap: 20px;" >
  <img src="https://user-images.githubusercontent.com/78911818/244276765-2918d457-f00d-4b99-8ae2-41a40ba31f85.png" width="150px">
  <img src="https://user-images.githubusercontent.com/78911818/244276471-5ce078da-5898-476f-b53c-263b4750ede2.png" width="150px">
</div>

<hr />

참고  
[Redux FAQ: Actions](https://redux.js.org/faq/actions#why-should-type-be-a-string-or-at-least-serializable-why-should-my-action-types-be-constants)  
[Working with Non-Serializable Data](https://redux-toolkit.js.org/usage/usage-guide#working-with-non-serializable-data)  
[github redux-persist](https://github.com/rt2zz/redux-persist#basic-usage)
