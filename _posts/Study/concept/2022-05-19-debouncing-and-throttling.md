---
title: "디바운싱 & 쓰로틀링 (Debouncing & Throttling)"
categories: Concept
---

## 디바운싱과 쓰로틀링이 필요한 이유

디바운싱과 쓰로틀링은 비동기 작동 시 작동의 횟수에 제한을 걸기 위해 사용된다. 예를 들어, 서버의 DB로부터 정보를 받아 자동 완성 검색 기능을 구현할 때 타자를 한번 입력할 때마다 매번 서버로 통신 요청이 가는 경우, 그리고 무한 스크롤 기능을 구현하기 위해 스크롤의 위치를 감지하려고 scroll 이벤트를 걸어놨을 때 스크롤을 움직일 때 마다 이벤트가 발생하는 경우 등이 있다.

## 디바운싱 (Debouncing)

디바운싱이란 타이머를 정해 놓고 입력이 발생할 때마다 타이머가 시작 되어 입력이 멈춘 후 일정 시간이 지나면 요청을 보내는 방식이다.

### 예시

```html
<input id="input" />
```

```js
document.querySelector("#input").addEventListener("input", (e) => {
  console.log(e.target.value);
});
```

이 상태에서 input에 '문가'라고 입력을 해보자.

![image](https://user-images.githubusercontent.com/80687334/169377154-b93d8599-551f-43f7-8a2a-c85e2c33a58e.png)

이런 식으로 자음과 모음이 하나 입력될 때 마다 출력이 된다.

하지만 여기에

```js
let timer;
document.querySelector("#input").addEventListener("input", (e) => {
  if (timer) {
    clearTimeout(timer);
  }
  timer = setTimeout(function () {
    console.log(e.target.value);
  }, 200);
});
```

와 같은 디바운싱을 적용해보면,

![image](https://user-images.githubusercontent.com/80687334/169380690-c806c7a1-a759-47ed-8f90-facc96e38e4f.png)

처럼 200ms 내에 입력된 것이 한번에 묶여나오게 된다. 예시의 경우 '문'을 200ms 내에 입력하고 기다렸다가 '가'를 200ms 내에 입력하고 기다린 것이다.

## 쓰로틀링 (Throttling)

쓰로틀링이란 타이머를 정해 놓고 입력이 발생하는 순간부터 타이머가 시작 되어 타이머가 종료되는 순간 요청을 보내는 방식이다.

### 예시

```js
let timer;
document.querySelector("#input").addEventListener("input", (e) => {
  if (!timer) {
    timer = setTimeout(() => {
      timer = null;
      console.log(e.target.value);
    }, 1000);
  }
});
```

상술했던 요소에 이와 같은 쓰로틀링을 설정하고 '문가'라고 입력해 보자.

![image](https://user-images.githubusercontent.com/80687334/169382622-112772e4-fead-4131-8b82-b5d650babe14.png)

1000ms 안에 '문가'라는 입력을 마쳤기 때문에 중간 입력이 모두 생략되고 '문가'만 콘솔된 모습이다.
