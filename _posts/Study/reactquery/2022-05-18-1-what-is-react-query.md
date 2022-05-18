---
title: "[React Query] React Query의 개념과 기본적인 사용법 - (1)"
categories: React_Query
---

## React Query란?

React Query는 data fetching 라이브러리 중 하나로, "fetching, caching, synchronizing, and updating server state"를 모토로 하는 라이브러리이다. 모토에 걸맞게 데이터가 페칭이 되었을 때 캐싱 중인 데이터와 내용이 다르다면 자동으로 데이터를 최신화하고 그 데이터로 컴포넌트를 업데이트 해주는 것도 가능하고, 서버에 데이터를 전송해 서버의 데이터를 업데이트할 수도 있다. 주로 비교되는 대상으로는 SWR, Apollo Clinet, RTK-Query, React Router가 있는데 [리액트 쿼리에서 제공하는 비교표](https://react-query.tanstack.com/comparison){:target="\_blank"}만 봐도 리액트 쿼리는 엄청나게 많은 기능을 지원한다는 것을 알 수 있다. 물론, 기능이 많은 만큼 배워야 할 기능도 많다는 뜻이며 러닝 커브가 높다고 볼 수 있다.

## 기본적인 사용법

리액트 쿼리를 앱에 적용하는 것 자체는 그렇게 어렵지 않다.

### QueryClient 연결

컴포넌트 전반에 걸쳐 사용할 수 있어야 하므로 `index.tsx`나 `App.tsx`에 연결하는 것이 일반적인데 주로 가독성을 위해 `index.tsx`에 임포트하는 게 좋다.

```jsx
// index.tsx
import { QueryClient } from "react-query";

const queryClient = new QueryClient();

const root = ReactDOM.createRoot(
  document.getElementById('root') as HTMLElement
);
root.render(
  <React.StrictMode>
    <QueryClientProvider client={queryClient}>
        <App />
    </QueryClientProvider>
  </React.StrictMode>
);
```

이런 식으로 `new QueryClient()`로 인스턴스를 생성하고, `<QueryClientProvider />`로 `<App />` 컴포넌트를 감싸준 다음 `props`로 인스턴스를 전달하면 된다.

```jsx
const queryClient = new QueryClient();
```

`QueryClient`에는 [다양한 옵션](https://react-query.tanstack.com/reference/QueryClient){:target="\_blank"}을 전달할 수 있는데, 여기에서 옵션을 추가하게 되면 앱 전반에 걸쳐 전역적인 옵션이 적용이 된다.

예를 들어,

```jsx
const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      cacheTime: Infinity,
      staleTime: Infinity,
    },
  },
});
```

이 옵션은 앱 전반에서 사용되는 `useQuery` 훅들에 대해 cacheTime과 staleTime을 Infinity로 설정한다는 의미이다.

### 데이터 페칭 함수 생성

데이터 페칭 함수란 말 그대로 데이터를 가져올 때 사용할 함수를 의미한다. 필요한 리액트 쿼리 메서드를 호출할 때 마다 콜백 함수로 정의해줘도 되고 사전에 미리 만들어두어도 된다. 이 때 사용하는 페칭 함수는 단순한 `fetch`도 가능하고 `axios`와 같은 라이브러리를 사용해도 무방하다.

간단한 예시를 들자면,

```jsx
const fetcher = async (url, params) => {
  try {
    const res = await axios.get(url, { params });
    return res.data;
  } catch (error) {
    return console.error(error);
  }
};
```

처럼 만들어서 필요에 따라 `fetcher('/api', { name: 'moon' })`처럼 가져다 쓰면 된다.

### useQuery 호출

이제 데이터를 페칭할 곳에서 `useQuery`를 호출하면 된다.

```jsx
import fetcher from "./fetcher";

const { isLoading, isError, data, error } = useQuery("data", fetcher);

if (isLoading) {
  return <span>Loading...</span>;

  if (isError) {
    return <span>Error: {error.message}</span>;
  }

  return (
    <ul>
      {data.map((item) => (
        <li key={item.id}>{item.name}</li>
      ))}
    </ul>
  );
}
```

`useQuery`에는 다양한 결과값이 존재하며, 그 중에서 `data`는 데이터 페칭 함수에 따른 결과값이 들어오게 된다. 또한 `isLoading`과 `isError`를 통해 데이터 페칭 중 보여줄 화면과 에러 발생 시 보여줄 화면을 분리할 수도 있고, 나아가 Suspense를 적용하는 것 또한 가능하다.

`useQuery`의 경우 기본적으로 페칭 이후 시간이 어느 정도 지났거나, 화면이 포커스 됐거나, 리페칭 시간을 정해둔 경우 자동으로 데이터가 업데이트가 되며 데이터가 업데이트가 되면 그에 맞춰 컴포넌트를 재렌더해주기 때문에 굉장히 reactive한 환경을 구축할 수 있게 해준다. 물론, 이러한 자동 업데이트를 지양하고 싶은 개발자들을 위해 이러한 옵션을 끄거나 조절하는 방법도 모두 존재하기 때문에 다양한 상황과 경우에 맞춰 데이터 페칭과 업데이트를 할 수 있다.

참고: [React Query 공식 문서](https://react-query.tanstack.com/overview){:target="\_blank"}
