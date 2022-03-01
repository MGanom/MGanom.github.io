---
title: "[TypeScript] Type Alias(타입 앨리어스)"
categories: TypeScript
---

## Type Alias(타입 앨리어스)란?

새로운 타입을 변수처럼 선언하여 사용하는 것을 의미한다.

```jsx
type Word = string;
const name: Word = "Moon";

type Num = number;
const age: Num = "20";
```

이렇게 원시타입을 선언할 수도 있고,

```jsx
type Info = {
  name: string,
  age: number,
};

const student: Info = {
  name: "Moon",
  age: "20",
};
```

이렇게 객체를 선언해줄 수도 있다. 즉, 어떠한 데이터에 대해 구체적인 형태를 정의할 수 있게 된다는 뜻이다.

### String Literal Types

```jsx
type Name = "name";
const name: Name = "name";

type JSON = "json";
const json: JSON = "json";
```

이렇게 타입에다가 문자열을 할당해주는 것을 String Literal Types라고 부른다.
