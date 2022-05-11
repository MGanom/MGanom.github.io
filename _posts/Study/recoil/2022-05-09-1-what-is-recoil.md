---
title: "[Recoil] Recoil의 개념 - (1)"
categories: Recoil
last_modified_at: 2022-05-11
---

## Recoil(리코일)이란?

리코일은 리액트 전역 상태 관리 라이브러리 중 하나로, 리액트에 내장되어 있는 전역 상태 관리의 의의를 최대한 유지하면서 단점을 보완하는 것을 목표로 하고 있다.

### 장점

- 지역 상태를 관리만큼 간단한 관리법을 사용한다.
- 높은 호환성으로 인해 리액트의 새로운 기능들과 호환이 된다.
- 상태 정의가 작은 단위로 이루어져 있어 코드 분할에 용이하다.
- 필요한 상태만 업데이트하고 해당 상태를 사용하는 컴포넌트나 원하는 컴포넌트만 업데이트함으로써 다른 컴포넌트들의 불필요한 재렌더를 방지할 수 있다.

## 주요 개념과 사용 방법

리코일은 기본적으로 atoms와 selectors로 이루어져 있으며 이 둘을 활용하여 컴포넌트들로 내려가는 데이터 흐름 그래프를 형성할 수 있다.

### RecoilRoot

Atoms와 selectors를 활용하기에 앞서 리코일을 사용할 컴포넌트를 RecoilRoot로 감싸줘야 한다. 이는 주로 `index.js`나 `App.js`와 같은 루트 컴포넌트에 사용된다.

```jsx
import { createRoot } from "react-dom/client";
import { RecoilRoot } from "recoil";
import App from "./App";

createRoot(document.getElementById("root")).render(
  <RecoilRoot>
    <App />
  </RecoilRoot>
);
```

### Atoms

Atoms는 상태의 단위로써 업데이트와 구독이 가능하다. Atom이 업데이트 되면 이것에 구독된 컴포넌트는 새로운 값으로 재렌더링이 이루어지며 동일한 atom을 사용하는 컴포넌트들은 상태를 공유하게 된다.

#### 사용 방법

Atom은 `atom()`으로 생성할 수 있고, 각각의 atom은 반드시 고유한 `key`를 가져야 하며 기본값인 `default`를 가진다.

```jsx
export const counterState = atom({
  key: "counterState",
  default: 0,
});
```

이렇게 사용할 전역 상태를 선언하고 export한다.

이후 필요한 곳에서 import하여 사용하면 된다.

```jsx
import { counterState } from "../../states";

function Counter() {
  const [counter, setCounter] = useRecoilState(counterState);

  const handleIncrement = () => {
    setCounter((count) => count + 1);
  };

  return (
    <div>
      <div>{counter}</div>
      <button onClick={handleIncrement}>+1</button>
    </div>
  );
}
```

기본적인 모양과 사용 방법은 `useState()`와 유사하며, `useRecoilState(atom)`을 사용해준다는 점만 다르다. 이렇게 atom을 사용하는 순간 이 컴포넌트는 해당 atom에 구독이 되며, 해당 atom이 업데이트되면 컴포넌트 또한 재렌더가 이루어진다.

만약 `counterState`가 여러 컴포넌트에서 사용된다면 이 atom이 업데이트될 시 구독 중인 모든 컴포넌트가 재렌더 된다.

### Selector

Selector는 atom으로부터 파생된 상태를 나타낸다. 이는 항상 동일한 값을 반환하는 부작용이 없는 "순수함수"이다.

```jsx
const counterState = atom({
  key: "counterState",
  default: 0,
});

const doubleCounterState = selector({
  key: "counterSelector",
  get: ({ get }) => {
    get(counterState);
  },
  set: ({ set }, newValue) => {
    set(
      counterState,
      newValue instanceof DefaultValue ? newValue : newValue + 1
    );
  },
});

const [doubleCounter, setDoubleCounter] = useRecoilState(doubleCounterState);
setDoubleCounter((prev) => prev + 1); // 2씩 증가
```

만약 `set`이 없다면 값을 불러오기만 할 수 있다.

참고: [Recoil 공식 문서](https://recoiljs.org/ko/docs/introduction/core-concepts){:target="\_blank"}
