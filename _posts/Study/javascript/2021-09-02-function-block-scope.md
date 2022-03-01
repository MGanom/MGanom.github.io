---
title: "[JavaScript] Function Scope & Block Scope(함수 스코프 & 블록 스코프)"
categories: JavaScript
---

## Function Scope(함수 스코프)

함수가 생성되면 그 함수에 대한 새로운 scope가 생기게 되는데 이를 함수스코프라고 한다.  
함수스코프 내에 있는 변수 등은 함수 내에서만 접근이 가능하기 때문에 함수 밖에서 접근을 시도할 시 오류가 발생한다.

```jsx
function scopeF() {
  var secret = "hello";
}
console.log(secret);
// result
// ReferenceError: secret is not defined
```

## Block Scope(블록 스코프)

블록({})이 생기게 되면 새로운 scope가 생기게 되는데 이를 블록스코프라고 한다.  
본래 자바스크립트는 함수스코프를 기반으로 했었지만, let 과 const의 등장으로 블록스코프 생성 또한 가능해졌다.

var의 경우,

```jsx
for (var i = 0; i < 5; i++) {
  console.log(i);
}
console.log("i=", i);
// result
// 'i=' 5
```

와 같이 블록 밖에서 접근하는 것이 가능해 i의 최종값인 5를 출력할 수 있지만,

let이나 const의 경우,

```jsx
for (let i = 0; i < 5; i++) {
  console.log(i);
}
console.log("i=", i);
// result
// ReferenceError: i is not defined
```

처럼 블록 밖에서 접근하는 것이 불가능해 오류가 발생한다.  
이는 let과 const가 블록({})안에 있는 내용을 다른 스코프, 즉 블록스코프를 형성했다고 볼 수 있는 것이다.

## 정리

- var는 함수스코프 내에서 선언되면 외부스코프에서 접근할 수 없으나,  
  블록스코프 내에서 선언되면 외부스코프에서 접근할 수 있다.  
  즉, var는 함수 내부에서 선언된 경우에만 지역 변수로 인정한다.
- let과 const는 함수스코프 내에서 선언되든 블록스코프에서 선언되든 외부스코프에서 접근할 수 없다.  
  즉, let과 const는 함수 내부는 물론 블록 내부에서 선언된 경우 지역 변수로 인정한다.
