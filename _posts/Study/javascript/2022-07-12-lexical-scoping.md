---
title: "[JavaScript] 렉시컬 스코핑(Lexical Scoping)"
categories: JavaScript
---

## 렉시컬 스코핑이란?

렉시컬 스코핑이란 함수가 호출되는 시점에 따라 상위 스코프를 결정하는 동적 스코핑과 반대되는 개념으로써, 함수가 선언되는 시점에 따라 상위 스코프를 결정하는 정적 스코핑을 의미한다. 즉, 자바스크립트의 경우 렉시컬 스코핑을 따르기 때문에 상위 스코프는 함수가 선언되는 시점에 따라 결정된다고 보면 된다.

### 예시

```js
const value = "global";

function first() {
  const value = "local";
  console.log(value);
  second();
}

function second() {
  console.log(value);
}

first(); // 'local' 'global'
second(); // 'global'
```

함수 `first`와 `second`는 모두 전역에서 선언되었으며, 렉시컬 스코핑을 따르기 때문에 상위 스코프는 전역 뿐이다. 따라서, `first` 함수는 다음과 같이 동작한다.

1. `console.log(value)`는 지역 스코프인 `first` 함수 내부에서 `value`를 탐색한다.
2. 지역 스코프에 값이 존재하므로 이 값(`"local"`)을 출력한다.
3. `second` 함수를 호출한다.
4. `console.log(value)`는 지역 스코프인 `second` 함수 내부에서 `value`를 탐색한다.
5. 지역 스코프에 값이 존재하지 않으므로 상위 스코프인 전역에서 `value`를 탐색한다.
6. 해당 스코프에 값이 존재하므로 이 값(`"global"`)을 출력한다.

만약 동적 스코핑을 따랐다면 `second` 함수가 호출되는 시점을 기준으로 상위 스코프를 결정하게 되어 5번부터 6번까지의 과정이 달랐을 것이다.

5. 지역 스코프에 값이 존재하지 않으므로 상위 스코프인 **`first` 함수 내부**에서 `value`를 탐색한다.
6. 해당 스코프에 값이 존재하므로 이 값(`"local"`)을 출력한다.

참고: [MDN - Scope](https://developer.mozilla.org/ko/docs/Glossary/Scope){:target="\_blank"}
