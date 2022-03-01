---
title: "[React] Virtual DOM(가상 DOM)"
categories: React
---

※ 가상 DOM을 이해하기 위해선 우선 [DOM](https://github.com/MGanom/Studying/blob/main/JavaScript/DOM.md)에 대해 알아야 한다.

## Virtual DOM (가상 DOM)이란?

가상 DOM이란 말 그대로 가상 Document Object Model, 즉 가상 문서 객체 모델을 의미한다.  
일반적으로 존재하는 DOM과는 별개로 React에서는 실제 DOM과 똑같은 구조의 DOM을 생성하여 이를 활용한다.

### 가상 DOM의 작동 순서

1. 실제 DOM과 똑같은 구조의 가상 DOM을 생성한다.
2. 가상 DOM을 렌더한다.
3. 실제 DOM에 변화를 줄 행동이 감지되면 이를 가상 DOM에 먼저 업데이트 한다.
4. 변화에 따른 가상 DOM을 리렌더한다.
5. 변화가 반영 되지 않은 실제 DOM과 변화가 반영 된 가상 DOM을 비교한다.
6. 두 DOM 간에 다른 부분을 감지하여 달라진 부분만 실제 DOM에 반영한다.
7. 변화가 발생한 부분만 리렌더한다.

### 가상 DOM의 장점

DOM에 변화를 줄 행동이 일어날 때 마다 DOM 전체를 탐색하고 리렌더하는 것은 메모리적으로나 속도적으로 효율적이지 못하다.  
하지만 가상 DOM을 활용하게 되면 실제 DOM에서는 전체를 탐색하거나 리렌더하는 절차가 생략되고 변화된 부분만 리렌더하면 되기 때문에  
성능적인 측면에서 효율적이고, 또한 부분 렌더만 이루어지기 때문에 SPA (Single Page Application) 구현이 가능하게끔 한다.
