---
title: "[React Redux] React Redux와 Redux Toolkit 함께 사용하기 - (3)"
categories: React_Redux
---

## Redux Toolkit을 적용하여 React Redux 사용하기

Redux Toolkit이 소개된 이후부터 React Redux는 이와 함께 사용하는 것이 효율적이기 때문에 두 가지를 합쳐서 사용하는 것이 일반적이다. 만약 타입스크립트를 사용한다면 추가적으로 설정해줘야 하는 부분들이 있다.

### configureStore

우선 store를 만들어야 한다.

```jsx
// store.ts
export const store = configureStore({
  reducer: {},
});

export type RootState = ReturnType<typeof store.getState>;
export type AppDispatch = typeof store.dispatch;
```

`configureStore`는 리덕스의 `createStore`를 추상화 한 것이다. 리덕스의 번거로운 설정 과정을 자동화 해주며, 이 안에는 `reducer`, `middleware`, `devTools`, `preloadedState`, `enhancers`를 정의해줄 수 있다.  
마지막 두 줄은 `store`를 정의하고 그 `store`에 있는 메서드의 타입을 활용하여 `RootState`와 `AppDispatch`의 타입을 정의해준 것이다.

### Provider

앱 내의 컴포넌트 어디에서든 store에 접근할 수 있도록 `index.tsx`에서 뿌려준다.

```jsx
// index.tsx
const root = ReactDOM.createRoot(
  document.getElementById('root') as HTMLElement
);
root.render(
  <React.StrictMode>
    <Provider store={store}>
      <App />
    </Provider>
  </React.StrictMode>
)
```

이로써 컴포넌트들은 store에 접근할 수 있게 된다.

### useDispatch, useSelector

리액트 리덕스에서 dispatch와 selector를 사용할 때, 일반적인 경우 그냥 `useDispatch`와 `useSelector`를 바로 사용하면 된다. 하지만 타입스크립트가 적용된 경우 두 메서드를 사용할 때마다 타입 선언을 해줘야 하기 때문에 이러한 번거로움을 최소화하기 위해 커스텀 훅을 만들어주는 게 좋다.

```jsx
// hooks.ts
export const useAppDispatch = () => useDispatch<AppDispatch>()
export const useAppSelector: TypedUseSelectorHook<RootState> = useSelector
```

이렇게 미리 작성을 해두면 `useDispatch`와 `useSelector`를 사용해야 하는 곳에서 `useAppDispatch`와 `useAppSelector`를 사용함으로써 반복적인 타입 선언을 생략할 수 있다.

### createSlice()

리덕스 툴킷에서 제공하는 가장 기본적이고 표준적인 접근 방법인 `createSlice()`를 생성해준다.

```jsx
interface CounterState {
  value: number;
}

const initialState: CounterState = {
  value: 0,
};

export const counterSlice = createSlice({
  name: "counter",
  initialState,
  reducers: {
    increment: (state) => {
      state.value += 1;
    },
    decrement: (state) => {
      state.value -= 1;
    },
    incrementByAmount: (state, action: PayloadAction<number>) => {
      state.value += action.payload;
    },
  },
});

export const { increment, decrement, incrementByAmount } = counterSlice.actions;

export const selectCount = (state: RootState) => state.counter.value;

export default counterSlice.reducer;
```

사용할 초기값의 타입과 값을 선언하고, `createSlice`를 선언한 후 export 해주면 된다.

```jsx
incrementByAmount: (state, action: PayloadAction<number>) => {
      state.value += action.payload;
    },
```

참고로 `action`의 경우 `PayloadAction<T>` 타입을 갖게 되는데, 이는 리덕스 툴킷에서 제공하는 타입이며 제네릭 `T`에는 `action.payload`의 타입을 적어주면 된다.

간혹 타입스크립트가 초기값 `initialState`의 타입을 strict하게 판독하여 오류로 잡아낼 수 있는데 이럴 경우

```jsx
const initialState = {
  value: 0,
} as CounterState
```

처럼 `as`를 활용해주면 된다.

### 컴포넌트에서 사용

```jsx
// component.tsx
export default function Counter() {
  const count = useAppSelector((state) => state.counter.value);
  const dispatch = useAppDispatch();

  const onIncrement = () => {
    dispatch(increment());
  };

  const onDecrement = () => {
    dispatch(decrement());
  };

  const onIncrementByTwo = () => {
    dispatch(incrementByAmount(2));
  };

  return (
    <div>
      <div>{count}</div>
      <button type="button" onClick={onIncrement}>
        ADD
      </button>
      <button type="button" onClick={onDecrement}>
        SUB
      </button>
      <button type="button" onClick={onIncrementByTwo}>
        ADD+2
      </button>
    </div>
  );
}
```

필요한 state를 `useAppSelector`로 불러오고, 사용할 `reducer`들을 `dispatch`에 넣어준 뒤 필요한 곳에 렌더해주면 된다.

참고: [React Redux 공식 문서](https://react-redux.js.org/introduction/getting-started){:target="\_blank"}  
참고: [Redux Toolkit 공식 문서](https://redux-toolkit.js.org/introduction/getting-started){:target="\_blank"}
