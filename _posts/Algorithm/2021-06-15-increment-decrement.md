---
title: "Increment & Decrement Operator(증감 연산자)"
categories: Algorithm
---

## 증감 연산자란?

증감 연산자란 피연산자를 일정 값으로 증가시키거나 감소시킬 때 사용하는 연산자이며 피연산자가 1개인 단항연산자이다.

```jsx
let i = 0;
let i2 = 0;

// Prefix increment(접두증가연산자) 예시
let obj = { ["++i = " + ++i]: "i = " + i };
let obj2 = { ["++i = " + ++i]: "i = " + i };

console.log("접두증가연산자");
// ++i는 i에 1을 더한 후의 값을 반환하고 i에 1을 더한다.
// i = 0에 1을 더한 후의 값을 반환하여 '++i = '과 연결하고 i에 1을 더해줌
console.log(obj);
// i = 1에 1을 더한 후의 값을 반환하여 '++i = '과 연결하고 i에 1을 더해줌
console.log(obj2);

// Postfix increment(접미증가연산자) 예시
let obj3 = { ["i2++ = " + i2++]: "i2 = " + i2 };
let obj4 = { ["i2++ = " + i2++]: "i2 = " + i2 };

console.log("접미증가연산자");
// i++은  i에 1을 더하기 전의 값을 반환하고 i에 1을 더한다.
// i2 = 0에 1을 더하기 전의 값을 반환하여 'i2++ = '과 연결하고 i2에 1을 더해줌
console.log(obj3);
// i2 = 1에 1을 더하기 전의 값을 반환하여 'i2++ = '과 연결하고 i2에 1을 더해줌
console.log(obj4);

console.log("===============");

console.log("접두증가연산자");
let j = 0;
let sum = 1 + ++j;

//1+1 계산이되고, j=1로 증가
console.log("1 + ++j = ", sum);
console.log("j = ", j);

console.log("접미증가연산자");
let j2 = 0;
let sum2 = 1 + j2++;

//1+0 계산이되고, j2=1로 증가
console.log("1 + j2++ = ", sum2);
console.log("j2 = ", j2);
```

![image](https://user-images.githubusercontent.com/80687334/122008462-0c7a7b80-cdf4-11eb-9a4c-c710f2cb42a1.png)
