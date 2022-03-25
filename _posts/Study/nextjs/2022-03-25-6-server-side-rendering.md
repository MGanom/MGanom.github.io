---
title: "[Next.js] 페이지를 완전한 SSR로 전환하기(getServerSideProps) - (6)"
categories: Next.js
---

## getServerSideProps란?

Next.js는 기본적으로 SSR을 통해 pre-render를 한 html과 js파일을 클라이언트에게 전송하고 클라이언트 측에서 그 pre-render된 html에 js를 활용하여 hydration과 같은 CSR 과정을 통해 페이지 렌더를 완료한다. 이렇게 하면 클라이언트 측에서 렌더가 완료되기 전까지 빈 html을 보고 있지 않아도 되기에 UX적인 측면에서 좋은 점도 있지만, pre-render된 html에 hydration이 이루어지는 동안 로딩 화면이나 상호 작용이 불가능한 태그들을 보고 있어야 한다는 점도 존재한다. 이러한 상황을 싫어하는 사람들을 위해 아예 모든 과정을 SSR로 전환함으로써 클라이언트 측에서는 완성된 html을 받아볼 수 있는 방법이 있고, 그것이 바로 getServerSideProps가 하는 역할이다.

### 장점

- 사용자가 상호작용이 불가능하거나 빈 태그들, 혹은 로딩 화면 등을 보고 있지 않아도 되기 때문에 UX적인 측면에서 만족스러운 부분이 생긴다.
- HTML에 다양한 정보가 담겨있는 상태로 전달되기 때문에 SEO 측면에서 유리하다.

### 단점

- 사용자의 인터넷이 느리거나 서버에서 렌더해야 할 데이터의 크기가 크다면 브라우저에 페이지가 나타나기까지 시간이 오래 걸릴 수 있다.

### getServerSideProps 사용 예시

영화 인기 목록을 출력하는 페이지를 SSR로 만들어 보려 한다.
우선 SSR을 하고 싶은 페이지에 `getServerSideProps` 함수를 만들어 준다.

```jsx
export async function getServerSideProps() {
  const popular = await fetch(`http://localhost:3000/api/movies`)
    .then((res) => res.json())
    .then((res) => res.results);
  return {
    props: {
      popular,
    },
  };
}
```

이 함수의 이름은 반드시 `getServerSideProps`여야 하며, `props`를 반환해야 한다. 여기에선 `popular`라는 변수에 인기 영화 목록을 `fetch`로 받아와 할당해줬으며, 이를 `props`로 반환했다.

이제 이 `props`를 사용할 페이지 컴포넌트의 코드를 작성하면 된다.

```jsx
export default function Movie({ popular }) {
  return (
    <>
      {popular ? (
        popular.map((movie) => {
          return (
            <div key={movie.id}>
              <Image
                src={`https://image.tmdb.org/t/p/original/${movie.poster_path}`}
                alt={`${movie.original_title}`}
                width={500}
                height={500}
              />
              <div>{movie.original_title}</div>;
            </div>
          );
        })
      ) : (
        <h4>Loading...</h4>
      )}
    </>
  );
}
```

페이지 컴포넌트에서 앞서 작성했던 `getServerSideProps`에서 반환해준 `props`인 `popular`를 프롭스로 전달해 준다. 그리고 이 프롭스를 활용하여 코드를 작성해 주면 된다. 이렇게 하면 이 페이지는 완전히 SSR로 작동하게 되며, 서버 측에서 렌더링이 끝나면 클라이언트 측에서 완성된 HTML을 수신하여 브라우저에 표시할 수 있게 된다.

```jsx
{
  popular ? (
    popular.map((movie) => {
      return <div>mycode</div>;
    })
  ) : (
    <h4>Loading...</h4>
  );
}
```

참고로, 이 부분의 코드는 클라이언트 측에서 절대 확인할 수 없다. 왜냐하면 `popular`가 존재하지 않을 경우에 로딩을 출력하는 코드를 작성하였지만 페이지는 이미 서버 측에서 완성이 되었고 클라이언트 측에는 완성된 HTML이 넘어왔기 때문에 `popular`가 존재하지 않는 경우가 없기 때문이다. 그렇기 때문에 이 코드는 작성해주지 않아도 되지만, 참고용으로 넣어놨다.

참고: [Next.js 공식문서](https://nextjs.org/docs){:target="\_blank"}
