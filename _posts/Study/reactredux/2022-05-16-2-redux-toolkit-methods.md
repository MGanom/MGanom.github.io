---
title: "[React Redux] Redux Toolkit의 메서드 - (2)"
categories: React_Redux
---

## Redux Toolkit의 다양한 메서드

리덕스 툴킷에는 리덕스를 더욱 편리하게 사용할 수 있도록 제공해주는 다양한 메서드들이 있다. 그 중 `createReducer()`, `createAction()`, `createSlice()`에 대해 알아보았다.

### createReducer()

리덕스에서의 `reducer`는 `action`의 타입과 해당 액션 타입에 따른 작동을 정의하여 `switch`와 `case`로 관리해야 할 뿐만 아니라 immutive한 로직을 작성해야 했다. 이는 중첩이 깊어질 수록 코드가 길어지고 실수로 원본 객체를 변형시키는 위험성이 높아지는 등의 여러 문제를 유발하는데, 반면에 `createReducer()`는 mutative한 로직을 작성해도 자동으로 immutative한 업데이트가 이루어져 걱정이 없다. 또한 `builder callback`이라는 것을 활용하여 중첩을 더 효율적으로 해결할 수 있게 해준다.

```jsx
const initialState = { value: 0 };

function counterReducer(state = initialState, action) {
  switch (action.type) {
    case "increment":
      return { ...state, value: state.value + 1 }; // immutative
    case "decrement":
      return { ...state, value: state.value - 1 }; // immutative
    default:
      return state;
  }
}
```

본래라면 이렇게 `...` 연산자를 사용하여 코드를 작성해야 하지만, `createReducer()`를 사용한다면

```jsx
import { createAction, createReducer } from '@reduxjs/toolkit'

interface CounterState {
  value: number
}

const increment = createAction('counter/increment')
const decrement = createAction('counter/decrement')

const initialState = { value: 0 } as CounterState

const counterReducer = createReducer(initialState, (builder) => {
  builder
    .addCase(increment, (state, action) => {
      state.value++ // mutative
    })
    .addCase(decrement, (state, action) => {
      state.value-- // mutative
    })
})
```

`builder callback`으로 이렇게 작성할 수 있는 것이다.

#### Builder Callback

Builder Callback은 `builder` 객체 내의 `addCase`, `addMatcher`, `addDefaultCase` 메서드를 호출하여 리듀서의 액션에 대한 처리를 정의할 수 있게 해준다.

##### addCase(actionCreator, reducer)

액션 타입이 정확히 일치하면 `reducer`로 처리한다.

##### addMathcer(matcher, reducer)

액션에 대해 `matcher`의 패턴과 일치하는지 확인하고, 만약 일치한다면 처리한다. 여러 `addMatcher`들과 패턴이 일치한다면 작성된 순서에 맞춰 차례로 `reducer`로 처리한다.

##### addDefaultCase(reducer)

일치하는 액션 타입이 없는 경우 `reducer`로 처리한다.

### createAction()

리덕스에선 `action`과 `action creator`를 따로 정의해야 했지만 `createAction()`은 이 두 가지를 결합한 형태이다.

```jsx
const INCREMENT = "counter/increment";

function increment(amount) {
  return {
    type: INCREMENT,
    payload: amount,
  };
}
```

처럼 타입과 생성자를 분리해야 했던 것을

```jsx
import { createAction } from "@reduxjs/toolkit";

const increment = createAction("counter/increment");
```

로 결합할 수 있다. 이 경우

```jsx
let action = increment();
console.log(action); // { type: 'counter/increment' }
```

처럼 인자에 아무것도 전달을 하지 않은 상태로 호출할 시 타입이 반환되는 것을 확인할 수 있다. 또,

```jsx
action = increment(10);
console.log(action); // { type: 'counter/increment', payload: 10 }
```

처럼 인자에 값을 전달하면 `payload`에 담기게 된다.

