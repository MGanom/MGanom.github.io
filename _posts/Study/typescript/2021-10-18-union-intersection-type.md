---
title: "[TypeScript] Union & Intersection Type(유니온 & 인터섹션 타입)"
categories: TypeScript
---

## Union Type(유니온 타입)

### Basics

```jsx
type Direction = "left" | "right" | "up" | "down";

function move(direction: Direction) {
  console.log(direction);
}
move("left"); // 'left'
move("back"); // error
```

유니언 타입은 이와 같이 타입 여러가지 중 하나를 선택해서 타입이 정의되도록 하는 OR의 역할을 한다고 볼 수 있다.

```jsx
type SuccessState = {
  response: {
    body: string,
  },
};

type FailState = {
  reason: string,
};

type LoginState = SuccessState | FailState;

function printLoginState(state: LoginState) {
  if ("response" in state) {
    console.log(`${state.response.body}`);
  } else {
    console.log(`${state.reason}`);
  }
}
```

예시로 로그인을 구현한다고 할 때, 이런 식으로 들어오는 아이디와 패스워드에 따라 LoginState라는 타입이 무엇인지를 판단하고  
그 LoginState가 어떤 타입인지를 또 판단해서 원하는 결과가 출력되도록 구현할 수 있다.

### Discriminated Union Type

코드의 효율성을 개편하기 위해 사용할 수 있는 방법으로는 Discriminated Union Type이 있다.  
위에서 언급한 코드에서는 `SuccessState`와 `FailState`에 담겨 있는 정보의 키 값이 다르기에 둘을 구분하기 위해서는  
if문을 활용하는 등의 방법을 통해 들어오는 정보를 구별해 줄 코드를 작성해줘야 한다.

```jsx
function printLoginState(state: LoginState) {
  if ("response" in state) {
    console.log(`${state.response.body}`);
  } else {
    console.log(`${state.reason}`);
  }
}
```

이런 식으로 `SuccessState`에만 존재하는 `'response'`라는 정보가 들어왔다면 `response`의 `body`를 불러오고,  
그렇지 않다면 `FailState`에만 존재하는 `reason`에 대한 내용을 불러오도록 코드를 작성해야 한다.  
만약 타입이 여러가지였다면 모든 상황에 맞는 if문을 작성해줘야 하기 때문에 직관성도 떨어졌을 것이다.

하지만 Discriminated Union Type을 활용한다면,

```jsx
type SuccessState = {
  result: "success", // 공통적인 result 추가
  response: {
    body: string,
  },
};

type FailState = {
  result: "fail", // 공통적인 result 추가
  reason: string,
};

type LoginState = SuccessState | FailState;

function printLoginState(state: LoginState) {
  if (state.result === "success") {
    // 두 타입 모두 result가 존재하기 때문에 state.result는 항상 존재
    console.log(`${state.response.body}`);
  } else {
    console.log(`${state.reason}`);
  }
}
```

이런 식으로 공통적인 `result`를 추가해줌으로써 좀 더 직관적이고 효율적인 코드를 작성할 수 있게 된다.

## Intersection Type(인터섹션 타입)

### Basics

```jsx
type Student = {
  name: string,
  score: number,
};

type Worker = {
  workerId: number,
  work: () => void,
};

function intern(person: Student & Worker) {
  console.log(person.name, person.workerId, person.score, person.work());
}

intern({
  name: "Moon",
  score: 100,
  workerId: 1234,
  work: () => {},
});
```

Union Type이 OR와 같은 역할을 하여 여러 타입 중 하나를 고른다면, Intersection Type은 AND와 같은  
역할을 하여 여러 타입을 합치는 역할을 한다고 볼 수 있다. `intern`이라는 함수에서 `Student`와 `Worker`라는  
두 타입을 `&`로 묶었기에 호출을 할 때 두 타입에 담긴 정보를 모두 제공해줘야 하고 그렇지 않으면 오류가 발생한다.
