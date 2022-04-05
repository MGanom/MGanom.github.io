---
title: "[JavaScript] Intersection Observer API"
categories: JavaScript
last_modified_at: 2022-04-01
---

## Intersection Observer API란?

Intersection observer API란 어떠한 특정한 요소를 `target`으로 설정하여 `observer`가 그 `target`이 상위 요소 혹은 뷰포트와 교차가 발생하는지를 비동기적으로 관찰(`observe`)하게 하는 API이다.  
이 API가 소개되기 전에는 스크롤의 위치를 관찰하거나, `Element.getBoundingClientRect()`를 사용하여 뷰포트 대비 요소의 위치에 따른 코드 실행을 구현하는 등의 방법으로 스크롤 위치 기반 코드를 구현해야 했는데, 이러한 방법들은 성능면에서 문제를 일으키는 경우가 빈번했었다.

## Intersection Observer API 사용하기

Intersection observer API를 사용하기 위해선 우선 관찰자가 관찰할 `target`을 생성하고, `IntersectionObserver` interface로부터 `IntersectionObserver()` 생성자를 통해 `target`을 관찰할 새로운 `IntersectionObserver` 객체를 생성해야 한다.

### target

```js
const target = document.querySelector("#target");
```

관찰자가 intersection이 발생하는지 관찰할 대상이다. 관찰자는 `target`이 뷰포트나 특정 요소와 교차하는 지에 대해 관찰을 하다가 교차가 발생하면 인자에 전달된 콜백 함수를 실행한다.

### IntersectionObserver()

```js
const observer = new IntersectionObserver(/*callback*/, /*option*/);
```

이 생성자에는 `callback`과 `option`에 해당하는 인자 두 개를 전달할 수 있다.

#### callback

```js
const onIntersect = (entries, observer) => console.log("Intersected!");
```

관찰자가 어떠한 `target`의 관찰을 시작하거나, 혹은 관찰하는 `target`의 교차 상태가 변했을 때 호출 될 콜백 함수이다.  
이 콜백 함수는 `entries`와 `observer`가 인자로 전달된다. `entries`는 교차 상태가 변화한 `target`들의 배열이고 `observer`는 이 콜백 함수를 호출한 `IntersectionObserver`를 의미한다.

#### entries

`entries`에는 교차 상태가 변화한 `target`들이 배열로 들어오는데, 이 배열의 `target`들에는 현재 상태에 대한 속성들이 존재한다.

##### boundingClientRect

`target` 요소의 크기와 요소의 테두리 위치를 나타낸다.

##### intersectionRatio

`target` 요소가 `root` 요소와 교차하고 있는 비율을 0.0부터 1.0의 숫자로 나타낸다.

##### intersectionRect

`target` 요소가 화면에 보여지고 있는 크기와 요소의 테두리 위치를 나타낸다.

##### isIntersecting

`target` 요소가 교차하고 있는지를 boolean으로 나타낸다.

##### rootBounds

`root` 요소의 크기와 요소의 테두리 위치를 나타낸다.

##### target

`target` 요소를 나타낸다.

##### time

문서의 생성 시간을 기준으로 교차 상태가 변화한 시간을 나타낸다.

#### option

```js
const options = {
  root: document.querySelector("#scrollArea"),
  rootMargin: "0px",
  threshold: 1.0,
};
```

관찰자의 콜백 함수가 호출 되는 상황을 정의하는 부분이다.  
`root`는 `target`의 조상 요소 중 교차 여부를 판단할 대상을 설정해줄 수 있다. 기본값은 뷰포트이다.  
`rootMargin`은 `root`가 가질 margin 값으로, `root`의 측면마다 경계의 크기를 조절할 수 있다. 기본값은 0이다.  
`threshold`는 관찰자가 `root`와 `target` 간의 교차가 이루어졌다고 판단하는 기준으로써, 만약 `0.3`으로 설정해준다면 `target`요소가 `root`에서 30%만큼 보였을 때 교차가 이루어졌다고 판단한다. 기본값은 0이다.

#### IntersectionObserver의 메서드

##### observe

```js
observer.observe(target);
```

관찰자가 `target`의 관찰을 시작한다.

##### unobserve

```js
observer.unobserve(target);
```

관찰자가 `target`의 관찰을 중단한다.

##### disconnect

```js
observer.disconnect();
```

관찰자가 모든 관찰을 중단한다.

참고: [MDN - Intersection Observer API](https://developer.mozilla.org/ko/docs/Web/API/Intersection_Observer_API){:target="\_blank"}