#### Prepare Callbacks

만약 `createAction()`으로 액션을 만들고, 그 액션에 인자 하나를 전달해서 payload를 생성하는 것이 아니라 인자에 따른 결과값으로 생성하고 싶다면 `prepare` 콜백 함수를 `createAction()`의 두 번째 인자로 전달하면 된다.

```jsx
import { createAction } from "@reduxjs/toolkit";

const addTodo = createAction("todos/add", function prepare(text) {
  return {
    payload: {
      text,
      author: "Moon",
      createdAt: new Date().toISOString(),
    },
  };
});

let newTodo = addTodo("Code more");
console.log(newTodo);
// {
//   type: 'todos/add',
//   payload: {
//     text: "Code more",
//     author: "Moon",
//     createdAt: "2022-05-16T11:47:36:571Z"
//   }
// }
```

이렇게 동적으로 원하는 값을 담는 것도 가능하다.

### createSlice()

Redux를 다루기 위해 Redux Toolkit에서 제공하는 가장 기본적이고 표준적인 접근 방법이다.  
`createSlice()`는 이름, 초기값, `reducer` 함수를 담은 객체를 인자로 받아 자동적으로 액션 타입과 액션 생성자를 만들어 주는 메서드이다. 심지어 내부적으로 `createAction()`과 `createReducer()`가 사용되기 떄문에 직접 작성해줄 필요도 없다.

```jsx
import { createSlice, PayloadAction } from '@reduxjs/toolkit'

interface CounterState {
  value: number
}

const initialState = { value: 0 } as CounterState

const counterSlice = createSlice({
  name: 'counter',
  initialState,
  reducers: {
    increment: (state) => {
      state.value++
    },
    decrement: (state) => {
      state.value--
    },
    addNumber: {
      reducer: (state, action: PayloadAction<number>) => {
        state += action.payload
      },
      prepare: (number: number) => {
        return { payload: number}
      },
      extraReducers: (builder) => {
        builder
          .addCase(multiply, (state, action) => {
            state *= action.payload
          })
      }
    }
  },
})

export const { increment, decrement } = counterSlice.actions
export default counterSlice.reducer
```

#### name

```jsx
name: 'counter',
```

액션 타입이 생성될 때 `/` 앞에 붙게 될 이름이다. 즉, 고유성을 부여해 준다고 생각하면 된다. 예시에선 `counter/increment`와 `counter/decrement`가 되는 것이다.

#### initialState

```jsx
initialState,
```

초기값을 의미한다.

#### reducers

```jsx
reducers: {
  increment: (state) => {
    state.value++
  },
  decrement: (state) => {
    state.value--
  },
}
```

액션 타입에 따른 리듀서를 의미한다. 각각의 키 값은 해당 리듀서를 dispatch하기 위한 용도가 된다.

#### Prepare Callback

```jsx
addNumber: {
  reducers: {
    reducer: (state, action: PayloadAction<number>) => {
      state += action.payload
    },
    prepare: (number: number) => {
      return { payload: number}
    }
  }
}
```

`createSlice()`에서도 prepare callback을 사용할 수 있다. `reducer`를 정의할 때 객체로 `reducer`와 `prepare`를 각각 정의해줌으로써 payload에 원하는 값을 전달할 수 있다.

#### extraReducers

```jsx
extraReducers: (builder) => {
  builder.addCase(multiply, (state, action) => {
    state *= action.payload;
  });
};
```

`createSlice()`가 생성한 액션 타입 외에 다른 액션 타입도 처리할 수 있도록 정의해준다. 내부에 정의된 액션만이 아닌 외부 액션에도 반응할 수 있도록 하기 위한 방법이다.

참고: [React Redux 공식 문서](https://react-redux.js.org/introduction/getting-started){:target="\_blank"}  
참고: [Redux Toolkit 공식 문서](https://redux-toolkit.js.org/introduction/getting-started){:target="\_blank"}
