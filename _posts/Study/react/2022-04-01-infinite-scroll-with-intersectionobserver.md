---
title: "[React] 무한스크롤 구현 (Intersection Observer)"
categories: React
last_modified_at: 2022-04-04
---

## 무한스크롤(Infinite Scroll)이란?

무한스크롤이란 사용자가 화면의 스크롤을 일정 수준까지 이동하면 새로운 데이터를 추가로 페칭(fetching)하여 화면에 나타냄으로써 말 그대로 스크롤을 무한히 이동하며 새로운 데이터를 볼 수 있는 것을 의미한다. 이를 구현하기 위한 방법엔 라이브러리 사용, 스크롤 위치 판단, intersection observer API 활용 등 다양한 방법이 있는데, 이 글에선 intersection observer API를 활용한 방법을 서술하려 한다.

### Intersection Observer API

[Intersection Observer API에 대한 글](https://moon-ga.github.io/javascript/intersectionobserver/)에서 설명했듯이, 이 API는 `target` 요소가 `root` 요소와 교차가 일어나는지를 판단하여 콜백 함수를 실행할 수 있다. 따라서 `target` 요소를 적절한 위치에 배치를 한다면 사용자가 스크롤을 이동하는 것에 따라 데이터 페칭을 구현할 수 있다.

### 구현 예시

1. `target` 생성

   ```jsx
   const [target, setTarget] = useState(null);
   const targetStyle = { width: "100%", height: "200px" };

   return (
     <div>
       <div ref={setTarget} style={targetStyle}>
         This is Target.
       </div>
     </div>
   );
   ```

   HTML이 생성되고 그 요소들 중 하나가 교차 상태를 판별할 대상인 `target`이 돼야 하므로 `ref`와 `useState`를 활용하여 `target`을 지정해 줬다. 또한 요소에 크기를 정해줌으로써 교차 상태를 판별하기 쉽게 해줬다.

2. `observer` 생성

   ```jsx
   useEffect(() => {
     let observer;
     if (target) {
       observer = new IntersectionObserver();
       observer.observe(target);
     }
   }, [target]);
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
   const page = 1;
   const fetchData = async () => {
     const response = await fetch(`/api/db/${page}`);
     const data = await response.json();
     setItems((prev) => prev.concat(data.results));
     page++;
   };
   ```

   async&await을 사용하여 api로부터 데이터를 페칭해오는 함수이다. fetch가 이루어질 때마다 page가 1씩 더해져 다음 fetch 때 다음 페이지의 데이터가 페칭되고, 그 데이터를 이전 `movies` state에 concat으로 합쳐줌으로써 `movies` 배열이 업데이트되고, 그에 따라 렌더가 다시 이루어져 화면에 나타나게 된다.

5. 최종 코드

   ```jsx
   const [items, setItems] = useState([]); // 추가된 부분
   const [target, setTarget] = useState(null);
   const page = 1;

   const fetchData = async () => {
     const response = await fetch(`/api/db/${page}`);
     const data = await response.json();
     setItems((prev) => prev.concat(data.results));
     page++;
   };

   // 추가된 부분
   useEffect(() => {
     fetchData();
   }, []);

   useEffect(() => {
     let observer;
     if (target) {
       const onIntersect = async ([entry], observer) => {
         if (entry.isIntersecting) {
           observer.unobserve(entry.target);
           await fetchData();
           observer.observe(entry.target);
         }
       };
       observer = new IntersectionObserver(onIntersect, { threshold: 1 }); // 추가된 부분
       observer.observe(target);
     }
     return () => observer && observer.disconnect();
   }, [target]);

   return (
     <div>
       {items.map((item, idx) => {
         <div>
           <img src=`/item/${item.image}` alt="item.img" />
           <div>{item.title}</div>
         </div>;
       })}
       <div ref={setTarget} style={targetStyle}>
         This is Target.
       </div>
     </div>
   );
   ```

   데이터를 담아줄 state를 생성하고, 초기값은 빈 배열로 설정해줬다. 또한 첫 렌더 때 첫 페칭이 이루어져야 하므로 `useEffect`로 첫 데이터를 호출해왔다. `IntersectionObserver()`에 `onIntersect`를 콜백함수로 전달해주고, `options`로는 `{threshold: 1}`을 전달해줌으로써 관찰하고 있는 대상이 화면에 완전히 보여야 `onIntersect`가 실행되도록 했다. 또한 `useEffect`의 `return`에 `disconnect()`를 넣어줬는데, 그 이유는 데이터 페칭이 완료되고 나면 업데이트 로직이 끝나기 때문에 clean-up을 통해 observer의 관찰을 일시정지하고 버그를 방지하기 위함이다.

### 발생할 수 있는 버그

- 첫 렌더 때 데이터 페칭이 2번 발생하는 현상  
   이는 리액트 생명주기 때문에 발생하는 버그이다. 생명주기 흐름 상 HTML이 먼저 렌더되고 이후 자바스크립트를 통해 hydration이 이루어지면서 데이터가 화면에 나타나게 되는데, 데이터가 화면에 나타나기 전에 observer가 관찰 중인 대상이 화면에 먼저 렌더되어 교차가 발생할 경우 `useEffect`와 `onIntersect`가 둘 다 실행되어 발생하는 버그이다. 이를 해결하기 위해선 조건부 렌더링을 활용하거나, Next.js와 같이 SSR 혹은 Static Site Generation이 가능한 프레임워크를 사용하여 초기화면을 구현해주면 해결할 수 있다.

참고: [문가네 개발 블로그 - Intersection Observer API란?](https://moon-ga.github.io/javascript/intersectionobserver/){:target="\_blank"}  
참고: [MDN - Intersection Observer API](https://developer.mozilla.org/ko/docs/Web/API/Intersection_Observer_API){:target="\_blank"}
