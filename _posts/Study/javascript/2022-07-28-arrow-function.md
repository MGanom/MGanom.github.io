---
title: "[JavaScript] 화살표 함수(Arrow Function)"
categories: JavaScript
---

# 화살표 함수(Arrow Function)란?

기본적으로 화살표 함수는 일반 함수를 보다 간략한 방법으로 선언할 수 있도록 해준다.

```js
function add1(x, y) {
  return x + y;
}

const add2 = (x, y) => x + y;

const add3 = (x, y) => {
  return x + y;
};

add1(1, 2); // 3
add2(5, 7); // 12
add3(2, 3); // 5
```

예시에 있는 함수들은 모두 같은 작동을 한다. 예시에서 볼 수 있듯이, 화살표 함수는 실행문이 한 줄의 `return`으로만 이루어져 있다면 중괄호와 `return`을 생략할 수 있다.

단, 객체를 반환하는 상황에서 중괄호와 `return`을 생략하려면 중괄호의 역할이 구분이 가능하도록 대괄호를 사용해줘야 한다.

```js
function calculator1(x, y) {
  return { add: x + y, sub: x - y };
}

const calculator2 = (x, y) => ({ add: x + y, sub: x - y });

calculator1(1, 2).add; // 3
calculator1(1, 2).sub; // -1
calculator2(5, 7).add; // 12
calculator2(5, 7).sub; // -2
```

# 일반 함수와 화살표 함수의 차이

일반 함수와 화살표 함수의 가장 큰 차이는 `this`에 있다. 일반 함수의 `this`는 함수 호출 방식에 따라 값이 동적으로 결정되지만, 화살표 함수의 `this`는 함수를 선언할 때에 값이 정적으로 결정된다. 즉, 일반 함수와 달리 화살표 함수의 `this`는 언제나 상위 스코프의 `this`를 가리키게 되는 것이다.

## 일반 함수에서의 `this`

```js
function BaseNumber(number) {
  this.number = number;
}

BaseNumber.prototype.add = function (arr) {
  // this (1)
  return arr.map(function (num) {
    // this (2)
    return this.number + num;
  });
};

const baseNumber = new BaseNumber(5);

console.log(baseNumber.add([1, 2, 3])); // [NaN, NaN, NaN]
```

이 코드의 경우 `this (1)`은 생성자 함수인 `BaseNumber`를, `this (2)`는 전역 객체 `window`를 가리키게 된다. 즉, `this (2)`의 값이 `undefined`가 된다는 것이다.

이러한 상황에서 `this (2)`의 값이 `BaseNumber`를 가리키게 하려면

```js
function BaseNumber(number) {
  this.number = number;
}

BaseNumber.prototype.add = function (arr) {
  // this
  const instanceThis = this;
  return arr.map(function (num) {
    // instanceThis
    return instanceThis.number + num;
  });
};

const baseNumber = new BaseNumber(5);

console.log(baseNumber.add([1, 2, 3])); // [6, 7, 8]
```

이렇게 생성자 함수의 인스턴스에서 `this` 값을 새로운 값(예시의 경우 `instanceThis`)에 할당하여 사용하는 방법도 있고,

```js
function BaseNumber(number) {
  this.number = number;
}

BaseNumber.prototype.add = function (arr) {
  // this (1)
  return arr.map(function (num) {
    // this(2)
    return this.number + num;
  }, this);
};

const baseNumber = new BaseNumber(5);

console.log(baseNumber.add([1, 2, 3])); // [6, 7, 8]
```

`map` 함수의 두 번째 인자로 인스턴스의 `this`를 전달하여 `map` 함수 내의 `this`를 명시해줄 수도 있고,

```js
function BaseNumber(number) {
  this.number = number;
}

BaseNumber.prototype.add = function (arr) {
  return arr.map(
    function (num) {
      return this.number + num;
    }.bind(this)
  );
};

const baseNumber = new BaseNumber(5);

console.log(baseNumber.add([1, 2, 3])); // [6, 7, 8]
```

`bind`를 활용하여 함수에 대한 `this`를 바인딩하는 방법도 있다.

하지만 이 방법들 모두 추가적인 코드를 작성해야 하기 때문에 번거로울 수 있는데, 이 때 화살표 함수를 사용하여 해결할 수 있다.

## 화살표 함수에서의 `this`

앞서 말했듯이 화살표 함수는 선언할 때 `this`의 값이 상위 스코프의 `this`로 정적으로 결정된다.

```js
function BaseNumber(number) {
  this.number = number;
}

BaseNumber.prototype.add = function (arr) {
  // this (1)
  return arr.map(
    (num) =>
      //this (2)
      this.number + num
  );
};

const baseNumber = new BaseNumber(5);

console.log(baseNumber.add([1, 2, 3])); // [6, 7, 8]
```

화살표 함수로 선언된 콜백 함수는 `this (2)`의 값이 상위 스코프인 생성자 함수의 인스턴스의 `this`로 정적으로 결정 되기 때문에 `this (1)`의 값과 같아진다.

참고: [MDN - Arrow Functions](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Functions/Arrow_functions){:target="\_blank"}  
참고: [PoiemaWeb](https://poiemaweb.com/es6-arrow-function){:target="\_blank"}
