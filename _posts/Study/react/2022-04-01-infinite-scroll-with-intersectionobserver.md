---
title: "[React] 무한스크롤 구현 (Intersection Observer)"
categories: React
---

## 무한스크롤(Infinite Scroll)이란?

무한스크롤이란 사용자가 화면의 스크롤을 일정 수준까지 이동하면 새로운 데이터를 추가로 페칭(fetching)하여 화면에 나타냄으로써 말 그대로 스크롤을 무한히 이동하며 새로운 데이터를 볼 수 있는 것을 의미한다. 이를 구현하기 위한 방법엔 라이브러리 사용, 스크롤 위치 판단, intersection observer API 활용 등 다양한 방법이 있는데, 이 글에선 intersection observer API를 활용한 방법을 서술하려 한다.

### Intersection Observer API

[Intersection Observer API에 대한 글](https://moon-ga.github.io/javascript/intersectionobserver/)에서 설명했듯이, 이 API는 `target` 요소가 `root` 요소와 교차가 일어나는지를 판단하여 콜백 함수를 실행할 수 있다. 따라서 `target` 요소를 적절한 위치에 배치를 한다면 사용자가 스크롤을 이동하는 것에 따라 데이터 페칭을 구현할 수 있다.

### 구현 예시

1. `target` 생성

   ```jsx
   const [target, setTarget] = useState(null);

   return (
     <div>
       <div ref={setTarget}>This is Target.</div>
     </div>
   );
   ```

   HTML이 생성되고 그 요소들 중 하나가 교차 상태를 판별할 대상인 `target`이 돼야 하므로 `ref`와 `useState`를 활용하여 `target`을 지정해 줬다.

2. `observer` 생성

   ```jsx
   useEffect(() => {
     let observer;
     if (target) {
       observer = new IntersectionObserver();
       observer.observe(target);
     }
   }, []);
   ```

   컴포넌트가 렌더가 완료됨에 따라 `observer`가 생성되어야 하므로 `useEffect`를 활용해야 한다. 또한 `target`이 생성되기 전에 `observe`를 시작할 수 없으므로 조건문을 넣어줬다.

3. 콜백함수 생성

   ```jsx
   const onIntersect = async ([entry], observer) => {
     if (entry.isIntersecting) {
       observer.unobserve(entry.target);
       await /* 데이터 페칭 함수*/
       observer.observe(entry.target);
     }
   };
   ```

   교차 상태가 변화했을 때, 교차된 `target`인 `[entry]`의 속성 중 하나인 `isIntersecting`을 활용하여 교차 상태가 `true`일 때 데이터 페칭이 이루어지도록 함수를 만들었다. 또한 사용자가 데이터 페칭이 완료되기 전에 교차 상태를 여러 번 변화시키는 상황이 발생하지 않도록 `unobserve`를 사용하여 관찰을 중단했다가 데이터 페칭이 완료되면 다시 `observe`를 하도록 했다.

4. 데이터 페칭 함수 생성

   ```jsx
   const fetchData = async () => {
     const response = await fetch(`/api/db/${page}`);
     const data = await response.json();
     setMovies((prev) => prev.concat(data.results));
     page++;
   };
   ```

참고: [문가네 개발 블로그 - Intersection Observer API란?](https://moon-ga.github.io/javascript/intersectionobserver/){:target="\_blank"}
참고: [MDN - Intersection Observer API](https://developer.mozilla.org/ko/docs/Web/API/Intersection_Observer_API){:target="\_blank"}

```

```
