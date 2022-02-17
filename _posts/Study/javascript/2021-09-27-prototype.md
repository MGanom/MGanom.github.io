---
title: "Prototype(프로토타입)"
categories: JavaScript
---

## Prototype(프로토타입)이란?

자바스크립트는 객체지향프로그래밍 언어이다.  
하지만 Java, Python, Ruby 등 다른 객체지향프로그래밍 언어들과는 다르게 클래스(Class) 기반의 언어가 아니다.  
자바스크립트는 클래스 문법이 추가된 ECMA6 전까지는 이 문법 조차 존재하지 않았었다.  
그렇다면 자바스크립트는 무엇을 기반으로 객체지향프로그래밍을 하고 있는걸까?  
그것은 바로 프로토타입(Prototype)이다.

자바스크립트의 모든 객체들은 메서드와 속성들을 상속 받기 위한 프로토타입 객체(prototype object)를 가진다.  
그리고 그 프로토타입 객체 또한 객체이기 때문에 또 다시 상위 프로토타입 객체가 존재할 수 있으며 이는 끝없이 연결될 수 있는데,  
이를 프로토타입 체인(prototype chain)이라고 부른다. 이러한 구조를 통해 객체지향프로그래밍이 가능해지기에  
자바스크립트는 프로토타입 기반 언어(prototype-based language)라고 불린다.

다시 말해 우리가 메서드를 사용하거나 속성에 접근하는 등의 행동을 하면 자바스크립트는 이 프로토타입 체인을 타고 올라가며  
메서드와 속성을 탐색해 이들이 정의된 생성자(constructor)의 프로토타입을 찾게 되는 것이다.

## Prototype의 필요성

```jsx
function Example() {
  this.a = 1;
  this.b = 4;
}

const ex1 = new Example();
const ex2 = new Example();

console.log(ex1.a); // result 1
console.log(ex2.b); // result 4
```

앞서 말했듯이 자바스크립트는 클래스 기반의 언어가 아니다. 하지만 이처럼 함수(function)를 통해 클래스를 흉내내는 것은 가능하다.  
하지만 이렇게 코드를 짤 경우 ex1과 ex2는 같은 함수를 상속했음에도 불구하고 함수 내의 a와 b에 대한 정보가 각각 상속 되어  
메모리에는 a 2개, b 2개, 총 4개의 변수가 할당되게 된다. 즉, 이 방법은 메모리 낭비를 초래하여 성능을 저하시킬 수 있다는 뜻이 된다.

그렇다면 이를 어떻게 개선할 수 있을까?  
자바스크립트는 해답을 프로토타입에서 찾았다.

```jsx
function Example() {}

Example.prototype.a = 1;
Example.prototype.b = 4;

const ex1 = new Example();
const ex2 = new Example();

console.log(ex1.a); // result 1
console.log(ex2.b); // result 4
```

우선 Example이라는 함수가 생성되는 순간 Example 함수는 constructor를 갖게 되고, 그렇기 때문에 `new`를 사용할 수 있게 된다.  
그리고 .prototype으로 함수 내의 프로토타입을 정의해주고 new를 사용하여 변수에 할당해주면 이전의 코드와는 다르게  
메모리에 a와 b가 새로 할당되는 것이 아닌 보이지 않는 Object 안에서 프로토타입을 통해 하나씩 꺼내오는 방식으로 작동하게 된다.  
즉, 메모리 관리에도 용이하고 좀 더 체계적인 환경이 갖춰지게 되는 것이다.

---

## Prototype의 원리

프로토타입의 원리는 간단하면서도 복잡해서 일단 간단하게 정리해보자면,  
우선 프로토타입은 Prototype Object(프로토타입 객체)와 Prototype Link(프로토타입 링크) 두 가지로 이루어져 있다.

### Protoype Object (프로토타입 객체)

```jsx
function Example() {}

const ex = new Example();
```

자바스크립트 내의 객체는 이와 같이 함수로부터 생성된다. 심지어

```jsx
const newObject = {};
```

또한 이미 자바스크립트에서 기본으로 제공하는 함수인 `function Object() { [native code] }` 로부터

```jsx
const newObject = new Object();
```

를 통해 생성되는 객체이며, 함수와 배열도 예외는 아니다.

이것이 가능한 이유는 자바스크립트에서 함수가 생성 되는 순간 해당 함수에 2가지 일이 일어나기 때문이다.

1. constructor(생성자) 자격이 부여되어 `new`를 통해 새로운 객체를 생성할 수 있게 된다.
2. 함수의 생성자와 `___proto___`(프로토타입 링크)를 속성으로써 가진 프로토타입 객체가 생성되고,  
   함수와 이 객체가 prototype이라는 속성을 통해 연결된다.

즉, constructor는 함수가 생성될 때 생성된 함수를 가리키고 있고 있기 때문에 `.prototype`을 통해서  
프로토타입 객체에 접근이 가능해지고, 사용자는 이것으로 프로토타입 객체에 속성을 추가하거나 삭제할 수 있게 되는 것이다.

```jsx
function Example() {}

Example.prototype.a = 1;

const ex1 = new Example();

console.log(ex1.a); // result 1
```

그런데 이 예시와 같이 Example에 prototype으로 접근하여 1을 추가해줬을 뿐,  
정작 ex1에는 a라는 속성을 추가하지 않았음에도 a의 값에 접근할 수 있는 이유가 무엇일까?

### Prototype Link (프로토타입 링크)

ex1에 실제로 a라는 속성이 없음에도 접근할 수 있는 이유는 바로 이 프로토타입 링크 때문이다.  
앞서 말했던 prototype 속성은 함수에만 있던 것과는 달리 `__proto__`(프로토타입 링크) 속성은 모든 객체가 갖고 있다.  
그리고 이 속성은 객체가 생성되는 순간 조상이었던 함수의 프로토타입 오브젝트를 가리킨다.  
즉, ex1 객체가 가리키는 프로토타입 오브젝트는 Example 함수의 것이라는 뜻이다.  
a라는 속성을 갖고 있지 않은 ex1 객체는 이 링크들이 연결된 프로토타입 체인을 따라 상위 프로토타입을 탐색하게 되고  
a를 발견하면 이에 접근하게 된다. 만약 상위 프로토타입에 a가 없었다면? undefined가 반환됐을 것이다.
